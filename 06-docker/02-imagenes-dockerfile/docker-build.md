# docker build

`docker build` lee un **Dockerfile**, usa un **contexto** (carpeta) y produce una **imagen** local.  
Luego la arrancas con `docker run` como cualquier otra.

> **Cómo leer la sintaxis:** `<algo>` = lo sustituyes tú · `[algo]` = opcional.  
> Detalle: [Cómo leer la sintaxis](../../README.md#cómo-leer-la-sintaxis-muy-importante).

## Ideas previas

| Concepto | En pocas palabras |
|----------|-------------------|
| Contexto | Carpeta que se envía al demonio (`.` = directorio actual) |
| `-t` / `--tag` | Nombre:tag de la imagen resultante |
| `-f` | Ruta al Dockerfile si no se llama `Dockerfile` o no está en la raíz del contexto |
| Caché | Si una capa no cambia, Docker reutiliza el resultado (build más rápido) |
| `.dockerignore` | Lista de ficheros/carpetas que **no** entran en el contexto |

```text
~/docker-demo-web/
├── Dockerfile
├── index.html
└── .dockerignore     (opcional pero recomendable)

docker build -t <nombre>:<tag> .
        │                    │
        │                    └── contexto
        └── etiqueta de la imagen nueva
```

## Sintaxis

```bash
docker build -t <nombre>[:<tag>] <ruta_contexto>
docker build -t <nombre>[:<tag>] -f <ruta_Dockerfile> <ruta_contexto>
```

| Opción | Qué hace |
|--------|----------|
| `-t <nombre>:<tag>` | Etiqueta la imagen (si omites tag → `latest`) |
| `-f <fichero>` | Dockerfile concreto |
| `--no-cache` | Ignora caché (build limpio, más lento) |
| `--pull` | Intenta actualizar la imagen base (`FROM`) antes de construir |

El `.` final es lo más habitual: “contexto = esta carpeta”.

## Ejemplos

### Demo completa (web estática)

```bash
mkdir -p ~/docker-demo-web
cd ~/docker-demo-web
```

Crea `index.html`:

```bash
cat > index.html << 'EOF'
<!DOCTYPE html>
<html lang="es">
  <head><meta charset="utf-8"><title>Demo</title></head>
  <body><h1>Hola desde mi imagen</h1></body>
</html>
EOF
```

Crea `Dockerfile`:

```bash
cat > Dockerfile << 'EOF'
FROM nginx:alpine
COPY index.html /usr/share/nginx/html/index.html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
EOF
```

Construye:

```bash
docker build -t demo-web:1 .
```

Salida típica (resumida):

```text
[+] Building …
 => [internal] load build definition from Dockerfile
 => [1/2] FROM docker.io/library/nginx:alpine
 => [2/2] COPY index.html /usr/share/nginx/html/index.html
 => exporting to image
 => => naming to docker.io/library/demo-web:1
```

Comprueba:

```bash
docker images demo-web
# REPOSITORY   TAG   …
# demo-web     1     …
```

Arranca:

```bash
docker run -d --name demo-web -p 8080:80 demo-web:1
curl http://127.0.0.1:8080
# Hola desde mi imagen

docker rm -f demo-web
```

### Otro tag de la misma construcción

```bash
docker build -t demo-web:1 -t demo-web:latest .
docker images demo-web
# dos tags, misma IMAGE ID si no cambió nada
```

### Dockerfile con otro nombre o ruta

```bash
docker build -t demo-web:1 -f Dockerfile.prod .
docker build -t demo-web:1 -f ./docker/Dockerfile ./app
#                  Dockerfile ahí ↑     contexto ↑
```

El contexto sigue siendo la carpeta que indiques al final: `COPY` solo puede copiar desde **dentro** de ese contexto.

### Ver efecto de la caché

```bash
docker build -t demo-web:1 .
# segunda vez, sin cambiar ficheros:
docker build -t demo-web:1 .
# muchas líneas “CACHED”
```

Si cambias `index.html`, la capa `COPY` y las siguientes se reconstruyen.

### Build sin caché

```bash
docker build --no-cache -t demo-web:1 .
```

### `.dockerignore` (intro)

Evita mandar basura al demonio (más rápido y más seguro):

```bash
cat > .dockerignore << 'EOF'
.git
.gitignore
*.md
tmp
*.log
EOF
```

Ejemplo: sin esto, un `.git` enorme entra en el contexto aunque no hagas `COPY` de él (igual se envía).

Ampliación: [dockerignore-contexto](../06-avanzado/dockerignore-contexto.md).

### Actualizar la base al construir

```bash
docker build --pull -t demo-web:1 .
```

Útil para pillar parches de `nginx:alpine` (u otra base) si hay tag móvil.

### Si el build falla a mitad

Lee la línea `ERROR` de la etapa que falló (`RUN`, `COPY`…).  
Corrige el Dockerfile o los ficheros, y vuelve a ejecutar el mismo `docker build`.

```bash
# ¿Existe el fichero que COPY intenta copiar?
ls -l index.html
```

## Errores frecuentes

- Olvidar el `.` (contexto) al final → `docker build` se queja de que faltan argumentos.
- Hacer `docker build -t demo` **sin** tag de carpeta desde otro sitio → contexto incorrecto; `COPY` no encuentra ficheros.
- `COPY ./secreto …` estando el secreto fuera del contexto → no se puede; mueve ficheros o cambia el contexto (con cuidado).
- Confundir nombre de **imagen** (`-t demo-web:1`) con nombre de **contenedor** (`--name` en `run`).
- No tener la base en caché ni red → falla el `FROM` / pull de `nginx:alpine`.
- Meter `node_modules`, `.git` o backups en el contexto sin `.dockerignore` → builds lentos y a veces capas enormes si haces `COPY .`.

## Relacionados

- Anterior: [dockerfile](dockerfile.md) · [imagenes](imagenes.md)  
- Arrancar: [docker-pull-run](../01-fundamentos/docker-pull-run.md)  
- Siguiente bloque: [03-volumes](../03-volumes/) — datos que sobreviven al contenedor  
- [README de la carpeta](README.md)  
- Doc oficial: [docker build](https://docs.docker.com/reference/cli/docker/build/) · [Build context](https://docs.docker.com/build/building/context/) · [.dockerignore](https://docs.docker.com/build/building/context/#dockerignore-files)  
