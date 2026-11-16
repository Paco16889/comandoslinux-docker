# Redirección (`>`, `>>`, `<`)

## Sintaxis

```bash
<comando> > <fichero>
<comando> >> <fichero>
<comando> < <fichero>
```

> Recuerda: `<comando>` y `<fichero>` los sustituyes tú.  
> Los símbolos `>` `>>` `<` **sí se escriben**.  
> Detalle: [Cómo leer la sintaxis](../../README.md#cómo-leer-la-sintaxis-muy-importante).

## `>` — crear / sobrescribir

```bash
echo "Hola" > <fichero>
# crea <fichero> o lo pisa por completo
# en pantalla: nada (la salida fue al fichero)
# cat <fichero> → Hola

ls -la > listado.txt
# en pantalla: nada; el listado queda en listado.txt
# cat listado.txt → mismas líneas que habría mostrado ls -la

date > fecha.txt
# cat fecha.txt → algo como: lun 16 nov 2026 23:20:01 CET
```

¡Cuidado! Es fácil borrar contenido sin querer:

```bash
echo "nuevo" > notas.txt
# si notas.txt tenía texto, se perdió
# cat notas.txt → solo: nuevo
```

## `>>` — añadir al final

```bash
echo "línea 1" > <fichero>
echo "línea 2" >> <fichero>
echo "línea 3" >> <fichero>
cat <fichero>
```

```text
línea 1
línea 2
línea 3
```

Log simple:

```bash
echo "$(date +%F\ %T) arranque" >> <fichero_log>.log
# añade una línea; no pisa el log anterior
# cat <fichero_log>.log → 2026-11-16 23:20:01 arranque  (fecha/hora de ejemplo)
```

## `<` — usar un fichero como entrada

```bash
sort < <fichero>
# ordena el contenido (equivalente práctico a sort <fichero> en muchos casos)
# en pantalla: las líneas de <fichero>, ya ordenadas

wc -l < <fichero>
# ejemplo si el fichero tiene 3 líneas: 3
```

## `/dev/null` — tirar la salida

```bash
comando > /dev/null
# descarta stdout
# en pantalla: nada de la salida “normal” (los errores sí pueden seguir saliendo)

ping -c 1 8.8.8.8 > /dev/null
# no ves las líneas “64 bytes from…”; solo vuelve el prompt (o un error si falló)
```

## Ejemplos combinados con lo que ya sabes

```bash
apt search htop > resultados.txt
# (sin sudo; puede ser largo)
# en pantalla: nada o poco; el texto largo está en resultados.txt

ps aux > procesos.txt
head -n 20 procesos.txt
# head muestra las 20 primeras líneas guardadas (USER, PID, COMMAND…)

grep "<texto>" <fichero> > coincidencias.txt
# en pantalla: nada si hubo coincidencias (fueron al fichero)
# cat coincidencias.txt → solo las líneas que coinciden
```

## Errores frecuentes

- Confundir `>` con `>>` y pisar un fichero importante.  
- Pensar que `>` guarda también los errores: no; solo stdout (ver [combinar-errores](combinar-errores.md)).  
- Redirigir y luego no encontrar el fichero: mira en el directorio actual (`pwd` / `ls`).

## Siguiente

[Pipes (`|`)](pipes.md)

## Relacionados

- [stdout-stderr](stdout-stderr.md)  
- [pipes](pipes.md)  
- [echo](../../01-fundamentos/01-basicos/echo.md)



