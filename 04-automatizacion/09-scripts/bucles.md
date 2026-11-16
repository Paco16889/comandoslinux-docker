# Bucles (`for`, `while`)

Repiten comandos varias veces.

## for — lista de valores

```bash
#!/bin/bash
for nombre in Ana Luis Mia; do
  echo "Hola, $nombre"
done
```

Si lo guardas como `saludos.sh` y lo lanzas:

```bash
bash saludos.sh
```

```text
Hola, Ana
Hola, Luis
Hola, Mia
```

```bash
#!/bin/bash
for fichero in *.txt; do
  echo "Archivo: $fichero"
done
```

```bash
bash listar_txt.sh
# Archivo: notas.txt
# Archivo: lista.txt
# (uno por cada .txt del directorio actual; si no hay ninguno, Bash deja el patrón literal *.txt)
```

```bash
#!/bin/bash
for arg in "$@"; do
  echo "Arg: $arg"
done
```

```bash
bash args_bucle.sh uno "dos tres" cuatro
```

```text
Arg: uno
Arg: dos tres
Arg: cuatro
```

## for — rangos (Bash)

```bash
#!/bin/bash
for i in {1..5}; do
  echo "Número $i"
done

for i in $(seq 1 5); do
  echo "seq: $i"
done
```

```bash
bash rangos.sh
```

```text
Número 1
Número 2
Número 3
Número 4
Número 5
seq: 1
seq: 2
seq: 3
seq: 4
seq: 5
```

## while — mientras se cumpla

```bash
#!/bin/bash
n=1
while [ "$n" -le 5 ]; do
  echo "n=$n"
  n=$((n + 1))
done
```

```bash
bash while_n.sh
```

```text
n=1
n=2
n=3
n=4
n=5
```

Leer un fichero línea a línea:

```bash
#!/bin/bash
fichero="$1"
while IFS= read -r linea; do
  echo "Línea: $linea"
done < "$fichero"
```

```bash
printf 'alpha\nbeta\n' > /tmp/demo_lineas.txt
bash leer_lineas.sh /tmp/demo_lineas.txt
```

```text
Línea: alpha
Línea: beta
```

## Ejemplo práctico: copias con fecha

```bash
#!/bin/bash
# uso: ./copia_txt.sh <directorio_origen> <directorio_destino>
origen="$1"
destino="$2"
mkdir -p "$destino"

for f in "$origen"/*.txt; do
  [ -e "$f" ] || continue
  base=$(basename "$f")
  cp "$f" "$destino/${base%.txt}-$(date +%F).txt"
done
```

```bash
mkdir -p /tmp/origen_demo /tmp/destino_demo
echo "hola" > /tmp/origen_demo/notas.txt
bash copia_txt.sh /tmp/origen_demo /tmp/destino_demo
ls /tmp/destino_demo
# notas-AAAA-MM-DD.txt   (la fecha de hoy, p. ej. notas-2026-11-16.txt)
```

## break y continue

```bash
#!/bin/bash
for i in 1 2 3 4 5; do
  if [ "$i" -eq 3 ]; then
    continue
    # salta el resto del cuerpo y sigue el bucle
  fi
  if [ "$i" -eq 5 ]; then
    break
    # sale del bucle
  fi
  echo "$i"
done
```

```bash
bash break_continue.sh
```

```text
1
2
4
```

(El `3` se salta con `continue`; el `5` dispara `break` antes del `echo`.)

## Errores frecuentes

- `for f in $(ls)`: problemas con espacios; mejor `for f in *.txt` o `find`…  
- Bucle infinito en `while`: asegúrate de que la condición acaba cambiando.  
- Olvidar `do` / `done`.

## Siguiente

[Códigos de salida (`$?`, `exit`)](codigos-salida.md)

## Relacionados

- [condicionales](condicionales.md)  
- [argumentos](argumentos.md)  
- Más adelante: pipes para combinar con `while read`



