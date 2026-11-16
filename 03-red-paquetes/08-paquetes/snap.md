# snap

Otro sistema de paquetes “en contenedor” impulsado sobre todo por **Ubuntu** (Canonical).  
Instala apps desde la Snap Store.

En **Linux Mint** a menudo se desaconseja o viene limitado (la filosofía de Mint prioriza Flatpak).  
Aquí lo documentamos para que lo reconozcas si ves tutoriales de Ubuntu.

## Sintaxis

```bash
snap <acción> [argumentos]...
sudo snap <acción> [argumentos]...
```

> Recuerda: `[ ]` = opcional.  
> Detalle: [Cómo leer la sintaxis](../../README.md#cómo-leer-la-sintaxis-muy-importante).

## ¿Está disponible?

```bash
snap version
```

Si está instalado, algo así:

```text
snap    2.x.x
snapd   2.x.x
series  16
ubuntu  24.04   (o la distro que sea)
kernel  …
```

Si falla:

```text
snap: command not found
```

→ no tienes `snapd` (o no está en el PATH).

Instalar el soporte (Ubuntu/Debian; en Mint puede no ser lo recomendado):

```bash
# Ubuntu / Debian
sudo apt install snapd

# Fedora
sudo dnf install snapd
```

## Ejemplos

```bash
snap find <texto>
snap find notepad
# Name      Version  Publisher  Notes  Summary
# notepad-plus-plus  …  …  -  …

snap info <nombre>
snap info vlc
# name:      vlc
# summary:   …
# publisher: …
# channels:  …
# (bloque largo con descripción y canales)

sudo snap install <nombre>
sudo snap install vlc
# vlc X.Y from … installed

snap list
# Name  Version  Rev  Tracking  Publisher  Notes
# core  …        …    …         canonical  …
# vlc   …        …    …         …          …

sudo snap refresh
# All snaps up to date.
# (o lista de snaps que actualiza)

sudo snap remove <nombre>
# <nombre> removed
```

## snap vs Flatpak vs apt (visión simple)

| Sistema | Encaje típico |
|---------|----------------|
| `apt` / `dnf` / `pacman` | Base del sistema |
| Flatpak | Apps de escritorio (muy común en Mint/Fedora) |
| Snap | Apps de escritorio (muy común en Ubuntu) |

```bash
# Ubuntu: a menudo verás snaps
sudo snap install <nombre>

# Mint: habitualmente Flatpak
flatpak install flathub <id_de_la_app>
```

## Errores frecuentes

- Seguir un tutorial de Ubuntu con `snap` en Mint y encontrar fricción: en Mint mira primero Flatpak o `apt`.
- Tener la misma app en snap y en apt/flatpak: duplicados y confusión en el menú.

## Relacionados

- [flatpak](flatpak.md) — alternativa más alineada con Mint  
- [apt](apt.md) — paquetes del sistema en Ubuntu/Mint  
