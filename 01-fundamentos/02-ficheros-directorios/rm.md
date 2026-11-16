# rm

Elimina ficheros y, con opciones, directorios (*remove*).

**Cuidado:** en Linux no hay “papelera” por defecto. Lo borrado suele ser irreversible.

## Sintaxis

```bash
rm [opciones] <fichero>...
rm [opciones] <ruta>...
```

## Ejemplos

```bash
rm <fichero>
# borra un fichero

rm <fichero1> <fichero2>
# borra varios

rm -i <fichero>
# pide confirmación

rm -v <fichero>
# muestra lo que borra
```

Directorios:

```bash
rm -r <directorio>
# borra la carpeta y TODO su contenido

rm -ri <directorio>
# recursivo preguntando (más seguro al aprender)

rm -rf <directorio>
# fuerza y recursivo: muy potente y peligroso
```

Patrones (la shell expande):

```bash
rm *.tmp
# borra todos los .tmp del directorio actual

rm <directorio>/*.log
# borra los .log dentro de <directorio>
```

## Opciones útiles

| Opción | Qué hace |
|--------|----------|
| `-i` | Pregunta antes de cada borrado |
| `-I` | Pregunta una vez si hay muchos ficheros o usas `-r` |
| `-r` / `-R` | Recursivo (necesario para directorios) |
| `-f` | No pide confirmación; ignora ficheros inexistentes |
| `-v` | Verbose |

## Errores frecuentes / peligros

```text
rm: cannot remove '<directorio>': Is a directory
```

- Falta `-r` para borrar carpetas.

```text
rm: cannot remove '<ruta>': Permission denied
```

- Sin permiso de escritura en el directorio padre (o atributos especiales).

**Nunca** experimentes con:

```bash
rm -rf /
# o rutas mal escritas con espacios/variables vacías
```

Mientras aprendes, prefieres `rm -i` o `rm -ri`.

## Siguiente

[rmdir](rmdir.md)

## Relacionados

- [rmdir](rmdir.md) — solo directorios vacíos (más seguro para carpetas)
- [mv](mv.md) — “quitar de aquí” sin borrar (mover a otro sitio)
- [ls](../01-basicos/ls.md) — revisar antes de borrar



