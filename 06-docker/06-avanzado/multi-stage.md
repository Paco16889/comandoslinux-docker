# Multi-stage builds

Un Dockerfile puede tener **varias etapas** (`FROM` … `AS <nombre>`).  
Compilas o preparas en una etapa “gorda” y en la **última** solo copias el resultado: imagen final más pequeña y limpia.

> **Cómo leer la sintaxis:** `<algo>` = lo sustituyes tú · `[algo]` = opcional.  
> Detalle: [Cómo leer la sintaxis](../../README.md#cómo-leer-la-sintaxis-muy-importante).

## Ideas previas

| Concepto | En pocas palabras |
|----------|-------------------|
| Etapa (`stage`) | Cada bloque que empieza por `FROM` |
| `AS <nombre>` | Ponerle nombre a una etapa para referenciarla |
| `COPY --from=<etapa>` | Copiar ficheros desde otra etapa (no desde el host) |
| Imagen final | Solo cuenta la **última** etapa del Dockerfile (salvo flags raros) |
| BuildKit | Motor moderno de build (suele venir activo); multi-stage funciona con él |

```text
Etapa "build"          Etapa "final"
(herramientas + src)   (solo runtime + artefacto)
       │                      ▲
       └── COPY --from=build ─┘
```

## Sintaxis

```dockerfile
FROM <imagen_build> AS <nombre_etapa>
WORKDIR /src
RUN <compilar_o_generar>

FROM <imagen_runtime> AS final
COPY --from=<nombre_etapa> <ruta_origen> <ruta_destino>
CMD ["…"]
```

```bash
docker build -t <nombre>[:<tag>] .
```

Construir **solo** hasta una etapa (útil para depurar):

```bash
docker build --target <nombre_etapa> -t <nombre>:build .
```

## Ejemplos

### Problema: una sola etapa “gorda”

Imagina generar un fichero con herramientas que luego no necesitas en producción:

```dockerfile
FROM alpine:latest
RUN apk add --no-cache build-base
WORKDIR /src
COPY mensaje.txt .
RUN cp mensaje.txt /out.txt
CMD ["cat", "/out.txt"]
```

La imagen final **sigue llevando** `build-base` aunque solo quieras `cat` un texto.

### Solución multi-stage (demo novata)

```bash
mkdir -p ~/demo-multistage
cd ~/demo-multistage
echo 'Hola multi-stage' > mensaje.txt
```

`Dockerfile`:

```bash
cat > Dockerfile << 'EOF'
# --- etapa build: tiene herramientas ---
FROM alpine:latest AS build
RUN apk add --no-cache build-base
WORKDIR /src
COPY mensaje.txt .
# "procesamos" el fichero (ejemplo tonto pero claro)
RUN cp mensaje.txt /artefacto.txt \
 && echo " (ok)" >> /artefacto.txt

# --- etapa final: solo Alpine + el artefacto ---
FROM alpine:latest AS final
COPY --from=build /artefacto.txt /artefacto.txt
CMD ["cat", "/artefacto.txt"]
EOF
```

```bash
docker build -t demo-ms:1 .
```

```text
[+] Building …
 => [build 1/4] FROM alpine:latest
 => [build 2/4] RUN apk add --no-cache build-base
 => [final 1/2] FROM alpine:latest
 => [final 2/2] COPY --from=build /artefacto.txt /artefacto.txt
 => naming to docker.io/library/demo-ms:1
```

```bash
docker run --rm demo-ms:1
# Hola multi-stage (ok)
```

Compara tamaños mentales: la etapa `build` tuvo compiladores; la `final` no los incluye porque partió de un `FROM` limpio y solo copió el fichero.

### Ver capas / historial de la imagen final

```bash
docker history demo-ms:1
```

```text
IMAGE     CREATED BY                                      SIZE
…         CMD ["cat" "/artefacto.txt"]                    0B
…         COPY /artefacto.txt /artefacto.txt # buildkit   …
…         /bin/sh -c #(nop)  CMD ["/bin/sh"]              0B
```

No deberías ver un `apk add build-base` en la imagen etiquetada `demo-ms:1` (eso quedó en la etapa intermedia, no en la final).

### Construir solo la etapa `build` (depurar)

```bash
docker build --target build -t demo-ms:build .
docker run --rm demo-ms:build cat /artefacto.txt
# Hola multi-stage (ok)
```

### Patrón típico (idea; no hace falta Go instalado en el host)

```dockerfile
FROM golang:1.22-alpine AS build
WORKDIR /src
COPY . .
RUN go build -o /app

FROM alpine:latest
COPY --from=build /app /usr/local/bin/app
CMD ["app"]
```

Misma idea: herramientas de compilación en `build`, binario solo en la final.

### `.dockerignore` + multi-stage

Sigue usando [dockerignore-contexto](dockerignore-contexto.md): aunque multi-stage limpie la imagen final, un contexto enorme **ralentiza** cada build.

### Nota BuildKit

En installs recientes el build suele usar BuildKit. Si ves avisos raros de builder:

```bash
docker buildx version
# BuildKit / buildx presentes en muchas instalaciones con el plugin buildx
```

Para el día a día novato: `docker build -t … .` basta; multi-stage no exige flags especiales.

## Errores frecuentes

- Olvidar `AS <nombre>` y luego no poder `COPY --from=…` con nombre claro.  
- `COPY --from=build` con rutas mal → fichero no existe en esa etapa.  
- Pensar que la etapa intermedia “desaparece del disco del demonio” al instante → puede quedar en caché de build; la **imagen etiquetada** es la final.  
- Meter secretos en la etapa build y copiar de más a la final → igual los filtras si haces `COPY --from=build / /` (demasiado amplio).  
- Saltar `.dockerignore` porque “ya uso multi-stage” → el contexto sigue importando.

## Siguiente

[Healthcheck](healthcheck.md)

## Relacionados

- Anterior: [dockerignore-contexto](dockerignore-contexto.md)  
- Dockerfile / build: [dockerfile](../02-imagenes-dockerfile/dockerfile.md) · [docker-build](../02-imagenes-dockerfile/docker-build.md)  
- Siguiente: [healthcheck](healthcheck.md)  
- [README de la carpeta](README.md)  
- Doc oficial: [Multi-stage builds](https://docs.docker.com/build/building/multi-stage/) · [BuildKit](https://docs.docker.com/build/buildkit/)



