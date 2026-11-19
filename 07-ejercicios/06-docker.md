# Ejercicios — Docker

Tras estudiar [06-docker](../06-docker/) (al menos intro + fundamentos; ideal volumes/compose).

> **Cómo leer la sintaxis:** `<algo>` = lo sustituyes tú · `[algo]` = opcional.  
> Detalle: [Cómo leer la sintaxis](../README.md#cómo-leer-la-sintaxis-muy-importante).

## Antes de empezar

```bash
docker version
# Client + Server OK
```

Si falla el socket: grupo `docker` o `sudo` ([instalacion-engine](../06-docker/00-introduccion/instalacion-engine.md)).

## Ejercicio 1 — Hola contenedor

### Objetivo

`pull` / `run` / `ps` / `rm`.

### Tareas

1. Ejecuta `hello-world` (con o sin `--rm`).  
2. Lista contenedores con `ps -a` y localiza el de hello-world si no usaste `--rm`.  
3. Límpialo si sigue ahí.

### Comprueba

Ves el mensaje *Hello from Docker!* y no dejas basura (o sabes borrarla).

## Ejercicio 2 — Nginx en el puerto 8080

### Objetivo

`run -d`, `-p`, `logs`, `stop`, `rm`.

### Tareas

1. Arranca `nginx:alpine` en segundo plano, nombre `ejer-web`, puerto host **8080** → 80.  
2. `curl -I http://127.0.0.1:8080`.  
3. Mira logs.  
4. Para y borra el contenedor.

### Comprueba

`HTTP/1.1 200` (o similar) y `docker ps` ya no muestra `ejer-web`.

## Ejercicio 3 — Volume que sobrevive

### Objetivo

Volume nombrado.

### Tareas

1. Crea volume `ejer-datos`.  
2. Monta en un contenedor Alpine en `/data`, escribe `hola > /data/nota.txt`, borra el contenedor.  
3. Arranca otro contenedor con el mismo volume y muestra la nota.

### Comprueba

Sigues leyendo `hola` tras borrar el primer contenedor.

## Ejercicio 4 — Compose mínimo

### Objetivo

`docker compose up/down`.

### Tareas

1. Carpeta `~/practica-linux/compose-ejer` con `compose.yaml` de un servicio `web` (`nginx:alpine`, `8080:80`).  
2. `docker compose up -d`, prueba curl, `docker compose down`.

### Comprueba

Up responde en 8080; down limpia contenedor/red del proyecto.

## Ejercicio 5 — (Opcional) Contexto Desktop

### Objetivo

Si tienes Desktop: `docker context ls` y saber si estás en `default` o `desktop-linux`.

### Comprueba

Sabes cambiar o interpretar el `*` del contexto activo.

## Siguiente

Fin de los ejercicios. Índice: [README ejercicios](README.md) · [README raíz](../README.md)

## Relacionados

- [06-docker](../06-docker/)  
- Solución: [soluciones/06-docker](soluciones/06-docker.md)  
- [README ejercicios](README.md)  
