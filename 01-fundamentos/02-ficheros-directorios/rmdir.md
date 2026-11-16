# rmdir

Elimina **directorios vacíos** (*remove directory*).

## Sintaxis

```bash
rmdir [opciones] <directorio>...
```

## Ejemplos

```bash
rmdir <directorio>
# si estaba vacía: no imprime nada (éxito silencioso) y la carpeta desaparece
# comprueba con: ls  → ya no debe aparecer <directorio>

rmdir <directorio1> <directorio2>
# varias a la vez (todas deben estar vacías)
# si una falla, las demás sí pueden borrarse; lee el mensaje de error

rmdir -p <directorio>/<subdirectorio>
# borra <subdirectorio> y, si quedan vacíos, también los padres de la ruta

rmdir -v <directorio>
# rmdir: removing directory, '<directorio>'
```

Flujo típico:

```bash
mkdir <directorio>
rmdir <directorio>
# ok: estaba vacía → sin mensaje

mkdir -p <directorio>
touch <directorio>/<fichero>
rmdir <directorio>
```

Salida esperada del fallo:

```text
rmdir: failed to remove '<directorio>': Directory not empty
```

Vaciar y luego borrar con `rmdir`:

```bash
rm <directorio>/<fichero>
rmdir <directorio>
# otra vez: silencio = bien; ls ya no muestra la carpeta
```

O borrar con contenido de un golpe (otro comando):

```bash
rm -r <directorio>
```

## Opciones útiles

| Opción | Qué hace |
|--------|----------|
| `-p` | Borra también padres vacíos de la ruta |
| `-v` | Verbose (en algunas implementaciones) |
| `--ignore-fail-on-non-empty` | No falla si no está vacío (GNU) |

## Errores frecuentes

```text
rmdir: failed to remove '<directorio>': Directory not empty
```

- Tiene ficheros o subcarpetas: bórralos antes, o usa `rm -r <directorio>` (con cuidado).

- Usar `rmdir` en un fichero: no sirve; `rmdir` solo es para directorios.

## Relacionados

- [mkdir](mkdir.md) — crear directorios
- [rm](rm.md) — borrar ficheros o directorios con contenido
- [ls](../01-basicos/ls.md) — comprobar si está vacío (`ls -A <directorio>`)
