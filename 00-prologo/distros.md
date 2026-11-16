# Distribuciones (distros): qué cambia y qué no

Una **distro** es un Linux empaquetado: instala el kernel, elige escritorio, repositorios y herramientas por defecto.  
Todas hablan “Linux”, pero los **nombres de paquetes** y algunos caminos cambian.

## Ideas previas

| Concepto | En pocas palabras |
|----------|-------------------|
| Familia Debian | Debian → Ubuntu → **Linux Mint** (y otras) |
| Familia Fedora / RHEL | Fedora, CentOS Stream, Rocky, Alma… |
| Familia Arch | Arch, EndeavourOS… (rolling release) |
| Rolling release | Se actualiza “en continuo” (Arch) |
| LTS | Versión de soporte largo (muy común en Ubuntu/Mint) |

## Tabla práctica (esta guía)

| Distro | Gestor | Notas para esta guía |
|--------|--------|----------------------|
| **Linux Mint** | `apt` + **Flatpak** | Ideal para escritorio; mucha atención en los ejemplos |
| Ubuntu | `apt` (+ a veces Snap) | Muy parecida a Mint en la terminal |
| Debian | `apt` | Más “pura”; versiones más conservadoras |
| Fedora | `dnf` | Nombres de servicio a veces distintos (`sshd`, `httpd`) |
| Arch | `pacman` | Muy actual; hay que actualizar con cuidado (`-Syu`) |

Detalle de comandos: [08-paquetes](../03-red-paquetes/08-paquetes/).

## Para qué suele usarse cada una (aficionado)

| Distro | Uso típico |
|--------|------------|
| **Linux Mint** | PC de casa/oficina: fácil, estable, parecido a Windows al empezar |
| **Ubuntu** | Escritorio popular + muchas guías; también servidores (`Ubuntu Server`) |
| **Debian** | Servidores estables; base de Ubuntu/Mint |
| **Fedora** | Escritorio “al día”; cercana al mundo RHEL/empresa |
| **Arch** | Aprender el sistema a fondo / siempre actual (más mantenimiento) |

## Lo que casi no cambia entre distros

- El árbol `/`, `/home`, `/etc`, `/var`… (con matices: [árbol de directorios](arbol-de-directorios.md))  
- Comandos básicos: `ls`, `cd`, `cp`, `grep`, `ps`, `ssh`…  
- La idea de usuarios, permisos y `sudo`  
- Redirecciones, pipes y scripts Bash (con matices de `sh`)

Si aprendes en Mint, **el 80–90 %** te sirve en un servidor Ubuntu o en Fedora.

## Lo que sí cambia

### 1) Instalar programas

```bash
# Mint / Ubuntu / Debian
sudo apt update
# Reading package lists... Done
sudo apt install <paquete>
# The following NEW packages will be installed: …
# Do you want to continue? [Y/n]

# Fedora
sudo dnf install <paquete>
# Dependencies resolved. … Is this ok [y/N]:

# Arch
sudo pacman -S <paquete>
# resolviendo dependencias… :: ¿Continuar con la instalación? [S/n]
```

En Mint, además:

```bash
flatpak search <texto>
# Name … Application ID … Remotes
flatpak install flathub <id_de_la_app>
# Looking for matches…  /  Confirmación con y
```

### 2) Nombre del servicio (systemd)

```bash
# SSH
systemctl status ssh      # Debian/Ubuntu/Mint a menudo
systemctl status sshd     # Fedora/Arch a menudo
# Active: active (running)   ← si el servicio está bien
# Unit ssh.service could not be found.  ← prueba el otro nombre (sshd)

# Apache
systemctl status apache2  # Debian/Mint
systemctl status httpd    # Fedora
# misma idea: active (running) o “could not be found”
```

### 3) Rutas y nombres (ejemplos reales)

| Cosa | Mint / Ubuntu / Debian | Fedora / RHEL | Arch (típico) |
|------|------------------------|---------------|---------------|
| Config paquetes | `/etc/apt/` | `/etc/yum.repos.d/` | `/etc/pacman.conf` |
| Apache | servicio `apache2`, `/etc/apache2/` | `httpd`, `/etc/httpd/` | `httpd` |
| Web files | `/var/www/html` | `/var/www/html` a menudo | a menudo `/srv/http` |
| Log auth (clásico) | `/var/log/auth.log` | `/var/log/secure` | más `journalctl` |
| USB auto | `/media/...` o `/run/media/...` | `/run/media/...` | `/run/media/...` |

Ampliación: [árbol de directorios](arbol-de-directorios.md).

```bash
# logs tradicionales (solo si existen) + journal (todas con systemd)
ls /var/log/syslog /var/log/auth.log /var/log/messages /var/log/secure 2>/dev/null
# en Mint/Ubuntu suele listar auth.log (y a veces syslog)
# en Fedora suele listar secure / messages
# si no imprime nada: esos paths no existen; usa journalctl

journalctl -n 10
# -- Logs begin at … --
# líneas recientes del sistema (fecha  hostname  servicio: mensaje)
```

### 4) Filosofía

| Distro | Sensación típica |
|--------|------------------|
| Mint | Escritorio amable, estable, Flatpak a mano |
| Ubuntu | Muy popular, mucha documentación |
| Debian | Estable y conservadora |
| Fedora | Tecnología reciente, ciclo más vivo |
| Arch | DIY, muy al día, más responsabilidad |

## ¿Cuál usar para seguir esta guía?

Cualquiera sirve. Si partes de cero en un PC de casa:

1. **Linux Mint** o Ubuntu → menos fricción.  
2. Practica los ejemplos con `apt` / Flatpak.  
3. Cuando veas un ejemplo `dnf`/`pacman`, es la misma idea con otra sintaxis.

```bash
cat /etc/os-release
```

Salida tipo (Mint; en otras distros cambian los valores):

```text
NAME="Linux Mint"
VERSION="22 …"
ID=linuxmint
ID_LIKE="ubuntu debian"
VERSION_ID="22"
…
```

## Mini laboratorio

```bash
cat /etc/os-release
# NAME=…  ID=…  VERSION_ID=…  (tu distro real)

hostnamectl
# Static hostname: …
# Operating System: Linux Mint …   (o Ubuntu/Fedora/…)
# Kernel: Linux …

which apt dnf pacman 2>/dev/null
# /usr/bin/apt          ← típico Mint/Ubuntu/Debian
# (en Fedora verías /usr/bin/dnf; en Arch, pacman)

ls /etc | head
# adduser.conf  alternatives  apt  …  (nombres varían un poco)

echo "$SHELL"
# /bin/bash   (lo más habitual en escritorio)
```

## Errores frecuentes

- Seguir un tutorial de Arch al pie de la letra en Mint (o al revés) sin adaptar el gestor de paquetes.  
- Instalar la misma app por `apt` y Flatpak y no saber cuál abres.  
- Pensar que “si no es Ubuntu no es Linux”.  
- Actualizar a lo loco en Arch el primer día sin leer (`pacman -Syu` con calma).

## Relacionados

- [08-paquetes](../03-red-paquetes/08-paquetes/)  
- [que-es-linux](que-es-linux.md)  
- [README distros](../README.md#distros-en-los-ejemplos)  
