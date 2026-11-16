# diff

Compara dos ficheros línea a línea y muestra las diferencias.

## Sintaxis

```bash
diff [opciones] <fichero1> <fichero2>
diff [opciones] <directorio1> <directorio2>
```

## Ejemplos

```bash
diff <fichero1> <fichero2>
# diferencias entre ambos

diff -u <fichero1> <fichero2>
# formato unificado (el más legible y usado en parches)

diff -y <fichero1> <fichero2>
# lado a lado

diff -y --suppress-common-lines <fichero1> <fichero2>
# lado a lado, solo lo distinto
```

Ignorar ciertos detalles:

```bash
diff -i <fichero1> <fichero2>
# ignora mayúsculas/minúsculas

diff -b <fichero1> <fichero2>
# ignora cambios en la cantidad de espacios

diff -w <fichero1> <fichero2>
# ignora todos los espacios en blanco

diff -B <fichero1> <fichero2>
# ignora líneas en blanco nuevas
```

Directorios:

```bash
diff -r <directorio1> <directorio2>
# compara recursivamente

diff -rq <directorio1> <directorio2>
# solo dice qué ficheros difieren (resumen)
```

Guardar un parche:

```bash
diff -u <fichero1> <fichero2> > <fichero>.patch
# luego se puede aplicar con patch -p0 < <fichero>.patch (tema avanzado)
```

Códigos breves en la salida clásica:

| Marca | Significado |
|-------|-------------|
| `a` | add (añadir líneas) |
| `c` | change (cambiar) |
| `d` | delete (borrar) |
| `<` | línea de `<fichero1>` |
| `>` | línea de `<fichero2>` |

En formato `-u`:

| Prefijo | Significado |
|---------|-------------|
| ` ` (espacio) | línea igual |
| `-` | línea en el primer fichero (quita) |
| `+` | línea en el segundo fichero (añade) |

## Opciones útiles

| Opción | Qué hace |
|--------|----------|
| `-u` | Formato unificado |
| `-y` | Lado a lado |
| `-i` | Ignora mayúsculas |
| `-b` / `-w` | Ignora espacios (parcial / total) |
| `-r` | Recursivo en directorios |
| `-q` | Solo indica si difieren |
| `-N` | Trata ficheros ausentes como vacíos |

## Errores frecuentes

```text
diff: <fichero>: No such file or directory
```

- Alguna de las dos rutas no existe.

- Comparar binarios: `diff` avisará; para binarios a veces se usa `cmp`.
- Salida “vacía”: no hay diferencias (código de salida 0). Con diferencias, el código suele ser 1.

```bash
diff <fichero1> <fichero2>
echo $?
# 0 = iguales, 1 = distintos, 2 = error
```

## Siguiente

[Leer `ls -l`](../04-permisos/leer-ls-l.md)

## Relacionados

- [cat](cat.md) / [less](less.md) — revisar cada fichero por separado
- [grep](grep.md) — buscar un cambio concreto
- [cp](../02-ficheros-directorios/cp.md) — crear una copia antes de editar y luego `diff`



