# Instalación de Docker Desktop (Linux)

Pasos alineados con la documentación oficial.  
**Antes:** conviene tener el **repositorio Docker** (y el CLI) como en [instalacion-engine](../00-introduccion/instalacion-engine.md). Desktop no sustituye ese “step one” en Ubuntu/Debian/Fedora.

> **Cómo leer la sintaxis:** `<algo>` = lo sustituyes tú · `[algo]` = opcional.  
> Detalle: [Cómo leer la sintaxis](../../README.md#cómo-leer-la-sintaxis-muy-importante).

Fuentes:

- [General / requisitos](https://docs.docker.com/desktop/setup/install/linux/)  
- [Ubuntu](https://docs.docker.com/desktop/setup/install/linux/ubuntu/) · [Debian](https://docs.docker.com/desktop/setup/install/linux/debian/) · [Fedora](https://docs.docker.com/desktop/setup/install/linux/fedora/) · [Arch](https://docs.docker.com/desktop/setup/install/linux/archlinux/)

## Ideas previas

| Concepto | En pocas palabras |
|----------|-------------------|
| KVM | Virtualización en el kernel; Desktop en Linux la necesita |
| `.deb` / `.rpm` | Paquete de Desktop que descargas e instalas |
| `systemctl --user` | Arranque de Desktop como servicio de **usuario** |
| `pass` + GPG | Gestor de credenciales que Desktop usa al iniciar sesión en Hub |
| x86_64 | Paquetes oficiales Desktop Linux documentados para amd64 |

## Requisitos generales (oficiales)

- CPU/kernel **64-bit** con virtualización.  
- **KVM** disponible; QEMU **≥ 5.2** (recomiendan lo último).  
- **systemd**.  
- Entorno gráfico: GNOME, KDE o MATE (otros pueden ir).  
- Al menos **4 GB** de RAM.  
- Inicializar **`pass`** si vas a iniciar sesión en Desktop (credenciales).  

Comprobar KVM (idea):

```bash
lsmod | grep kvm
# deberías ver kvm y kvm_intel o kvm_amd

ls -al /dev/kvm
sudo usermod -aG kvm "$USER"
# cierra sesión y vuelve a entrar
```

Opcional:

```bash
# si tienes el paquete cpu-checker (Ubuntu/Debian):
kvm-ok
```

**Anidado:** Docker no soporta Desktop Linux dentro de otra VM de forma oficial; mejor en máquina nativa.

### Plataformas con paquete oficial

| Distro | Paquete | Nota |
|--------|---------|------|
| Ubuntu | `.deb` | LTS actuales según doc (comprueba la página) |
| Debian | `.deb` | p. ej. Debian 12 en la doc |
| Fedora | `.rpm` | versión reciente según doc |
| RHEL | `.rpm` | hay página oficial aparte |
| Arch | experimental | Docker no lo verifica igual |
| **Linux Mint** | no listado propio | Suele seguirse la vía **Ubuntu** (como con Engine); no es soporte oficial de Desktop |

## 0) Repo Docker + CLI (lo que “pide de primeras”)

### Ubuntu / Debian / Mint / Fedora

La install de Desktop dice: **configura el repositorio de Docker** (paso 1 de Engine) **y después** instala el paquete Desktop.

Si ya hiciste [instalacion-engine](../00-introduccion/instalacion-engine.md): **este paso ya lo tienes**.

Si solo quieres Desktop y aún no hay repo:

1. Sigue solo la parte de **añadir el repo** de tu distro en `instalacion-engine.md`.  
2. Asegúrate de tener el CLI disponible (`docker-ce-cli` al instalar Engine, o el conjunto completo).  
3. Continúa con la descarga del paquete Desktop (§ siguientes).

Comprobar CLI:

```bash
docker --version
# Docker version …
which docker
# suele ser /usr/bin/docker
```

### Arch (experimental, doc oficial)

Piden el **cliente** `docker` antes del paquete Desktop (binario estático). La versión del tarball **cambia**; mira la [página Arch](https://docs.docker.com/desktop/setup/install/linux/archlinux/) o [binarios Engine](https://download.docker.com/linux/static/stable/x86_64/) para el nombre actual:

```bash
# Ejemplo de idea (ajusta la versión a la que indique la doc):
# wget …/docker-<version>.tgz …
# sudo cp docker /usr/local/bin/
docker --version
```

Si ya instalaste Engine con `pacman -S docker`, ya tienes CLI; igual revisa la nota experimental de Desktop en Arch.

## 1) Descargar el paquete Desktop

Entra en la [página de tu distro](https://docs.docker.com/desktop/setup/install/linux/) o descarga directa (amd64), por ejemplo:

```bash
cd ~/Descargas
# o ~/Downloads según idioma del sistema

# Ubuntu / Debian / Mint (paquete .deb)
wget -O docker-desktop-amd64.deb \
  "https://desktop.docker.com/linux/main/amd64/docker-desktop-amd64.deb"

# Fedora (paquete .rpm) — usa esta en lugar del .deb
# wget -O docker-desktop-x86_64.rpm \
#   "https://desktop.docker.com/linux/main/amd64/docker-desktop-x86_64.rpm"
```

Checksums: [Release notes de Desktop](https://docs.docker.com/desktop/release-notes/).

## 2) Instalar según distro

### Ubuntu / Linux Mint (vía .deb)

```bash
sudo apt-get update
sudo apt install ./docker-desktop-amd64.deb
```

Puede salir un aviso `N: Download is performed unsandboxed…` al instalar un `.deb` local: la doc dice que **puedes ignorarlo**.

Si no usas GNOME:

```bash
sudo apt install gnome-terminal
```

En GNOME, la doc recomienda extensiones AppIndicator / KStatusNotifierItem para el icono de bandeja.

### Debian

```bash
sudo apt-get update
sudo apt-get install ./docker-desktop-amd64.deb
```

(Misma nota del aviso unsandboxed.)

### Fedora

```bash
sudo dnf install ./docker-desktop-x86_64.rpm
```

Sin GNOME:

```bash
sudo dnf install gnome-terminal
```

### Arch (experimental)

```bash
# Tras tener el CLI (paso 0):
sudo pacman -U ./docker-desktop-x86_64.pkg.tar.zst
```

(Descarga el `.pkg.tar.zst` desde la doc/release notes de Arch.)

Instalación típica en: `/opt/docker-desktop`.

## 3) Credenciales (`pass`) — si vas a iniciar sesión

Desktop en Linux usa **`pass`** (GPG) para credenciales. Si no está inicializado, verás avisos al hacer login.

```bash
gpg --generate-key
# nombre, email…

pass init <ID_GPG>
# el ID aparece en la salida de gpg (línea pub)
```

Detalle: sección *Signing in* en [Install Desktop on Linux](https://docs.docker.com/desktop/setup/install/linux/).

## 4) Arrancar Desktop

Desde el menú de aplicaciones: **Docker Desktop** → acepta el acuerdo de servicio (si no aceptas, no arranca).

O por terminal:

```bash
systemctl --user start docker-desktop
```

Comprobar CLI apuntando a Desktop (cuando esté arriba):

```bash
docker context ls
```

```text
NAME            DESCRIPTION     DOCKER ENDPOINT
default         …               unix:///var/run/docker.sock
desktop-linux * …               unix:///home/<usuario>/.docker/desktop/docker.sock
```

(`*` = contexto actual; al arrancar Desktop suele ser `desktop-linux`.)

```bash
docker version
```

```text
Client: …
Server: Docker Desktop
…
```

```bash
docker run --rm hello-world
```

```text
Hello from Docker!
This message shows that your installation appears to be working correctly.
…
```

Arranque al iniciar sesión:

```bash
systemctl --user enable docker-desktop
```

Parar:

```bash
systemctl --user stop docker-desktop
# o Quit desde el menú de la bandeja
```

## 5) Actualizar

Vuelve a descargar el paquete nuevo e instálalo encima.

```bash
# Debian/Ubuntu/Mint
sudo apt install ./docker-desktop-amd64.deb

# Fedora (doc: quitar e instalar)
sudo dnf remove docker-desktop
sudo dnf install ./docker-desktop-x86_64.rpm
```

## Errores frecuentes

- Instalar el `.deb` **sin** haber configurado el repo Docker → faltan dependencias; haz el step one de Engine.  
- KVM / `/dev/kvm` sin permisos → añade el usuario al grupo `kvm` y re-login.  
- “No veo mis contenedores de Engine en Desktop” → **normal**: almacenes distintos (VM vs host).  
- Puertos ocupados con Engine y Desktop a la vez → para Engine o cambia `-p` (ver [engine-y-desktop-juntos](engine-y-desktop-juntos.md)).  
- Empresa grande sin licencia → revisa términos de suscripción.  
- Mint / derivados: no están en la tabla oficial de Desktop; vía Ubuntu “mejor esfuerzo”.

## Siguiente

[Engine y Desktop a la vez (contextos)](engine-y-desktop-juntos.md)

## Relacionados

- Anterior: [que-es-desktop](que-es-desktop.md)  
- Siguiente: [engine-y-desktop-juntos](engine-y-desktop-juntos.md)  
- Repo/CLI/Engine: [instalacion-engine](../00-introduccion/instalacion-engine.md)  
- [README de la carpeta](README.md)  
- Doc oficial: [Ubuntu](https://docs.docker.com/desktop/setup/install/linux/ubuntu/) · [Debian](https://docs.docker.com/desktop/setup/install/linux/debian/) · [Fedora](https://docs.docker.com/desktop/setup/install/linux/fedora/) · [Arch](https://docs.docker.com/desktop/setup/install/linux/archlinux/) · [RHEL](https://docs.docker.com/desktop/setup/install/linux/rhel/) · [Release notes](https://docs.docker.com/desktop/release-notes/)



