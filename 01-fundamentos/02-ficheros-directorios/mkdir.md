# mkdir

Crea uno o más directorios (*make directory*).

## Sintaxis

```bash
mkdir [opciones] <directorio>...
```

## Ejemplos

```bash
mkdir <directorio>
# crea una carpeta en el directorio actual

mkdir /home/<usuario>/<directorio>
# crea con ruta absoluta

mkdir <directorio1> <directorio2> <directorio3>
# crea varias de golpe

mkdir -p <directorio>/<subdirectorio>/<otro>
# crea toda la ruta intermedia si no existe

mkdir -p proyectos/<directorio>/src
# típico al montar una estructura de proyecto

mkdir -v <directorio>
# modo verbose: confirma lo que crea
```

Comprobar después:

```bash
mkdir -p <directorio>/<subdirectorio>
ls -la <directorio>
# deberías ver <subdirectorio> dentro
```

## Opciones útiles

| Opción | Qué hace |
|--------|----------|
| `-p` | Crea padres que falten; no falla si ya existe |
| `-v` | Muestra cada directorio creado |
| `-m <permisos>` | Crea con permisos concretos (ej. `-m 755`) |

## Errores frecuentes

```text
mkdir: cannot create directory ‘<directorio>’: File exists
```

- Ya existe: usa `mkdir -p` si solo quieres asegurarte de que esté.

```text
mkdir: cannot create directory ‘a/b/c’: No such file or directory
```

- Faltan carpetas intermedias: usa `mkdir -p a/b/c`.

## Relacionados

- [rmdir](rmdir.md) — borrar directorios vacíos
- [rm](rm.md) — borrar con contenido (`rm -r`)
- [touch](touch.md) — crear ficheros dentro del directorio
