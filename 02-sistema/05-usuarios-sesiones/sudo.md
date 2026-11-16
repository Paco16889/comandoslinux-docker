# sudo

Ejecuta comandos con privilegios elevados (o como otro `<usuario>`).

Ampliación de la intro en [04-permisos/sudo](../../01-fundamentos/04-permisos/sudo.md).

## Sintaxis

```bash
sudo [opciones] <comando>
sudo -u <usuario> <comando>
```

## Ejemplos cotidianos

```bash
sudo apt update
# Debian / Ubuntu / Linux Mint (paquetes apt)

sudo dnf update
# Fedora

# Linux Mint: muchas apps también se actualizan con Flatpak
flatpak update

sudo cat /etc/shadow
sudo less /var/log/auth.log
sudo nano /etc/<fichero>
```

Permisos y propiedad:

```bash
sudo chmod 644 <fichero>
sudo chown <usuario>:<grupo> <fichero>
sudo chown -R <usuario>:<grupo> <directorio>
sudo mkdir /opt/<directorio>
```

Como otro usuario:

```bash
sudo -u <usuario> whoami
sudo -u <usuario> id
sudo -u <usuario> bash -lc 'cd ~; pwd; whoami'
sudo -u <usuario> -i
# shell de login de <usuario>
```

Shell de root:

```bash
sudo -i
# entorno de root

sudo -s
# shell elevada (matiz de entorno según distro)

exit
# volver
```

Repetir el último comando:

```bash
systemctl restart <servicio>
# falló: Permission denied

sudo !!
# reintenta con sudo
```

## Consultar y refrescar credenciales

```bash
sudo -l
# qué puedes ejecutar

sudo -v
# renueva la “cache” de contraseña sin lanzar comando

sudo -k
# obliga a pedir contraseña en el próximo sudo
```

## Fichero sudoers (solo lectura/edición cuidadosa)

```bash
sudo cat /etc/sudoers
sudo visudo
# ÚNICA forma recomendada de editar sudoers (valida sintaxis)
```

Reglas típicas (concepto; no copies a ciegas):

```text
<usuario>  ALL=(ALL:ALL) ALL
%<grupo>   ALL=(ALL:ALL) ALL
```

`%sudo` o `%wheel` suelen ser los grupos de administradores según distro.

## Errores frecuentes

```text
<usuario> is not in the sudoers file.  This incident will be reported.
```

- Tu cuenta no está autorizada. Un admin debe añadirte al grupo `sudo` o `wheel`:

```bash
# ejecutado por un admin:
sudo usermod -aG sudo <usuario>
# o: sudo usermod -aG wheel <usuario>
# luego <usuario> debe cerrar sesión y volver a entrar
```

```text
sudo: a password is required
```

- En entornos sin TTY (algunos scripts/CI) falla si no hay `NOPASSWD` configurado.

- Contraseña: la de **tu usuario**, no la de root (en el esquema habitual).

## Relacionados

- [su](su.md) — cambiar de usuario con su contraseña
- [useradd-usermod-userdel](useradd-usermod-userdel.md) — meter a alguien en el grupo sudo/wheel
- [whoami](whoami.md) — `sudo whoami` → `root`
