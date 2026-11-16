# useradd, usermod y userdel

Alta, modificación y baja de cuentas de usuario. Normalmente requieren `sudo`.

> En Ubuntu/Debian de escritorio a veces se recomienda `adduser` (interactivo).  
> Aquí documentamos las herramientas clásicas POSIX/`shadow-utils`.

## useradd — crear

### Sintaxis

```bash
sudo useradd [opciones] <usuario>
```

### Ejemplos

```bash
sudo useradd <usuario>
# crea la cuenta con defaults del sistema (a veces SIN home)

sudo useradd -m <usuario>
# crea también /home/<usuario>

sudo useradd -m -s /bin/bash <usuario>
# home + shell bash

sudo useradd -m -s /bin/bash -G <grupo> <usuario>
# además lo mete en <grupo> suplementario

sudo useradd -m -c "Nombre Real" <usuario>
# comentario/GECOS (nombre completo)
```

Flujo mínimo usable:

```bash
sudo useradd -m -s /bin/bash <usuario>
sudo passwd <usuario>
# defines contraseña

id <usuario>
ls -ld /home/<usuario>
```

## usermod — modificar

### Sintaxis

```bash
sudo usermod [opciones] <usuario>
```

### Ejemplos

```bash
sudo usermod -s /bin/bash <usuario>
# cambiar shell

sudo usermod -d /home/<nuevo_directorio> -m <usuario>
# cambiar home y mover contenidos (-m)

sudo usermod -l <usuario_nuevo> <usuario>
# renombrar la cuenta (no siempre mueve el home; revisa después)
```

Grupos:

```bash
sudo usermod -aG <grupo> <usuario>
# Añade a <grupo> SIN quitar los demás (-a es importante con -G)

sudo usermod -G <grupo> <usuario>
# ¡OJO! establece la lista de grupos suplementarios a solo <grupo>
# (puede eliminarlo de sudo/wheel sin querer)
```

Bloquear / desbloquear (alternativa a passwd -l):

```bash
sudo usermod -L <usuario>
sudo usermod -U <usuario>
```

Añadir a administradores (según distro):

```bash
sudo usermod -aG sudo <usuario>
# Debian / Ubuntu / Linux Mint

sudo usermod -aG wheel <usuario>
# Fedora / RHEL / Arch (habitual)
```

Tras cambiar grupos, `<usuario>` suele necesitar **cerrar sesión y volver a entrar**.

## userdel — borrar

### Sintaxis

```bash
sudo userdel [opciones] <usuario>
```

### Ejemplos

```bash
sudo userdel <usuario>
# borra la cuenta; deja el home

sudo userdel -r <usuario>
# borra también /home/<usuario> y el mail spool
```

Comprobar antes:

```bash
id <usuario>
who | grep <usuario>
ps -u <usuario>
# si tiene sesión/procesos, cierra o mata antes (con cuidado)
```

## Ficheros relacionados

| Fichero | Contenido |
|---------|-----------|
| `/etc/passwd` | Usuario, UID, GID, home, shell (legible) |
| `/etc/shadow` | Contraseñas hash (solo root) |
| `/etc/group` | Grupos |
| `/home/<usuario>` | Directorio personal |

```bash
getent passwd <usuario>
getent group <grupo>
grep "^<usuario>:" /etc/passwd
```

## Errores frecuentes

```text
useradd: user '<usuario>' already exists
```

- Elige otro nombre o modifica el existente con `usermod`.

- Olvidar `-m`: usuario sin home.
- `usermod -G` sin `-a`: te puedes cargar la membresía a `sudo`/`docker`/etc.

## Siguiente

[groupadd, groupmod y groupdel](groupadd-groupdel.md)

## Relacionados

- [passwd](passwd.md) — poner contraseña tras el alta
- [groupadd-groupdel](groupadd-groupdel.md) — crear grupos antes de `-G`
- [sudo](sudo.md) — privilegios de administración



