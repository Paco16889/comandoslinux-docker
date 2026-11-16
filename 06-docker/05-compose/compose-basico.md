# Compose básico (`up`, `down`, `ps`, `logs`)

Ciclo mínimo: escribir un `compose.yaml`, **levantar** el proyecto, **ver** estado y logs, y **bajar** todo.  
Equivale a varios `docker run` / `stop` / `rm`, pero ordenado.

> **Cómo leer la sintaxis:** `<algo>` = lo sustituyes tú · `[algo]` = opcional.  
> Detalle: [Cómo leer la sintaxis](../../README.md#cómo-leer-la-sintaxis-muy-importante).

## Ideas previas

| Concepto | En pocas palabras |
|----------|-------------------|
| `up` | Crea/actualiza y arranca los servicios del fichero |
| `-d` | Segundo plano (detached), como en `docker run -d` |
| `down` | Para y **borra** contenedores (y red del proyecto); volumes según flags |
| `ps` | Lista servicios del proyecto (no todo Docker) |
| `logs` | Logs de uno o todos los servicios |
| Directorio del proyecto | Donde está el `compose.yaml` al ejecutar los comandos |

```text
cd ~/compose-demo
docker compose up -d    →  servicios Up
docker compose ps
docker compose logs -f
docker compose down     →  limpia el proyecto
```

## Sintaxis

```bash
docker compose up [-d] [servicio…]
docker compose down [--volumes] [--rmi local]
docker compose ps
docker compose logs [-f] [--tail <n>] [servicio…]
docker compose stop
docker compose start
docker compose restart [servicio…]
```

| Comando | Qué hace |
|---------|----------|
| `up -d` | Levanta en segundo plano |
| `down` | Para y elimina contenedores + red del proyecto |
| `down -v` | Además borra volumes **declarados** en el fichero |
| `ps` | Estado de los servicios |
| `logs -f` | Seguir logs (Ctrl+C solo deja de mirar) |
| `stop` / `start` | Parar/arrancar sin borrar contenedores |

## Ejemplos

### Crear el proyecto de un servicio

```bash
mkdir -p ~/compose-demo
cd ~/compose-demo
```

`compose.yaml`:

```bash
cat > compose.yaml << 'EOF'
services:
  web:
    image: nginx:alpine
    ports:
      - "8080:80"
EOF
```

### Levantar

```bash
docker compose up -d
```

Salida típica:

```text
[+] Running 2/2
 ✔ Network compose-demo_default    Created
 ✔ Container compose-demo-web-1    Started
```

(Los nombres llevan el prefijo de la **carpeta** `compose-demo`.)

### Listar

```bash
docker compose ps
```

```text
NAME                   IMAGE          STATUS         PORTS
compose-demo-web-1     nginx:alpine   Up …           0.0.0.0:8080->80/tcp
```

Probar:

```bash
curl -I http://127.0.0.1:8080
# HTTP/1.1 200 OK
```

### Logs

```bash
docker compose logs
docker compose logs --tail 20 web
docker compose logs -f web
# Ctrl+C para dejar de seguir
```

### Parar sin borrar / arrancar de nuevo

```bash
docker compose stop
docker compose ps
# Status: Exited …

docker compose start
docker compose ps
# Up otra vez
```

### Reiniciar un servicio

```bash
docker compose restart web
```

### Bajar el proyecto (`down`)

```bash
docker compose down
```

Salida típica:

```text
[+] Running 2/2
 ✔ Container compose-demo-web-1    Removed
 ✔ Network compose-demo_default    Removed
```

```bash
docker compose ps
# (vacío / no containers)
```

`down` **no** borra la imagen `nginx:alpine` (sigue en `docker images`).

### Fichero con otro nombre

```bash
docker compose -f docker-compose.yml up -d
docker compose -f docker-compose.yml down
```

### Nombre de proyecto explícito

```bash
docker compose -p mi-lab up -d
# contenedores tipo mi-lab-web-1
docker compose -p mi-lab down
```

### Ver también con Docker “normal”

```bash
docker compose up -d
docker ps --filter "name=compose-demo"
# mismos contenedores; Compose es la forma cómoda de gestionarlos en grupo
```

### Limpiar si dejaste algo a medias

```bash
cd ~/compose-demo
docker compose down
```

## Errores frecuentes

- `no configuration file provided` → no hay `compose.yaml` en el directorio actual; `cd` al proyecto o usa `-f`.
- `port is already allocated` → `8080` (u otro) pillado; cambia el mapeo en el YAML o para el otro contenedor.
- Ejecutar `docker compose down` en otra carpeta → no toca el proyecto que creías; el contexto es el directorio / `-p`.
- Confundir `down` con `stop`: `stop` deja contenedores; `down` los **elimina** (hay que `up` de nuevo).
- `permission denied` al socket → igual que siempre: grupo `docker` o `sudo` (ver install).


## Siguiente

[Hola Compose](compose-servicios.md)

## Relacionados

- Anterior: [que-es-compose](que-es-compose.md)  
- Siguiente: [compose-servicios](compose-servicios.md) — 2+ servicios, volumes y networks  
- Equivalente manual: [docker-pull-run](../01-fundamentos/docker-pull-run.md) · [docker-ps-logs](../01-fundamentos/docker-ps-logs.md)  
- [README de la carpeta](README.md)  
- Doc oficial: [docker compose up](https://docs.docker.com/reference/cli/docker/compose/up/) · [down](https://docs.docker.com/reference/cli/docker/compose/down/) · [Compose CLI](https://docs.docker.com/reference/cli/docker/compose/)  
