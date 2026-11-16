# cat

Muestra el contenido de ficheros o los concatena (*concatenate*).

Para ficheros largos suele ser mejor [less](less.md).

## Sintaxis

```bash
cat [opciones] [<fichero>...]
```

## Ejemplos

```bash
cat <fichero>
# imprime todo el contenido en pantalla

cat <fichero1> <fichero2>
# uno detrás de otro

cat <directorio>/<fichero>
# con ruta
```

Numerar líneas:

```bash
cat -n <fichero>
# numera todas las líneas

cat -b <fichero>
# numera solo las que tienen texto (salta vacías)
```

Crear o sobrescribir un fichero (redirección; más en `10-pipes-redireccion`):

```bash
cat > <fichero>
# escribes líneas; terminas con Ctrl+D

cat >> <fichero>
# añade al final; terminas con Ctrl+D
```

Copiar contenido de un fichero a otro:

```bash
cat <fichero> > <fichero_copia>
# equivalente práctico a copiar texto (no metadatos)

cat <fichero1> <fichero2> > <fichero_unido>
# une dos ficheros en uno nuevo
```

Ver caracteres no imprimibles:

```bash
cat -A <fichero>
# muestra finales de línea ($) y otros caracteres especiales

cat -T <fichero>
# muestra tabuladores como ^I
```

## Opciones útiles

| Opción | Qué hace |
|--------|----------|
| `-n` | Numera todas las líneas |
| `-b` | Numera solo líneas no vacías |
| `-s` | Comprime varias líneas vacías seguidas en una |
| `-A` | Muestra caracteres “ocultos” de forma visible |
| `-T` | Muestra tabuladores |

## Errores frecuentes

```text
cat: <fichero>: No such file or directory
```

- Ruta mal escrita o fichero inexistente.

- Usar `cat` en logs enormes: la terminal se llena; usa `less <fichero>` o `tail <fichero>`.
- `cat <fichero> | less` funciona, pero `less <fichero>` es más directo.

## Relacionados

- [less](less.md) — lectura cómoda página a página
- [head](head.md) / [tail](tail.md) — solo un trozo
- [echo](../01-basicos/echo.md) — escribir una línea rápida
