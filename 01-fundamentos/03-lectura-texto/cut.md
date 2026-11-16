# cut

Recorta columnas o campos de cada línea de un fichero.

## Sintaxis

```bash
cut [opciones] [<fichero>...]
```

Casi siempre usarás `-d` (delimitador) y `-f` (campos), o `-c` (caracteres).

## Ejemplos — por campos

```bash
cut -d ':' -f 1 <fichero>
# campo 1, separado por :

cut -d ':' -f 1 /etc/passwd
# nombres de usuario del sistema

cut -d ':' -f 1,3 /etc/passwd
# usuario y UID

cut -d ':' -f 1-3 <fichero>
# campos del 1 al 3

cut -d ',' -f 2 <fichero>
# segunda columna de un CSV simple

cut -d ' ' -f 1 <fichero>
# primer campo separado por espacio
```

Varios ficheros / rutas:

```bash
cut -d ':' -f 1 <directorio>/<fichero>
cut -d ';' -f 1,2 <fichero1> <fichero2>
```

## Ejemplos — por caracteres

```bash
cut -c 1-10 <fichero>
# caracteres del 1 al 10 de cada línea

cut -c 1,2,5 <fichero>
# caracteres 1, 2 y 5

cut -c 5- <fichero>
# desde el carácter 5 hasta el final
```

## Combinaciones útiles

```bash
grep "<usuario>" /etc/passwd | cut -d ':' -f 6
# home del usuario que coincida

cut -d ':' -f 7 /etc/passwd | sort | uniq -c
# shells y su frecuencia

head -n 1 <fichero>
cut -d ',' -f 1,3 <fichero> | head
# explorar columnas de un CSV
```

Sin delimitador repetido “colapsado”: `cut` no trata varios espacios como uno solo. Si el fichero alinea columnas con espacios variables, a veces conviene `awk` (más adelante en avanzado).

## Opciones útiles

| Opción | Qué hace |
|--------|----------|
| `-d <carácter>` | Delimitador de campos |
| `-f <lista>` | Campos a mostrar (`1`, `1,3`, `2-5`) |
| `-c <lista>` | Caracteres a mostrar |
| `--complement` | Muestra lo contrario de `-f`/`-c` (GNU) |
| `-s` | Omite líneas sin delimitador |

```bash
cut -d ':' -f 1 --complement /etc/passwd
# todo excepto el campo 1
```

## Errores frecuentes

```text
cut: the delimiter must be a single character
```

- `-d` solo admite un carácter (no una cadena).

- Esperar que `cut -d ' ' -f 2` funcione bien con espacios múltiples: no colapsa huecos.
- Olvidar `-d` al usar `-f`: por defecto el delimitador es tabulación (`Tab`), no coma ni dos puntos.

## Relacionados

- [grep](grep.md) — filtrar líneas antes de cortar
- [sort](sort.md) / [uniq](uniq.md) — ordenar y desduplicar columnas
- [head](head.md) — ver una muestra
