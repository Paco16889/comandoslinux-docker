# 10 — Pipes y redirección

Cómo conectar comandos entre sí y enviar la salida a ficheros (o leer desde ellos).

> **Cómo leer la sintaxis:** `<algo>` = lo sustituyes tú · `[algo]` = opcional.  
> Guía completa: [Cómo leer la sintaxis](../../README.md#cómo-leer-la-sintaxis-muy-importante).

## Ideas previas

| Concepto | En pocas palabras |
|----------|-------------------|
| Salida estándar (stdout) | Lo “normal” que imprime un comando (pantalla) |
| Error estándar (stderr) | Mensajes de error (también a pantalla por defecto) |
| Entrada estándar (stdin) | Lo que el comando lee (teclado, o un pipe) |
| Redirigir | Mandar stdout/stderr a un fichero en lugar de la pantalla |
| Pipe `\|` | La salida de un comando alimenta la entrada del siguiente |

## Orden recomendado

1. [stdout-stderr](stdout-stderr.md) — las dos salidas
2. [redireccion](redireccion.md) — `>`, `>>`, `<`
3. [pipes](pipes.md) — `|`
4. [combinar-errores](combinar-errores.md) — `2>`, `2>&1`, `&>`
5. [and-or](and-or.md) — `&&` y `||`
6. [tee](tee.md) — pantalla y fichero a la vez
7. [xargs](xargs.md) — convertir líneas en argumentos
