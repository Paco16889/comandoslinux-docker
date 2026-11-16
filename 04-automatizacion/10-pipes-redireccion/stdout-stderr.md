# Salida estándar y error estándar

Casi todos los comandos pueden escribir en **dos canales**:

| Canal | Número | Uso típico |
|-------|--------|------------|
| stdout | `1` | Resultado “bueno” / datos |
| stderr | `2` | Errores y avisos |

Por defecto **ambos** van a la pantalla, por eso a veces parece “todo lo mismo”.

> Recuerda: `<fichero>` y `<ruta>` los sustituyes tú (sin `<` `>`).  
> Detalle: [Cómo leer la sintaxis](../../README.md#cómo-leer-la-sintaxis-muy-importante).

## Demostración: se ven igual… hasta que rediriges

```bash
ls /tmp
# listado → stdout (pantalla)

ls /ruta/que/no/existe
# mensaje de error → stderr (también pantalla)
```

Ahora separarlos:

```bash
ls /tmp > <fichero_ok>.txt
# el listado va al fichero; la pantalla queda limpia (stdout redirigido)

cat <fichero_ok>.txt
```

```bash
ls /no/existe > <fichero_vacio>.txt
# el error SIGUE en pantalla (stderr no se movió)
# <fichero_vacio>.txt puede quedar vacío

wc -l <fichero_vacio>.txt
# 0   → no hubo stdout que guardar
```

## Ejemplos — matriz para entenderlo

```bash
# Solo stdout a fichero
ls /tmp /no/existe > solo_out.txt
# ves el error en pantalla; solo_out.txt tiene (como mucho) lo de /tmp

# Solo stderr a fichero
ls /tmp /no/existe 2> solo_err.txt
# ves el listado de /tmp; solo_err.txt guarda el error

# Cada uno a su sitio
ls /tmp /no/existe > out.txt 2> err.txt
cat out.txt
cat err.txt

# Todo junto (stdout + stderr)
ls /tmp /no/existe > todo.txt 2>&1
# o en Bash:
ls /tmp /no/existe &> todo.txt
cat todo.txt
```

Comprobar con contadores:

```bash
ls /tmp > a.txt 2> b.txt
wc -l a.txt b.txt
# a.txt tiene líneas; b.txt suele tener 0

ls /no/existe > a.txt 2> b.txt
wc -l a.txt b.txt
# a.txt ~0; b.txt tiene el error
```

## Con pipes (detalle importante)

El pipe `|` solo lleva **stdout** al siguiente comando. stderr sigue a pantalla.

```bash
ls /tmp /no/existe | wc -l
# ves el error en pantalla
# wc solo cuenta las líneas del listado de /tmp
```

Para meter también los errores en el pipe:

```bash
ls /tmp /no/existe 2>&1 | wc -l
# ahora wc cuenta listado + línea(s) de error
```

## Mini laboratorio (hazlo en orden)

```bash
mkdir -p <directorio>
cd <directorio>

echo "hola" > ok.txt
ls ok.txt no_existe.txt > salida.txt 2> errores.txt

echo "=== SALIDA ==="
cat salida.txt
echo "=== ERRORES ==="
cat errores.txt
```

Script de log separado (idea):

```bash
#!/bin/bash
# guardar_logs.sh
ls "$1" > "$2.out" 2> "$2.err"
echo "stdout → $2.out"
echo "stderr → $2.err"
```

```bash
bash guardar_logs.sh /tmp prueba
cat prueba.out
cat prueba.err
```

## Idea clave (chuleta)

```text
comando > fichero      → mueve canal 1 (stdout)
comando 2> fichero     → mueve canal 2 (stderr)
comando > f 2>&1       → ambos al mismo fichero
comando 2>&1 | otro    → ambos entran al pipe
```

Lo practicamos más en [redireccion](redireccion.md) y [combinar-errores](combinar-errores.md).

## Errores frecuentes

- Creer que `> fichero` guarda también los errores: **no**.  
- Ver un error en pantalla y pensar que el pipe lo “comió”: el pipe no lleva stderr por defecto.  
- Confundir el número `2` con “dos ficheros”: es el **canal** stderr.

## Relacionados

- [redireccion](redireccion.md)  
- [combinar-errores](combinar-errores.md)  
- [pipes](pipes.md)  
