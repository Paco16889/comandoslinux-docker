# Árbol de directorios: qué va en cada carpeta

Estas carpetas son las del estándar habitual en Linux (**FHS**).  
No inventamos rutas: son las que verás en Mint, Ubuntu, Fedora, Arch… con **algunas variaciones** que marcamos abajo.

No hace falta memorizarlas el día 1. Sí reconocerlas cuando salgan en un tutorial.

> `<usuario>` = tu nombre de usuario (sin escribir `<` `>`).

## Mapa rápido: qué es y para qué se usa

| Ruta | ¿Qué es? | ¿Para qué se usa (en la práctica)? | ¿La tocas al aprender? |
|------|----------|-------------------------------------|-------------------------|
| `/` | Raíz de todo el árbol | Punto de partida del sistema de ficheros | Solo `ls /` |
| `/home/<usuario>` | Tu carpeta personal | Documentos, descargas, configs tuyas (`.bashrc`, `.ssh`) | **Sí** |
| `/root` | Home del usuario `root` | Trabajo del admin cuando entra como root | Casi nunca |
| `/etc` | Configuración del sistema | Ajustar servicios, red, usuarios, paquetes… | Leer; editar con guía + `sudo` |
| `/var` | Datos que cambian | Logs, cachés, colas, webs, bases de datos del sistema | Mirar logs a veces |
| `/usr` | Software “del sistema” | Programas instalados por el gestor de paquetes | No a mano |
| `/bin`, `/sbin` | Comandos básicos (históricamente) | Hoy suelen ser enlaces a `/usr/bin` y `/usr/sbin` | Los usa la shell |
| `/usr/bin`, `/usr/sbin` | Ejecutables de usuario / admin | Donde están `ls`, `grep`, `systemctl`… | `which <comando>` |
| `/usr/local` | Software instalado “a mano” en la máquina | Cosas compiladas o instaladas fuera del repo | A veces |
| `/tmp` | Temporal | Pruebas, descargas a medias; puede borrarse al reiniciar | Sí |
| `/opt` | Apps opcionales / de terceros | Suites que traen su propia carpeta (`/opt/google`, etc.) | A veces |
| `/mnt` | Montajes manuales | Admin monta un disco aquí a propósito | Según caso |
| `/media` | Medios extraíbles | USB/DVD que monta el escritorio | Sí, al conectar USB |
| `/boot` | Arranque | Kernel, initramfs, a menudo GRUB | **No tocar** |
| `/dev` | Dispositivos | Discos (`sda`), terminales, etc. como “ficheros” | No a lo loco |
| `/proc` | Info del kernel (virtual) | Procesos, CPU… (lo usan `ps`, etc.) | Solo consulta |
| `/sys` | Info del hardware (virtual) | Drivers, dispositivos | Solo consulta |
| `/run` | Datos de ejecución (tmpfs) | PID de servicios, mounts recientes… | Rara vez a mano |
| `/srv` | Datos de **servicios** servidos por esta máquina | FTP, sitios, datos que “ofreces” en red (si se usa) | En labs de servidor |
| `/lib`, `/lib64` | Librerías | Las necesitan los programas; hoy a menudo → `/usr/lib` | No |

## Las carpetas del día a día (con comprobaciones reales)

Estos bloques son para **mirar** en tu PC, no para “inventar” ejercicios raros.

### `/home/<usuario>` — tu casa

**Para qué:** todo lo tuyo. Si reinstalas el sistema y guardas `/home` en otra partición, conservas documentos y configs personales.

```bash
echo "$HOME"
# /home/<usuario>

ls "$HOME"
ls -a "$HOME"
# incluye ocultos: .bashrc .config .ssh …
```

| Sitio típico | Uso |
|--------------|-----|
| `~/Documentos`, `~/Descargas`… | Ficheros del escritorio (nombres según idioma) |
| `~/.config/` | Preferencias de apps |
| `~/.ssh/` | Claves SSH |
| `~/.bashrc` | Config de tu shell Bash |
| `~/.local/share/flatpak/` | Datos Flatpak del usuario (Mint/Fedora…) |

### `/etc` — configuración del sistema

**Para qué:** decirle al sistema *cómo* comportarse (red, servicios, usuarios, paquetes).

```bash
ls /etc
cat /etc/os-release
# qué distro eres
```

| Ruta | Uso habitual |
|------|----------------|
| `/etc/passwd`, `/etc/group` | Usuarios y grupos |
| `/etc/hostname` | Nombre del equipo |
| `/etc/fstab` | Qué discos montar al arrancar |
| `/etc/ssh/` | Servidor/cliente SSH |
| `/etc/nginx/` o `/etc/apache2/` | Web (si están instalados) |

**Variaciones por distro (paquetes / red):**

| Familia | Dónde mirar a menudo |
|---------|----------------------|
| Debian / Ubuntu / **Mint** | `/etc/apt/` (fuentes), `/etc/netplan/` (a veces), NetworkManager |
| Fedora / RHEL | `/etc/yum.repos.d/` o dnf, `/etc/NetworkManager/` |
| Arch | `/etc/pacman.conf`, `/etc/pacman.d/mirrorlist` |

```bash
# Mint / Ubuntu / Debian
ls /etc/apt

# Fedora
ls /etc/yum.repos.d 2>/dev/null

# Arch
ls /etc/pacman.d 2>/dev/null
```

### `/var` — lo que crece

**Para qué:** logs, cachés, datos de servicios. Si el disco se llena, mira `/var` y tu `$HOME`.

```bash
ls /var
ls /var/log
ls /var/cache
```

| Ruta | Uso |
|------|-----|
| `/var/log/` | Registros |
| `/var/cache/apt/` | Caché de `.deb` (familia Debian) |
| `/var/cache/dnf/` o `yum` | Caché de paquetes (Fedora/RHEL) |
| `/var/lib/` | Datos persistentes de programas del sistema |
| `/var/www/` | DocumentRoot web clásico (Debian/Mint/Ubuntu; también muchas guías) |
| `/srv/` | A veces preferido para “datos servidos” (según admin) |

**Logs: variación típica**

| Familia | Tradicional | Moderno (casi todas) |
|---------|-------------|----------------------|
| Debian / Ubuntu / Mint | `/var/log/syslog`, `/var/log/auth.log` | `journalctl` |
| Fedora / RHEL | `/var/log/messages`, `/var/log/secure` | `journalctl` |
| Arch | menos ficheros clásicos; más journal | `journalctl` |

```bash
# funciona en distros con systemd (Mint, Ubuntu, Fedora, Arch…)
journalctl -n 20

# solo si existe el fichero en tu distro:
sudo tail -n 20 /var/log/syslog
sudo tail -n 20 /var/log/messages
```

### `/usr` — programas del sistema

**Para qué:** donde el gestor de paquetes coloca la mayor parte del software “oficial”.

```bash
which ls
# /usr/bin/ls

ls /usr/bin | head
ls /usr/share | head
# documentación, iconos, datos compartidos…
```

| Ruta | Uso |
|------|-----|
| `/usr/bin` | Comandos de usuario |
| `/usr/sbin` | Comandos de administración |
| `/usr/lib` | Librerías |
| `/usr/share/man` | Páginas `man` |
| `/usr/local` | Lo instalado fuera del repo (prioridad a veces distinta) |

En distros modernas (incluido Mint/Ubuntu recientes), `/bin` → `/usr/bin` y `/sbin` → `/usr/sbin` (enlaces). Es normal:

```bash
ls -l /bin
# a menudo: /bin -> usr/bin
```

### `/tmp` — borrador

**Para qué:** ficheros temporales. **No** guardes aquí el trabajo del curso.

```bash
touch /tmp/prueba_<usuario>.txt
ls /tmp/prueba_<usuario>.txt
```

### `/opt` — software “de caja”

**Para qué:** aplicaciones que traen su árbol propio (ej. algunas tools propietarias).

```bash
ls /opt
```

### `/mnt` y `/media` (+ `/run/media`) — discos y USB

**Para qué:** enganchar otros sistemas de ficheros al árbol.

| Ruta | Uso típico |
|------|------------|
| `/media/<usuario>/...` | USB montado por el escritorio (Mint/Ubuntu clásico) |
| `/run/media/<usuario>/...` | Muy habitual en Fedora y también en escritorios modernos |
| `/mnt/...` | Montaje manual (`sudo mount ... /mnt/disco`) |

```bash
ls /media
ls /run/media/<usuario> 2>/dev/null
ls /mnt
```

### `/boot` — arranque

**Para qué:** kernel y arranque del sistema. Si lo rompes, el PC puede no encender el SO.

```bash
ls /boot
# solo mirar
```

### `/srv` — datos de servicio (menos usado en escritorio)

**Para qué:** en servidores, a veces se pone aquí lo que ofreces (web, FTP…). En un Mint de casa puede estar vacío o casi.

```bash
ls /srv
```

En esta guía, Samba de ejemplo usaba `/srv/samba/...` precisamente por esta convención.

## Variaciones importantes por familia (resumen)

| Tema | Debian / Ubuntu / **Mint** | Fedora / RHEL | Arch |
|------|----------------------------|---------------|------|
| Paquetes (config) | `/etc/apt/` | `/etc/yum.repos.d/` | `/etc/pacman.conf` + `mirrorlist` |
| Web Apache | `/etc/apache2/`, servicio `apache2` | `/etc/httpd/`, servicio `httpd` | `/etc/httpd/` (paquete `apache`) |
| Web nginx | `/etc/nginx/` | `/etc/nginx/` | `/etc/nginx/` |
| DocumentRoot habitual | `/var/www/html` | `/var/www/html` o configs propias | suele `/srv/http` (nginx/apache Arch) |
| USB auto | `/media/...` o `/run/media/...` | sobre todo `/run/media/...` | `/run/media/...` o `/media/...` |
| Flatpak (usuario) | `~/.local/share/flatpak` | igual | igual |
| Snap (Ubuntu) | `/snap`, `/var/lib/snapd` | menos habitual | poco habitual |

**Arch – `/srv/http`:** muchas guías de Arch sirven la web desde ahí; en Mint/Ubuntu verás más `/var/www/html`. Misma idea (ficheros de la web), **ruta distinta**.

```bash
# Comprueba qué existe en TU máquina:
ls /var/www/html 2>/dev/null
ls /srv/http 2>/dev/null
cat /etc/os-release | head
```

## ¿Por qué este orden de carpetas?

Convención FHS (Filesystem Hierarchy Standard):

1. Separar **lo tuyo** (`/home`) de **lo del sistema** (`/usr`, `/etc`).  
2. Poder respaldar o reinstalar sin mezclar documentos y SO.  
3. Saber dónde mirar configs (`/etc`) y logs (`/var` / `journalctl`).  
4. Que paquetes y scripts encuentren rutas previsibles.

Las distros pueden mover detalles; el **mapa grande** se mantiene.

## Mini laboratorio (solo consulta)

```bash
ls /
echo "$HOME"
ls -a "$HOME" | head
ls /etc | head
ls /var/log | head
ls -l /bin /usr/bin | head
df -h
lsblk
cat /etc/os-release

# variaciones: ¿qué hay en tu distro?
ls /etc/apt 2>/dev/null
ls /etc/yum.repos.d 2>/dev/null
ls /var/www/html 2>/dev/null
ls /srv/http 2>/dev/null
ls /run/media/"$USER" 2>/dev/null
ls /media 2>/dev/null
```

## Errores frecuentes

- Guardar lo importante en `/tmp`.  
- Editar `/etc` o tocar `/boot` sin saber.  
- Confundir `/` (raíz) con `/root` (home de root) o con `/home`.  
- Seguir una ruta de Arch (`/srv/http`) en Mint sin mirar si existe.  
- Buscar `C:\Program Files`: aquí es más `/usr`, `/opt`, Flatpak y tu `$HOME`.

## Relacionados

- [sistema-de-ficheros](sistema-de-ficheros.md)  
- [distros](distros.md) — gestores y filosofía  
- [usuarios-y-permisos-idea](usuarios-y-permisos-idea.md)  
- [02-ficheros-directorios](../01-fundamentos/02-ficheros-directorios/)  
