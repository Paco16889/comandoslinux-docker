# Qué es Docker Compose

Compose sirve para definir **varios contenedores** (servicios) en un fichero YAML y levantarlos/pararlos **juntos** con un solo comando.  
En esta guía usamos el plugin oficial: **`docker compose`** (con espacio), que ya instalaste con el Engine (`docker-compose-plugin`).

> **Cómo leer la sintaxis:** `<algo>` = lo sustituyes tú · `[algo]` = opcional.  
> Detalle: [Cómo leer la sintaxis](../../README.md#cómo-leer-la-sintaxis-muy-importante).

## Ideas previas

| Concepto | En pocas palabras |
|----------|-------------------|
| Servicio | Un contenedor “con rol” en el YAML (`web`, `db`…) |
| `compose.yaml` | Fichero de definición (también verás `docker-compose.yml`) |
| Proyecto | Carpeta + nombre; Compose agrupa contenedores/redes/volumes de ese proyecto |
| Plugin | Se invoca como `docker compose …` (no hace falta un binario aparte) |
| Stack | El conjunto de servicios del fichero (idea informal) |

```text
Antes (a mano):
  docker network create …
  docker run --network … --name web …
  docker run --network … --name db …

Con Compose:
  un fichero YAML
  docker compose up -d
```

## `docker compose` vs `docker-compose`

| | `docker compose` (plugin) | `docker-compose` (v1, guion) |
|--|---------------------------|------------------------------|
| Esta guía | **Sí** | No lo usamos como vía principal |
| Cómo se instala aquí | `docker-compose-plugin` con Engine | Paquete antiguo / pip en muchos tutoriales viejos |
| Comando | `docker compose up` | `docker-compose up` |

Si `docker compose version` funciona, vas bien. Detalle de install: [instalacion-engine](../00-introduccion/instalacion-engine.md).

## Sintaxis

```bash
docker compose version
docker compose [comando] [opciones]
```

El fichero se busca por defecto en el directorio actual:

- `compose.yaml` / `compose.yml`
- o `docker-compose.yaml` / `docker-compose.yml`

```bash
docker compose -f <fichero> <comando>
```

## Ejemplos

### Comprobar que el plugin está

```bash
docker compose version
```

Salida típica:

```text
Docker Compose version v2.x.x
```

Si falla:

```text
docker: unknown command: docker compose
```

→ revisa que instalaste `docker-compose-plugin` (Debian/Ubuntu/Mint/Fedora) o el paquete equivalente en Arch.

### Esqueleto mínimo de un fichero

`compose.yaml`:

```yaml
services:
  web:
    image: nginx:alpine
    ports:
      - "8080:80"
```

Idea: un servicio `web` = lo que antes hacías con `docker run -d -p 8080:80 nginx:alpine`.

Mini prueba (detalle en [compose-basico](compose-basico.md)):

```bash
cd ~/compose-demo
docker compose up -d
```

```text
[+] Running 2/2
 ✔ Network compose-demo_default  Created
 ✔ Container compose-demo-web-1  Started
```

```bash
curl -I http://127.0.0.1:8080
# HTTP/1.1 200 OK

docker compose down
```

```text
[+] Running 2/2
 ✔ Container compose-demo-web-1  Removed
 ✔ Network compose-demo_default  Removed
```

### Dónde lo pones

```bash
mkdir -p ~/compose-demo
cd ~/compose-demo
# aquí creas compose.yaml
pwd
# /home/<usuario>/compose-demo
```

Compose toma el **nombre de la carpeta** (salvo que pongas `-p <proyecto>`) para prefijar recursos.

### Relación con lo que ya sabes

| En `docker run` | En Compose (YAML) |
|-----------------|-------------------|
| `--name` / imagen | `services:` + `image:` |
| `-p 8080:80` | `ports: ["8080:80"]` |
| `-v datos:/data` | `volumes:` |
| `--network lab` | `networks:` (Compose crea una red del proyecto) |
| varios `run` | varios servicios bajo `services:` |

Los comandos del día a día (`up`, `down`, `ps`, `logs`) van en [compose-basico](compose-basico.md).

## Errores frecuentes

- Escribir `docker-compose` (con guion) porque un tutorial de 2019 lo dice → prueba primero `docker compose`.
- Crear el YAML en otra carpeta y ejecutar Compose desde `$HOME` → “no encuentra servicios” / fichero; haz `cd` al proyecto.
- Mezclar Desktop y Engine en la cabeza: aquí solo CLI + plugin, sin GUI.
- Pensar que Compose reemplaza Docker: **usa** el Engine; solo orquesta contenedores.

## Relacionados

- Install (plugin): [instalacion-engine](../00-introduccion/instalacion-engine.md)  
- Redes / volumes (idea): [04-networks](../04-networks/) · [03-volumes](../03-volumes/)  
- Siguiente: [compose-basico](compose-basico.md) — `up -d`, `down`, `ps`, `logs`  
- [README de la carpeta](README.md)  
- Doc oficial: [Why Compose?](https://docs.docker.com/compose/) · [Compose specification](https://docs.docker.com/reference/compose-file/)  
