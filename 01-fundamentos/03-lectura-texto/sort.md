# sort

Ordena las líneas de un fichero o de la entrada estándar.

## Sintaxis

```bash
sort [opciones] [<fichero>...]
```

## Ejemplos

```bash
sort <fichero>
# orden alfabético (según locale)

sort <fichero1> <fichero2>
# junta y ordena

sort -o <fichero_ordenado> <fichero>
# escribe el resultado en <fichero_ordenado>
```

Orden numérico y reverso:

```bash
sort -n <fichero>
# orden numérico (1, 2, 10… no 1, 10, 2)

sort -r <fichero>
# orden inverso (Z→A)

sort -nr <fichero>
# numérico e inverso (mayores primero)
```

Únicos y campos:

```bash
sort -u <fichero>
# ordena y deja líneas únicas (parecido a sort + uniq)

sort -k <número> <fichero>
# ordena por el campo <número> (campos separados por blanco)

sort -t ':' -k 1 <fichero>
# separador ':' (típico en /etc/passwd) y ordena por campo 1

sort -t ',' -k 2,2 <fichero>
# CSV: ordena solo por la columna 2
```

Human-readable sizes y mes:

```bash
sort -h <fichero>
# entiende 1K, 2M, 3G… (GNU sort)

du -h <directorio> | sort -h
# tamaños de menor a mayor

ls -l <directorio> | sort -k 5 -n
# ordenar listado por tamaño (columna 5 de ls -l; frágil pero didáctico)
```

Tuberías típicas:

```bash
cat <fichero> | sort
cut -d ':' -f 1 <fichero> | sort
```

## Opciones útiles

| Opción | Qué hace |
|--------|----------|
| `-n` | Orden numérico |
| `-r` | Inverso |
| `-u` | Únicas |
| `-k <campo>` | Ordenar por campo |
| `-t <carácter>` | Separador de campos |
| `-o <fichero>` | Fichero de salida |
| `-h` | Tamaños humanos (GNU) |
| `-f` | Ignora mayúsculas al ordenar |
| `-b` | Ignora blancos iniciales |

## Errores frecuentes

- Ordenar números con `sort` sin `-n`: `10` queda antes que `2`.
- Pensar que `sort -u` es igual que `uniq` siempre: `uniq` exige que los duplicados estén **juntos**; `sort -u` ordena primero.
- Separador mal elegido en CSV: si hay comas dentro de campos entrecomillados, `sort`/`cut` se quedan cortos.

## Relacionados

- [uniq](uniq.md) — trabajar con duplicados (normalmente tras `sort`)
- [cut](cut.md) — extraer columnas antes de ordenar
- [grep](grep.md) — filtrar antes de ordenar
