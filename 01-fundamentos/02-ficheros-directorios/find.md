# find

Busca ficheros y directorios según criterios (nombre, tipo, tamaño, fecha…).

## Sintaxis

```bash
find [<ruta>] [expresión]
```

Si omites `<ruta>`, busca desde el directorio actual (`.`).

## Ejemplos

Por nombre:

```bash
find . -name "<fichero>"
# busca ese nombre exacto desde el directorio actual

find /home/<usuario> -name "*.txt"
# todos los .txt bajo el home de <usuario>

find <directorio> -iname "<patrón>"
# igual que -name pero sin distinguir mayúsculas
```

Por tipo:

```bash
find <ruta> -type f
# solo ficheros normales

find <ruta> -type d
# solo directorios

find <ruta> -type l
# solo enlaces simbólicos
```

Combinaciones habituales:

```bash
find <directorio> -type f -name "*.log"
# ficheros .log dentro de <directorio>

find . -type d -name "<directorio>"
# carpetas con ese nombre

find /home/<usuario> -type f -mtime -7
# ficheros modificados en los últimos 7 días

find . -type f -size +10M
# ficheros de más de 10 megabytes
```

Ejecutar algo sobre lo encontrado (con cuidado):

```bash
find <directorio> -type f -name "*.tmp" -print
# solo lista (seguro)

find <directorio> -type f -name "*.tmp" -delete
# borra lo encontrado (peligroso: prueba antes con -print)
```

## Opciones / tests útiles

| Expresión | Qué hace |
|-----------|----------|
| `-name <patrón>` | Nombre (comodines entre comillas) |
| `-iname <patrón>` | Nombre sin importar mayúsculas |
| `-type f\|d\|l` | Fichero / directorio / symlink |
| `-mtime -<número>` | Modificado en los últimos N días |
| `-size +<número>M` | Mayor que N megabytes |
| `-maxdepth <número>` | Limita profundidad de carpetas |
| `-print` | Imprime (por defecto en muchos casos) |
| `-delete` | Borra coincidencias (¡cuidado!) |

Limitar profundidad:

```bash
find <directorio> -maxdepth 1 -type f
# solo ficheros en ese nivel, sin bajar a subcarpetas
```

## Errores frecuentes

- Olvidar las comillas en patrones: `find . -name *.txt` puede expandirlo la shell antes; mejor `find . -name "*.txt"`.
- Empezar en `/` sin `-maxdepth`: puede ser lento y listar cosas sin permiso.
- Usar `-delete` o `-exec rm` sin haber listado antes el resultado.

```text
find: ‘<ruta>’: Permission denied
```

- Carpetas a las que tu `<usuario>` no puede entrar; es normal en rutas del sistema.

## Siguiente

[tree](tree.md)

## Relacionados

- [ls](../01-basicos/ls.md) — listar un directorio concreto
- [tree](tree.md) — ver estructura en árbol
- [rm](rm.md) — borrar a mano lo que find te mostró



