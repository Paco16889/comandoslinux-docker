# docker pull / docker run

`docker pull` **baja** una imagen a tu máquina.  
`docker run` **crea** un contenedor a partir de una imagen y (por defecto) lo **arranca**. Si la imagen no está en local, `run` suele hacer el `pull` por ti.

> **Cómo leer la sintaxis:** `<algo>` = lo sustituyes tú · `[algo]` = opcional.  
> Detalle: [Cómo leer la sintaxis](../../README.md#cómo-leer-la-sintaxis-muy-importante).

## Ideas previas

| Concepto | En pocas palabras |
|----------|-------------------|
| Registry | Sitio remoto de imágenes (por defecto: Docker Hub) |
| Tag | Etiqueta de versión de la imagen. Ej.: `latest`, `1.27`, `alpine` |
| Nombre de imagen | Suele ser `<repositorio>[:<tag>]`. Ej.: `nginx:alpine` |
| Primer plano | El contenedor “ocupa” la terminal hasta que termina o lo paras |
| Detached (`-d`) | El contenedor sigue en segundo plano; recuperas el prompt |
| Publicar puerto (`-p`) | Abrir un puerto del host hacia un puerto del contenedor |

```text
Docker Hub (o registry)
        │  docker pull
        ▼
   Imagen en disco
        │  docker run
        ▼
   Contenedor (en marcha)
```

## Sintaxis

```bash
docker pull <imagen>[:<tag>]

docker run [opciones] <imagen>[:<tag>] [comando…]
```

Opciones que verás ya el primer día:

| Opción | Qué hace |
|--------|----------|
| `-it` | Terminal interactiva (`-i` + `-t`): entrar / teclear dentro |
| `-d` | Segundo plano (detached) |
| `--name <nombre>` | Nombre fijo del contenedor (si no, Docker inventa uno) |
| `-p <host>:<contenedor>` | Publicar puerto (ej.: `8080:80`) |
| `--rm` | Borrar el contenedor al salir / terminar |
| `-e <VAR>=<valor>` | Variable de entorno (lo verás más en servicios) |

## Ejemplos

### Probar que todo funciona

```bash
docker pull hello-world
```

Salida tipo del `pull`:

```text
Using default tag: latest
latest: Pulling from library/hello-world
… Pull complete …
Status: Downloaded newer image for hello-world:latest
docker.io/library/hello-world:latest
```

(Si ya la tenías: `Status: Image is up to date for hello-world:latest`.)

```bash
docker run hello-world
```

```text
Hello from Docker!
This message shows that your installation appears to be working correctly.
…
```

El contenedor **termina** solo; queda parado (salvo que uses `--rm`).

Equivalente en un solo paso (si no tienes la imagen, `run` la descarga):

```bash
docker run --rm hello-world
# mismo “Hello from Docker!”; al terminar el contenedor se borra solo
```

### Bajar una imagen concreta (con tag)

```bash
docker pull nginx:alpine
docker pull ubuntu:24.04
docker pull alpine:latest
```

Sin tag, Docker usa `latest` (que **no** significa siempre “la más nueva estable para ti”; mejor poner tag cuando puedas).

### Contenedor interactivo (shell)

```bash
docker run -it --rm alpine:latest sh
```

Entras a un shell dentro del contenedor. Al salir (`exit`), `--rm` lo borra.

Con Ubuntu (suele traer `bash`):

```bash
docker run -it --rm ubuntu:24.04 bash
```

Solo `-it` (sin `--rm`): el contenedor **queda** al salir; luego lo gestionas con `ps` / `rm` (siguiente tema).

```bash
docker run -it ubuntu:24.04 bash
# exit
# el contenedor sigue existiendo (parado)
```

### Segundo plano (`-d`) + nombre

```bash
docker run -d --name <mi-nginx> nginx:alpine
```

Ejemplo real:

```bash
docker run -d --name web-demo nginx:alpine
# imprime un ID largo, ej.: a1b2c3d4e5f6…
```

Recuperas el prompt al momento. El contenedor sigue corriendo (`docker ps` → `STATUS` Up).

### Publicar un puerto (`-p`)

El puerto **izquierdo** es el de tu Linux (host). El **derecho**, el del contenedor.

```bash
docker run -d --name web-8080 -p 8080:80 nginx:alpine
```

Luego, en el navegador o con curl:

```bash
curl http://127.0.0.1:8080
# HTML de la página de bienvenida de nginx (Welcome to nginx!)
```

Otro puerto del host:

```bash
docker run -d --name web-9000 -p 9000:80 nginx:alpine
```

Varios mapeos:

```bash
docker run -d --name <app> -p 8080:80 -p 8443:443 <imagen>
```

### Combinación típica de aprendizaje

```bash
docker pull nginx:alpine

docker run -d \
  --name <mi-web> \
  --rm \
  -p 8080:80 \
  nginx:alpine
```

Nota: con `--rm` + `-d`, el contenedor se borra **cuando se detiene** (p. ej. tras `docker stop`). Útil en pruebas; en servicios “serios” a veces prefieres no usar `--rm` para poder reiniciar el mismo contenedor.

### `run` con un comando distinto al de la imagen

```bash
docker run --rm alpine:latest echo "hola desde el contenedor"
# hola desde el contenedor

docker run --rm alpine:latest uname -a
# Linux <id> … x86_64 … (kernel del host, típico en contenedores)
```

### Nombre ya usado

```bash
docker run -d --name web-demo nginx:alpine
# si web-demo ya existe → error
```

Solución (idea): otro nombre, o borrar/parar el anterior (tema [docker-stop-start-rm](docker-stop-start-rm.md)).

## Errores frecuentes

- `permission denied` al socket → grupo `docker` o usa `sudo` (ver [instalacion-engine](../00-introduccion/instalacion-engine.md)).
- `Unable to find image … locally` y luego fallo de red → no hay imagen y no puede hacer `pull` (DNS, firewall, sin internet).
- `port is already allocated` / `address already in use` → ese puerto del **host** ya lo usa otro proceso o contenedor. Cambia el izquierdo de `-p` (ej. `8081:80`).
- `Conflict. The container name "/<nombre>" is already in use` → elige otro `--name` o elimina el contenedor viejo.
- Confundir `-p 80:8080` con `-p 8080:80`: el orden es **host:contenedor**. En nginx la web suele escuchar en **80** dentro del contenedor.
- Olvidar `-d` en un servicio largo: la terminal se queda “enganchada”; `Ctrl+C` puede parar el contenedor según el caso. Para servicios, `-d` es lo habitual.
- Pensar que `pull` arranca algo: solo descarga. Arrancar = `run` (o `start` de uno ya creado).


## Siguiente

[… nginx:alpine   Up …   0.0.0.0:8080->80/tcp   web-demo](docker-ps-logs.md)

## Relacionados

- Anterior: [conceptos-rapidos](conceptos-rapidos.md)  
- Siguiente: [docker-ps-logs](docker-ps-logs.md) — listar contenedores y ver logs  
- Luego: [docker-stop-start-rm](docker-stop-start-rm.md) — parar, arrancar, borrar  
- Install: [instalacion-engine](../00-introduccion/instalacion-engine.md)  
- [README de la carpeta](README.md)  
- Doc oficial: [docker pull](https://docs.docker.com/reference/cli/docker/image/pull/) · [docker run](https://docs.docker.com/reference/cli/docker/container/run/)  
