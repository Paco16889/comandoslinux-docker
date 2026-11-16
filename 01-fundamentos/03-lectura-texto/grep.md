# grep

Busca líneas que coinciden con un texto o patrón (*global regular expression print*).

## Sintaxis

```bash
grep [opciones] <patrón> [<fichero>...]
comando | grep [opciones] <patrón>
```

## Ejemplos básicos

```bash
grep "<texto>" <fichero>
# líneas que contienen <texto>

grep <patrón> <fichero>
# si <patrón> no tiene espacios ni símbolos raros, a veces sin comillas

grep -n "<texto>" <fichero>
# muestra también el número de línea

grep -i "<texto>" <fichero>
# ignora mayúsculas/minúsculas

grep -v "<texto>" <fichero>
# líneas que NO contienen <texto>
```

Varios ficheros y recursivo:

```bash
grep "<texto>" <fichero1> <fichero2>
# indica en qué fichero aparece

grep -r "<texto>" <directorio>
# busca en todos los ficheros bajo <directorio>

grep -rn "<texto>" <directorio>
# recursivo + números de línea

grep -r --include="*.txt" "<texto>" <directorio>
# solo en .txt
```

Contexto alrededor de la coincidencia:

```bash
grep -A <número> "<texto>" <fichero>
# <número> líneas Después (After)

grep -B <número> "<texto>" <fichero>
# <número> líneas Antes (Before)

grep -C <número> "<texto>" <fichero>
# <número> líneas de Contexto a cada lado

grep -C 2 "error" <fichero>
```

Contar y listar solo nombres:

```bash
grep -c "<texto>" <fichero>
# cuántas líneas coinciden

grep -l "<texto>" <directorio>/*
# solo nombres de ficheros con coincidencia

grep -L "<texto>" <directorio>/*
# ficheros sin coincidencia
```

Con tuberías:

```bash
ps aux | grep <usuario>
# procesos relacionados con <usuario> (ver nota abajo)

ls -la <directorio> | grep "<patrón>"
dmesg | grep -i error
```

Expresiones regulares básicas:

```bash
grep "^<texto>" <fichero>
# líneas que empiezan por <texto>

grep "<texto>$" <fichero>
# líneas que terminan por <texto>

grep -E "<patrón1>|<patrón2>" <fichero>
# extended regex: una u otra (también egrep en sistemas antiguos)

grep -w "<texto>" <fichero>
# palabra completa (no subcadenas dentro de otra palabra)
```

## Opciones útiles

| Opción | Qué hace |
|--------|----------|
| `-i` | Ignora mayúsculas |
| `-n` | Número de línea |
| `-v` | Invierte la coincidencia |
| `-r` / `-R` | Recursivo en directorios |
| `-c` | Cuenta coincidencias |
| `-l` / `-L` | Solo nombres de fichero (con / sin match) |
| `-A` `-B` `-C` | Contexto |
| `-w` | Palabra completa |
| `-E` | Regex extendida |
| `-F` | Texto fijo (no interpreta regex) |
| `--include=<patrón>` | Filtra nombres de fichero (con `-r`) |

## Errores frecuentes

- En `ps aux | grep <texto>`, a veces aparece el propio proceso `grep`. Alternativa: `ps aux | grep <texto> | grep -v grep` o `pgrep`.
- Olvidar comillas con espacios o `|`: usa `grep "<patrón>" <fichero>`.
- Caracteres especiales de regex (`.`, `*`, `[]`): si quieres texto literal, `grep -F "<texto>" <fichero>`.

```text
grep: <directorio>: Is a directory
```

- Sin `-r`, `grep` no entra en carpetas.

## Relacionados

- [less](less.md) — leer y buscar interactivamente (`/<texto>`)
- [find](../02-ficheros-directorios/find.md) — buscar por nombre de fichero, no por contenido
- [wc](wc.md) — contar líneas del resultado (`grep ... \| wc -l`)
