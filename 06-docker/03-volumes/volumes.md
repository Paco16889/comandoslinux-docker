# docker volume

Los **volumes** los gestiona Docker: los creas, los montas en un contenedor con `-v <nombre>:<ruta>` y los datos **siguen** aunque borres el contenedor.

> **Cómo leer la sintaxis:** `<algo>` = lo sustituyes tú · `[algo]` = opcional.  
> Detalle: [Cómo leer la sintaxis](../../README.md#cómo-leer-la-sintaxis-muy-importante).

## Ideas previas

| Concepto | En pocas palabras |
|----------|-------------------|
| Volume nombrado | Tiene nombre legible (`datos-web`) |
| Driver `local` | El habitual en un solo PC/servidor |
| Montar | Hacer visible el volume en una ruta **dentro** del contenedor |
| `volume rm` | Borra el volume (y sus datos) si nadie lo usa |
| `volume prune` | Limpia volumes **no usados** por ningún contenedor |

```text
docker volume create datos-web
        │
        ▼
docker run -v datos-web:/usr/share/nginx/html …
        │
        ▼
docker rm contenedor   →  el volume datos-web sigue
```

## Sintaxis

```bash
docker volume create <nombre>
docker volume ls
docker volume inspect <nombre>
docker volume rm <nombre>
docker volume prune
```

Montar al crear el contenedor:

```bash
docker run … -v <nombre_volume>:<ruta_contenedor> …
docker run … --mount type=volume,source=<nombre_volume>,target=<ruta_contenedor> …
```

| Comando | Qué hace |
|---------|----------|
| `create` | Crea un volume vacío (nombre tuyo) |
| `ls` | Lista volumes |
| `inspect` | Detalle (punto de montaje en el host, labels…) |
| `rm` | Borra uno o varios (si no están en uso) |
| `prune` | Borra los que no usa ningún contenedor |

Si montas un nombre que **aún no existe**, Docker suele **crearlo solo** al hacer `run`. Crear antes con `create` es más claro para novatos.

## Ejemplos

### Crear y listar

```bash
docker volume create datos-web
docker volume ls
```

Salida típica:

```text
DRIVER    VOLUME NAME
local     datos-web
```

### Inspeccionar

```bash
docker volume inspect datos-web
```

Campos útiles (resumidos):

```text
[
  {
    "Name": "datos-web",
    "Driver": "local",
    "Mountpoint": "/var/lib/docker/volumes/datos-web/_data",
    ...
  }
]
```

Ese `Mountpoint` es la carpeta real en el host (hace falta root/`sudo` para mirarla a mano). Para el día a día **no hace falta** entrar ahí: monta el volume en un contenedor.

```bash
docker volume inspect --format '{{.Mountpoint}}' datos-web
# /var/lib/docker/volumes/datos-web/_data
```

### Montar en un contenedor (`-v`)

```bash
docker run -d --name web-datos \
  -p 8080:80 \
  -v datos-web:/usr/share/nginx/html \
  nginx:alpine
```

Al montar sobre `/usr/share/nginx/html`, si el volume está vacío puede “tapar” la página por defecto de nginx (a veces ves 403 o vacío hasta que pongas un `index.html`).

Pon contenido **usando** el volume:

```bash
docker run --rm -v datos-web:/data alpine:latest \
  sh -c 'echo "<h1>Persistente</h1>" > /data/index.html'

curl http://127.0.0.1:8080
# Persistente
```

### Borrar el contenedor: los datos siguen

```bash
docker rm -f web-datos

docker run --rm -v datos-web:/data alpine:latest cat /data/index.html
# <h1>Persistente</h1>
```

Recrear el servicio con el **mismo** volume:

```bash
docker run -d --name web-datos \
  -p 8080:80 \
  -v datos-web:/usr/share/nginx/html \
  nginx:alpine

curl http://127.0.0.1:8080
# Persistente
```

### Varios contenedores, mismo volume (idea)

```bash
docker run -d --name lector -v datos-web:/data alpine:latest sleep 3600
docker exec lector cat /data/index.html
# <h1>Persistente</h1>
```

Ojo: dos procesos escribiendo a la vez en los mismos ficheros puede liar (bases de datos, etc.). Para aprender a leer/escribir simple, vale.

### Crear el volume “al vuelo”

```bash
docker run -d --name web-auto \
  -v datos-auto:/usr/share/nginx/html \
  nginx:alpine

docker volume ls
# aparece datos-auto aunque no hicieras create antes
```

### Borrar un volume

Primero quita contenedores que lo usen:

```bash
docker rm -f web-datos web-auto lector 2>/dev/null

docker volume rm datos-web
# datos-web

docker volume ls
# datos-web ya no está
```

Si aún está en uso:

```text
Error response from daemon: remove datos-web: volume is in use
```

### Limpiar volumes huérfanos

```bash
docker volume prune
# WARNING! …
# borra volumes no usados por ningún contenedor
```

## Errores frecuentes

- `volume is in use` → `docker ps -a` y elimina/para el contenedor que lo monta; luego `volume rm`.
- Montar `-v datos-web:/ruta` y no ver los ficheros que esperabas → el mount **oculta** el contenido original de esa ruta en la imagen.
- Confundir nombre del volume con nombre del contenedor (`--name`).
- Hacer `volume rm` pensando que es seguro “por si acaso” → **borra los datos** de ese volume.
- Buscar los datos solo con `ls` en tu home: el volume “oficial” vive bajo `/var/lib/docker/volumes/…` (gestionado por Docker).


## Siguiente

[Hola bind](bind-mounts.md)

## Relacionados

- Anterior: [conceptos-datos](conceptos-datos.md)  
- Siguiente: [bind-mounts](bind-mounts.md) — montar carpetas de tu Linux  
- Contenedores: [docker-pull-run](../01-fundamentos/docker-pull-run.md) · [docker-exec](../01-fundamentos/docker-exec.md)  
- [README de la carpeta](README.md)  
- Doc oficial: [Volumes](https://docs.docker.com/engine/storage/volumes/) · [docker volume](https://docs.docker.com/reference/cli/docker/volume/)  
