# who, w y users

Comandos para ver **quién está conectado** y qué hace (con distinto nivel de detalle).

## who

Lista sesiones abiertas.

### Sintaxis

```bash
who [opciones]
```

### Ejemplos

```bash
who
# <usuario>  tty/pts    fecha    (host)

who -a
# más detalle (incluye idle, procesos init de sesión, etc. según sistema)

who -b
# cuándo se arrancó el sistema (boot)

who -r
# runlevel actual (sistemas que aún lo exponen)

who am i
# o: who mom likes
# muestra la sesión asociada a tu terminal actual
```

```bash
who | grep <usuario>
# ¿hay sesión de ese usuario?

who | wc -l
# cuántas sesiones listadas
```

## w

Quién está conectado y **qué comando** está ejecutando (resumen de carga del sistema arriba).

### Sintaxis

```bash
w [opciones] [<usuario>]
```

### Ejemplos

```bash
w
# cabecera con load average + tabla de usuarios

w <usuario>
# solo ese usuario

w -h
# sin cabecera

w -i
# muestra IP en lugar de hostname (según versión)
```

Interpretación rápida de la cabecera:

```text
load average: 0.10, 0.05, 0.01
# media de carga a 1, 5 y 15 minutos
```

## users

Lista corta de nombres de usuarios conectados (puede repetir si hay varias sesiones).

### Sintaxis

```bash
users
```

### Ejemplos

```bash
users
# <usuario1> <usuario2> ...

users | tr ' ' '\n' | sort | uniq
# usuarios únicos conectados
```

## Cuándo usar cada uno

| Comando | Para qué |
|---------|----------|
| `who` | Sesiones (tty, hora, origen) |
| `w` | Sesiones + carga + comando actual |
| `users` | Vista mínima de nombres |

## Errores frecuentes

- Ver tu usuario dos veces: es normal si tienes varias terminales/SSH.
- En entornos gráficos puede haber sesiones adicionales (`pts`, `tty`, seat…).

## Relacionados

- [whoami](whoami.md) — solo tu identidad
- [last](last.md) — historial, no solo los conectados ahora
- [exit-logout](exit-logout.md) — cerrar una sesión
