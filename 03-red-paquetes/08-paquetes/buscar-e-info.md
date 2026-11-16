# Buscar e informar — chuleta multi-distro

Misma necesidad, distinto comando según el gestor.

> Recuerda: `<texto>`, `<paquete>`, `<id_de_la_app>` los sustituyes tú.  
> Detalle: [Cómo leer la sintaxis](../../README.md#cómo-leer-la-sintaxis-muy-importante).

## Laboratorio Mint: “quiero instalar htop”

Hazlo en este orden (funciona igual en Ubuntu/Debian con `apt`):

```bash
sudo apt update
# refresca el catálogo

apt search htop
# busca candidatos; mira el nombre exacto del paquete

apt show htop
# descripción, tamaño, dependencias…

sudo apt install htop
# instalas

htop
# o: which htop
# /usr/bin/htop

apt list --installed | grep htop
# confirma que está instalado
```

Misma idea si prefieres Flatpak (apps de escritorio; htop a veces también):

```bash
flatpak search htop
# anota el ID (columna Application ID)

flatpak install flathub <id_de_la_app>
flatpak list | grep -i htop
```

Regla en Mint: terminal/sistema → `apt`; app gráfica grande → Flatpak suele ser mejor.

## Buscar un programa

```bash
# apt (Debian / Ubuntu / Linux Mint)
apt search <texto>
apt search "image editor"

# Flatpak (Mint, Fedora, etc.)
flatpak search <texto>
flatpak search gimp

# dnf (Fedora)
dnf search <texto>

# pacman (Arch)
pacman -Ss <texto>

# snap (Ubuntu y otras)
snap find <texto>
```

## Ver detalles de un paquete

```bash
# apt
apt show <paquete>
apt show curl

# dnf
dnf info <paquete>

# pacman (en repos)
pacman -Si <paquete>

# flatpak
flatpak info <id_de_la_app>
flatpak info org.gimp.GIMP

# snap
snap info <nombre>
```

## ¿Está instalado?

```bash
# apt
apt list --installed | grep <texto>
dpkg -l | grep <texto>
dpkg -l | grep htop

# dnf
dnf list installed | grep <texto>

# pacman
pacman -Qs <texto>

# flatpak
flatpak list | grep <texto>
flatpak list --app

# snap
snap list | grep <texto>
```

¿El comando existe en el PATH?

```bash
which <comando>
command -v <comando>
type <comando>
```

## Instalar (recordatorio)

```bash
# apt
sudo apt update
sudo apt install <paquete>
sudo apt install tree curl wget

# flatpak
flatpak install flathub <id_de_la_app>
flatpak install flathub org.gimp.GIMP

# dnf
sudo dnf install <paquete>

# pacman
sudo pacman -Syu
sudo pacman -S <paquete>

# snap
sudo snap install <nombre>
```

## Actualizar “todo lo razonable”

```bash
# Mint / Ubuntu / Debian
sudo apt update && sudo apt upgrade
flatpak update

# Fedora
sudo dnf upgrade
flatpak update

# Arch
sudo pacman -Syu
flatpak update

# Ubuntu con snaps
sudo apt update && sudo apt upgrade
sudo snap refresh
```

## Errores frecuentes al buscar/instalar

```text
E: Unable to locate package <paquete>
```

```bash
sudo apt update
apt search <texto>
# el nombre puede ser distinto: python3 vs python, freerdp2-x11 vs remmina…
```

```text
No match for argument / target not found
```

- En dnf/pacman: mismo truco → `search` / `-Ss` primero.

- Paquete ≠ nombre del comando: a veces instalas `iputils-ping` para tener `ping`.
- En Mint no aparece en `apt` pero sí en Flathub → `flatpak search <texto>`.

## Consejo si no encuentras un paquete

1. `search` / `find` con otra palabra.  
2. Actualiza índices (`apt update`, `pacman -Syu`…).  
3. En Mint: prueba también Flatpak.  
4. Revisa si el comando y el paquete se llaman distinto.  

## Siguiente

[sh vs bash vs `./fichero`](../../04-automatizacion/09-scripts/sh-vs-bash-vs-ejecutable.md)

## Relacionados

- [apt](apt.md) · [flatpak](flatpak.md) · [dnf](dnf.md) · [pacman](pacman.md) · [snap](snap.md)  
- [conceptos](conceptos.md) — por qué a veces “no está”



