# man

Muestra el manual de un comando (*manual*).

## Sintaxis

```bash
man [<sección>] <comando>
```

> Recuerda: `[ ]` = opcional · `<comando>` lo sustituyes tú.  
> Detalle: [Cómo leer la sintaxis](../../README.md#cómo-leer-la-sintaxis-muy-importante).

## Laboratorio rápido

```bash
man ls
# Espacio = bajar página · /texto = buscar · n = siguiente · q = salir

man ls
# dentro: /sort
# n
# q
```

## Ejemplos

```bash
man ls
# manual de ls

man <comando>
# manual del comando que indiques

man 5 passwd
# sección 5: formato del fichero passwd (no el comando)

man 1 passwd
# sección 1: el comando passwd

man -a passwd
# todas las páginas disponibles para passwd

man -k <patrón>
# busca en resúmenes (equivale a apropos)

man -k copy
man -f ls
# descripción corta (whatis)
```

Si no hay manuales (Mint/Ubuntu):

```bash
# Debian / Ubuntu / Linux Mint
sudo apt install man-db manpages
```

man -k copy
# páginas relacionadas con "copy"

man -f <comando>
# descripción corta (equivale a whatis)
```

Dentro del visor (suele ser `less`):

| Tecla | Acción |
|-------|--------|
| `Espacio` / `f` | Página siguiente |
| `b` | Página anterior |
| `/<texto>` | Buscar hacia delante |
| `n` | Siguiente coincidencia |
| `q` | Salir |

## Secciones habituales

| Sección | Contenido |
|---------|-----------|
| 1 | Comandos de usuario |
| 2 | Llamadas al sistema |
| 3 | Funciones de biblioteca |
| 5 | Formatos de ficheros |
| 8 | Comandos de administración |

## Errores frecuentes

```text
No manual entry for <comando>
```

- El comando no está instalado, o no tiene página `man` (a veces solo `--help`).
- Sección equivocada: prueba `man -a <comando>`.
- Quedarse atrapado en el manual: pulsa `q`.

## Relacionados

- [help](help.md) — ayuda de builtins de la shell
- `<comando> --help` — resumen corto de muchos programas
