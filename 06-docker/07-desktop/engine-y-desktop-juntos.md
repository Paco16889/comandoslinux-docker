# Engine y Desktop a la vez (contextos)

En Linux puedes tener **Docker Engine** (host) y **Docker Desktop** (VM) instalados juntos.  
Comparten el comando `docker`, pero **no** el mismo almacén ni el mismo socket. La doc oficial recomienda **parar Engine** mientras usas Desktop para evitar choques de puertos y recursos.

> **Cómo leer la sintaxis:** `<algo>` = lo sustituyes tú · `[algo]` = opcional.  
> Detalle: [Cómo leer la sintaxis](../../README.md#cómo-leer-la-sintaxis-muy-importante).

Fuente: [Installing Desktop and Engine](https://docs.docker.com/desktop/setup/install/linux/#installing-docker-desktop-and-docker-engine).

## Ideas previas

| Concepto | En pocas palabras |
|----------|-------------------|
| Contexto `default` | CLI → Engine del host (`unix:///var/run/docker.sock`) |
| Contexto `desktop-linux` | CLI → Desktop (socket bajo `~/.docker/desktop/…`) |
| Arrancar Desktop | Suele poner `desktop-linux` como contexto actual |
| Parar Desktop | Suele devolver el contexto anterior (`default`) |
| Imágenes | Las de Engine **no** están en Desktop y al revés |

```text
docker context use default         →  Engine (temario 00–06)
docker context use desktop-linux   →  Desktop (GUI / VM)
```

## Sintaxis

```bash
docker context ls
docker context use <nombre>

sudo systemctl stop docker docker.socket containerd
sudo systemctl disable docker docker.socket containerd

sudo systemctl start docker
# (cuando quieras volver al Engine del host)
```

## Ejemplos

### Ver contextos

```bash
docker context ls
```

Salida típica (resumida):

```text
NAME            DESCRIPTION                               DOCKER ENDPOINT
default *       Current DOCKER_HOST based configuration   unix:///var/run/docker.sock
desktop-linux                                             unix:///home/<usuario>/.docker/desktop/docker.sock
```

El `*` marca el contexto **activo**.

### Cambiar a mano

```bash
docker context use default
# Current context is now "default"

docker info | head -n 5
# habla con el Engine del host

docker context use desktop-linux
# Current context is now "desktop-linux"
# (Desktop debe estar arrancado; si no, fallará la conexión)
```

### Parar Engine mientras usas Desktop (recomendación oficial)

```bash
sudo systemctl stop docker docker.socket containerd
```

Para que no arranque solo al reiniciar el PC:

```bash
sudo systemctl disable docker docker.socket containerd
```

Cuando quieras volver al flujo de esta guía (solo Engine):

```bash
systemctl --user stop docker-desktop
sudo systemctl enable --now docker
docker context use default
docker run --rm hello-world
```

### Conflicto de puertos (el clásico)

Si Engine tiene `nginx` en `8080` y Desktop lanza otro con `-p 8080:80`:

```text
port is already allocated
# o address already in use
```

Soluciones: para uno de los dos motores, o usa otro puerto del host.

### Comprobar “quién me responde”

```bash
docker context show
# default   o   desktop-linux

docker ps
# lista SOLO lo de ese contexto
```

### SDK / herramientas raras

Desktop en Linux usa un socket **por usuario**, no siempre `/var/run/docker.sock`.  
Scripts que asumen el socket del sistema pueden necesitar `DOCKER_HOST` (doc: *Using Docker SDKs with Docker Desktop*). Para el CLI normal con contextos, suele bastar `docker context use`.

## Errores frecuentes

- “Perdí mis contenedores” tras instalar Desktop → estás en `desktop-linux`; cambia a `default` (y arranca el servicio `docker`).  
- Dejar Engine y Desktop **los dos** Up con los mismos `-p` → conflictos.  
- Hacer `disable` de `docker` y olvidar cómo volver → `sudo systemctl enable --now docker`.  
- Pensar que `docker pull` en Desktop rellena el disco de Engine → no; son mundos aparte.  
- Arch / Mint: más fricción; prioriza contextos y logs si “no conecta”.

## Relacionados

- Install Desktop: [instalacion-desktop](instalacion-desktop.md) · [que-es-desktop](que-es-desktop.md)  
- Engine: [instalacion-engine](../00-introduccion/instalacion-engine.md)  
- Fundamentos CLI: [01-fundamentos](../01-fundamentos/)  
- [README de la carpeta](README.md)  
- Doc oficial: [Desktop + Engine](https://docs.docker.com/desktop/setup/install/linux/#installing-docker-desktop-and-docker-engine) · [Docker contexts](https://docs.docker.com/engine/manage-resources/contexts/) · [docker context](https://docs.docker.com/reference/cli/docker/context/)  
