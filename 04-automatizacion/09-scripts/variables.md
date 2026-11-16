# Variables en Bash

Una variable guarda un valor para reutilizarlo.

## Sintaxis

```bash
<nombre_variable>=<valor>
echo $<nombre_variable>
echo ${<nombre_variable>}
```

> **Importante:** no pongas espacios alrededor del `=`.  
> Mal: `nombre = Paco` · Bien: `nombre=Paco`

## Ejemplos

```bash
#!/bin/bash
nombre="<usuario>"
saludo="Hola"

echo "$saludo, $nombre"
echo "Home: $HOME"
echo "Usuario: $USER"
```

```bash
bash vars_demo.sh
```

```text
Hola, <usuario>
Home: /home/<usuario>
Usuario: <usuario>
```

(Si en el script pones tu nombre real en lugar del marcador, verás ese nombre.)

```bash
directorio="$HOME/Documentos"
fichero="notas.txt"
ruta="$directorio/$fichero"
echo "$ruta"
# /home/<usuario>/Documentos/notas.txt
```

Comillas:

```bash
#!/bin/bash
texto="Hola mundo"
echo "$texto"
# Hola mundo

echo '$texto'
# $texto   (literal; no expande)

echo "Saludo: $texto"
# Saludo: Hola mundo
```

## Capturar la salida de un comando

```bash
#!/bin/bash
hoy=$(date +%F)
archivos=$(ls | wc -l)

echo "Hoy: $hoy"
echo "Entradas listadas: $archivos"
```

```bash
bash captura.sh
```

```text
Hoy: AAAA-MM-DD
Entradas listadas: <número>
```

(Ejemplo: `Hoy: 2026-11-16` y un número según cuántos ficheros/carpetas haya en el directorio actual.)

También verás la forma antigua `` `comando` ``; mejor usa `$(comando)`.

## Variables de entorno vs del script

```bash
echo "$HOME" "$USER" "$PWD" "$PATH"
# /home/<usuario>  <usuario>  /home/<usuario>/...  /usr/local/bin:...
# suelen existir en cualquier sesión

MI_VAR="solo en este script"
export MI_VAR
# export la hace disponible para comandos hijos
echo "$MI_VAR"
# solo en este script
```

## Leer del teclado

```bash
#!/bin/bash
echo -n "¿Cómo te llamas? "
read nombre
echo "Hola, $nombre"
```

```text
¿Cómo te llamas? Ana
Hola, Ana
```

```bash
read -p "Directorio: " directorio
mkdir -p "$directorio"
# te pide "Directorio: "; escribes p. ej. /tmp/demo_mkdir y lo crea
```

## Buenas prácticas

```bash
#!/bin/bash
directorio="$1"
# usa comillas al expandir rutas:
mkdir -p "$directorio"
ls -la "$directorio"
```

```bash
bash buenas.sh "/tmp/carpeta con espacios"
# crea esa carpeta (si no existía) y lista su contenido
```

Sin comillas, un espacio en el nombre parte el argumento en trozos.

## Errores frecuentes

```bash
nombre = Paco
# bash: nombre: command not found
```

```bash
echo $ficheros_con_espacios
# se rompe; usa "$ficheros_con_espacios"
```

- Confundir `$nombre` (valor) con `nombre` (texto literal).

## Siguiente

[Argumentos del script (`$1`, `$2`, `$@`…)](argumentos.md)

## Relacionados

- [argumentos](argumentos.md) — variables especiales `$1`, `$#`…  
- [primer-script](primer-script.md)



