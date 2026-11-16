# 06 — Docker (Engine / CLI + Desktop opcional)

Bloque aparte del temario Linux general: contenedores con **Docker Engine** (línea de comandos) y, si quieres, **Docker Desktop** en Linux.

> **Cómo leer la sintaxis:** `<algo>` = lo sustituyes tú · `[algo]` = opcional.  
> Guía: [Cómo leer la sintaxis](../README.md#cómo-leer-la-sintaxis-muy-importante).

## Requisitos previos (del temario Linux)

Conviene haber visto al menos:

- Terminal básica y permisos (`sudo`)  
- [08-paquetes](../03-red-paquetes/08-paquetes/)  
- [systemctl](../04-automatizacion/11-avanzado/systemctl.md)  

## Orden de estudio

1. [00-introduccion](00-introduccion/) — qué es Docker + **instalación Engine**  
2. [01-fundamentos](01-fundamentos/) — run, ps, logs, stop, exec…  
3. [02-imagenes-dockerfile](02-imagenes-dockerfile/) — imágenes + Dockerfile  
4. [03-volumes](03-volumes/) — datos persistentes  
5. [04-networks](04-networks/) — redes entre contenedores  
6. [05-compose](05-compose/) — varios servicios con un fichero  
7. [06-avanzado](06-avanzado/) — healthcheck, límites, multi-stage…  
8. [07-desktop](07-desktop/) — **opcional:** Docker Desktop en Linux (GUI + VM)  

## Engine vs Desktop (para no liar)

| | Docker Engine | Docker Desktop |
|--|---------------|----------------|
| Qué es | Motor + CLI en Linux | App con GUI (+ VM en Linux) |
| Esta guía | Camino principal (00–06) | Opcional ([07-desktop](07-desktop/)) |
| Comando típico | `docker ...` → contexto `default` | Mismo CLI → contexto `desktop-linux` |

## Estado

| Carpeta | Estado |
|---------|--------|
| 00-introduccion | Listo (concepto + install Debian/Ubuntu/Mint/Fedora/Arch) |
| 01-fundamentos | Listo |
| 02-imagenes-dockerfile | Listo |
| 03-volumes | Listo |
| 04-networks | Listo |
| 05-compose | Listo |
| 06-avanzado | Listo |
| 07-desktop | Listo (concepto, install, Engine+Desktop / contextos) |

## Documentación oficial (atajos)

| Tema | Doc Docker |
|------|------------|
| Overview | [Docker overview](https://docs.docker.com/get-started/docker-overview/) |
| Install Engine | [Engine install](https://docs.docker.com/engine/install/) · [post-install](https://docs.docker.com/engine/install/linux-postinstall/) |
| CLI reference | [docker CLI](https://docs.docker.com/reference/cli/docker/) |
| Dockerfile | [Dockerfile reference](https://docs.docker.com/reference/dockerfile/) |
| Build | [docker build](https://docs.docker.com/reference/cli/docker/build/) · [Build overview](https://docs.docker.com/build/) |
| Storage | [Volumes](https://docs.docker.com/engine/storage/volumes/) · [Bind mounts](https://docs.docker.com/engine/storage/bind-mounts/) |
| Network | [Networking](https://docs.docker.com/engine/network/) |
| Compose | [Compose](https://docs.docker.com/compose/) · [Compose CLI](https://docs.docker.com/reference/cli/docker/compose/) |
| Desktop Linux | [Install Desktop](https://docs.docker.com/desktop/setup/install/linux/) |
| Contexts | [Docker contexts](https://docs.docker.com/engine/manage-resources/contexts/) |
| Hub | [Docker Hub](https://docs.docker.com/docker-hub/) |

Cada fichero del temario enlaza también la página oficial concreta en **Relacionados**.
