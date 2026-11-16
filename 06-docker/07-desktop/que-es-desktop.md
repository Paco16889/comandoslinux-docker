# Qué es Docker Desktop (en Linux)

Docker Desktop es una aplicación con **interfaz gráfica** (bandeja, ajustes, panel de contenedores…).  
En Linux **no** es “solo una ventana sobre tu Engine”: arranca una **máquina virtual (VM)** y el CLI habla con ella por el contexto **`desktop-linux`**.

> **Cómo leer la sintaxis:** `<algo>` = lo sustituyes tú · `[algo]` = opcional.  
> Detalle: [Cómo leer la sintaxis](../../README.md#cómo-leer-la-sintaxis-muy-importante).

Fuentes oficiales: [Install Docker Desktop on Linux](https://docs.docker.com/desktop/setup/install/linux/).

## Ideas previas

| Concepto | En pocas palabras |
|----------|-------------------|
| Engine (esta guía, bloques 00–06) | Motor nativo en tu Linux + `docker` en terminal |
| Desktop | App GUI + VM + contexto `desktop-linux` |
| Contexto | “A qué motor apunta el CLI” (`default` vs `desktop-linux`) |
| Repo Docker | El de `download.docker.com` (mismo que para Engine) |
| CLI (`docker`) | Cliente de terminal; Desktop **también** lo usa / actualiza |

```text
Engine clásico:
  tu Linux → dockerd (systemd) → socket /var/run/docker.sock → contexto default

Desktop en Linux:
  tu Linux → app Desktop → VM → motor dentro → contexto desktop-linux
```

## ¿Pide Engine o CLI instalados?

Según la doc oficial de Desktop en Linux:

| Distro | Qué pide / asume de primeras |
|--------|------------------------------|
| **Ubuntu / Debian / Fedora** | **Paso 1:** configurar el **repositorio apt/dnf de Docker** (el mismo “step one” que en la install de Engine). Luego descargas el `.deb` / `.rpm` de **Desktop** e instalas ese paquete. |
| **Arch (experimental)** | Instalar primero el **binario cliente** `docker` (CLI) y después el paquete de Desktop. |

En la práctica, si ya seguiste [instalacion-engine](../00-introduccion/instalacion-engine.md):

- Ya tienes el **repo** + **`docker-ce-cli`** (y casi seguro el Engine completo).  
- Te falta sobre todo **descargar e instalar el paquete Desktop**.  
- El instalador de Desktop crea un enlace para que su CLI wrapper use el CLI clásico en `/usr/bin/docker` (detalle en la doc de Ubuntu/Debian/Fedora).

No hace falta “desinstalar Engine” para poner Desktop: pueden **convivir**, pero con matices (siguiente tema: [engine-y-desktop-juntos](engine-y-desktop-juntos.md)).

## Engine vs Desktop (tabla)

| | Docker Engine | Docker Desktop (Linux) |
|--|---------------|-------------------------|
| Interfaz | Terminal | GUI + también CLI |
| Dónde corre el motor | En el host | En una **VM** |
| Almacenamiento de imágenes/contenedores | `/var/lib/docker` (típico) | Aislado en la VM |
| Contexto CLI | `default` | `desktop-linux` (al arrancar Desktop) |
| Esta guía (bloques 00–06) | Camino principal | Opcional (este bloque 07) |

Importante (doc oficial): lo que tengas en Engine **no aparece mágicamente** en Desktop (almacenes distintos).

## ¿Para qué lo usa un aficionado?

- Preferir GUI (ver contenedores, logs, settings).  
- Límites de recursos desde la app.  
- Extensiones / herramientas que vienen con Desktop.  
- Misma “experiencia Desktop” que en Windows/Mac (con matices Linux).

Si solo quieres aprender comandos: **Engine basta** (y es más ligero en muchos labs).

## Licencia / suscripción (aviso)

Docker indica que el uso **comercial** de Desktop en empresas grandes (más de 250 empleados **o** más de 10 M USD de ingresos anuales) requiere **suscripción de pago**.  
Uso personal / aprendizaje: revisa siempre los [términos actuales](https://docs.docker.com/subscription/desktop-license/) — cambian con el tiempo.

## Mini vista de contextos (cuando ya esté instalado)

```bash
docker context ls
```

```text
NAME            DOCKER ENDPOINT
default         unix:///var/run/docker.sock          ← Engine del host
desktop-linux   unix:///home/<usuario>/.docker/desktop/docker.sock  ← Desktop
```

Detalle y cambio de contexto: [engine-y-desktop-juntos](engine-y-desktop-juntos.md).

## Siguiente

[Instalación de Docker Desktop (Linux)](instalacion-desktop.md)

## Relacionados

- Engine: [que-es-docker](../00-introduccion/que-es-docker.md) · [instalacion-engine](../00-introduccion/instalacion-engine.md)  
- Siguiente: [instalacion-desktop](instalacion-desktop.md)  
- Convivencia: [engine-y-desktop-juntos](engine-y-desktop-juntos.md)  
- [README de la carpeta](README.md)  
- Doc oficial: [Desktop on Linux](https://docs.docker.com/desktop/setup/install/linux/) · [Desktop vs Engine](https://docs.docker.com/get-started/docker-overview/#docker-desktop-and-the-cli) · [License](https://docs.docker.com/subscription/desktop-license/)



