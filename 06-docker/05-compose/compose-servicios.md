# Varios servicios (volumes y networks en YAML)

Un `compose.yaml` con **2+ servicios**, una **red** del proyecto y un **volume** para datos.  
Ejemplo novato: `web` (nginx) + `caja` (Alpine) que habla con `web` **por nombre**, más datos persistentes.

> **Cómo leer la sintaxis:** `<algo>` = lo sustituyes tú · `[algo]` = opcional.  
> Detalle: [Cómo leer la sintaxis](../../README.md#cómo-leer-la-sintaxis-muy-importante).

## Ideas previas

| Concepto | En pocas palabras |
|----------|-------------------|
| Varios `services:` | Cada clave es un servicio (`web`, `caja`…) |
| Red del proyecto | Compose crea una red; los servicios se ven por **nombre de servicio** |
| Volume nombrado | Bajo `volumes:` en el YAML + montaje en el servicio |
| DNS | Igual que en [dns-entre-contenedores](../04-networks/dns-entre-contenedores.md): `http://web:80` |
| Dependencia | `depends_on` solo espera a que **arranque** el contenedor (no “ya está listo del todo”) |

```text
compose.yaml
├── services:
│   ├── web   (nginx + volume + ports)
│   └── caja  (alpine; curl a http://web)
├── volumes:
│   └── web-data
└── networks:
    └── appnet   (opcional; si omites, Compose crea una default)
```

## Sintaxis (piezas del YAML)

```yaml
services:
  <servicio>:
    image: <imagen>[:<tag>]
    ports:
      - "<host>:<contenedor>"
    volumes:
      - <nombre_volume>:<ruta_contenedor>
    networks:
      - <nombre_red>
    depends_on:
      - <otro_servicio>

volumes:
  <nombre_volume>:

networks:
  <nombre_red>:
```

Si **no** declares `networks:`, Compose crea automáticamente `<proyecto>_default` y mete ahí todos los servicios (para novatos suele bastar).

## Ejemplos

### Proyecto demo

```bash
mkdir -p ~/compose-multi
cd ~/compose-multi
```

### Fichero completo (recomendado para practicar)

```bash
cat > compose.yaml << 'EOF'
services:
  web:
    image: nginx:alpine
    ports:
      - "8080:80"
    volumes:
      - web-data:/usr/share/nginx/html
    networks:
      - appnet

  caja:
    image: alpine:latest
    command: sleep 3600
    depends_on:
      - web
    networks:
      - appnet

volumes:
  web-data:

networks:
  appnet:
EOF
```

### Levantar y comprobar

```bash
docker compose up -d
```

Salida típica (resumida):

```text
[+] Running 4/4
 ✔ Network compose-multi_appnet      Created
 ✔ Volume compose-multi_web-data     Created
 ✔ Container compose-multi-web-1     Started
 ✔ Container compose-multi-caja-1    Started
```

```bash
docker compose ps
```

```text
NAME                      IMAGE          STATUS    PORTS
compose-multi-caja-1      alpine:…       Up …
compose-multi-web-1       nginx:alpine   Up …      0.0.0.0:8080->80/tcp
```

### Poner HTML en el volume y verlo desde el host

```bash
docker compose exec web sh -c 'echo "<h1>Hola Compose</h1>" > /usr/share/nginx/html/index.html'

curl http://127.0.0.1:8080
# Hola Compose
```

### DNS: `caja` llama a `web` por nombre de servicio

```bash
docker compose exec caja apk add --no-cache curl
docker compose exec caja curl -s -o /dev/null -w "%{http_code}\n" http://web:80
# 200

docker compose exec caja curl -s http://web:80
# Hola Compose
```

El hostname es el nombre del **servicio** en el YAML (`web`), no hace falta inventar IPs.

### Logs de un servicio o de todos

```bash
docker compose logs --tail 15 web
docker compose logs -f
# Ctrl+C
```

### Versión mínima (solo red default, sin bloquear `networks:`)

También válido para aprender:

```yaml
services:
  web:
    image: nginx:alpine
    ports:
      - "8080:80"
  caja:
    image: alpine:latest
    command: sleep 3600
    depends_on:
      - web
```

Compose crea la red `…_default` sola; `http://web:80` sigue funcionando.

### Bind mount en Compose (código del host)

```yaml
services:
  web:
    image: nginx:alpine
    ports:
      - "8080:80"
    volumes:
      - /home/<usuario>/mi-web:/usr/share/nginx/html:ro
```

O con ruta relativa al proyecto (Compose la resuelve desde la carpeta del fichero):

```yaml
    volumes:
      - ./html:/usr/share/nginx/html:ro
```

```bash
mkdir -p ~/compose-multi/html
echo '<h1>Bind</h1>' > ~/compose-multi/html/index.html
# ajusta el YAML y:
docker compose up -d
curl http://127.0.0.1:8080
# Bind
```

### Bajar y qué se conserva

```bash
docker compose down
# contenedores + red del proyecto fuera
```

El volume `web-data` **sigue** (salvo que uses `-v`):

```bash
docker volume ls | grep web-data
# local   compose-multi_web-data
```

Borrar también volumes declarados:

```bash
docker compose down -v
# Volume compose-multi_web-data  Removed
```

### Ejecutar un comando puntual en un servicio

```bash
docker compose up -d
docker compose exec web hostname
# … (hostname del contenedor)

docker compose exec caja uname -a
```

Equivale a `docker exec` sobre el contenedor de ese servicio.

## Errores frecuentes

- Usar `http://web:8080` entre servicios cuando nginx escucha en **80** dentro → puerto interno, no el del host.
- `depends_on` no espera a que nginx “ya sirva HTTP”; solo al arranque del contenedor. Si fallas por timing, reintenta o más adelante healthcheck (avanzado).
- `docker compose down -v` sin querer → borra datos del volume del proyecto.
- Nombre de servicio con mayúsculas/espacios raros → mejor `web`, `db`, `api` en minúsculas.
- Olvidar `command: sleep 3600` en Alpine → el contenedor `caja` sale al instante (`Exited`) y no puedes hacer `exec`.


## Siguiente

[Contexto de build y `.dockerignore`](../06-avanzado/dockerignore-contexto.md)

## Relacionados

- Anterior: [compose-basico](compose-basico.md) · [que-es-compose](que-es-compose.md)  
- Manual: [04-networks](../04-networks/) · [03-volumes](../03-volumes/)  
- Siguiente bloque: [06-avanzado](../06-avanzado/)  
- [README de la carpeta](README.md)  
- Doc oficial: [Compose file](https://docs.docker.com/reference/compose-file/) · [depends_on](https://docs.docker.com/reference/compose-file/services/#depends_on) · [Networks in Compose](https://docs.docker.com/compose/how-tos/networking/)  
