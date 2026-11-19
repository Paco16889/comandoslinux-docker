# Ejercicios — Red y paquetes

Tras estudiar [03-red-paquetes](../03-red-paquetes/).

> **Cómo leer la sintaxis:** `<algo>` = lo sustituyes tú · `[algo]` = opcional.  
> Detalle: [Cómo leer la sintaxis](../README.md#cómo-leer-la-sintaxis-muy-importante).

## Antes de empezar

Hace falta red. Si un comando no está instalado, instálalo con el gestor de **tu** distro (apt/dnf/pacman) como en el temario.

## Ejercicio 1 — ¿Tengo red?

### Objetivo

`ping`, `ip` / hostname.

### Tareas

1. Haz ping a `127.0.0.1` con solo **3** intentos.  
2. Haz ping a un DNS público (p. ej. `8.8.8.8`) con 3 intentos.  
3. Muestra tu hostname.  
4. Lista direcciones IP de tus interfaces (`ip`).

### Comprueba

Local responde. Si `8.8.8.8` falla pero local no, sospecha de salida a Internet / firewall — no de “Linux roto”.

## Ejercicio 2 — Puertos y escucha

### Objetivo

`ss` (o `netstat` si lo tienes).

### Tareas

1. Lista sockets en escucha TCP.  
2. Busca si tienes algo en el puerto **22** (SSH) o **80**/**443**.  
3. Anota: ¿hay servicio escuchando o no?

### Comprueba

Sabes leer una línea de `ss -tulpn` / similar (puerto y proceso si hay permisos).

## Ejercicio 3 — HTTP rápido

### Objetivo

`curl` o `wget`.

### Tareas

1. Con `curl -I` pide cabeceras de `https://example.com` (o un sitio que permita).  
2. Descarga una página o fichero pequeño a `~/practica-linux/descarga.html` (crea la carpeta si falta).

### Comprueba

Ves un código tipo `HTTP/2 200` (o 301/302) y existe el fichero descargado.

## Ejercicio 4 — Paquetes en tu distro

### Objetivo

Buscar e informar de un paquete (sin instalar basura a lo loco).

### Tareas

Según tu sistema:

| Distro | Haz esto |
|--------|----------|
| Debian/Ubuntu/Mint | `apt update` (si puedes) y busca el paquete `tree` |
| Fedora | busca `tree` con `dnf` |
| Arch | busca `tree` con `pacman` |

1. ¿Está instalado?  
2. Si no, **instálalo** (con sudo) y ejecuta `tree -L 1 ~` o `tree -L 1 ~/practica-linux`.  
3. En Mint: recuerda que apps gráficas grandes a menudo van por **Flatpak** (no hace falta instalar una ahora; solo responde cuándo usarías Flatpak vs apt).

### Comprueba

`tree` muestra un listado. Sabes qué gestor usaste.

## Siguiente

[Ejercicios — Automatización](04-automatizacion.md)

## Relacionados

- [03-red-paquetes](../03-red-paquetes/)  
- Solución: [soluciones/03-red-paquetes](soluciones/03-red-paquetes.md)  
- [README ejercicios](README.md)  
