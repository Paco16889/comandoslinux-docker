# docker exec

`docker exec` ejecuta un comando **dentro de un contenedor que ya está en marcha**.  
Lo más habitual: abrir un shell (`bash` o `sh`) para mirar ficheros, procesos o probar algo.

> **Cómo leer la sintaxis:** `<algo>` = lo sustituyes tú · `[algo]` = opcional.  
> Detalle: [Cómo leer la sintaxis](../../README.md#cómo-leer-la-sintaxis-muy-importante).

## Ideas previas

| Concepto | En pocas palabras |
|----------|-------------------|
| Contenedor Up | `exec` solo funciona si el contenedor está **en marcha** |
| `run -it` | Crea **y** entra (contenedor nuevo) |
| `exec -it` | Entra en uno **que ya existe** y está corriendo |
| `sh` vs `bash` | Imágenes mínimas (Alpine) suelen tener `sh`, no `bash` |
| Proceso principal | El de `run`/CMD; `exec` lanza **otro** proceso aparte |

```text
docker run -d --name web-demo nginx:alpine
              │
              └── docker exec -it web-demo sh
                        (segundo proceso dentro del mismo contenedor)
```

## Sintaxis

```bash
docker exec [opciones] <contenedor> <comando> [args…]
```

| Opción | Qué hace |
|--------|----------|
| `-i` | Mantener entrada estándar (stdin) |
| `-t` | Pseudo-TTY (terminal) |
| `-it` | Combinación habitual para shell interactivo |
| `-u <usuario>` | Ejecutar como ese usuario dentro del contenedor |
| `-e <VAR>=<valor>` | Variable de entorno solo para ese comando |
| `-w <ruta>` | Directorio de trabajo del comando |

## Ejemplos

### Preparar un contenedor y entrar

```bash
docker run -d --name web-demo -p 8080:80 nginx:alpine
docker exec -it web-demo sh
```

Dentro del contenedor (ejemplos):

```bash
pwd
# /   (o el WORKDIR de la imagen)

ls /
# bin  etc  usr  …  (árbol Linux mínimo de la imagen)

hostname
# suele ser el ID corto del contenedor

exit
# vuelves a tu prompt del host; el contenedor nginx sigue Up
```

### Alpine → `sh` · Ubuntu → `bash`

```bash
docker run -d --name caja-alpine alpine:latest sleep 3600
docker exec -it caja-alpine sh

docker run -d --name caja-ubuntu ubuntu:24.04 sleep 3600
docker exec -it caja-ubuntu bash
```

Si pides `bash` en Alpine y no existe:

```bash
docker exec -it caja-alpine bash
# executable file not found…  → usa sh
```

### Un solo comando (sin shell interactivo)

```bash
docker exec web-demo ls /usr/share/nginx/html
# 50x.html  index.html

docker exec web-demo cat /etc/os-release
# NAME="Alpine Linux" …

docker exec web-demo nginx -v
# nginx version: nginx/1.x.x
```

Con `-it` solo cuando necesites interactuar (editores, shells, programas que pidan teclado).

### Como otro usuario

```bash
docker exec -u root -it web-demo sh
docker exec -u www-data web-demo id
```

### Directorio de trabajo

```bash
docker exec -w /usr/share/nginx/html web-demo ls
```

### Variable de entorno puntual

```bash
docker exec -e SALUDO=hola web-demo printenv SALUDO
# hola
```

### ¿Está en marcha? Si no, `exec` falla

```bash
docker stop web-demo
docker exec -it web-demo sh
# Error: Container … is not running

docker start web-demo
docker exec -it web-demo sh
```

### Diferencia rápida `run -it` vs `exec -it`

| Quieres… | Usa… |
|----------|------|
| Contenedor **nuevo** y entrar | `docker run -it --rm <imagen> sh` |
| Entrar en uno **ya levantado** (servicio) | `docker exec -it <nombre> sh` |

```bash
# nuevo (y se borra al salir)
docker run -it --rm alpine:latest sh

# servicio ya creado
docker exec -it web-demo sh
```

## Errores frecuentes

- `Container … is not running` → arráncalo con `docker start <nombre>` o créalo de nuevo con `run -d`.
- `executable file not found` con `bash` → esa imagen no trae bash; prueba `sh`.
- Usar `exec` sobre un contenedor **Exited** (p. ej. `hello-world` ya terminado) → no hay proceso; no puedes “entrar”.
- Confundir `exec` con `attach`: `attach` te engancha al proceso **principal**; `exec` abre otro comando. Para novatos, `exec -it … sh` suele ser más claro.
- Olvidar `-it` en un shell: a veces “no responde” o se ve raro; para interactivo usa `-it`.
- Salir con `exit` **no** para el contenedor cuando entraste con `exec` (solo cierras esa sesión). El servicio sigue (`docker ps`).


## Siguiente

[80/tcp -> 0.0.0.0:8080](docker-inspect-port.md)

## Relacionados

- Anterior: [docker-stop-start-rm](docker-stop-start-rm.md)  
- Siguiente: [docker-inspect-port](docker-inspect-port.md) — puertos e inspección suave  
- Crear/arrancar: [docker-pull-run](docker-pull-run.md)  
- [README de la carpeta](README.md)  
- Doc oficial: [docker exec](https://docs.docker.com/reference/cli/docker/container/exec/)  
