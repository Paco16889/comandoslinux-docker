# 09 — Scripts

Un **script** es un fichero de texto con comandos que la shell ejecuta en orden.  
Sirve para no repetir a mano lo mismo una y otra vez.

> **Cómo leer la sintaxis:** `<algo>` = lo sustituyes tú (no escribas `<` `>`).  
> `[algo]` = opcional (no escribas `[` `]`).  
> Explicación completa: [Cómo leer la sintaxis](../../README.md#cómo-leer-la-sintaxis-muy-importante).

## La duda más frecuente (léela ya)

| Forma de lanzar | Qué ocurre |
|-----------------|------------|
| `sh <fichero>.sh` | Lo interpreta **`sh`** (en Ubuntu/Mint suele ser `dash`: más limitado) |
| `bash <fichero>.sh` | Lo interpreta **Bash**, aunque no tenga shebang |
| `./<fichero>.sh` | El sistema mira la 1ª línea (`#!/bin/bash` o `#!/bin/sh`) y hace falta **permiso de ejecución** (`chmod +x`) |

Detalle con ejemplos: [sh-vs-bash-vs-ejecutable](sh-vs-bash-vs-ejecutable.md).

## Orden recomendado

1. [sh-vs-bash-vs-ejecutable](sh-vs-bash-vs-ejecutable.md) — cómo se lanza un script
2. [shebang](shebang.md) — la primera línea `#!/...`
3. [primer-script](primer-script.md) — tu primer script paso a paso
4. [variables](variables.md) — guardar valores
5. [argumentos](argumentos.md) — `$1`, `$2`, `$@`…
6. [condicionales](condicionales.md) — `if`, test, `[[ ]]`
7. [bucles](bucles.md) — `for`, `while`
8. [codigos-salida](codigos-salida.md) — `$?` y `exit`
9. [source-y-permisos](source-y-permisos.md) — `source`, `chmod`, buenos hábitos
