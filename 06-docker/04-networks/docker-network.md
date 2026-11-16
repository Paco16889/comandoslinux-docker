# docker network

Crear redes propias, listarlas, conectar/desconectar contenedores y borrarlas.  
Una red de usuario (bridge) es el cimiento para que varios contenedores se vean **por nombre**.

> **Cómo leer la sintaxis:** `<algo>` = lo sustituyes tú · `[algo]` = opcional.  
> Detalle: [Cómo leer la sintaxis](../../README.md#cómo-leer-la-sintaxis-muy-importante).

## Ideas previas

| Concepto | En pocas palabras |
|----------|-------------------|
| Red de usuario | La que creas con `docker network create <nombre>` |
| Driver `bridge` | El habitual en un solo PC (default al crear) |
| `--network` | Engancha el contenedor a esa red **al crearlo** |
| `connect` | Engancha un contenedor **ya existente** a otra red |
| `disconnect` | Lo saca de esa red |
| `rm` | Borra la red (si no tiene contenedores enganchados) |

```text
docker network create lab
        │
        ├── docker run --network lab --name web …
        └── docker run --network lab --name api …
                 (ambos en "lab" → pueden hablarse)
```

## Sintaxis

```bash
docker network create <nombre>
docker network create --driver bridge <nombre>

docker network ls
docker network inspect <nombre>

docker network connect <red> <contenedor>
docker network disconnect <red> <contenedor>

docker network rm <nombre>
docker network prune
```

Al crear el contenedor:

```bash
docker run -d --name <nombre> --network <red> [ -p … ] <imagen>
```

| Comando | Qué hace |
|---------|----------|
| `create` | Crea la red |
| `ls` | Lista redes |
| `inspect` | Detalle (subnet, contenedores…) |
| `connect` / `disconnect` | Mover un contenedor vivo entre redes |
| `rm` | Borrar red vacía |
| `prune` | Borrar redes de usuario no usadas |

No borres a la ligera `bridge`, `host` ni `none` (redes del sistema).

## Ejemplos

### Crear y listar

```bash
docker network create lab
docker network ls
```

Salida típica:

```text
NETWORK ID     NAME      DRIVER    SCOPE
…              bridge    bridge    local
…              lab       bridge    local
…              host      host      local
…              none      null      local
```

### Inspeccionar

```bash
docker network inspect lab
```

Útil en formato corto:

```bash
docker network inspect lab --format '{{.Name}} {{.Driver}}'
# lab bridge

docker network inspect lab --format '{{range .IPAM.Config}}{{.Subnet}}{{end}}'
# 172.18.0.0/16   (el número concreto puede variar)
```

### Contenedores en esa red desde el `run`

```bash
docker run -d --name web-lab --network lab -p 8080:80 nginx:alpine
docker run -d --name caja-lab --network lab alpine:latest sleep 3600

docker network inspect lab --format '{{range $k,$v := .Containers}}{{$v.Name}} {{end}}'
# web-lab caja-lab
```

Comprobar que el contenedor está en `lab`:

```bash
docker inspect --format '{{json .NetworkSettings.Networks}}' web-lab
# … "lab": { … "IPAddress": "172.18.0.2" … }
```

(IPs de ejemplo; en tu máquina serán otras.)

### Conectar un contenedor que nació en otra red

```bash
docker run -d --name web-default -p 8081:80 nginx:alpine
# por defecto está en "bridge"

docker network connect lab web-default
docker network inspect lab --format '{{range $k,$v := .Containers}}{{$v.Name}} {{end}}'
# … web-default …
```

Un contenedor puede estar en **varias** redes a la vez.

### Desconectar

```bash
docker network disconnect lab web-default
```

### Publicar puerto y red a la vez

```bash
docker run -d --name web-pub \
  --network lab \
  -p 8090:80 \
  nginx:alpine

curl -I http://127.0.0.1:8090
# HTTP/1.1 200 OK
```

`--network` = cómo habla con **otros contenedores**.  
`-p` = cómo entras **tú** desde el host. Las dos se combinan sin problema.

### Borrar la red

Primero quita contenedores (o desconéctalos):

```bash
docker rm -f web-lab caja-lab web-default web-pub 2>/dev/null

docker network rm lab
# lab

docker network ls
# lab ya no aparece
```

Si aún hay alguien dentro:

```text
Error response from daemon: network lab has active endpoints
```

### Limpiar redes huérfanas (opcional)

```bash
docker network prune
# borra redes de usuario sin contenedores
```

## Errores frecuentes

- `network … has active endpoints` → `docker network inspect <red>` y `rm -f` / `disconnect` los contenedores.
- Escribir mal el nombre en `--network` → `network … not found`; mira `docker network ls`.
- Confundir nombre de **red** con nombre de **contenedor**.
- Intentar `network rm bridge` → no (red del sistema).
- Crear la red y olvidar `--network` en el `run` → el contenedor se va a `bridge` default y no entra en tu `lab`.

## Relacionados

- Anterior: [conceptos-red](conceptos-red.md)  
- Siguiente: [dns-entre-contenedores](dns-entre-contenedores.md) — resolver por nombre  
- Fundamentos: [docker-pull-run](../01-fundamentos/docker-pull-run.md) · [docker-inspect-port](../01-fundamentos/docker-inspect-port.md)  
- [README de la carpeta](README.md)  
- Doc oficial: [docker network](https://docs.docker.com/reference/cli/docker/network/) · [Bridge network driver](https://docs.docker.com/engine/network/drivers/bridge/)  
