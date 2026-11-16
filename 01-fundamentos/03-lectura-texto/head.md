# head

Muestra las primeras líneas de un fichero (por defecto 10).

## Sintaxis

```bash
head [opciones] [<fichero>...]
```

## Ejemplos

```bash
head <fichero>
# primeras 10 líneas

head -n <número> <fichero>
# primeras <número> líneas

head -n 5 <fichero>
# primeras 5

head -n 20 <directorio>/<fichero>
```

Varios ficheros:

```bash
head <fichero1> <fichero2>
# muestra un encabezado con el nombre de cada uno

head -n 3 <fichero1> <fichero2>
```

Por bytes (no líneas):

```bash
head -c <número> <fichero>
# primeros <número> bytes

head -c 100 <fichero>
```

Con tuberías (adelanto):

```bash
ls -lt <directorio> | head
# los 10 primeros del listado

ps aux | head -n 15
# primeras líneas de procesos
```

Cabeceras de CSV / configs:

```bash
head -n 1 <fichero>
# solo la primera línea (p. ej. nombres de columnas)
```

## Opciones útiles

| Opción | Qué hace |
|--------|----------|
| `-n <número>` | Cuántas líneas (también se admite `-<número>` en muchas versiones) |
| `-c <número>` | Cuántos bytes |
| `-q` | No muestra el nombre del fichero al leer varios |
| `-v` | Siempre muestra el nombre del fichero |

```bash
head -n -5 <fichero>
# en GNU head: todo excepto las últimas 5 líneas
```

## Errores frecuentes

```text
head: cannot open '<fichero>' for reading: No such file or directory
```

- Fichero inexistente o ruta mal escrita.

- Confundir `head` con `tail`: `head` es el **principio**, `tail` el **final**.

## Siguiente

[tail](tail.md)

## Relacionados

- [tail](tail.md) — últimas líneas
- [less](less.md) — recorrer el fichero entero
- [cat](cat.md) — todo el contenido



