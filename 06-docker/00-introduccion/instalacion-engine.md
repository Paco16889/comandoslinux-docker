# Instalación de Docker Engine (CLI)

Instalamos el **motor oficial** en Linux (camino principal de la guía).  
Docker Desktop (GUI) es **opcional** y va en [07-desktop](../07-desktop/) — ahí reutilizas este **repo** / CLI.  
Basado en la documentación oficial:

- [Install overview](https://docs.docker.com/engine/install/)  
- [Debian](https://docs.docker.com/engine/install/debian/)  
- [Ubuntu](https://docs.docker.com/engine/install/ubuntu/)  
- [Fedora](https://docs.docker.com/engine/install/fedora/)  
- [Post-install](https://docs.docker.com/engine/install/linux-postinstall/)  

> **Cómo leer la sintaxis:** `<algo>` = lo sustituyes tú · `[algo]` = opcional.  
> Detalle: [Cómo leer la sintaxis](../../README.md#cómo-leer-la-sintaxis-muy-importante).

## Elige tu distro

| Distro | Qué sección seguir | Repo / URL de paquetes |
|--------|--------------------|-------------------------|
| **Debian** | [Opción A — Debian](#opción-a--debian) | `download.docker.com/linux/debian` |
| **Ubuntu** | [Opción B — Ubuntu](#opción-b--ubuntu) | `download.docker.com/linux/ubuntu` |
| **Linux Mint** | [Opción B — Ubuntu](#opción-b--ubuntu) (Mint deriva de Ubuntu) | igual que Ubuntu + `UBUNTU_CODENAME` |
| **Fedora** | [Opción C — Fedora](#opción-c--fedora) | `download.docker.com/linux/fedora` |
| **Arch** | [Opción D — Arch](#opción-d--arch) | repos de Arch (`pacman`) |

```bash
cat /etc/os-release
# ID=debian | ubuntu | linuxmint | fedora | arch …
```

**Mint:** no está en la tabla “oficial” de Docker como distro propia, pero suele ir bien con la guía de **Ubuntu** usando el campo `UBUNTU_CODENAME` de `/etc/os-release`.  
**Debian puro** (o LMDE, basado en Debian): usa la sección **Debian**, no la de Ubuntu.

## Ideas previas

| Concepto | En pocas palabras |
|----------|-------------------|
| `docker-ce` | Community Edition = Engine oficial |
| Repo Docker | Mejor que el paquete `docker.io` de la distro (para seguir la doc oficial) |
| Grupo `docker` | Permite `docker` sin `sudo` (casi como privilegios altos) |
| `hello-world` | Contenedor de prueba |

## 1) Quitar paquetes conflictivos

### Debian / Ubuntu / Linux Mint

```bash
sudo apt remove docker.io docker-compose docker-compose-v2 docker-doc docker-buildx podman-docker containerd runc 2>/dev/null
sudo apt update
```

Si `apt` dice que no hay nada que quitar: perfecto.

### Fedora

```bash
sudo dnf remove docker docker-client docker-client-latest docker-common docker-latest \
  docker-latest-logrotate docker-logrotate docker-selinux docker-engine-selinux docker-engine
```

> Esto no borra solos los datos de `/var/lib/docker/` si ya existían.

---

## 2) Instalar desde el repositorio oficial

### Opción A — Debian

Versiones que documenta Docker actualmente (comprueba la [página oficial](https://docs.docker.com/engine/install/debian/) por si cambian): p. ej. Debian 12/13 (`bookworm`, `trixie`).

```bash
cat /etc/os-release
# VERSION_CODENAME=bookworm   (ejemplo)
```

Clave GPG + repo **debian** (ojo: no uses la URL de ubuntu):

```bash
sudo apt update
sudo apt install ca-certificates curl

sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/debian/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

sudo tee /etc/apt/sources.list.d/docker.sources <<EOF
Types: deb
URIs: https://download.docker.com/linux/debian
Suites: $(. /etc/os-release && echo "$VERSION_CODENAME")
Components: stable
Architectures: $(dpkg --print-architecture)
Signed-By: /etc/apt/keyrings/docker.asc
EOF

sudo apt update
```

Instalar:

```bash
sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

Servicio:

```bash
sudo systemctl status docker
# si hace falta:
sudo systemctl enable --now docker
```

Si usas un Debian “testing” o derivado (p. ej. Kali) y el codename no existe en el repo Docker, la doc oficial dice sustituir `Suites:` por un codename Debian estable conocido (`bookworm`, `trixie`…).

### Opción B — Ubuntu / Linux Mint

```bash
cat /etc/os-release
# Ubuntu: VERSION_CODENAME=noble (ejemplo)
# Mint:   UBUNTU_CODENAME=noble  ← usa este en Mint
```

Clave GPG + repo **ubuntu**:

```bash
sudo apt update
sudo apt install ca-certificates curl

sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

sudo tee /etc/apt/sources.list.d/docker.sources <<EOF
Types: deb
URIs: https://download.docker.com/linux/ubuntu
Suites: $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}")
Components: stable
Architectures: $(dpkg --print-architecture)
Signed-By: /etc/apt/keyrings/docker.asc
EOF

sudo apt update
```

En Mint, `${UBUNTU_CODENAME:-$VERSION_CODENAME}` suele coger el codename **Ubuntu** correcto. Si pusieras el codename solo de Mint, el repo fallaría.

```bash
sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
sudo systemctl status docker
sudo systemctl enable --now docker
```

### Opción C — Fedora

```bash
sudo dnf config-manager addrepo --from-repofile https://download.docker.com/linux/fedora/docker-ce.repo

sudo dnf install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

sudo systemctl enable --now docker
```

### Opción D — Arch

```bash
sudo pacman -S docker docker-compose
sudo systemctl enable --now docker
```

### Script get.docker.com (solo labs)

```bash
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh ./get-docker.sh --dry-run
# revisa la salida; si te convence:
# sudo sh get-docker.sh
```

Útil para pruebas; en máquinas “serias” preferimos el repo oficial a mano (A/B/C).

---

## 3) Verificar: hello-world

En **Debian, Ubuntu, Mint, Fedora, Arch**:

```bash
sudo docker run hello-world
```

Debe descargar una imagen pequeña, mostrar un mensaje y salir. Salida típica (resumida):

```text
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
…
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.
…
```

```bash
sudo docker version
```

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
sudo docker info
# Containers: …  Images: …  Server Version: …
```

## 4) Post-instalación: Docker sin sudo (opcional)

```bash
sudo groupadd docker
# si ya existe, no pasa nada

sudo usermod -aG docker "$USER"
```

Cierra sesión y vuelve a entrar (o reinicia la VM). Mientras tanto:

```bash
newgrp docker
docker run hello-world
# Hello from Docker!
# (sin pedir sudo)
```

Si aún falla:

```text
permission denied while trying to connect to the Docker daemon socket …
```

→ cierra sesión del todo (o reinicia) tras `usermod -aG docker`.

**Seguridad:** el grupo `docker` otorga un poder muy cercano a root. En PC de aprendizaje personal suele usarse; en servidores compartidos, con cuidado.

Si aparece error de permisos en `~/.docker` por haber usado `sudo` antes:

```bash
sudo chown "$USER":"$USER" "$HOME/.docker" -R
sudo chmod g+rwx "$HOME/.docker" -R
```

## 5) Arranque al encender

En Debian/Ubuntu/Mint suele quedar habilitado. En Fedora:

```bash
sudo systemctl enable docker.service
sudo systemctl enable containerd.service
```

## 6) Nota sobre firewall (ufw)

Al publicar puertos de contenedores (`-p`), Docker puede saltarse expectativas de ufw.  
Para `hello-world` no necesitas abrir nada. Cuando publiques webs/SSH en contenedores, revisa la doc “Docker and ufw” y nuestra guía [ufw](../../05-extras/seguridad/ufw.md).

## Mini checklist

| Paso | Debian | Ubuntu | Mint | Fedora |
|------|--------|--------|------|--------|
| Quitar `docker.io` / conflictos | ✓ | ✓ | ✓ | ✓ (dnf) |
| Repo `linux/debian` | ✓ | | | |
| Repo `linux/ubuntu` | | ✓ | ✓ (+ UBUNTU_CODENAME) | |
| Repo fedora docker-ce | | | | ✓ |
| `docker-ce` + plugins | ✓ | ✓ | ✓ | ✓ |
| `hello-world` OK | ✓ | ✓ | ✓ | ✓ |
| Grupo `docker` (opcional) | ✓ | ✓ | ✓ | ✓ |

## Actualizar

```bash
# Debian / Ubuntu / Mint
sudo apt update
sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

# Fedora
sudo dnf upgrade docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

## Desinstalar (referencia)

```bash
# Debian / Ubuntu / Mint
sudo apt purge docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin docker-ce-rootless-extras
sudo rm /etc/apt/sources.list.d/docker.sources
sudo rm /etc/apt/keyrings/docker.asc

# Fedora
sudo dnf remove docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin docker-ce-rootless-extras
```

Borrar **todos** los datos de Docker (destructivo):

```bash
# sudo rm -rf /var/lib/docker
# sudo rm -rf /var/lib/containerd
```

## Errores frecuentes

- En **Debian** usar la URL `.../linux/ubuntu` (o al revés en Ubuntu/Mint) → repo incorrecto.  
- En **Mint** usar el codename de Mint en `Suites:` en lugar de `UBUNTU_CODENAME`.  
- Mezclar `docker.io` (repos de la distro) con `docker-ce` (Docker Inc.).  
- `permission denied` al socket → grupo `docker` o falta re-login.  
- Fedora: olvidar `systemctl enable --now docker`.

## Siguiente

[27.x.x](../01-fundamentos/conceptos-rapidos.md)

## Relacionados

- [que-es-docker](que-es-docker.md)  
- [01-fundamentos](../01-fundamentos/)  
- Opcional Desktop: [07-desktop](../07-desktop/)  
- Doc oficial: [Install overview](https://docs.docker.com/engine/install/) · [Debian](https://docs.docker.com/engine/install/debian/) · [Ubuntu](https://docs.docker.com/engine/install/ubuntu/) · [Fedora](https://docs.docker.com/engine/install/fedora/) · [Arch (engine)](https://wiki.archlinux.org/title/Docker) · [Post-install](https://docs.docker.com/engine/install/linux-postinstall/)



