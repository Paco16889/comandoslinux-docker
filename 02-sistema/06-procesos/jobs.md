# jobs

Lista los **trabajos** (jobs) de la shell actual: procesos que lanzaste en **esta terminal** y que la shell aún controla.

No muestra todos los procesos del sistema (eso es `ps`/`top`). Solo “los tuyos de esta sesión”.

## Sintaxis

```bash
jobs [opciones]
```

> Recuerda: `[ ]` = opcional.  
> Detalle: [Cómo leer la sintaxis](../../README.md#cómo-leer-la-sintaxis-muy-importante).

## Conceptos en lenguaje sencillo

| Idea | Significado |
|------|-------------|
| Job | Un trabajo gestionado por tu shell |
| `%1`, `%2`… | Número de job (no es el PID) |
| Foreground | En primer plano (ocupa la terminal) |
| Background | En segundo plano (te deja escribir) |
| Stopped | Pausado (p. ej. con `Ctrl+Z`) |

## Ejemplos — crear trabajos para verlos

Lanzar en segundo plano con `&`:

```bash
sleep 300 &
# el comando sleep “duerme” 300 segundos en background

jobs
# [1]+  Running                 sleep 300 &
```

Pausar un comando en primer plano:

```bash
sleep 300
# mientras corre, pulsa: Ctrl+Z
# verás algo como: Stopped

jobs
# [1]+  Stopped                 sleep 300
```

Más de uno:

```bash
sleep 200 &
sleep 400 &
jobs
# [1]-  Running    sleep 200 &
# [2]+  Running    sleep 400 &
```

El `+` marca el job “actual” (el que usan `fg`/`bg` por defecto).  
El `-` es el anterior.

## Ver PID de un job

```bash
jobs -l
# muestra también el PID

jobs -p
# solo PIDs
```

## Referirse a un job

```bash
kill %1
# manda TERM al job 1

kill -9 %2
# fuerza el fin del job 2

fg %1
bg %1
# ver [bg-fg](bg-fg.md)
```

## Opciones útiles

| Opción | Qué hace |
|--------|----------|
| `-l` | Incluye PID |
| `-p` | Solo PIDs |
| `-r` | Solo los que están Running |
| `-s` | Solo los Stopped |

```bash
jobs -r
jobs -s
jobs -l
```

## Errores frecuentes

- Abrir otra terminal y hacer `jobs`: sale vacío. Los jobs son **por shell/sesión**.
- Confundir `%1` con PID `1`: el job `%1` no es el proceso init; es “tu trabajo número 1”.
- Cerrar la terminal con jobs en background: según el caso pueden recibir SIGHUP y morir (ver [nohup](nohup.md)).

## Siguiente

[bg y fg](bg-fg.md)

## Relacionados

- [bg-fg](bg-fg.md) — reanudar en background o foreground
- [kill](kill.md) — `kill %1` o `kill <PID>`
- [nohup](nohup.md) — sobrevivir al cierre de la terminal



