# Qué es Docker (introducción)

Docker sirve para empaquetar una aplicación **con lo que necesita** (código, runtime, librerías, configs…) en una unidad llamada **contenedor**.  
Esa unidad puedes lanzarla en tu PC, en un servidor o en un lab y comportarse de forma muy parecida en todos sitios.

Esta página es solo la idea. Luego instalamos el **Engine** (CLI) y en fundamentos practicas con comandos.

> **Cómo leer la sintaxis:** cuando veas ejemplos más adelante, `<algo>` se sustituye y `[algo]` es opcional.  
> Detalle: [Cómo leer la sintaxis](../../README.md#cómo-leer-la-sintaxis-muy-importante).

## Ideas previas

| Concepto | En pocas palabras |
|----------|-------------------|
| Imagen | Plantilla empaquetada (solo lectura). Ej.: `nginx`, `hello-world` |
| Contenedor | Copia **en marcha** (o parada) de una imagen |
| Dockerfile | Receta en texto para **construir** una imagen |
| Engine | Motor + comando `docker` en la terminal |
| Registry | Sitio de donde bajas/subes imágenes (p. ej. Docker Hub) |
| Host | Tu Linux (Mint, Ubuntu, Debian, Fedora…) donde corre Docker |

## ¿Para qué lo usan los aficionados?

- Probar un servicio (web, base de datos…) sin ensuciar el sistema.  
- Repetir el mismo entorno (“en mi máquina funciona” → menos drama).  
- Seguir tutoriales modernos (casi todos traen `docker run` o Compose).  

## Contenedor vs máquina virtual (idea)

| | Contenedor (Docker) | VM clásica |
|--|---------------------|------------|
| Arranque | Suele ser rápido | Más pesado |
| Sistema | Comparte el **kernel** del host | Lleva su propio SO completo |
| Peso | Más ligero en general | Más aislamiento “de sistema entero” |
| Uso típico | Apps y servicios | Sistemas completos aislados |

```text
Tu Linux (host)
└── Docker Engine
    ├── contenedor A  (p. ej. web)
    └── contenedor B  (p. ej. base de datos)
```

## Analogía rápida

```text
Imagen     ≈ molde / plantilla de galleta
Contenedor ≈ galleta ya hecha (puedes hornear muchas del mismo molde)
Dockerfile ≈ la receta escrita para fabricar el molde
```

## Engine vs Desktop (otra vez, porque confunde)

| | Docker Engine | Docker Desktop |
|--|---------------|----------------|
| Qué usamos primero | **Sí** — terminal (bloques 00–06) | Opcional después ([07-desktop](../07-desktop/)) |
| Dónde (Linux) | Motor nativo en el host | App GUI + **VM** + contexto `desktop-linux` |
| Comando | `docker ...` | El mismo CLI (cambia el **contexto**) |

Desktop en Linux suele pedir antes el **repo Docker** / CLI (mismo step one que Engine). Detalle: [que-es-desktop](../07-desktop/que-es-desktop.md).

## Flujo mental del primer día

```text
1. Instalar Engine
2. docker run hello-world     → ¿funciona?
3. docker run …               → contenedores reales (fundamentos)
4. Imágenes / Dockerfile
5. Volumes, networks, Compose
```

## Mini vocabulario que verás en la terminal

```bash
docker version
```

Cuando el Engine ya responde, verás **dos** bloques (números según tu install):

```text
Client: Docker Engine - Community
 Version:           27.x.x
 …
Server: Docker Engine - Community
 Engine:
  Version:          27.x.x
  …
```

```bash
docker info
# Containers: 0
# Running: 0
# Images: 0
# Server Version: 27.x.x
# … (mucho más texto: storage, plugins…)
```

Si aún no instalaste, típico:

```text
Cannot connect to the Docker daemon at unix:///var/run/docker.sock.
Is the docker daemon running?
```

Es normal hasta terminar la [instalación](instalacion-engine.md).

## Relacionados

- Siguiente: [instalacion-engine](instalacion-engine.md) (Debian, Ubuntu, Mint, Fedora…)  
- Luego: [01-fundamentos](../01-fundamentos/)  
- Opcional: [07-desktop](../07-desktop/)  
- Doc oficial: [Docker overview](https://docs.docker.com/get-started/docker-overview/) · [Get started](https://docs.docker.com/get-started/)  
