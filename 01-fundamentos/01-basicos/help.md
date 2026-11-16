# help

Muestra ayuda de **comandos internos** de la shell Bash (*builtins*).

## Sintaxis

```bash
help [<patrón>]
```

## Ejemplos

```bash
help
# lista builtins disponibles

help cd
# ayuda de cd

help <comando>
# si <comando> es un builtin (cd, history, echo…)

help history
# ayuda de history

help -d
# descripción corta de todos los builtins

help -m cd
# formato parecido a una página man
```

Comprobar si algo es builtin:

```bash
type cd
# cd is a shell builtin

type ls
# ls is /usr/bin/ls  (o similar)

type <comando>
# te dice si es builtin, alias, fichero, etc.
```

## Cuándo usar `help` y cuándo `man`

| Situación | Usa |
|-----------|-----|
| Builtin de Bash (`cd`, `echo`, `history`, `alias`…) | `help <comando>` |
| Programa externo (`ls`, `grep`, `find`…) | `man <comando>` o `<comando> --help` |

## Errores frecuentes

- Buscar `man cd` y encontrar poco útil: `cd` es builtin → `help cd`.
- Ejecutar `help ls`: `ls` no es builtin; usa `man ls`.

## Siguiente

[history](history.md)

## Relacionados

- [man](man.md) — manuales de programas externos
- [history](history.md) — ejemplo típico de builtin



