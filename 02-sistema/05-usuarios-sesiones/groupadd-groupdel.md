# groupadd, groupmod y groupdel

Gestión de grupos. Suele requerir `sudo`.

## groupadd — crear grupo

### Sintaxis

```bash
sudo groupadd [opciones] <grupo>
```

### Ejemplos

```bash
sudo groupadd <grupo>
# crea el grupo

sudo groupadd -g <número> <grupo>
# con GID concreto

getent group <grupo>
# <grupo>:x:<GID>:
```

Añadir usuarios al grupo:

```bash
sudo usermod -aG <grupo> <usuario>
# forma habitual

sudo gpasswd -a <usuario> <grupo>
# alternativa

id <usuario>
# debe listar <grupo> (tras nueva sesión)
```

## groupmod — renombrar / cambiar GID

### Sintaxis

```bash
sudo groupmod [opciones] <grupo>
```

### Ejemplos

```bash
sudo groupmod -n <grupo_nuevo> <grupo>
# renombrar

sudo groupmod -g <número> <grupo>
# cambiar GID (cuidado con ficheros que ya tenían el GID antiguo)
```

## groupdel — borrar grupo

### Sintaxis

```bash
sudo groupdel <grupo>
```

### Ejemplos

```bash
getent group <grupo>
sudo groupdel <grupo>
```

No podrás borrar un grupo si es el **grupo primario** de algún usuario.

```bash
# primero cambia el grupo primario de esos usuarios, luego:
sudo groupdel <grupo>
```

## Ver grupos y miembros

```bash
groups
groups <usuario>

id <usuario>
getent group <grupo>
# <grupo>:x:<GID>:<usuario1>,<usuario2>

cut -d: -f1 /etc/group
# nombres de todos los grupos
```

Quitar un usuario de un grupo:

```bash
sudo gpasswd -d <usuario> <grupo>
# o herramientas específicas de la distro

id <usuario>
```

## Flujo completo de ejemplo

```bash
sudo groupadd <grupo>
sudo useradd -m -s /bin/bash <usuario>
sudo passwd <usuario>
sudo usermod -aG <grupo> <usuario>

sudo mkdir /home/<usuario>/<directorio>
sudo chown <usuario>:<grupo> /home/<usuario>/<directorio>
sudo chmod 2775 /home/<usuario>/<directorio>
# 2 = setgid: ficheros nuevos heredan el grupo del directorio (tema avanzado de permisos)
```

## Errores frecuentes

```text
groupadd: group '<grupo>' already exists
```

```text
groupdel: cannot remove the primary group of user '<usuario>'
```

- Cambia el GID primario del usuario antes de borrar el grupo.

- Añadir a un grupo y no ver el cambio: falta cerrar sesión / `newgrp <grupo>` en la sesión actual.

```bash
newgrp <grupo>
# inicia subshell con ese grupo como primario temporal
id
exit
```

## Relacionados

- [useradd-usermod-userdel](useradd-usermod-userdel.md) — `usermod -aG`
- [chgrp](../../01-fundamentos/04-permisos/chgrp.md) — grupo de ficheros
- [id](../../01-fundamentos/04-permisos/id.md) — comprobar membresía
