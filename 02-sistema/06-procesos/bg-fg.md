# bg y fg

Controlan si un job de **esta terminal** corre en segundo plano (`bg`) o en primer plano (`fg`).

Suelen usarse junto con `Ctrl+Z` y `jobs`.

## Sintaxis

```bash
bg [%<número_job>]
fg [%<número_job>]
```

> Recuerda: `[ ]` = opcional. Si no pones job, usan el job actual (el marcado con `+` en `jobs`).  
> Detalle: [Cómo leer la sintaxis](../../README.md#cómo-leer-la-sintaxis-muy-importante).

## Atajos de teclado relacionados

| Teclas | Qué hacen |
|--------|-----------|
| `Ctrl+Z` | Pausa el proceso en primer plano (Stopped) |
| `Ctrl+C` | Intenta terminar el de primer plano (SIGINT) |
| `&` al final del comando | Lo lanzas ya en segundo plano |

## Ejemplos guiados

### 1) Pausar y seguir en segundo plano

```bash
ping 8.8.8.8
# Ctrl+Z   → se pausa

jobs
# [1]+  Stopped   ping 8.8.8.8

bg
# o: bg %1
# sigue pingueando en background

jobs
# [1]+  Running   ping 8.8.8.8 &
```

Para cortarlo después:

```bash
kill %1
# o Ctrl+C si lo traes antes a primer plano con fg
```

### 2) Traer a primer plano

```bash
sleep 300 &
jobs
fg
# o: fg %1
# ahora sleep ocupa la terminal otra vez
# Ctrl+C para cancelarlo, o espera a que termine
```

### 3) Varios jobs

```bash
sleep 100 &
sleep 200 &
jobs
# [1]-  Running    sleep 100 &
# [2]+  Running    sleep 200 &

fg %1
# trae el primero

# Ctrl+Z
bg %1
# lo devuelve a background
```

### 4) Lanzar directo en background

```bash
./<fichero>.sh &
jobs
# no hace falta Ctrl+Z ni bg si ya usaste &
```

## Resumen del ciclo más común

```text
comando            → corre en primer plano
Ctrl+Z             → Stopped
bg                 → Running en segundo plano
fg                 → vuelve a primer plano
kill %1 / Ctrl+C   → terminar
```

## Errores frecuentes

```text
bg: no current job
fg: no current job
```

- No hay jobs en esta shell. Mira `jobs`. Quizá el proceso es de otra terminal (`ps`/`kill`).

```text
bash: bg: job has terminated
```

- Ese job ya acabó.

- Hacer `fg` de un programa ruidoso (`ping`, `tail -f`) y no saber salir: `Ctrl+C` (terminar) o `Ctrl+Z` (pausar).

## Relacionados

- [jobs](jobs.md) — ver números `%1`, `%2`…
- [nohup](nohup.md) — si necesitas que siga tras cerrar la terminal
- [kill](kill.md) — terminar el job
