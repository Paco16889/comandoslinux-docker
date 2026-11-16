# tail

Muestra las últimas líneas de un fichero (por defecto 10). Muy usado con logs.

## Sintaxis

```bash
tail [opciones] [<fichero>...]
```

## Ejemplos

```bash
tail <fichero>
# últimas 10 líneas

tail -n <número> <fichero>
# últimas <número> líneas

tail -n 20 <fichero>
tail -n 50 /var/log/<fichero>
```

Seguir un fichero en vivo (logs):

```bash
tail -f <fichero>
# imprime líneas nuevas conforme se escriben
# salir: Ctrl+C

tail -F <fichero>
# como -f, pero reabre si el fichero se rota/renombra (útil en logs del sistema)
```

Desde una línea concreta (GNU):

```bash
tail -n +<número> <fichero>
# desde la línea <número> hasta el final

tail -n +2 <fichero>
# todo excepto la primera línea (útil para quitar cabecera de CSV)
```

Varios ficheros:

```bash
tail <fichero1> <fichero2>
tail -n 5 <fichero1> <fichero2>
```

Con tuberías:

```bash
journalctl | tail -n 30
# últimas líneas de un volcado largo (si aplica)

cat <fichero> | tail -n 15
# funciona, pero `tail -n 15 <fichero>` es más directo
```

## Opciones útiles

| Opción | Qué hace |
|--------|----------|
| `-n <número>` | Cuántas últimas líneas (`+N` = desde la línea N) |
| `-c <número>` | Últimos N bytes |
| `-f` | Follow: seguir el final del fichero |
| `-F` | Follow reabriendo si cambia el inode/nombre |
| `-q` | Sin encabezados de nombre de fichero |
| `-v` | Siempre muestra el nombre |

## Errores frecuentes

- Dejar `tail -f` abierto y pensar que la shell se colgó: está esperando; `Ctrl+C` para salir.
- Sin permiso en `/var/log/...`: prueba con tu usuario o `sudo tail -f <fichero>` solo si corresponde.
- Confundir `-n 20` (últimas 20) con `-n +20` (desde la 20 hasta el final).

## Relacionados

- [head](head.md) — primeras líneas
- [less](less.md) — navegar y buscar (`less +F` también puede seguir)
- [grep](grep.md) — filtrar líneas de un log
