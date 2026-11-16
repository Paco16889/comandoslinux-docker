# wc

Cuenta líneas, palabras y bytes/caracteres de un fichero (*word count*).

## Sintaxis

```bash
wc [opciones] [<fichero>...]
```

## Ejemplos

```bash
wc <fichero>
# líneas  palabras  bytes  <fichero>

wc -l <fichero>
# solo número de líneas

wc -w <fichero>
# solo palabras

wc -c <fichero>
# bytes

wc -m <fichero>
# caracteres (puede diferir de -c con UTF-8)
```

Varios ficheros:

```bash
wc <fichero1> <fichero2>
# una fila por fichero + total

wc -l <fichero1> <fichero2> <fichero3>
```

Con rutas:

```bash
wc -l <directorio>/<fichero>
wc -l /home/<usuario>/<directorio>/*.txt
# cuenta líneas de todos los .txt (la shell expande *)
```

Desde tuberías:

```bash
ls <directorio> | wc -l
# cuántas entradas listó ls (aprox. cuántos elementos visibles)

grep -c "<patrón>" <fichero>
# alternativa: grep ya puede contar coincidencias

cat <fichero> | wc -l
# igual que wc -l <fichero>
```

## Opciones útiles

| Opción | Qué hace |
|--------|----------|
| `-l` | Líneas |
| `-w` | Palabras |
| `-c` | Bytes |
| `-m` | Caracteres |
| `-L` | Longitud de la línea más larga (GNU) |

```bash
wc -L <fichero>
# útil para detectar líneas enormes
```

## Errores frecuentes

- Interpretar “palabras” como en un procesador de texto: `wc` separa por espacios/blancos; no entiende gramática.
- `wc -l` en un fichero sin salto final en la última línea: el conteo puede sorprender (la última línea a veces no cuenta igual según el caso).
- Contar ficheros con `ls \| wc -l`: los ocultos y el formato de `ls` afectan; para contar ficheros suele ser más fiable `find`.

## Relacionados

- [cat](cat.md) — ver el contenido que estás contando
- [grep](grep.md) — contar coincidencias con `-c`
- [find](../02-ficheros-directorios/find.md) — listar ficheros para contarlos
