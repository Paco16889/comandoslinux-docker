# Ejercicios — Fundamentos

Tras estudiar [01-fundamentos](../01-fundamentos/) (básicos, ficheros, texto, permisos).

> **Cómo leer la sintaxis:** `<algo>` = lo sustituyes tú · `[algo]` = opcional.  
> Detalle: [Cómo leer la sintaxis](../README.md#cómo-leer-la-sintaxis-muy-importante).

## Antes de empezar

Trabaja en tu home (no en carpetas del sistema). Crea una zona de práctica:

```bash
mkdir -p ~/practica-linux
cd ~/practica-linux
pwd
```

## Ejercicio 1 — Mini árbol de carpetas

### Objetivo

Usar `mkdir`, `touch`, `ls`, `cd`, `pwd`.

### Tareas

1. Dentro de `~/practica-linux`, crea:

```text
proyecto/
├── docs/
├── datos/
└── README.txt
```

2. En `docs/` crea `notas.txt` con el texto: `hola fundamentos` (puedes usar `echo` o un editor).  
3. Lista en largo `proyecto/` y entra en `docs/`; comprueba `pwd`.

### Comprueba

```bash
find ~/practica-linux/proyecto -print
# debe mostrar las carpetas y los ficheros
cat ~/practica-linux/proyecto/docs/notas.txt
# hola fundamentos
```

### Pista

`mkdir -p`, `touch`, `echo "…" > fichero`.

## Ejercicio 2 — Copiar, mover, borrar con cuidado

### Objetivo

`cp`, `mv`, `rm` (sin tragedias).

### Tareas

1. Copia `notas.txt` a `datos/notas-copia.txt`.  
2. Renombra `datos/notas-copia.txt` → `datos/backup.txt`.  
3. Crea un fichero basura `datos/borrar-me.txt` y elimínalo.  
4. **No** uses `rm -rf /` ni rutas fuera de `practica-linux`.

### Comprueba

```bash
ls ~/practica-linux/proyecto/datos
# backup.txt  (y no borrar-me.txt)
```

## Ejercicio 3 — Texto: buscar y contar

### Objetivo

`cat`, `grep`, `wc`, redirección simple.

### Tareas

1. Crea `datos/lista.txt` con al menos 5 líneas (nombres, frutas, lo que sea); **una** línea debe contener la palabra `linux`.  
2. Muestra solo las líneas con `linux` (mayúsculas/minúsculas: prueba también `-i`).  
3. Cuenta cuántas líneas tiene el fichero.

### Comprueba

Sabes el número de líneas (`wc -l`) y ves al menos una coincidencia con `grep`.

## Ejercicio 4 — Permisos mínimos

### Objetivo

Leer `ls -l` y usar `chmod` de forma segura.

### Tareas

1. Crea `scripts/hola.sh` con:

```bash
#!/usr/bin/env bash
echo "hola"
```

2. Mira `ls -l scripts/hola.sh` e interpreta el modo (¿tiene `x`?).  
3. Dale permiso de ejecución **solo a tu usuario** (numérico o simbólico).  
4. Ejecútalo con `./scripts/hola.sh` desde la carpeta correcta (o con ruta).

### Comprueba

```bash
./hola.sh
# hola
# (estando en scripts/ o usando la ruta completa)
```

### Pista

`chmod u+x` o `chmod 700` / `755` según quieras.

## Siguiente

[Ejercicios — Sistema](02-sistema.md)

## Relacionados

- [01-fundamentos](../01-fundamentos/)  
- Solución: [soluciones/01-fundamentos](soluciones/01-fundamentos.md)  
- [README ejercicios](README.md)  
