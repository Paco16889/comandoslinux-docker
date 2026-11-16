# Healthcheck

Un **healthcheck** pregunta al contenedor de vez en cuando: “¿sigues sano?”.  
Docker marca el estado como `starting` → `healthy` / `unhealthy`. En Compose, eso ayuda más que un `depends_on` que solo espera el **arranque**.

> **Cómo leer la sintaxis:** `<algo>` = lo sustituyes tú · `[algo]` = opcional.  
> Detalle: [Cómo leer la sintaxis](../../README.md#cómo-leer-la-sintaxis-muy-importante).

## Ideas previas

| Concepto | En pocas palabras |
|----------|-------------------|
| Healthcheck | Comando periódico dentro del contenedor; código 0 = OK |
| `depends_on` | Espera a que el contenedor **exista/arranque**, no a que la app responda |
| `healthy` | El check está pasando |
| `unhealthy` | El check falla varias veces seguidas |
| Interval / timeout / retries | Cada cuánto, cuánto espera, cuántos fallos hasta unhealthy |

```text
depends_on:     "el proceso del contenedor arrancó"
healthcheck:    "el comando de salud responde OK (app lista)"
```

## Sintaxis

### En Dockerfile

```dockerfile
HEALTHCHECK --interval=<tiempo> --timeout=<tiempo> --retries=<n> --start-period=<tiempo> \
  CMD <comando>
```

O formato exec:

```dockerfile
HEALTHCHECK CMD curl -f http://127.0.0.1/ || exit 1
```

### En `docker run`

```bash
docker run -d --name <nombre> \
  --health-cmd='<comando>' \
  --health-interval=<tiempo> \
  --health-timeout=<tiempo> \
  --health-retries=<n> \
  <imagen>
```

### En Compose

```yaml
services:
  web:
    image: <imagen>
    healthcheck:
      test: ["CMD", "curl", "-f", "http://127.0.0.1:80"]
      interval: 10s
      timeout: 3s
      retries: 3
      start_period: 5s
```

Ver estado:

```bash
docker ps
docker inspect --format '{{.State.Health.Status}}' <contenedor>
```

## Ejemplos

### Nginx + healthcheck con `wget` (Alpine)

La imagen `nginx:alpine` suele traer `wget`:

```bash
docker run -d --name web-hc -p 8080:80 \
  --health-cmd='wget -q -O /dev/null http://127.0.0.1:80 || exit 1' \
  --health-interval=5s \
  --health-timeout=3s \
  --health-retries=3 \
  nginx:alpine
```

Espera unos segundos y mira:

```bash
docker ps --filter "name=web-hc"
```

Salida típica (columna STATUS):

```text
Up 20 seconds (healthy)
```

```bash
docker inspect --format '{{.State.Health.Status}}' web-hc
# healthy
```

### Forzar unhealthy (parar nginx dentro)

```bash
docker exec web-hc nginx -s stop
# el proceso principal puede morir y el contenedor salir;
# alternativa didáctica: health-cmd que falle a propósito en otro contenedor
```

Demo clara de fallo:

```bash
docker rm -f web-hc

docker run -d --name always-bad \
  --health-cmd='exit 1' \
  --health-interval=2s \
  --health-retries=2 \
  alpine:latest sleep 3600

sleep 6
docker ps --filter "name=always-bad"
# Up … (unhealthy)

docker inspect --format '{{.State.Health.Status}}' always-bad
# unhealthy
```

### HEALTHCHECK en Dockerfile

```bash
mkdir -p ~/demo-health
cd ~/demo-health

cat > Dockerfile << 'EOF'
FROM nginx:alpine
HEALTHCHECK --interval=5s --timeout=3s --retries=3 \
  CMD wget -q -O /dev/null http://127.0.0.1:80 || exit 1
EOF

docker build -t demo-hc:1 .
docker run -d --name web-df -p 8081:80 demo-hc:1

sleep 8
docker inspect --format '{{.State.Health.Status}}' web-df
# healthy
```

### Compose: healthcheck + `depends_on` con condición

```bash
mkdir -p ~/compose-health
cd ~/compose-health

cat > compose.yaml << 'EOF'
services:
  web:
    image: nginx:alpine
    ports:
      - "8080:80"
    healthcheck:
      test: ["CMD", "wget", "-q", "-O", "/dev/null", "http://127.0.0.1:80"]
      interval: 5s
      timeout: 3s
      retries: 3
      start_period: 2s

  caja:
    image: alpine:latest
    command: sleep 3600
    depends_on:
      web:
        condition: service_healthy
EOF

docker compose up -d
docker compose ps
```

Salida típica cuando va bien:

```text
… web-1    …  Up … (healthy)  …
… caja-1   …  Up …
```

`caja` no arranca del todo hasta que `web` está **healthy** (con la condición `service_healthy`).

Sin healthcheck, un `depends_on: [web]` clásico solo esperaba el start del contenedor (como en [compose-servicios](../05-compose/compose-servicios.md)).

### Ver el historial de checks

```bash
docker inspect --format '{{range .State.Health.Log}}{{.Start}} exit={{.ExitCode}}{{println}}{{end}}' web-df
```

## Errores frecuentes

- Usar `curl` en Alpine/`nginx:alpine` sin estar instalado → check falla siempre → `unhealthy`. Prueba `wget` o instala el cliente en la imagen.  
- Check contra `localhost:8080` **dentro** del contenedor cuando la app escucha en `80` → puerto interno incorrecto.  
- Confundir `healthy` con “publicado en el host”: el check corre **dentro**; `-p` es otro tema.  
- `depends_on` sin `condition: service_healthy` → no espera al healthcheck.  
- Intervalos muy cortos + comando pesado → ruido y carga inútil.

### Limpiar demos

```bash
docker rm -f web-hc always-bad web-df 2>/dev/null
cd ~/compose-health 2>/dev/null && docker compose down 2>/dev/null
```

## Siguiente

[Límites de recursos (`memory`, `cpus`)](limites-recursos.md)

## Relacionados

- Compose / depends_on: [compose-servicios](../05-compose/compose-servicios.md) · [compose-basico](../05-compose/compose-basico.md)  
- Anterior: [multi-stage](multi-stage.md)  
- Siguiente: [limites-recursos](limites-recursos.md)  
- [README de la carpeta](README.md)  
- Doc oficial: [HEALTHCHECK](https://docs.docker.com/reference/dockerfile/#healthcheck) · [Compose healthcheck](https://docs.docker.com/reference/compose-file/services/#healthcheck)



