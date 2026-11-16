# sed (introducción)

`sed` = *stream editor*. Transforma texto línea a línea: sustituir, borrar, imprimir…

Al empezar, el 90 % de los usos es **sustituir**.

## Sintaxis

```bash
sed [opciones] '<script>' [<fichero>...]
```

> Recuerda: `[ ]` = opcional · `<patrón>` / `<fichero>` los sustituyes tú.  
> Detalle: [Cómo leer la sintaxis](../../README.md#cómo-leer-la-sintaxis-muy-importante).

## Sustituir (lo esencial)

```bash
sed 's/<patrón>/<reemplazo>/' <fichero>
# sustituye la PRIMERA coincidencia de cada línea

sed 's/<patrón>/<reemplazo>/g' <fichero>
# g = todas las coincidencias de cada línea
```

Ejemplos:

```bash
echo "hola hola" | sed 's/hola/adios/'
# adios hola

echo "hola hola" | sed 's/hola/adios/g'
# adios adios

sed 's/error/ERROR/g' <fichero>
```

## No modifica el fichero (por defecto)

`sed` imprime el resultado; el original queda igual.

```bash
sed 's/foo/bar/g' <fichero> > <fichero_nuevo>
```

Edición “in-place” (con cuidado):

```bash
sed -i 's/foo/bar/g' <fichero>
# modifica el fichero

sed -i.bak 's/foo/bar/g' <fichero>
# guarda copia de seguridad <fichero>.bak (GNU sed)
```

## Borrar líneas / imprimir

```bash
sed '3d' <fichero>
# quita la línea 3 (en la salida)

sed '/<patrón>/d' <fichero>
# quita líneas que contienen el patrón

sed -n '1,5p' <fichero>
# imprime solo líneas 1-5 (-n silencia la impresión automática)

sed -n '10,20p' <fichero>
# rango de líneas 10 a 20

sed -n '/<patrón>/p' <fichero>
# solo líneas que coinciden (parecido a grep)
```

## Laboratorio corto

```bash
printf "uno\ndos error\ntres\nerror final\n" > /tmp/demo_sed.txt

sed 's/error/ERROR/g' /tmp/demo_sed.txt
sed -n '1,2p' /tmp/demo_sed.txt
sed '/error/d' /tmp/demo_sed.txt
```

## Ejemplo práctico (con backup)

```bash
cp <fichero> <fichero>.bak
sed -i 's/^Puerto=22/Puerto=2222/' <fichero>
diff <fichero>.bak <fichero>
# si algo salió mal: mv <fichero>.bak <fichero>
```

## Errores frecuentes

- Olvidar que sin `-i` no se guarda.  
- Usar `/` dentro del patrón: cambia el separador → `sed 's|#include|# import|'`  
- Editar con `-i` sin backup en ficheros importantes.

## Relacionados

- [grep](../../01-fundamentos/03-lectura-texto/grep.md) — buscar  
- [awk](awk.md) — más lógica por campos  
- [pipes](../10-pipes-redireccion/pipes.md)  
