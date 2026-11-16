# Conceptos de red (bridge y puertos)

Por defecto, cada contenedor se conecta a la red **bridge** de Docker.  
Desde tu PC entras a un servicio del contenedor publicando puertos con **`-p host:contenedor`**. Entre contenedores, lo cómodo es una red propia y hablarse **por nombre** (siguiente temas).

> **Cómo leer la sintaxis:** `<algo>` = lo sustituyes tú · `[algo]` = opcional.  
> Detalle: [Cómo leer la sintaxis](../../README.md#cómo-leer-la-sintaxis-muy-importante).

## Ideas previas

| Concepto | En pocas palabras |
|----------|-------------------|
| Host | Tu Linux (Mint, Ubuntu, Debian, Fedora, Arch…) |
| Bridge | Red virtual tipo “switch” que Docker crea; la default se llama `bridge` |
| Publicar puerto (`-p`) | Abrir un puerto del host hacia un puerto del contenedor |
| IP del contenedor | Dirección interna en la red Docker (cambia si recreas el contenedor) |
| Nombre del contenedor | Más estable para hablar entre contenedores (en redes de usuario) |
| `EXPOSE` | Documenta un puerto en la imagen; **no** lo publica al host |

```text
Tu PC (host)                         Red bridge Docker
┌──────────────────┐                 ┌─────────────────────────┐
│ navegador/curl   │── -p 8080:80 ──►│ contenedor nginx (:80)  │
│ 127.0.0.1:8080   │                 └─────────────────────────┘
└──────────────────┘
```

## Bridge por defecto vs red tuya

| | Red `bridge` (default) | Red que creas tú (`docker network create`) |
|--|------------------------|--------------------------------------------|
| ¿Cuándo se usa? | Si no pones `--network` | Cuando quieres aislar un “proyecto” |
| DNS por nombre entre contenedores | Limitado / poco práctico para novatos | **Sí**: se resuelven por `--name` |
| Uso típico | Un solo contenedor + `-p` | Web + base de datos, varios servicios |

Para un nginx solo con `-p`, la bridge default basta.  
Para **dos contenedores que se hablen**, crea una red (siguiente página).

## Sintaxis (recordatorio de puertos)

```bash
docker run -d --name <nombre> -p <puerto_host>:<puerto_contenedor> <imagen>
docker port <nombre>
```

| Parte de `-p` | Significado |
|---------------|-------------|
| Izquierda (`8080`) | Puerto en **tu** Linux |
| Derecha (`80`) | Puerto **dentro** del contenedor |

```bash
-p 8080:80
# host:8080  →  contenedor:80
```

## Ejemplos

### Ver redes que ya existen

```bash
docker network ls
```

Salida típica:

```text
NETWORK ID     NAME      DRIVER    SCOPE
a1b2c3d4e5f6   bridge    bridge    local
…              host      host      local
…              none      null      local
```

| Nombre | Idea novata |
|--------|-------------|
| `bridge` | La red por defecto de los `docker run` normales |
| `host` | El contenedor usa la red del host (avanzado; no ahora) |
| `none` | Sin red |

### Un contenedor en bridge + puerto publicado

```bash
docker run -d --name web-bridge -p 8080:80 nginx:alpine
docker ps --filter "name=web-bridge"
# … 0.0.0.0:8080->80/tcp   web-bridge

docker port web-bridge
# 80/tcp -> 0.0.0.0:8080
```

Desde el host:

```bash
curl -I http://127.0.0.1:8080
# HTTP/1.1 200 OK
# Server: nginx/…
```

### Sin `-p`: desde el host “no hay puerta”

```bash
docker run -d --name web-sin-p nginx:alpine
docker port web-sin-p
# (sin mapeo)

curl -I http://127.0.0.1:80
# Connection refused  (o te responde OTRA cosa de tu sistema, no este contenedor)
```

La app escucha en el 80 **interno**; tu navegador en el host no tiene mapeo.

### Orden host:contenedor (el clásico lío)

```bash
# Bien para nginx (escucha en 80 dentro):
docker run -d --name web-ok -p 8080:80 nginx:alpine

# Mal si pensabas “abrir 8080 dentro”:
# docker run -d --name web-mal -p 80:8080 nginx:alpine
#   → host:80 → contenedor:8080 (nginx no está ahí)
```

### Varios puertos

```bash
docker run -d --name web-multi \
  -p 8080:80 \
  -p 8443:443 \
  nginx:alpine

docker port web-multi
# 80/tcp -> 0.0.0.0:8080
# 443/tcp -> 0.0.0.0:8443
```

### Inspección suave de la red default

```bash
docker network inspect bridge --format '{{.Name}} driver={{.Driver}}'
# bridge driver=bridge
```

Para ver qué contenedores están enganchados (nombres pueden variar):

```bash
docker network inspect bridge --format '{{range $k,$v := .Containers}}{{$v.Name}} {{end}}'
# web-bridge web-sin-p …
```

### Limpiar demos de esta página

```bash
docker rm -f web-bridge web-sin-p web-ok web-multi 2>/dev/null
```

## Errores frecuentes

- Invertir `-p`: es **`host:contenedor`**. nginx → derecha suele ser `80`.
- `port is already allocated` → ese puerto del host ya está pillado; usa otro izquierdo (`8081:80`).
- Confundir `EXPOSE 80` en el Dockerfile con “ya puedo entrar desde el PC” → falta `-p`.
- Pensar que la IP interna del contenedor es estable para tutoriales largos → mejor **nombre + red de usuario** (siguientes ficheros).
- Abrir `http://127.0.0.1:80` cuando mapeaste `8080:80` → en el host es **8080**.

## Relacionados

- Puertos en fundamentos: [docker-inspect-port](../01-fundamentos/docker-inspect-port.md) · [docker-pull-run](../01-fundamentos/docker-pull-run.md)  
- Siguiente: [docker-network](docker-network.md) — crear redes, connect/disconnect  
- Luego: [dns-entre-contenedores](dns-entre-contenedores.md) — hablarse por nombre  
- [README de la carpeta](README.md)  
- Doc oficial: [Networking overview](https://docs.docker.com/engine/network/) · [Publish ports](https://docs.docker.com/get-started/docker-concepts/running-containers/publishing-ports/)  
