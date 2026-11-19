# Ejercicios — Prólogo

Tras leer [00-prologo](../00-prologo/).  
Aquí no hace falta “machacar” comandos: se trata de **orientarte** en Linux.

> **Cómo leer la sintaxis:** `<algo>` = lo sustituyes tú · `[algo]` = opcional.  
> Detalle: [Cómo leer la sintaxis](../README.md#cómo-leer-la-sintaxis-muy-importante).

## Antes de empezar

| Necesitas | Notas |
|-----------|--------|
| Una distro Linux | Mint, Ubuntu, Debian, Fedora, Arch… |
| Terminal | Abrir una ventana de terminal |
| Temario | [00-prologo](../00-prologo/) |

## Ejercicio 1 — ¿Dónde estoy?

### Objetivo

Identificar usuario, home y distro.

### Tareas

1. Abre la terminal.  
2. Escribe (y ejecuta) lo que creas que muestra **quién eres**.  
3. Averigua tu carpeta personal (home).  
4. Mira `/etc/os-release` y anota `ID=` y el nombre bonito de la distro.

### Comprueba

- Sabes tu nombre de usuario.  
- Sabes una ruta tipo `/home/<usuario>`.  
- Sabes si estás en Debian/Ubuntu/Mint/Fedora/Arch (o derivado).

### Pista

`whoami`, `echo $HOME`, `cat /etc/os-release`.

## Ejercicio 2 — El árbol mental

### Objetivo

Relacionar rutas del sistema con su función (FHS).

### Tareas

Di, con tus palabras, para qué sirve cada una (sin memorizar de memoria; puedes mirar el temario):

| Ruta | ¿Para qué? |
|------|------------|
| `/` | |
| `/home` | |
| `/etc` | |
| `/var/log` | |
| `/tmp` | |

### Comprueba

Puedes explicar cada fila en una frase sin confundir `/etc` con `/home`.

### Pista

[arbol-de-directorios](../00-prologo/arbol-de-directorios.md).

## Ejercicio 3 — Permisos: la idea

### Objetivo

Entender *por qué* existen usuarios y permisos (aún sin `chmod`).

### Tareas

1. Responde: ¿qué pasaría si **todos** pudieran borrar `/etc`?  
2. Diferencia en una frase: **usuario normal** vs **root** / `sudo`.

### Comprueba

Tu respuesta habla de seguridad / no romper el sistema, no de “está feo”.

## Siguiente

[Ejercicios — Fundamentos](01-fundamentos.md)

## Relacionados

- [00-prologo](../00-prologo/)  
- Solución: [soluciones/00-prologo](soluciones/00-prologo.md)  
- [README ejercicios](README.md)  
