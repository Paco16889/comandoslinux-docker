# passwd

Cambia la contraseña de un usuario.

## Sintaxis

```bash
passwd [opciones] [<usuario>]
```

Sin `<usuario>`, cambia la contraseña del usuario actual.

## Ejemplos

Tu propia contraseña:

```bash
passwd
# Current password:
# New password:
# Retype new password:
```

Admin cambia la de otro usuario:

```bash
sudo passwd <usuario>
# no pide la contraseña antigua (como root/sudo)
```

Bloquear / desbloquear cuenta (según implementación):

```bash
sudo passwd -l <usuario>
# bloquea la contraseña (el usuario no entra con password)

sudo passwd -u <usuario>
# desbloquea
```

Estado de la cuenta:

```bash
sudo passwd -S <usuario>
# estado: P=usable, L=bloqueada, NP=sin contraseña... (códigos según distro)

sudo passwd -S
# tu estado
```

Caducidad (administración):

```bash
sudo passwd -e <usuario>
# fuerza cambio de contraseña en el próximo login

sudo chage -l <usuario>
# política de caducidad (comando relacionado)
```

## Buenas prácticas

- Elige contraseñas largas; el sistema puede rechazarlas si son débiles (`pam` / `cracklib`).
- No uses `echo "<texto>" | passwd` en ejemplos reales: deja la contraseña en historial.
- Tras crear un usuario con `useradd`, suele hacer falta `sudo passwd <usuario>`.

## Errores frecuentes

```text
passwd: Authentication token manipulation error
```

- Problemas con PAM, disco de solo lectura, o ficheros `/etc/shadow` inconsistentes.

```text
passwd: You must be a root to change the password for <usuario>
```

- Solo root/sudo puede cambiar la de otros.

```text
BAD PASSWORD: ...
```

- No cumple la política; prueba una más larga/compleja.

## Siguiente

[useradd, usermod y userdel](useradd-usermod-userdel.md)

## Relacionados

- [useradd-usermod-userdel](useradd-usermod-userdel.md) — crear el usuario antes
- [sudo](sudo.md) — para administrar otras cuentas
- [su](su.md) — probar login tras el cambio



