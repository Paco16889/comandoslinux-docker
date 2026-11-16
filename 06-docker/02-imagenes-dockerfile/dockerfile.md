# Dockerfile (receta de una imagen)

Un **Dockerfile** es un fichero de texto con instrucciones para **construir** una imagen.  
Orden habitual: de qué imagen partes (`FROM`), qué instalas (`RUN`), qué copias (`COPY`), dónde trabajas (`WORKDIR`) y qué se ejecuta al arrancar (`CMD` / `ENTRYPOINT`).

> **Cómo leer la sintaxis:** `<algo>` = lo sustituyes tú · `[algo]` = opcional.  
> Detalle: [Cómo leer la sintaxis](../../README.md#cómo-leer-la-sintaxis-muy-importante).

## Ideas previas

| Concepto | En pocas palabras |
|----------|-------------------|
| Dockerfile | Receta; por convención el fichero se llama `Dockerfile` (sin extensión) |
| Capa | Casi cada instrucción genera una capa (cacheable) |
| Contexto | Carpeta que envías al demonio al hacer `build` (siguiente tema) |
| `FROM` | Imagen base de la que partes |
| `CMD` | Comando por defecto al hacer `docker run` (se puede sustituir) |
| `ENTRYPOINT` | Comando “fijo” del contenedor (los args de `run` se añaden / matizan) |
| `EXPOSE` | Documenta un puerto; **no** publica al host (eso es `-p`) |

```text
Dockerfile  ──docker build──►  imagen  ──docker run──►  contenedor
```

## Sintaxis (instrucciones que verás ya)

```dockerfile
FROM <imagen>[:<tag>]

WORKDIR <ruta>

RUN <comando>
COPY <origen_en_host> <destino_en_imagen>
ADD <origen> <destino>

ENV <VAR>=<valor>
EXPOSE <puerto>

CMD ["ejecutable", "arg1", "arg2"]
ENTRYPOINT ["ejecutable"]
```

| Instrucción | Qué hace (idea novata) |
|-------------|------------------------|
| `FROM` | Obligatoria (casi siempre la primera): base |
| `WORKDIR` | `cd` dentro de la imagen; crea la ruta si no existe |
| `RUN` | Ejecuta un comando **al construir** (instalar paquetes, etc.) |
| `COPY` | Copia ficheros del contexto → imagen (claro y preferido) |
| `ADD` | Como `COPY`, pero también puede descomprimir/URLs; para novatos: usa `COPY` |
| `ENV` | Variables de entorno en la imagen |
| `EXPOSE` | “Esta app escucha en este puerto” (documentación) |
| `CMD` | Qué arrancar por defecto |
| `ENTRYPOINT` | Ejecutable principal del contenedor |

Formato **exec** (JSON) recomendado para `CMD` / `ENTRYPOINT`:

```dockerfile
CMD ["nginx", "-g", "daemon off;"]
```

Formato shell (menos predecible con señales):

```dockerfile
CMD nginx -g "daemon off;"
```

## Ejemplos

### Dockerfile mínimo (página estática con nginx)

Crea una carpeta de práctica:

```bash
mkdir -p ~/docker-demo-web
cd ~/docker-demo-web
```

`index.html`:

```html
<!DOCTYPE html>
<html lang="es">
  <head><meta charset="utf-8"><title>Demo</title></head>
  <body><h1>Hola desde mi imagen</h1></body>
</html>
```

`Dockerfile` (en la misma carpeta):

```dockerfile
FROM nginx:alpine

WORKDIR /usr/share/nginx/html

# Quita la página por defecto y pon la tuya
COPY index.html /usr/share/nginx/html/index.html

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
```

(El build concreto está en [docker-build](docker-build.md).)

### `RUN` — instalar algo en la base

Ejemplo Alpine (un solo `RUN` agrupa pasos y deja menos capas “sucias”):

```dockerfile
FROM alpine:latest

RUN apk add --no-cache curl

CMD ["sh"]
```

En Debian/Ubuntu base sería `apt-get` (con limpieza de caché); aquí Alpine es más corto para practicar.

### `WORKDIR` + `COPY` de una carpeta

```dockerfile
FROM alpine:latest

WORKDIR /app
COPY . /app

CMD ["ls", "-la", "/app"]
```

`COPY . /app` copia el **contexto** (ojo: no metas secretos ni carpetas enormes; ver `.dockerignore` en build).

### `ENV`

```dockerfile
FROM alpine:latest

ENV SALUDO=hola
ENV MODO=demo

CMD ["sh", "-c", "echo $SALUDO-$MODO"]
```

Tras construir (`docker build -t demo-env:1 .` — ver [docker-build](docker-build.md)):

```bash
docker run --rm demo-env:1
# hola-demo
```

### `CMD` vs argumento en `run`

Si la imagen tiene:

```dockerfile
CMD ["echo", "por defecto"]
```

```bash
docker run --rm <tu-imagen>
# por defecto

docker run --rm <tu-imagen> echo "otro"
# otro   ← sustituyes el CMD
```

### `ENTRYPOINT` + `CMD` (idea)

```dockerfile
FROM alpine:latest

ENTRYPOINT ["echo"]
CMD ["mundo"]
```

```bash
docker run --rm <tu-imagen>
# mundo

docker run --rm <tu-imagen> hola
# hola   ← reemplaza el CMD; ENTRYPOINT se mantiene
```

Para el primer día: con **`CMD`** suele bastar. `ENTRYPOINT` lo verás en imágenes “tipo comando”.

### `EXPOSE` no es `-p`

```dockerfile
EXPOSE 80
```

Documenta. Para entrar desde el host sigue haciendo falta:

```bash
docker run -d --name web-expose -p 8080:80 <tu-imagen>
# imprime un ID largo

curl -I http://127.0.0.1:8080
# HTTP/1.1 200 OK   (si la imagen sirve HTTP en 80)
```

### Orden que suele ir bien (plantilla mental)

```dockerfile
FROM <base>

WORKDIR /app

# Dependencias (cambia poco → buena caché)
RUN <instalar paquetes>

# Código (cambia más → al final)
COPY . /app

ENV <VAR>=<valor>
EXPOSE <puerto>
CMD ["<comando>", "<args>"]
```

## Errores frecuentes

- Llamar al fichero `dockerfile.txt` o `DockerFile.md` → el build busca `Dockerfile` por defecto (o indícalo con `-f`).
- Confundir `RUN` (en **build**) con lo que pasa en `docker run`.
- Pensar que `EXPOSE` abre el puerto en tu PC → no; falta `-p`.
- Usar `ADD` por costumbre cuando `COPY` basta → más sorpresas (tar, URLs).
- Muchos `RUN` sueltos con cachés de apt/apk sin limpiar → imagen más gorda.
- Poner secretos (contraseñas, tokens) en el Dockerfile o en `COPY` → quedan en capas de la imagen.


## Siguiente

[REPOSITORY   TAG   …](docker-build.md)

## Relacionados

- Anterior: [imagenes](imagenes.md)  
- Siguiente: [docker-build](docker-build.md) — construir y etiquetar  
- Arrancar lo construido: [docker-pull-run](../01-fundamentos/docker-pull-run.md)  
- [README de la carpeta](README.md)  
- Doc oficial: [Dockerfile reference](https://docs.docker.com/reference/dockerfile/) · [Best practices](https://docs.docker.com/build/building/best-practices/)  
