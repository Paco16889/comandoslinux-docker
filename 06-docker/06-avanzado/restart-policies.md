# Políticas de reinicio (`--restart`)

Una **política de reinicio** dice qué hacer si el contenedor se para o si reinicias el PC/servicio Docker.  
Útil para un nginx o un bot que quieres que “vuelva solo”.

> **Cómo leer la sintaxis:** `<algo>` = lo sustituyes tú · `[algo]` = opcional.  
> Detalle: [Cómo leer la sintaxis](../../README.md#cómo-leer-la-sintaxis-muy-importante).

## Ideas previas

| Concepto | En pocas palabras |
|----------|-------------------|
| Política `no` | Por defecto: si para, se queda parado |
| `on-failure` | Reinicia solo si salió con **error** (código ≠ 0) |
| `always` | Reinicia siempre (también tras reinicio del demonio) |
| `unless-stopped` | Como `always`, pero si tú lo paraste a mano (`stop`), no lo revive al arrancar Docker |
| Compose `restart:` | Misma idea en el YAML del servicio |

```text
Contenedor cae / host reinicia
        │
        └── ¿restart policy?  →  ¿lo vuelvo a levantar?
```

## Sintaxis

### `docker run`

```bash
docker run -d --name <nombre> --restart <politica> <imagen>
```

| Política | Comportamiento |
|----------|----------------|
| `no` | No reinicia (default) |
| `on-failure[:<max>]` | Solo si falló; opcional tope de intentos |
| `always` | Siempre reinicia |
| `unless-stopped` | Siempre, salvo que lo hayas parado tú |

Cambiar la política de un contenedor ya creado:

```bash
docker update --restart <politica> <contenedor>
```

### Compose

```yaml
services:
  web:
    image: nginx:alpine
    restart: unless-stopped
```

## Ejemplos

### `unless-stopped` (la más habitual en lab/servidor casero)

```bash
docker run -d --name web-restart \
  --restart unless-stopped \
  -p 8080:80 \
  nginx:alpine
# a1b2c3d4e5f6…   (ID del contenedor)

docker inspect --format '{{.HostConfig.RestartPolicy.Name}}' web-restart
# unless-stopped

docker ps --filter "name=web-restart"
# … Up …   0.0.0.0:8080->80/tcp   web-restart
```

Si el proceso muere o reinicias el servicio `docker`, tenderá a volver.  
Si haces `docker stop web-restart`, al arrancar Docker **no** lo levanta solo (porque lo paraste tú).

```bash
docker stop web-restart
# web-restart

docker ps -a --filter "name=web-restart"
# … Exited (0) …   web-restart
```

### `always`

```bash
docker run -d --name web-always \
  --restart always \
  -p 8081:80 \
  nginx:alpine
# ID…

docker inspect --format '{{.HostConfig.RestartPolicy.Name}}' web-always
# always
```

Aunque lo pares con `stop`, tras reiniciar el **daemon** Docker puede volver a arrancarlo. Por eso muchos prefieren `unless-stopped` en casa.

### `on-failure`

```bash
docker run -d --name solo-si-falla \
  --restart on-failure:5 \
  alpine:latest sleep 3600
# ID…

docker inspect --format \
  'name={{.HostConfig.RestartPolicy.Name}} max={{.HostConfig.RestartPolicy.MaximumRetryCount}}' \
  solo-si-falla
# name=on-failure max=5
```

Si sale con código 0 (éxito), no reinicia. Si falla, reintenta hasta 5 veces.

### Ver la política

```bash
docker inspect --format \
  'name={{.HostConfig.RestartPolicy.Name}} max={{.HostConfig.RestartPolicy.MaximumRetryCount}}' \
  web-restart
# name=unless-stopped max=0
```

### Cambiar sin recrear

```bash
docker update --restart no web-restart
docker inspect --format '{{.HostConfig.RestartPolicy.Name}}' web-restart
# no
```

### Compose

```bash
mkdir -p ~/compose-restart
cd ~/compose-restart

cat > compose.yaml << 'EOF'
services:
  web:
    image: nginx:alpine
    ports:
      - "8080:80"
    restart: unless-stopped
EOF

docker compose up -d
```

```text
[+] Running 2/2
 ✔ Network compose-restart_default  Created
 ✔ Container compose-restart-web-1  Started
```

```bash
docker compose ps
# … Up …   0.0.0.0:8080->80/tcp

docker inspect --format '{{.HostConfig.RestartPolicy.Name}}' compose-restart-web-1
# unless-stopped
```

(El nombre exacto del contenedor puede variar; mira `docker compose ps`.)

### Relación con healthcheck

`restart` actúa cuando el contenedor **termina** o según la política al arrancar Docker.  
`unhealthy` **no** implica por sí solo un reinicio (hace falta orquestación extra o que el proceso muera). Ver [healthcheck](healthcheck.md).

## Errores frecuentes

- Usar `always` y sorprenderte de que un contenedor que paraste “vuelve” tras reiniciar el PC.  
- Confundir `docker stop` + `unless-stopped` (no revive al boot) con `always` (sí puede).  
- Pensar que `restart` arregla una app mal configurada: si falla al instante en bucle, solo tienes un bucle de reinicios.  
- Olvidar la política en Compose y recrear a mano cada vez que cae el servicio.

### Limpiar

```bash
docker rm -f web-restart web-always solo-si-falla 2>/dev/null
cd ~/compose-restart 2>/dev/null && docker compose down 2>/dev/null
```

## Siguiente

[Seguridad básica (tips prácticos)](seguridad-basica.md)

## Relacionados

- Anterior (pack 4): [limites-recursos](limites-recursos.md)  
- Siguiente: [seguridad-basica](seguridad-basica.md)  
- Ciclo de vida: [docker-stop-start-rm](../01-fundamentos/docker-stop-start-rm.md)  
- Compose: [compose-basico](../05-compose/compose-basico.md)  
- [README de la carpeta](README.md)  
- Doc oficial: [Start containers automatically](https://docs.docker.com/engine/containers/start-containers-automatically/) · [Compose restart](https://docs.docker.com/reference/compose-file/services/#restart)



