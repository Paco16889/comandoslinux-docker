# ls

Lista el contenido de un directorio (*list*).

## Sintaxis

```bash
ls [opciones] [<ruta>]
```

> Recuerda: `[ ]` = opcional (no se escriben). `<ruta>` = lo sustituyes tú (sin `<` `>`).  
> Detalle: [Cómo leer la sintaxis](../../README.md#cómo-leer-la-sintaxis-muy-importante).

Si no pasas una ruta, lista el directorio actual.

## Ejemplos

```bash
ls
# nombres visibles del directorio actual

ls <directorio>
# contenido de ese directorio sin entrar en él

ls -l
# formato largo: permisos, dueño, tamaño, fecha

ls -a
# incluye ficheros ocultos (empiezan por .)

ls -la
# combinación muy usada: largo + ocultos

ls -lh <directorio>
# tamaños legibles (K, M, G)

ls -lt
# ordenado por fecha de modificación (más reciente primero)

ls -ltr
# por fecha, el más antiguo primero (útil al final de logs)

ls -1
# un nombre por línea (cómodo para copiar o contar)

ls -R <directorio>
# lista también subdirectorios (recursivo)

ls <fichero1> <fichero2>
# comprueba/lista varios elementos concretos
```

Filtros con comodines (la shell los expande):

```bash
ls *.txt
# solo ficheros que terminan en .txt

ls <directorio>/*.md
# solo .md dentro de <directorio>

ls -ld <directorio>
# info del propio directorio, no de su contenido
```

## Opciones útiles

| Opción | Qué hace |
|--------|----------|
| `-l` | Formato detallado |
| `-a` | Incluye ocultos |
| `-d` | Lista el directorio en sí, no su contenido |
| `-h` | Tamaños legibles (con `-l`) |
| `-t` | Ordena por fecha |
| `-r` | Invierte el orden |
| `-R` | Lista también subdirectorios (recursivo) |
| `-1` | Un nombre por línea |

## Errores frecuentes

- No ver ficheros como `.bashrc`: faltaba `-a`.
- Confundir el tamaño de un directorio en `ls -l` con el espacio total de su contenido.
- `ls <directorio>` vs `ls -ld <directorio>`: el primero lista dentro; el segundo describe la carpeta.

## Siguiente

[cd](cd.md)

## Relacionados

- [pwd](pwd.md) — saber en qué directorio estás
- [cd](cd.md) — entrar en otro directorio
- Ver también [02-ficheros-directorios](../02-ficheros-directorios/)



