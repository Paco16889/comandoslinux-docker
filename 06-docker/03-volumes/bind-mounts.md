# Bind mounts (carpetas del host)

Un **bind mount** engancha una ruta **de tu Linux** (host) a una ruta **dentro** del contenedor.  
Ideal para editar código/configs en tu carpeta y que el contenedor lo vea al momento.

> **Cómo leer la sintaxis:** `<algo>` = lo sustituyes tú · `[algo]` = opcional.  
> Detalle: [Cómo leer la sintaxis](../../README.md#cómo-leer-la-sintaxis-muy-importante).

## Ideas previas

| Concepto | En pocas palabras |
|----------|-------------------|
| Host | Tu Mint/Ubuntu/Debian/Fedora/Arch |
| Ruta absoluta | Empieza por `/` (ej. `/home/<usuario>/mi-web`) — más fiable en `-v` |
| Bind | “Esta carpeta mía = esa carpeta del contenedor” |
| Permisos | Usuario del contenedor debe poder leer/escribir lo montado |
| Solo lectura (`:ro`) | El contenedor puede leer, no modificar el host |
| `--mount` | Sintaxis más larga y clara (menos ambigüedad que `-v`) |

```text
/home/<usuario>/mi-web   (host)
         │  bind
         ▼
/usr/share/nginx/html    (contenedor)
```

## Sintaxis

```bash
docker run … -v <ruta_host>:<ruta_contenedor>[:ro|:rw] …
docker run … --mount type=bind,source=<ruta_host>,target=<ruta_contenedor>[,readonly] …
```

| Forma | Ejemplo |
|-------|---------|
| `-v` corto | `-v /home/<usuario>/mi-web:/usr/share/nginx/html` |
| Solo lectura | `-v /home/<usuario>/mi-web:/usr/share/nginx/html:ro` |
| `--mount` | `--mount type=bind,source=/home/<usuario>/mi-web,target=/usr/share/nginx/html` |

Diferencia práctica con volumes:

| Escribes… | Docker entiende… |
|-----------|------------------|
| `-v datos-web:/data` (sin `/` al inicio del origen) | **Volume** llamado `datos-web` |
| `-v /home/…/carpeta:/data` (ruta con `/`) | **Bind mount** |

## Ejemplos

### Preparar carpeta en el host

```bash
mkdir -p /home/<usuario>/mi-web
echo '<h1>Hola bind</h1>' > /home/<usuario>/mi-web/index.html
```

Sustituye `<usuario>` por el tuyo, o usa `$HOME`:

```bash
mkdir -p "$HOME/mi-web"
echo '<h1>Hola bind</h1>' > "$HOME/mi-web/index.html"
```

### Montar con `-v` y servir con nginx

```bash
docker run -d --name web-bind \
  -p 8080:80 \
  -v "$HOME/mi-web":/usr/share/nginx/html:ro \
  nginx:alpine

curl http://127.0.0.1:8080
# Hola bind
```

Edita en el host y vuelve a pedir (sin reconstruir imagen):

```bash
echo '<h1>Cambio en el host</h1>' > "$HOME/mi-web/index.html"
curl http://127.0.0.1:8080
# Cambio en el host
```

### Misma idea con `--mount`

```bash
docker rm -f web-bind

docker run -d --name web-bind \
  -p 8080:80 \
  --mount type=bind,source="$HOME/mi-web",target=/usr/share/nginx/html,readonly \
  nginx:alpine
```

`--mount` exige que la ruta **source** exista (con `-v`, a veces Docker crea un directorio vacío y te confunde).

### Comprobar desde dentro del contenedor

```bash
docker exec web-bind ls /usr/share/nginx/html
# index.html

docker exec web-bind cat /usr/share/nginx/html/index.html
# <h1>…
```

### Solo lectura vs escritura

```bash
# :ro — el contenedor no debería poder borrar tu index
docker run --rm \
  -v "$HOME/mi-web":/data:ro \
  alpine:latest sh -c 'echo prueba > /data/x.txt'
# Read-only file system  (o similar)
```

Escritura (por defecto `:rw`):

```bash
docker run --rm \
  -v "$HOME/mi-web":/data \
  alpine:latest sh -c 'echo desde-contenedor > /data/desde-contenedor.txt'

cat "$HOME/mi-web/desde-contenedor.txt"
# desde-contenedor
```

### Permisos (el escollo típico)

Dentro del contenedor el proceso puede ser `root` o un usuario con **UID** distinto al tuyo.

Síntoma habitual:

```text
Permission denied
```

Ideas para novatos:

1. Mira quién eres dentro:

```bash
docker exec web-bind id
# uid=0(root) …   o uid=101(nginx) …
```

2. En el host, revisa permisos de la carpeta:

```bash
ls -la "$HOME/mi-web"
```

3. Para pruebas, carpeta legible (ajusta a tu caso; no abras el mundo en un servidor real):

```bash
chmod -R u+rwX "$HOME/mi-web"
# a veces hace falta también que el UID del contenedor pueda entrar en directorios padres
```

4. Si el servicio (nginx, node…) no es root y no puede leer, o bien das permisos/ACL adecuados, o pruebas temporalmente con una imagen que corra como root **solo para aprender**.

SELinux (Fedora y similares) a veces exige etiquetas (`:Z` / `:z` en `-v`). Si en Fedora el mount “no se ve” o deniega, busca ese tema o prueba:

```bash
# Fedora / entornos con SELinux (idea; no lo abuses)
docker run -d --name web-bind-sel \
  -p 8081:80 \
  -v "$HOME/mi-web":/usr/share/nginx/html:ro,Z \
  nginx:alpine
```

### Montar un solo fichero

```bash
echo 'server { ... }' > "$HOME/mi-nginx.conf"
# (ejemplo conceptual; la config real de nginx es más larga)

docker run -d --name web-conf \
  -p 8082:80 \
  -v "$HOME/mi-web":/usr/share/nginx/html:ro \
  nginx:alpine
```

Montar fichero concreto (la ruta host y la destino deben cuadrar):

```bash
docker run --rm \
  -v "$HOME/mi-web/index.html":/tmp/index.html:ro \
  alpine:latest cat /tmp/index.html
```

### Limpiar la demo

```bash
docker rm -f web-bind web-bind-sel web-conf 2>/dev/null
# tu carpeta $HOME/mi-web sigue en el host
```

## Errores frecuentes

- Usar ruta relativa rara en `-v` (`./datos:/data`) y no saber **desde qué carpeta** la resolvió Docker → prefiere `"$HOME/…"` o `/home/<usuario>/…`.
- Confundir `-v nombre:/data` (volume) con `-v /ruta:/data` (bind): el `/` inicial importa.
- `Permission denied` → UID/permisos/SELinux; no siempre es “Docker roto”.
- Montar `:ro` y luego intentar que el contenedor escriba logs ahí → falla; monta otra ruta escribible o un volume.
- Borrar ficheros **desde** el contenedor en un bind de escritura → también los borras en el host (es la misma carpeta).
- Esperar que `docker rm` borre tu carpeta del home → **no**: el bind es tuyo; solo se desengancha.

## Relacionados

- Anterior: [volumes](volumes.md) · [conceptos-datos](conceptos-datos.md)  
- Contenedores: [docker-pull-run](../01-fundamentos/docker-pull-run.md) · [docker-exec](../01-fundamentos/docker-exec.md)  
- Siguiente bloque: [04-networks](../04-networks/)  
- [README de la carpeta](README.md)  
- Doc oficial: [Bind mounts](https://docs.docker.com/engine/storage/bind-mounts/) · [--mount](https://docs.docker.com/engine/storage/bind-mounts/#choose-the--v-or---mount-flag)  
