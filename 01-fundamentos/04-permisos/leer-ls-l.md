# Leer `ls -l`

Antes de cambiar permisos, hay que leer lo que muestra el listado largo.

## Sintaxis

```bash
ls -l [<ruta>]
ls -ld <directorio>
```

> Recuerda: `[ ]` = opcional · `<ruta>` = lo sustituyes tú.  
> Detalle: [Cómo leer la sintaxis](../../README.md#cómo-leer-la-sintaxis-muy-importante).

## Laboratorio: crea y lee permisos

```bash
mkdir -p <directorio>
cd <directorio>

touch a.txt b.txt secreto.txt
mkdir carpeta

chmod 644 a.txt
chmod 755 b.txt
chmod 600 secreto.txt
chmod 700 carpeta

ls -l
ls -ld carpeta
```

Intenta interpretar cada línea:

```text
-rw-r--r--  ... a.txt        → 644
-rwxr-xr-x  ... b.txt        → 755 (ejecutable)
-rw-------  ... secreto.txt  → 600
drwx------  ... carpeta      → 700
```

“No puedo borrar el fichero” suele ser el directorio padre:

```bash
ls -ld <directorio>
# hace falta w en el directorio para borrar entradas dentro
```

Enlace simbólico:

```bash
ln -s a.txt enlace.txt
ls -l enlace.txt
# lrwxrwxrwx ... enlace.txt -> a.txt
readlink enlace.txt
```

## Ejemplo de línea

```text
-rw-r--r-- 1 <usuario> <grupo> 4096 Nov 14 18:30 <fichero>
drwxr-xr-x 2 <usuario> <grupo> 4096 Nov 14 18:30 <directorio>
lrwxrwxrwx 1 <usuario> <grupo>   11 Nov 14 18:30 <enlace> -> <origen>
```

## Partes de izquierda a derecha

| Campo | Ejemplo | Significado |
|-------|---------|-------------|
| Tipo + permisos | `-rw-r--r--` | Tipo de entrada + quién puede qué |
| Enlaces | `1` | Número de enlaces duros |
| Dueño | `<usuario>` | Usuario propietario |
| Grupo | `<grupo>` | Grupo propietario |
| Tamaño | `4096` | Bytes (en directorios no es el tamaño total del contenido) |
| Fecha | `Nov 14 18:30` | Última modificación |
| Nombre | `<fichero>` | Nombre (o `nombre -> destino` si es symlink) |

## Primer carácter: tipo

| Carácter | Tipo |
|----------|------|
| `-` | Fichero normal |
| `d` | Directorio |
| `l` | Enlace simbólico |
| `c` / `b` | Dispositivo carácter / bloque |
| `s` | Socket |
| `p` | Pipe (FIFO) |

## Los 9 caracteres de permisos

Se leen en tres bloques de `rwx`:

```text
rwx rwx rwx
│   │   └── otros (resto del mundo)
│   └────── grupo
└────────── usuario dueño (owner)
```

| Letra | Fichero | Directorio |
|-------|---------|------------|
| `r` | Leer contenido | Listar nombres (`ls`) |
| `w` | Modificar/borrar contenido* | Crear/borrar entradas dentro* |
| `x` | Ejecutar | Entrar (`cd`) y atravesar la ruta |
| `-` | Ese permiso no está | Ese permiso no está |

\*Borrar un fichero depende sobre todo del permiso `w` **del directorio que lo contiene**, no solo del fichero.

## Ejemplos de lectura

```bash
ls -l <fichero>
# -rw-r--r--  → dueño rw, grupo r, otros r  (fichero típico 644)

ls -ld <directorio>
# drwxr-xr-x  → dueño rwx, grupo rx, otros rx  (directorio típico 755)

ls -l <enlace>
# lrwxrwxrwx → symlink; los permisos “efectivos” suelen venir del destino
```

Más casos:

```text
-rw-------   solo el dueño lee/escribe (600), típico de claves privadas
-rwxr-xr-x   ejecutable por todos (755), típico de programas
-rw-rw-r--   dueño y grupo escriben (664)
drwx------   solo el dueño entra en la carpeta (700)
```

## Comprobar varios

```bash
ls -l <fichero1> <fichero2>
ls -la <directorio>
# -a incluye ocultos; mira permisos de . y ..
```

## Errores frecuentes

- Mirar solo el fichero y no el directorio padre cuando “no puedo borrar”.
- Creer que `r` en un directorio permite `cd`: para entrar hace falta `x`.
- En symlinks, obsesionarse con `lrwxrwxrwx`: lo importante suele ser el destino.

## Siguiente

[chmod](chmod.md)

## Relacionados

- [chmod](chmod.md) — cambiar esos `rwx`
- [chown](chown.md) — cambiar `<usuario>` / `<grupo>`
- [ls](../01-basicos/ls.md) — opciones de listado



