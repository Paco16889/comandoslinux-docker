# DNS entre contenedores (por nombre)

En una **red de usuario** que tú creas, Docker resuelve el **nombre del contenedor** a su IP.  
Así `web` puede llamar a `db` sin memorizar IPs que cambian al recrear contenedores.

> **Cómo leer la sintaxis:** `<algo>` = lo sustituyes tú · `[algo]` = opcional.  
> Detalle: [Cómo leer la sintaxis](../../README.md#cómo-leer-la-sintaxis-muy-importante).

## Ideas previas

| Concepto | En pocas palabras |
|----------|-------------------|
| DNS interno | Docker hace de “agenda” de nombres dentro de esa red |
| Nombre (`--name`) | Hostname por el que otros te encuentran |
| Misma red | Sin estar en la misma red de usuario, el nombre suele **no** resolverse |
| Alias | Nombre extra (`--network-alias`) por si quieres otro apodo |
| Puerto interno | Entre contenedores usas el puerto **del servicio** (ej. `80`), no el `-p` del host |

```text
Red "lab"
┌─────────────┐         nombre "web"          ┌─────────────┐
│ caja-lab    │ ──── curl http://web:80 ────► │ web-lab :80 │
└─────────────┘                               └─────────────┘
```

Desde el host sigues usando `-p` y `127.0.0.1:<puerto_host>`.  
Entre contenedores: `http://<nombre>:<puerto_interno>`.

## Sintaxis

```bash
docker network create <red>

docker run -d --name <nombre_a> --network <red> <imagen>
docker run -d --name <nombre_b> --network <red> <imagen>

# Probar resolución / HTTP interno (ejemplo)
docker exec <nombre_a> <comando_que_use_el_nombre_b>
```

Alias opcional:

```bash
docker run -d --name <nombre> --network <red> --network-alias <apodo> <imagen>
```

## Ejemplos

### Montar la demo (red + dos contenedores)

```bash
docker network create lab

docker run -d --name web-lab --network lab nginx:alpine
docker run -d --name caja-lab --network lab alpine:latest sleep 3600
```

Alpine mínimo no trae `curl`/`ping` a veces; instalamos `curl` solo para la prueba:

```bash
docker exec caja-lab apk add --no-cache curl
# OK: … curl …
```

### Resolver el nombre (DNS)

```bash
docker exec caja-lab nslookup web-lab
```

Salida típica (varía el formato/IP):

```text
Name:      web-lab
Address 1: 172.18.0.2 web-lab.lab
```

Si no hay `nslookup`:

```bash
docker exec caja-lab getent hosts web-lab
# 172.18.0.2        web-lab
```

### Hablar por HTTP usando el nombre

```bash
docker exec caja-lab curl -s -o /dev/null -w "%{http_code}\n" http://web-lab:80
# 200

docker exec caja-lab curl -s http://web-lab:80 | head -n 3
# <!DOCTYPE html>
# <html>
# <head>
```

Importante: es **`web-lab:80`** (puerto del nginx **dentro**), no `8080`. El `8080` solo existiría si publicaste `-p 8080:80` para el host.

### ¿Y si NO están en la misma red?

```bash
docker run -d --name web-otra nginx:alpine
# se va a bridge default, no a "lab"

docker exec caja-lab curl -s --connect-timeout 3 http://web-otra:80
# curl: … Could not resolve host: web-otra
#   o timeout / falló
```

Arreglo: misma red desde el principio, o:

```bash
docker network connect lab web-otra
docker exec caja-lab curl -s -o /dev/null -w "%{http_code}\n" http://web-otra:80
# 200
```

### Alias de red

```bash
docker rm -f web-lab

docker run -d --name web-lab --network lab --network-alias frontend nginx:alpine

docker exec caja-lab curl -s -o /dev/null -w "%{http_code}\n" http://frontend:80
# 200
```

Útil cuando el nombre del contenedor es largo y la app espera un hostname corto (`db`, `api`…).

### Mini “app + fake db” (idea novata)

No hace falta una base real para pillar el concepto: un contenedor escucha y el otro lo llama por nombre.

```bash
docker network create appnet

docker run -d --name db --network appnet \
  nginx:alpine
# (simulamos "db" con nginx solo para el DNS/HTTP)

docker run -d --name app --network appnet alpine:latest sleep 3600
docker exec app apk add --no-cache curl

docker exec app curl -s -o /dev/null -w "%{http_code}\n" http://db:80
# 200
```

En la vida real, `app` usaría algo tipo `postgres://db:5432/…` — la clave es el hostname **`db`**.

### Publicar al host y hablar entre contenedores a la vez

```bash
docker rm -f web-lab

docker run -d --name web-lab --network lab -p 8080:80 nginx:alpine

# Desde el HOST:
curl -I http://127.0.0.1:8080
# HTTP/1.1 200 OK

# Desde OTRO CONTENEDOR (mismo lab):
docker exec caja-lab curl -s -o /dev/null -w "%{http_code}\n" http://web-lab:80
# 200
```

Dos caminos distintos: host → `-p`; contenedor → nombre + puerto interno.

### Limpiar

```bash
docker rm -f web-lab caja-lab web-otra app db 2>/dev/null
docker network rm lab appnet 2>/dev/null
```

## Errores frecuentes

- Probar nombres en la red **`bridge` default** y frustrarse → usa `docker network create` + `--network`.
- Usar el puerto del **host** entre contenedores (`http://web:8080`) cuando el servicio escucha en `80` dentro → usa el puerto interno.
- Olvidar `--name` → Docker inventa un nombre raro; el DNS sigue existiendo, pero tú no lo conoces.
- `Could not resolve host` → no están en la misma red, typo en el nombre, o el contenedor destino no existe/`Exited`.
- Instalar `curl` en Alpine en cada prueba y pensar que “la imagen cambió para siempre” → `apk add` en un contenedor **en marcha** no modifica la imagen; al recrearlo hay que instalar otra vez (o hazte una imagen con Dockerfile).


## Siguiente

[HTTP/1.1 200 OK](../05-compose/que-es-compose.md)

## Relacionados

- Anterior: [docker-network](docker-network.md) · [conceptos-red](conceptos-red.md)  
- Puertos al host: [docker-inspect-port](../01-fundamentos/docker-inspect-port.md)  
- Siguiente bloque: [05-compose](../05-compose/) — varios servicios con un fichero  
- [README de la carpeta](README.md)  
- Doc oficial: [Networking](https://docs.docker.com/engine/network/) · [Embedded DNS](https://docs.docker.com/engine/network/#dns-services)  
