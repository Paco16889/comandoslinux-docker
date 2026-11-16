# free y uptime

No gestionan procesos directamente, pero ayudan a **entender el estado de la máquina** mientras miras CPU/procesos.

## free — memoria

Muestra memoria RAM y swap.

### Sintaxis

```bash
free [opciones]
```

> Recuerda: `[ ]` = opcional.  
> Detalle: [Cómo leer la sintaxis](../../README.md#cómo-leer-la-sintaxis-muy-importante).

### Ejemplos

```bash
free
# en kibibytes (o unidades del sistema)

free -h
# human: Gi, Mi… más legible (recomendado al empezar)

free -m
# en mebibytes

free -h -s <número>
# refresca cada <número> segundos (Ctrl+C para salir)

free -h -s 2
```

### Cómo leerlo (versión simple)

```text
              total        used        free      shared  buff/cache   available
Mem:
Swap:
```

| Campo | Idea |
|-------|------|
| `total` | Memoria instalada |
| `used` | En uso |
| `free` | Libre “del todo” (a menudo poca: Linux usa RAM para caché) |
| `buff/cache` | Caché de disco; se puede liberar si hace falta |
| `available` | Estimación de lo que puedes usar sin problemas (**mírala con preferencia**) |
| `Swap` | Disco usado como memoria extra (si crece mucho, el sistema va justo de RAM) |

```bash
free -h
# fíjate sobre todo en available y en si Swap used está alto
```

## uptime — tiempo encendido y carga

### Sintaxis

```bash
uptime
```

### Ejemplos

```bash
uptime
# 18:55:01 up 3 days,  4:12,  2 users,  load average: 0.15, 0.10, 0.05
```

Qué significa:

| Parte | Significado |
|-------|-------------|
| `up ...` | Cuánto tiempo lleva el sistema encendido |
| `users` | Sesiones de usuario |
| `load average` | Cola de trabajo a 1, 5 y 15 minutos |

Sobre el *load average* (idea inicial):

- En una máquina de **1 CPU**, un load de `1.00` ≈ un núcleo a tope de trabajo pendiente.
- En una de **4 CPUs**, un load de `4.00` ≈ saturación similar.
- Valores puntuales altos no siempre son un drama; mira la tendencia 1 / 5 / 15.

Otras formas:

```bash
uptime -p
# “up 3 days, 4 hours, 12 minutes” (más legible; según versión)

uptime -s
# desde cuándo está encendido (fecha/hora de arranque)
```

Relacionado con lo que ya viste:

```bash
w
# también muestra load average arriba

top
# load average en la cabecera
```

## Mini flujo de diagnosis (principiante)

```bash
uptime
# ¿carga alta?

free -h
# ¿memoria justa / mucha swap?

ps aux --sort=-%cpu | head
# procesos que más CPU usan (GNU ps)

ps aux --sort=-%mem | head
# procesos que más memoria usan
```

## Errores frecuentes

- Ver poca columna `free` y pensar “no me queda RAM”: en Linux es normal; mira `available`.
- Comparar load average entre máquinas sin mirar cuántos núcleos tienen.
- Confundir `free` el comando con liberar memoria a la fuerza: este comando solo **informa**.

## Siguiente

[ping](../../03-red-paquetes/07-red/ping.md)

## Relacionados

- [top-htop](top-htop.md) — CPU/MEM en vivo
- [ps](ps.md) — quién consume recursos
- [who-w-users](../05-usuarios-sesiones/who-w-users.md) — `w` también enseña carga



