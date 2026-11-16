# docker port / docker inspect (suave)

Cómo ver **qué puertos** tiene publicados un contenedor y echar un vistazo a su configuración con `inspect` (sin ahogarte en JSON).  
Complementa el `-p` de [docker-pull-run](docker-pull-run.md).

> **Cómo leer la sintaxis:** `<algo>` = lo sustituyes tú · `[algo]` = opcional.  
> Detalle: [Cómo leer la sintaxis](../../README.md#cómo-leer-la-sintaxis-muy-importante).

## Ideas previas

| Concepto | En pocas palabras |
|----------|-------------------|
| Puerto del contenedor | Donde escucha la app **dentro** (ej. nginx → `80`) |
| Puerto del host | El de tu Linux que mapeas con `-p` (ej. `8080`) |
| Publicar | Sin `-p` (o equivalente), desde el host **no** entras fácil a ese servicio |
| `docker port` | Resumen claro: contenedor → host |
| `docker inspect` | Ficha completa en JSON (IP, mounts, env, puertos…) |

```text
Navegador / curl  →  host:8080  ─-p─►  contenedor:80  →  nginx
```

## Sintaxis

```bash
docker port <contenedor> [<puerto_contenedor>]

docker inspect <contenedor_o_imagen>
docker inspect --format '<plantilla>' <contenedor>
```

Al crear el contenedor (recordatorio):

```bash
docker run -d --name <nombre> -p <puerto_host>:<puerto_contenedor> <imagen>
```

## Ejemplos

### Crear con puerto y comprobar

```bash
docker run -d --name web-8080 -p 8080:80 nginx:alpine

docker port web-8080
# 80/tcp -> 0.0.0.0:8080
# (a veces también [::]:8080)
```

Probar desde el host:

```bash
curl -I http://127.0.0.1:8080
# HTTP/1.1 200 OK
# Server: nginx/…
```

### Solo un puerto concreto del contenedor

```bash
docker port web-8080 80
# 0.0.0.0:8080

docker port web-8080 80/tcp
# 0.0.0.0:8080
```

### Varios mapeos

```bash
docker run -d --name web-multi \
  -p 8080:80 \
  -p 8443:443 \
  nginx:alpine

docker port web-multi
```

### Ver puertos también en `ps`

```bash
docker ps --filter "name=web-8080"
# columna PORTS: 0.0.0.0:8080->80/tcp
```

### `inspect` suave — IP del contenedor

```bash
docker inspect --format '{{.Name}} → {{.NetworkSettings.IPAddress}}' web-8080
```

En redes bridge modernas a veces la IP “clásica” sale vacía en ese campo; entonces:

```bash
docker inspect --format '{{json .NetworkSettings.Networks}}' web-8080
```

(Lo de redes se amplía en [04-networks](../04-networks/).)

### Estado y nombre de la imagen

```bash
docker inspect --format 'Estado: {{.State.Status}}' web-8080
# Estado: running

docker inspect --format 'Imagen: {{.Config.Image}}' web-8080
# Imagen: nginx:alpine
```

### Variables de entorno (resumen)

```bash
docker inspect --format '{{range .Config.Env}}{{println .}}{{end}}' web-8080
```

### Bindings de puertos vía inspect

```bash
docker inspect --format '{{json .NetworkSettings.Ports}}' web-8080
```

Para el día a día, **`docker port`** suele ser más legible.

### Contenedor sin `-p`

```bash
docker run -d --name web-sin-p nginx:alpine
docker port web-sin-p
# (sin salida de mapeo host)

docker ps --filter "name=web-sin-p"
# PORTS puede mostrar solo 80/tcp sin 0.0.0.0:…
```

La app escucha **dentro**; desde tu navegador en el host no tienes mapeo. Solución: recrear con `-p` o usar redes avanzadas más adelante.

### Otro puerto del host (evitar conflictos)

```bash
docker run -d --name web-9000 -p 9000:80 nginx:alpine
curl -I http://127.0.0.1:9000
docker port web-9000
```

## Errores frecuentes

- Invertir `-p`: es **`host:contenedor`**. nginx sirve en **80** dentro → `-p 8080:80`, no `-p 80:8080` (salvo que sepas lo que haces).
- `port is already allocated` → ese puerto del host ya está pillado; cambia el izquierdo (`8081:80`) o para el otro contenedor.
- `docker port` sin salida → no hay publicación a host, o el nombre está mal (`docker ps -a`).
- Abrir `http://127.0.0.1:80` cuando mapeaste `8080:80` → debes usar **8080** en el host.
- Ahogarse con `inspect` sin `--format`: son pantallas de JSON; empieza por `port`, `ps` y un `--format` concreto.
- Firewall del host (ufw, etc.) bloqueando el puerto publicado → el contenedor está Up pero no entras desde fuera de la máquina (tema seguridad/red del temario Linux).

## Relacionados

- Anterior: [docker-exec](docker-exec.md)  
- `-p` al crear: [docker-pull-run](docker-pull-run.md)  
- Listar / logs: [docker-ps-logs](docker-ps-logs.md)  
- Siguiente bloque: [02-imagenes-dockerfile](../02-imagenes-dockerfile/)  
- Redes con más detalle: [04-networks](../04-networks/)  
- [README de la carpeta](README.md)  
- Doc oficial: [docker port](https://docs.docker.com/reference/cli/docker/container/port/) · [docker inspect](https://docs.docker.com/reference/cli/docker/inspect/)  
