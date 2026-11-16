# ss

Muestra sockets: conexiones de red y **puertos** en uso (*socket statistics*).

Es el reemplazo moderno de `netstat` (más rápido; suele venir en `iproute2`).

## Sintaxis

```bash
ss [opciones]
```

> Recuerda: `[ ]` = opcional.  
> Detalle: [Cómo leer la sintaxis](../../README.md#cómo-leer-la-sintaxis-muy-importante).

## Ideas previas

| Idea | Ejemplo |
|------|---------|
| Escuchar (LISTEN) | Un servicio espera clientes (ej. servidor web en `:80`) |
| Establecida (ESTAB) | Hay una conexión activa |
| Puerto local | Dónde “atiende” o desde dónde sales en **tu** máquina |

## Ejemplos

Todo lo habitual TCP/UDP con procesos:

```bash
ss -tulpn
# cabecera + filas Netid / State / Local Address:Port / Process
# muchas líneas LISTEN (servicios) y a veces otras
```

Qué significa esa combinación (mnemotecnia útil):

| Opción | Significado |
|--------|-------------|
| `-t` | TCP |
| `-u` | UDP |
| `-l` | Solo escuchando (listening) |
| `-p` | Mostrar proceso (a veces hace falta sudo para ver todos) |
| `-n` | Números (no traduzcas puertos a nombres) |

Solo TCP escuchando:

```bash
ss -tlnp
# menos ruido: solo TCP en estado LISTEN
# ejemplo de fila: LISTEN 0 128 0.0.0.0:22 … users:(("sshd",…
```

Solo conexiones establecidas:

```bash
ss -tp
# conexiones TCP con proceso; verás ESTAB hacia webs, SSH, etc.

ss -tan state established
# mismo filtro por estado; -n deja IPs/puertos en números
```

Filtrar por puerto:

```bash
ss -tlnp | grep <puerto>
ss -tlnp | grep 22
# ¿hay algo escuchando en SSH (22)?
# si sí → una línea LISTEN … :22 …
# si no → no sale nada (o solo la cabecera no, porque grep la filtra)

ss -tlnp | grep 80
# típico si tienes un servidor web local escuchando en 80
```

Resumen numérico:

```bash
ss -s
# estadísticas globales
```

```text
Total: 450
TCP:   120 (estab 15, closed 80, orphaned 0, …)
Transport Total IP IPv6
RAW       1     0   1
UDP       20    16  4
TCP       40    30 10
…
```

Con sudo (más nombres de procesos):

```bash
sudo ss -tulpn
# igual que ss -tulpn, pero a menudo ves más nombres en la columna Process
```

## Ejemplo de línea (idea)

```text
LISTEN 0 128 0.0.0.0:22 0.0.0.0:* users:(("sshd",pid=1234,fd=3))
```

| Parte | Idea |
|-------|------|
| `LISTEN` | Esperando conexiones |
| `:22` | Puerto 22 (SSH) |
| `sshd` | Proceso que atiende |

## netstat (por si lo ves en tutoriales viejos)

```bash
netstat -tulpn
# similar en espíritu a ss -tulpn
# columnas clásicas: Proto, Local Address, State, PID/Program name
```

Si no está:

```bash
# Debian / Ubuntu / Linux Mint
sudo apt install net-tools

# Fedora
sudo dnf install net-tools

# Arch
sudo pacman -S net-tools
```

Para aprender hoy, prioriza `ss`.

## Errores frecuentes

- Sin `-p` no ves el programa; con `-p` a veces necesitas `sudo`.
- Confundir puerto remoto y local: mira las columnas `Local Address` y `Peer Address`.
- Pensar que “puerto cerrado” en `ss` significa lo mismo que en un escaneo externo: `ss` mira **tu** máquina.

## Siguiente

[curl](curl.md)

## Relacionados

- [ip](ip.md) — interfaces y rutas
- [ssh](ssh.md) — servicio típico en el puerto 22
- [ping](ping.md) — conectividad básica (no muestra puertos)



