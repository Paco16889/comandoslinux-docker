# chgrp

Cambia el **grupo** propietario de un fichero o directorio (*change group*).

Equivalente parcial a `chown :<grupo> <ruta>`.

## Sintaxis

```bash
chgrp [opciones] <grupo> <ruta>...
```

Según la política del sistema, puede hacer falta ser dueño del fichero y pertenecer al grupo destino, o usar `sudo`.

## Ejemplos

```bash
chgrp <grupo> <fichero>
# éxito: suele no imprimir nada
chgrp <grupo> <directorio>

sudo chgrp <grupo> <fichero>
# si no tienes permiso suficiente (pide contraseña de sudo)
```

Comprobar el cambio:

```bash
ls -l <fichero>
# -rw-r--r-- 1 <usuario> <grupo> … <fichero>
#                    ↑ dueño     ↑ grupo (aquí debe verse el nuevo)
```

Recursivo:

```bash
chgrp -R <grupo> <directorio>
sudo chgrp -R <grupo> <directorio>
# cambia el grupo de la carpeta y de lo de dentro (sin listar, salvo -v)

chgrp -v <grupo> <fichero>
# changed group of '<fichero>' from <grupo_viejo> to <grupo>
```

Varios elementos:

```bash
chgrp <grupo> <fichero1> <fichero2> <fichero3>
chgrp <grupo> <directorio>/<fichero>
```

Flujo típico de colaboración:

```bash
# 1) el fichero pasa al grupo de trabajo
sudo chgrp <grupo> <fichero>

# 2) el grupo puede escribir
chmod g+w <fichero>

ls -l <fichero>
```

Salida tipo (nombres inventados; mira las columnas 3 y 4 y la `w` del grupo):

```text
-rw-rw-r-- 1 ana desarrolladores 120 … informe.txt
     ↑↑
     el bloque del grupo tiene w → quien esté en ese grupo puede escribir
```

Comprobar grupos a los que perteneces:

```bash
id
# uid=1000(<usuario>) gid=1000(<grupo>) grupos=1000(<grupo>),…,<otro_grupo>,…

id <usuario>
groups
# <usuario> : <grupo> <otro_grupo> …

groups <usuario>
```

## Opciones útiles

| Opción | Qué hace |
|--------|----------|
| `-R` | Recursivo |
| `-v` | Verbose |
| `-c` | Solo muestra cambios |
| `--reference=<fichero>` | Copia el grupo de otro fichero |

```bash
chgrp --reference=<fichero_modelo> <fichero>
# copia el grupo de <fichero_modelo>; éxito suele ser silencio
# luego: ls -l <fichero>  → misma columna de grupo que el modelo
```

## Errores frecuentes

```text
chgrp: invalid group: ‘<grupo>’
```

- El grupo no existe. En muchas distros se crea con `sudo groupadd <grupo>` (admin).

```text
chgrp: changing group of '<ruta>': Operation not permitted
```

- No eres dueño, o no perteneces a `<grupo>`, o la política exige `sudo`.

- Confundir `<usuario>` y `<grupo>`: pueden llamarse igual (`<usuario>:<usuario>`), pero no siempre.

## Siguiente

[umask](umask.md)

## Relacionados

- [chown](chown.md) — dueño y/o grupo
- [chmod](chmod.md) — dar `g+w` / `g+rx` tras cambiar el grupo
- [id](id.md) — ver membresía de grupos



