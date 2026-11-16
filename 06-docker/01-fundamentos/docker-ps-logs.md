# docker ps / docker logs

`docker ps` lista contenedores. `docker logs` muestra lo que el contenedor ha escrito por pantalla (stdout/stderr).  
Son los dos primeros sitios donde mirar cuando “no sé si está corriendo” o “no veo qué pasa”.

> **Cómo leer la sintaxis:** `<algo>` = lo sustituyes tú · `[algo]` = opcional.  
> Detalle: [Cómo leer la sintaxis](../../README.md#cómo-leer-la-sintaxis-muy-importante).

## Ideas previas

| Concepto | En pocas palabras |
|----------|-------------------|
| Contenedor en marcha | Aparece en `docker ps` (sin `-a`) |
| Contenedor parado / exited | Solo con `docker ps -a` |
| ID / nombre | Puedes usar el ID corto o el `--name` que le diste |
| Logs | Salida del proceso principal del contenedor (no es un fichero mágico aparte) |
| Seguir logs (`-f`) | Como `tail -f`: se van mostrando líneas nuevas |

```text
docker run -d --name web-demo …
        │
        ├── docker ps          → ¿está Up?
        └── docker logs web-demo
```

## Sintaxis

```bash
docker ps
docker ps -a
docker ps [opciones]

docker logs <contenedor>
docker logs [opciones] <contenedor>
```

| Opción (`ps`) | Qué hace |
|---------------|----------|
| `-a` | Todos (también parados) |
| `-q` | Solo IDs (útil para scripts) |
| `--filter <filtro>` | Filtrar (ej. por nombre o estado) |
| `--format …` | Formato personalizado |

| Opción (`logs`) | Qué hace |
|-----------------|----------|
| `-f` / `--follow` | Seguir en vivo |
| `--tail <n>` | Últimas `n` líneas |
| `-t` / `--timestamps` | Marca de tiempo |
| `--since <tiempo>` | Desde hace un rato (ej. `10m`, `1h`) |

## Ejemplos

### Solo los que están en marcha

```bash
docker ps
```

Cómo leer una fila típica:

```text
CONTAINER ID   IMAGE          STATUS         PORTS                  NAMES
a1b2c3d4e5f6   nginx:alpine   Up 2 minutes   0.0.0.0:8080->80/tcp   web-demo
```

| Columna | Idea |
|---------|------|
| `IMAGE` | Plantilla usada |
| `STATUS` | `Up …` = en marcha |
| `PORTS` | Mapeo host→contenedor (si publicaste con `-p`) |
| `NAMES` | Nombre (`--name`) o uno inventado |

### Todos (incluye Exited)

```bash
docker ps -a
```

Tras `docker run hello-world` verás algo como `STATUS Exited (0) …`: es normal; el proceso terminó bien (código 0).

### Preparar un contenedor de demo y listarlo

```bash
docker run -d --name web-demo -p 8080:80 nginx:alpine
docker ps
# … nginx:alpine   Up …   0.0.0.0:8080->80/tcp   web-demo
```

### Filtrar por nombre

```bash
docker ps -a --filter "name=web-demo"
docker ps -a --filter "status=exited"
docker ps --filter "status=running"
```

### Solo IDs

```bash
docker ps -q
docker ps -aq
```

### Ver logs

```bash
docker logs <contenedor>
docker logs web-demo
# líneas del arranque de nginx, p. ej. "start worker processes"
```

### Últimas líneas + seguir en vivo

```bash
docker logs --tail 50 web-demo
docker logs -f web-demo
# se quedan “pegados” mostrando líneas nuevas (peticiones, errores…)
```

Salir del seguimiento: `Ctrl+C` (no para el contenedor; solo dejas de mirar los logs).

### Con marcas de tiempo

```bash
docker logs -t --tail 20 web-demo
```

### Desde hace un rato

```bash
docker logs --since 10m web-demo
docker logs --since 1h web-demo
```

### Contenedor que ya terminó

```bash
docker run --name hola-tmp hello-world
docker ps -a --filter "name=hola-tmp"
# … hello-world   Exited (0) …   hola-tmp

docker logs hola-tmp
# texto "Hello from Docker!" …
```

Los logs siguen disponibles aunque el contenedor esté **Exited** (hasta que lo borres con `rm`).

## Errores frecuentes

- “No veo mi contenedor” con `docker ps` → prueba `docker ps -a` (puede estar parado).
- `Error: No such container` → nombre/ID mal escrito; revisa con `docker ps -a`.
- Confundir `logs -f` con “el contenedor se ha parado”: `Ctrl+C` solo corta la vista de logs.
- Contenedor `Exited (1)` u otro código ≠ 0 → el proceso falló; `docker logs <nombre>` suele explicar el motivo.
- Pensar que `ps` lista **imágenes**: las imágenes van con `docker images` (bloque de imágenes más adelante).


## Siguiente

[… Up …   web-demo](docker-stop-start-rm.md)

## Relacionados

- Anterior: [docker-pull-run](docker-pull-run.md)  
- Siguiente: [docker-stop-start-rm](docker-stop-start-rm.md) — parar, arrancar, borrar  
- Luego: [docker-exec](docker-exec.md) — entrar al contenedor en marcha  
- [README de la carpeta](README.md)  
- Doc oficial: [docker ps](https://docs.docker.com/reference/cli/docker/container/ls/) · [docker logs](https://docs.docker.com/reference/cli/docker/container/logs/)  
