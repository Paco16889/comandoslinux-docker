# Imágenes (`docker images`)

Una **imagen** es la plantilla (solo lectura) a partir de la cual nacen los contenedores.  
Aquí: listarlas, entender **tags**, ver el **historial** de capas, borrar con `rmi` y la idea de Docker Hub.

> **Cómo leer la sintaxis:** `<algo>` = lo sustituyes tú · `[algo]` = opcional.  
> Detalle: [Cómo leer la sintaxis](../../README.md#cómo-leer-la-sintaxis-muy-importante).

## Ideas previas

| Concepto | En pocas palabras |
|----------|-------------------|
| Imagen | Plantilla empaquetada (`nginx`, `alpine`…) |
| Tag | Etiqueta de versión: `alpine`, `24.04`, `latest`… |
| IMAGE ID | Identificador de esa imagen en tu disco |
| Capas | Trozo reutilizable; varias imágenes pueden compartir capas |
| Docker Hub | Registry público habitual (`docker pull` sin más datos) |
| `rmi` | Borrar una imagen local (no borra contenedores) |

```text
Docker Hub ──pull──► imagen en disco ──run──► contenedor
                         │
                         ├── docker images
                         ├── docker history
                         └── docker rmi
```

## Sintaxis

```bash
docker images
docker image ls

docker pull <imagen>[:<tag>]
docker rmi <imagen>[:<tag>]
docker rmi <IMAGE_ID>

docker history <imagen>[:<tag>]
docker tag <imagen_origen>[:<tag>] <nuevo_nombre>[:<tag>]
```

| Comando | Qué hace |
|---------|----------|
| `docker images` | Lista imágenes locales (alias: `docker image ls`) |
| `docker images -a` | Incluye capas intermedias (suele liarte al principio) |
| `docker images -q` | Solo IDs |
| `docker history` | Capas / “receta” aproximada de esa imagen |
| `docker tag` | Ponerle otro nombre:tag a la **misma** imagen |
| `docker rmi` | Borrar imagen (falla si un contenedor la sigue usando) |

## Ejemplos

### Listar lo que tienes

```bash
docker pull hello-world
docker pull nginx:alpine
docker pull alpine:latest

docker images
```

Salida típica:

```text
REPOSITORY     TAG       IMAGE ID       CREATED        SIZE
nginx          alpine    c1a3e2f4…      2 weeks ago    40.7MB
alpine         latest    91ef0fc3…      3 weeks ago    7.38MB
hello-world    latest    d2c94e1b…      4 months ago   5.26kB
```

| Columna | Idea |
|---------|------|
| `REPOSITORY` | Nombre (a veces `usuario/repo`) |
| `TAG` | Versión / variante |
| `IMAGE ID` | ID corto; sirve para `rmi` / `history` |
| `SIZE` | Tamaño aparente (capas compartidas → el disco real puede ser menos) |

### Filtrar por nombre

```bash
docker images nginx
docker images alpine
docker images --filter "reference=nginx*"
```

### Tags: por qué importan

```bash
docker pull ubuntu:24.04
docker pull ubuntu:22.04
docker images ubuntu
```

Misma familia, **dos** imágenes distintas. `latest` no es magia: apunta a un tag concreto que el mantenedor decide.

```bash
docker pull nginx:latest
docker pull nginx:alpine
# latest suele ser más “completa”; alpine es más pequeña
```

### Ver el historial de capas

```bash
docker history nginx:alpine
```

Algo así (resumido):

```text
IMAGE          CREATED        CREATED BY                                      SIZE
c1a3e2f4…      2 weeks ago    /bin/sh -c #(nop)  CMD ["nginx" "-g" "dae…   0B
…              …              /bin/sh -c #(nop)  EXPOSE 80                   0B
…              …              /bin/sh -c apk add … nginx …                   15MB
```

Te ayuda a intuir qué instrucciones del Dockerfile generaron cada capa (el siguiente tema).

### Poner otro nombre local (`tag`) — no descarga otra vez

```bash
docker tag nginx:alpine mi-nginx:prueba
docker images
# misma IMAGE ID, dos nombres
```

Útil para practicar o antes de un `docker build -t`.

### Borrar una imagen

```bash
docker rmi mi-nginx:prueba
# Untagged: mi-nginx:prueba
# (si nadie más usa ese ID, también Deleted: …)
```

Si un contenedor (aunque esté **Exited**) la usa:

```bash
docker run -d --name web-tmp nginx:alpine
docker rmi nginx:alpine
# Error: conflict: unable to remove repository reference … (must force)
# o: image is being used by …

docker rm -f web-tmp
docker rmi nginx:alpine
```

### Limpiar imágenes huérfanas (sin tag) — opcional

```bash
docker image prune
# pide confirmación; borra “dangling” (sin nombre útil)
```

Más agresivo (ojo: borra imágenes no usadas por ningún contenedor):

```bash
# docker image prune -a
```

### Idea de Docker Hub

Sin registry raro, `docker pull nginx:alpine` busca en **Docker Hub**.  
En el navegador: [hub.docker.com](https://hub.docker.com) → busca `nginx`, `alpine`, etc. y mira tags oficiales.

No hace falta cuenta para **bajar** imágenes públicas habituales.

## Errores frecuentes

- Confundir `docker images` con `docker ps`: uno lista **plantillas**, el otro **contenedores**.
- `rmi` falla porque queda un contenedor parado → `docker ps -a` y `docker rm` primero.
- Borrar `nginx:alpine` pensando que borras “todos los nginx”: solo ese **tag**; otros tags pueden seguir.
- Usar siempre `latest` en tutoriales serios: mejor un tag fijo (`24.04`, `1.27-alpine`…) para repetir el mismo resultado.
- Asustarte con `SIZE`: capas compartidas; no sumes a ciegas todos los tamaños como si fueran exclusivos.


## Siguiente

[Quita la página por defecto y pon la tuya](dockerfile.md)

## Relacionados

- Contenedores: [01-fundamentos](../01-fundamentos/) · [docker-stop-start-rm](../01-fundamentos/docker-stop-start-rm.md) (`rmi` básico)  
- Siguiente: [dockerfile](dockerfile.md) — receta para construir tu imagen  
- Luego: [docker-build](docker-build.md) — `docker build -t`  
- [README de la carpeta](README.md)  
- Doc oficial: [docker image](https://docs.docker.com/reference/cli/docker/image/) · [Docker Hub](https://docs.docker.com/docker-hub/)  
