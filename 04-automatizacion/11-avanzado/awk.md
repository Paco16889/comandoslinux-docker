# awk (introducción)

`awk` trabaja por **líneas y campos** (columnas). Ideal para sacar la columna 1, sumar, filtrar…

## Sintaxis

```bash
awk [opciones] '<programa>' [<fichero>...]
```

> Recuerda: `[ ]` = opcional · el programa awk suele ir entre **comillas simples**.  
> Detalle: [Cómo leer la sintaxis](../../README.md#cómo-leer-la-sintaxis-muy-importante).

Por defecto los campos se separan por espacios/tabs: `$1`, `$2`, … `$0` = línea entera.

## Ejemplos esenciales

```bash
echo "ana 25 madrid" | awk '{ print $1 }'
# ana

echo "ana 25 madrid" | awk '{ print $1, $3 }'
# ana madrid

echo "ana 25 madrid" | awk '{ print NF }'
# 3   → número de campos de esa línea

awk '{ print $1 }' <fichero>
awk '{ print NR, $0 }' <fichero>
# NR = número de línea
```

Separador personalizado (como `cut -d`):

```bash
awk -F: '{ print $1 }' /etc/passwd
# usuarios

awk -F: '{ print $1, $7 }' /etc/passwd
# usuario y shell

awk -F',' '{ print $2 }' <fichero.csv>
```

Separador de salida:

```bash
awk -F: -v OFS='\t' '{ print $1, $7 }' /etc/passwd | head
# columnas separadas por tabulador
```

## Filtrar y calcular

```bash
awk -F: '$3 >= 1000 { print $1 }' /etc/passwd
# UIDs de usuario “normales” (el umbral depende de la distro)

awk -F: '$7 ~ /bash/ { print $1 }' /etc/passwd
# usuarios cuyo shell contiene "bash"
```

Sumar una columna (fichero con números en el campo 2):

```bash
awk '{ s += $2 } END { print s }' <fichero>
```

Contar líneas:

```bash
awk 'END { print NR }' <fichero>
```

BEGIN / END:

```bash
awk 'BEGIN { print "Inicio" } { print $1 } END { print "Fin:", NR, "líneas" }' <fichero>
```

## Con pipes

```bash
ps aux | awk '{ print $1, $2, $11 }' | less
# USER PID COMMAND (aprox.; COMMAND puede partirse)

ps aux | awk 'NR==1 || $1=="<usuario>"' | head
# cabecera + procesos de un usuario

df -h | awk 'NR==1 || /\/$/'
# cabecera y línea del sistema de ficheros raíz (orientativo)

free -h | awk 'NR==1 || /^Mem:/'
```

## Laboratorio corto

```bash
printf "manzanas 3\nperas 5\nuvas 2\n" > /tmp/frutas.txt
awk '{ print $1 }' /tmp/frutas.txt
awk '{ s+=$2 } END { print "Total:", s }' /tmp/frutas.txt
awk '$2 >= 3 { print $0 }' /tmp/frutas.txt
```

## awk vs cut vs grep (cuándo)

| Necesidad | Empieza por |
|-----------|-------------|
| Buscar texto | `grep` |
| Cortar columnas simples | `cut` o `awk` |
| Condiciones / cálculos | `awk` |

## Errores frecuentes

- Olvidar las comillas simples del programa awk: la shell te rompe `$1`.  
- Pensar que `$1` de awk es el `$1` del script Bash: viven en mundos distintos.  
- Columnas de `ps`/`ls` “bailar” — a veces hace falta otro enfoque.

## Siguiente

[alias](alias.md)

## Relacionados

- [cut](../../01-fundamentos/03-lectura-texto/cut.md)  
- [sed](sed.md)  
- [pipes](../10-pipes-redireccion/pipes.md)



