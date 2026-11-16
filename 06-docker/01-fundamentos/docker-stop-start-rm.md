# docker stop / start / restart / rm / rmi

Ciclo de vida del contenedor: **parar**, **arrancar de nuevo**, **reiniciar** y **borrar**.  
Al final, un `rmi` básico para borrar **imágenes** (no confundir con borrar contenedores).

> **Cómo leer la sintaxis:** `<algo>` = lo sustituyes tú · `[algo]` = opcional.  
> Detalle: [Cómo leer la sintaxis](../../README.md#cómo-leer-la-sintaxis-muy-importante).

## Ideas previas

| Concepto | En pocas palabras |
|----------|-------------------|
| `stop` | Pide al proceso que termine (apagado ordenado) y deja el contenedor **parado** |
| `start` | Vuelve a arrancar un contenedor **ya creado** (mismo nombre/ID) |
| `restart` | `stop` + `start` en uno |
| `rm` | Borra un **contenedor** (suele hacer falta que esté parado) |
| `rmi` | Borra una **imagen** del disco |
| `--rm` en `run` | Borra el contenedor al terminar (no es el comando `rm`) |

```text
run  →  Up  →  stop  →  Exited  →  start  →  Up
                      ↘
                       rm  →  (desaparece)
```

Borrar un contenedor **no** borra la imagen. Borrar una imagen **no** borra contenedores que la usen (y a menudo falla si aún existen).

## Sintaxis

```bash
docker stop <contenedor> [<contenedor>…]
docker start <contenedor> [<contenedor>…]
docker restart <contenedor> [<contenedor>…]

docker rm <contenedor> [<contenedor>…]
docker rm -f <contenedor>

docker rmi <imagen> [<imagen>…]
```

| Comando / opción | Qué hace |
|------------------|----------|
| `stop -t <seg>` | Espera N segundos antes de forzar (SIGKILL) |
| `rm -f` | Fuerza: para y borra (útil si está Up) |
| `rm -v` | Intenta borrar también volúmenes anónimos asociados |
| `rmi -f` | Fuerza borrado de imagen (con cuidado) |

## Ejemplos

### Parar un contenedor en marcha

```bash
docker run -d --name web-demo -p 8080:80 nginx:alpine
docker ps
# … Up …   web-demo

docker stop web-demo
# web-demo   ← imprime el nombre (o el ID) al pararlo

docker ps -a
# … Exited (0) …   web-demo
```

### Arrancar de nuevo el mismo contenedor

```bash
docker start web-demo
# web-demo

docker ps
# otra vez Up; el nombre y el -p que tenía al crearlo se mantienen
```

### Reiniciar

```bash
docker restart web-demo
# web-demo
```

Útil tras cambiar algo en el host o si el servicio “se ha quedado raro”.

### Parar varios a la vez

```bash
docker stop web-demo otro-nombre
docker stop $(docker ps -q)
# para TODOS los que están en marcha (ojo en máquinas compartidas)
```

### Borrar un contenedor parado

```bash
docker stop web-demo
docker rm web-demo
docker ps -a --filter "name=web-demo"
# ya no debería aparecer
```

### Borrar aunque esté en marcha (`-f`)

```bash
docker rm -f web-demo
```

### Limpiar contenedores que ya terminaron (Exited)

```bash
docker ps -aq --filter "status=exited"
docker rm $(docker ps -aq --filter "status=exited")
```

O el atajo moderno (cuando lo tengas):

```bash
docker container prune
# pide confirmación; borra contenedores parados
```

### Borrar una imagen (`rmi`) — básico

Primero mira qué tienes:

```bash
docker images
```

Salida típica:

```text
REPOSITORY   TAG       IMAGE ID       CREATED        SIZE
nginx        alpine    c1a3e2…        2 weeks ago    40MB
alpine       latest    91ef0f…        3 weeks ago    7MB
```

Luego:

```bash
docker rmi <imagen>:<tag>
docker rmi nginx:alpine
# Untagged: nginx:alpine
# Deleted: sha256:…

docker rmi <IMAGE_ID>
```

Si un contenedor (aunque esté parado) **sigue referenciando** esa imagen, `rmi` fallará hasta que borres esos contenedores.

```bash
docker rm <contenedor>
docker rmi nginx:alpine
```

### Flujo completo de práctica

```bash
docker pull alpine:latest

docker run -d --name prueba-ciclo alpine:latest sleep 3600
docker ps

docker stop prueba-ciclo
docker start prueba-ciclo
docker restart prueba-ciclo

docker rm -f prueba-ciclo
# la imagen alpine sigue en el disco
docker images
```

## Errores frecuentes

- `You cannot remove a running container` → haz `docker stop` antes, o `docker rm -f`.
- `Conflict… name already in use` al hacer `run --name …` → el contenedor viejo sigue existiendo (`ps -a`); `rm` o elige otro nombre.
- Confundir `rm` (contenedor) con `rmi` (imagen): borras lo incorrecto o te frustas porque “sigue ocupando sitio”.
- `image is being used by running container` / `… by stopped container` → borra primero el contenedor, luego la imagen.
- `docker start` en un nombre que nunca existió → error; `start` no descarga ni crea, solo reanima uno ya creado con `run`.
- Hacer `docker stop $(docker ps -q)` en un servidor ajeno sin pensarlo: paras **todos** los contenedores en marcha.

## Relacionados

- Anterior: [docker-ps-logs](docker-ps-logs.md)  
- Siguiente: [docker-exec](docker-exec.md) — ejecutar comandos / shell dentro  
- Imágenes con más detalle: [02-imagenes-dockerfile](../02-imagenes-dockerfile/)  
- [README de la carpeta](README.md)  
- Doc oficial: [docker stop](https://docs.docker.com/reference/cli/docker/container/stop/) · [start](https://docs.docker.com/reference/cli/docker/container/start/) · [rm](https://docs.docker.com/reference/cli/docker/container/rm/) · [rmi](https://docs.docker.com/reference/cli/docker/image/rm/)  
