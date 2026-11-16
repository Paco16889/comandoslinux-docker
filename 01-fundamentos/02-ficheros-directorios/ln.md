# ln

Crea enlaces a ficheros o directorios (*link*).

Hay dos tipos principales:

| Tipo | Comando | Idea |
|------|---------|------|
| Enlace simbólico (symlink) | `ln -s` | Atajo/ruta a otro sitio (lo habitual) |
| Enlace duro | `ln` (sin `-s`) | Otro nombre para el mismo fichero en disco |

## Sintaxis

```bash
ln [opciones] <origen> <enlace>
ln -s <origen> <enlace>
```

## Ejemplos — enlaces simbólicos (lo más usado)

```bash
ln -s <fichero> <enlace>
# <enlace> apunta a <fichero>

ln -s /home/<usuario>/<directorio>/<fichero> <enlace>
# el origen suele ir en ruta absoluta (más fiable)

ln -s <directorio> <enlace>
# enlace a una carpeta

ln -s <origen> <directorio>/<enlace>
# crea el enlace dentro de otra carpeta
```

Comprobar:

```bash
ls -l <enlace>
# ... <enlace> -> <origen>

pwd
# no cambia por tener un enlace; cd <enlace> te lleva al destino
```

## Ejemplos — enlaces duros

```bash
ln <fichero> <enlace>
# mismo contenido en disco; dos nombres

ls -li <fichero> <enlace>
# mismo número de inode (primera columna)
```

Los enlaces duros **no** funcionan entre sistemas de ficheros distintos ni, normalmente, sobre directorios.

## Opciones útiles

| Opción | Qué hace |
|--------|----------|
| `-s` | Enlace simbólico |
| `-i` | Pregunta antes de sobrescribir el nombre del enlace |
| `-f` | Fuerza (sobrescribe el nombre del enlace si existía) |
| `-v` | Verbose |
| `-r` | Con `-s`, crea la ruta del origen relativa al enlace (GNU) |

## Errores frecuentes

```text
ln: failed to create symbolic link '<enlace>': File exists
```

- Ya hay algo con ese nombre: bórralo o elige otro `<enlace>`, o usa `ln -sf` con cuidado.

- Origen relativo confuso: si haces `ln -s <fichero> <directorio>/<enlace>`, la ruta relativa se interpreta **desde la ubicación del enlace**, no desde donde ejecutaste el comando. Por eso a menudo se usa ruta absoluta en `<origen>`.

- Borrar el enlace simbólico **no** borra el fichero original (`rm <enlace>` solo quita el atajo).

## Relacionados

- [ls](../01-basicos/ls.md) — `ls -l` muestra `->` en symlinks
- [rm](rm.md) — eliminar un enlace
- [cp](cp.md) — copiar de verdad (otro fichero independiente)
