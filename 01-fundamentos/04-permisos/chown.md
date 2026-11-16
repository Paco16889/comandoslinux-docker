# chown

Cambia el usuario dueño y/o el grupo de un fichero o directorio (*change owner*).

En la práctica cotidiana suele hacer falta privilegios de administrador (`sudo`).

## Sintaxis

```bash
chown [opciones] <usuario>[:<grupo>] <ruta>...
chown [opciones] :<grupo> <ruta>...
```

## Ejemplos

Solo dueño:

```bash
sudo chown <usuario> <fichero>
sudo chown <usuario> <directorio>
```

Dueño y grupo a la vez:

```bash
sudo chown <usuario>:<grupo> <fichero>
sudo chown <usuario>:<grupo> <directorio>
```

Solo grupo (también puedes usar [chgrp](chgrp.md)):

```bash
sudo chown :<grupo> <fichero>
sudo chown :<grupo> <directorio>
```

Recursivo:

```bash
sudo chown -R <usuario>:<grupo> <directorio>
# cambia dueño/grupo de la carpeta y de todo lo de dentro
```

Casos típicos:

```bash
sudo chown <usuario>:<usuario> /home/<usuario>/<directorio>
# recuperar propiedad de algo copiado como root

sudo chown -R www-data:www-data <directorio>
# ejemplo habitual en servidores web (nombre de usuario/grupo según distro)

sudo chown <usuario>: <fichero>
# dueño = <usuario> y grupo = grupo primario de <usuario> (sintaxis GNU)
```

Comprobar:

```bash
ls -l <fichero>
# ... <usuario> <grupo> ... <fichero>

ls -ld <directorio>
```

## Opciones útiles

| Opción | Qué hace |
|--------|----------|
| `-R` | Recursivo |
| `-v` | Verbose |
| `-c` | Solo muestra cambios reales |
| `-h` | Afecta al symlink en sí, no al destino (si aplica) |
| `--reference=<fichero>` | Copia dueño/grupo de otro fichero |

```bash
sudo chown --reference=<fichero_modelo> <fichero>
```

## Errores frecuentes

```text
chown: changing ownership of '<ruta>': Operation not permitted
```

- Sin privilegios suficientes: normalmente `sudo chown ...`.

```text
chown: invalid user: ‘<usuario>’
```

- Ese `<usuario>` no existe en el sistema (`id <usuario>` para comprobar).

```text
chown: invalid group: ‘<usuario>:<grupo>’
```

- El `<grupo>` no existe.

- Hacer `chown -R` sobre `/` o rutas críticas: puede romper el sistema. Limita siempre a `<directorio>` concreto.

## Relacionados

- [chgrp](chgrp.md) — solo grupo
- [chmod](chmod.md) — permisos rwx
- [id](id.md) — ver usuarios/grupos válidos
- [sudo](sudo.md) — ejecutar con privilegios
