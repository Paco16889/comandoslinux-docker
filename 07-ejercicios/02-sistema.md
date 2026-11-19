# Ejercicios — Sistema

Tras estudiar [02-sistema](../02-sistema/) (usuarios/sesiones y procesos).

> **Cómo leer la sintaxis:** `<algo>` = lo sustituyes tú · `[algo]` = opcional.  
> Detalle: [Cómo leer la sintaxis](../README.md#cómo-leer-la-sintaxis-muy-importante).

## Antes de empezar

Algunas tareas de usuarios requieren `sudo`. Si no tienes permisos de administrador, haz solo lo marcado **(sin sudo)** y lee el resto.

## Ejercicio 1 — ¿Quién hay en el sistema? (sin sudo)

### Objetivo

`whoami`, `id`, `who` / `w`.

### Tareas

1. Muestra tu usuario actual.  
2. Muestra tu UID/GID y grupos (`id`).  
3. Lista sesiones / quién está conectado (`who` o `w`).

### Comprueba

Reconoces tu usuario en la salida de `id` y entiendes UID ≠ “nombre bonito”.

## Ejercicio 2 — Procesos: mirar y filtrar (sin sudo)

### Objetivo

`ps`, `grep`, idea de PID.

### Tareas

1. Lista tus procesos (`ps` con opciones que ya viste en el temario).  
2. Filtra algo conocido (p. ej. la propia `bash` o `gnome-terminal` / lo que uses).  
3. Anota un **PID** de ejemplo (no mates nada todavía).

### Comprueba

Sabes señalar la columna PID y el nombre del comando.

### Pista

`ps aux | grep …` (y recuerda que `grep` puede aparecer en el resultado).

## Ejercicio 3 — Un proceso de prueba

### Objetivo

Crear un proceso largo, verlo y terminarlo con `kill`.

### Tareas

1. En una terminal: `sleep 300 &` (o `sleep 300` en otra pestaña).  
2. Encuentra el PID de ese `sleep`.  
3. Termínalo con `kill <PID>`.  
4. Comprueba que ya no está.

### Comprueba

```bash
ps aux | grep '[s]leep'
# no debería quedar tu sleep 300
```

## Ejercicio 4 — sudo con cabeza (con sudo)

### Objetivo

Entender `sudo` sin destrozar el sistema.

### Tareas

1. Ejecuta un comando inofensivo con sudo, p. ej. `sudo whoami`.  
2. Explica por qué sale `root`.  
3. **No** crees ni borres usuarios reales en una máquina compartida sin permiso; si es tu PC de práctica y quieres, crea un usuario de prueba y bórralo siguiendo el temario (`useradd` / `userdel`) — opcional.

### Comprueba

`sudo whoami` → `root`. Entiendes que sudo no “es un usuario”, es “ejecutar como”.

## Siguiente

[Ejercicios — Red y paquetes](03-red-paquetes.md)

## Relacionados

- [02-sistema](../02-sistema/)  
- Solución: [soluciones/02-sistema](soluciones/02-sistema.md)  
- [README ejercicios](README.md)  
