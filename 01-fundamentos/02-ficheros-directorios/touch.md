# touch

Crea ficheros vacíos o actualiza su fecha de modificación.

## Sintaxis

```bash
touch [opciones] <fichero>...
```

## Ejemplos

```bash
touch <fichero>
# si no existe, lo crea vacío; si existe, actualiza su fecha

touch <fichero1> <fichero2> <fichero3>
# crea varios de golpe

touch <directorio>/<fichero>
# crea el fichero dentro de esa carpeta (la carpeta debe existir)

touch notas.txt leeme.md
# nombres concretos
```

Actualizar fecha sin cambiar contenido:

```bash
ls -l <fichero>
# mira la fecha actual

touch <fichero>
ls -l <fichero>
# la fecha de modificación pasa a “ahora”
```

Crear estructura mínima:

```bash
mkdir -p <directorio>
touch <directorio>/<fichero>
ls -la <directorio>
```

## Opciones útiles

| Opción | Qué hace |
|--------|----------|
| `-c` | No crea el fichero si no existe (solo actualiza fecha) |
| `-a` | Solo actualiza fecha de acceso |
| `-m` | Solo actualiza fecha de modificación |
| `-t <marca>` | Usa una fecha concreta (formato `[[CC]YY]MMDDhhmm[.ss]`) |

Ejemplo de fecha concreta:

```bash
touch -t 202611141830 <fichero>
# pone fecha 2026-11-14 18:30
```

## Errores frecuentes

```text
touch: cannot touch '<directorio>/<fichero>': No such file or directory
```

- El directorio padre no existe: créalo antes con `mkdir -p <directorio>`.

- Pensar que `touch` escribe contenido: solo crea vacío o toca la fecha; para texto usa un editor o `echo "<texto>" > <fichero>`.

## Siguiente

[cp](cp.md)

## Relacionados

- [mkdir](mkdir.md) — crear la carpeta antes
- [cp](cp.md) — copiar un fichero ya existente
- [echo](../01-basicos/echo.md) — escribir una línea en un fichero



