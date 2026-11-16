# Argumentos del script (`$1`, `$2`, `$@`…)

Los argumentos son lo que escribes **después** del nombre del script.

```bash
./backup.sh Documentos /tmp/copia
#            ^$1        ^$2
```

## Variables especiales

| Variable | Significado |
|----------|-------------|
| `$0` | Nombre del script |
| `$1` | Primer argumento |
| `$2` | Segundo (y así hasta `$9`, luego `${10}`…) |
| `$#` | Cuántos argumentos hay |
| `"$@"` | Todos, **bien separados** (recomendado) |
| `"$*"` | Todos en uno (menos flexible) |
| `$$` | PID del script |

## Ejemplos

```bash
#!/bin/bash
echo "Script: $0"
echo "Número de argumentos: $#"
echo "Primero: $1"
echo "Segundo: $2"
```

```bash
bash args.sh uno dos tres
./args.sh uno dos tres
```

```text
Script: args.sh
Número de argumentos: 3
Primero: uno
Segundo: dos
```

(`$0` puede verse como `./args.sh` si lo lanzas así.)

Recorrer todos:

```bash
#!/bin/bash
for arg in "$@"; do
  echo "Argumento: $arg"
done
```

```bash
bash recorre.sh manzana pera "kiwi verde"
```

```text
Argumento: manzana
Argumento: pera
Argumento: kiwi verde
```

Comprobar que hay argumentos:

```bash
#!/bin/bash
if [ $# -lt 1 ]; then
  echo "Uso: $0 <directorio>"
  exit 1
fi

directorio="$1"
echo "Voy a listar: $directorio"
ls -la "$directorio"
```

```bash
bash listar_dir.sh
# Uso: listar_dir.sh <directorio>

bash listar_dir.sh /tmp
# Voy a listar: /tmp
# (después, el listado largo de /tmp)
```

## Ejemplo práctico: saludar

```bash
#!/bin/bash
# uso: ./saluda.sh <nombre>
nombre="${1:-mundo}"
# si no hay $1, usa "mundo"
echo "Hola, $nombre"
```

```bash
./saluda.sh
# Hola, mundo

./saluda.sh Ana
# Hola, Ana

./saluda.sh "<usuario>"
# Hola, <usuario>
```

## Errores frecuentes

- Usar `$@` sin comillas: se rompe con espacios. Prefiere `"$@"`.  
- Pensar que `$1` es el script: el script es `$0`.  
- Olvidar pasar el argumento y no comprobar `$#`.

## Siguiente

[Condicionales (`if`)](condicionales.md)

## Relacionados

- [variables](variables.md)  
- [condicionales](condicionales.md)  
- [codigos-salida](codigos-salida.md)



