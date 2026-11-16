# xargs

Convierte líneas que llegan por un pipe en **argumentos** de otro comando.

Muchos comandos no leen una lista por stdin de la forma que esperas; `xargs` ayuda a montar:

```text
línea1
línea2    →   comando línea1 línea2
```

## Sintaxis

```bash
... | xargs [opciones] <comando>
```

## Ejemplos

```bash
echo "uno dos tres" | xargs echo
# echo uno dos tres

ls *.txt | xargs cat
# concatena esos txt (ojo con espacios en nombres)
```

Borrar ficheros listados por `find` (con cuidado):

```bash
find <directorio> -type f -name "*.tmp" | xargs rm -f
```

Forma más segura con `find` (preferible cuando hay espacios):

```bash
find <directorio> -type f -name "*.tmp" -print0 | xargs -0 rm -f
# -print0 y -0 usan separador nulo (soporta nombres raros)
```

Un argumento por invocación:

```bash
printf "a\nb\nc\n" | xargs -n 1 echo
# echo a
# echo b
# echo c
```

Placeholder con `-I`:

```bash
ls *.jpg | xargs -I {} cp {} <directorio>/
# {} se sustituye por cada elemento
```

## Cuándo usarlo

| Situación | Herramienta |
|-----------|-------------|
| Filtrar texto | `grep`, pipes normales |
| Pasar muchos paths a un comando | `xargs` o `find -exec` |
| Nombres con espacios | `find -print0` + `xargs -0` |

## Errores frecuentes

- Nombres con espacios rotos sin `-0` / `-print0`.  
- `xargs rm` sin listar antes qué se va a borrar: peligroso. Prueba antes con `echo`:

```bash
find <directorio> -name "*.tmp" | xargs echo rm
# muestra qué haría; quita echo cuando estés seguro
```

## Siguiente

[tar y gzip](../11-avanzado/tar-gzip.md)

## Relacionados

- [pipes](pipes.md)  
- [find](../../01-fundamentos/02-ficheros-directorios/find.md)  
- [bucles](../09-scripts/bucles.md) — a veces un `for` es más claro al aprender



