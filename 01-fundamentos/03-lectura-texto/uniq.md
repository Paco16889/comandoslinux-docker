# uniq

Reporta o filtra **líneas consecutivas** duplicadas.

Importante: `uniq` solo detecta duplicados **uno detrás de otro**. Si están separados, primero usa [sort](sort.md).

## Sintaxis

```bash
uniq [opciones] [<fichero>]
```

## Ejemplos

```bash
uniq <fichero>
# elimina líneas duplicadas consecutivas

sort <fichero> | uniq
# patrón clásico: primero ordenar, luego únicos

sort -u <fichero>
# a menudo equivalente práctico a sort + uniq
```

Contar repeticiones:

```bash
uniq -c <fichero>
# antepone el número de veces que se repite cada bloque consecutivo

sort <fichero> | uniq -c
# frecuencia de cada línea

sort <fichero> | uniq -c | sort -nr
# las más repetidas primero
```

Solo duplicadas o solo únicas:

```bash
sort <fichero> | uniq -d
# solo líneas que estaban duplicadas

sort <fichero> | uniq -u
# solo líneas que aparecen una vez
```

Ignorar mayúsculas / campos:

```bash
uniq -i <fichero>
# ignora mayúsculas (en duplicados consecutivos)

uniq -f <número> <fichero>
# ignora los primeros <número> campos al comparar

uniq -s <número> <fichero>
# ignora los primeros <número> caracteres
```

Casos reales:

```bash
cut -d ':' -f 7 /etc/passwd | sort | uniq
# shells de login distintas en el sistema

grep "<texto>" <fichero> | sort | uniq -c | sort -nr
# valores únicos y su frecuencia tras un filtro
```

## Opciones útiles

| Opción | Qué hace |
|--------|----------|
| `-c` | Cuenta ocurrencias consecutivas |
| `-d` | Solo duplicadas |
| `-u` | Solo únicas (aparecen una vez) |
| `-i` | Ignora mayúsculas |
| `-f <número>` | Omite N campos al comparar |
| `-s <número>` | Omite N caracteres al comparar |

## Errores frecuentes

- Usar `uniq <fichero>` sin `sort` y “no quita duplicados”: es porque no eran consecutivos.
- Confundir `uniq -u` (líneas que aparecen una sola vez) con “salida sin duplicados” (eso es `uniq` a secas tras `sort`, o `sort -u`).

## Siguiente

[cut](cut.md)

## Relacionados

- [sort](sort.md) — casi siempre antes de `uniq`
- [cut](cut.md) — extraer el campo a desduplicar
- [wc](wc.md) — `sort <fichero> \| uniq \| wc -l` cuenta valores únicos



