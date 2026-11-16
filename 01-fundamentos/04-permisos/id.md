# id

Muestra el usuario y los grupos del proceso actual (o de otro `<usuario>`).

Útil para entender **por qué** un permiso de grupo aplica o no.

## Sintaxis

```bash
id [opciones] [<usuario>]
```

## Ejemplos

```bash
id
# uid=1000(<usuario>) gid=1000(<grupo>) grupos=...

id <usuario>
# datos de ese usuario

id -u
# solo UID numérico

id -un
# solo nombre de usuario

id -g
# GID primario

id -gn
# nombre del grupo primario

id -G
# todos los GIDs

id -Gn
# todos los nombres de grupo
```

Comprobar pertenencia antes de `chgrp` / permisos de grupo:

```bash
id
# mira si aparece <grupo> en la lista

id <usuario> | grep <grupo>
# ¿está <usuario> en <grupo>?
```

Comparar con el dueño de un fichero:

```bash
ls -l <fichero>
# ... <usuario_dueño> <grupo_dueño> ...

id
# si tu usuario no es el dueño ni estás en <grupo_dueño>, aplican permisos de "otros"
```

Comandos relacionados directos:

```bash
whoami
# solo el nombre de usuario actual (más simple)

groups
groups <usuario>
# lista de grupos
```

## Opciones útiles

| Opción | Qué hace |
|--------|----------|
| `-u` | Solo UID |
| `-g` | Solo GID primario |
| `-G` | Todos los GIDs |
| `-n` | Mostrar nombres (con `-u`/`-g`/`-G`) |
| `-un` / `-gn` / `-Gn` | Combinaciones habituales |

## Errores frecuentes

```text
id: ‘<usuario>’: no such user
```

- El usuario no existe en el sistema.

- Pensar que “estoy en el grupo” tras un `usermod` sin cerrar sesión: a menudo hace falta **nueva sesión** para que el grupo nuevo sea efectivo.

## Siguiente

[sudo](sudo.md)

## Relacionados

- [leer-ls-l](leer-ls-l.md) — dueño/grupo del fichero vs tu `id`
- [chown](chown.md) / [chgrp](chgrp.md) — cambiar propietario
- [sudo](sudo.md) — actuar como root u otro usuario
- Más adelante: carpeta [05-usuarios-sesiones](../../02-sistema/05-usuarios-sesiones/)



