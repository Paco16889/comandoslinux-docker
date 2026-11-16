# ps

Muestra una **foto** de los procesos en este momento (*process status*).

No se actualiza solo: cada vez que ejecutas `ps`, ves el estado de ese instante.
Si quieres una vista en vivo, usa [top-htop](top-htop.md).

## Sintaxis

```bash
ps [opciones]
```

> Recuerda: `[ ]` = opcional (no se escriben).  
> Detalle: [Cómo leer la sintaxis](../../README.md#cómo-leer-la-sintaxis-muy-importante).

## Ejemplos — empezar simple

```bash
ps
# procesos asociados a tu terminal actual (pocos)

ps -f
# mismo conjunto, con más columnas (usuario, padre, hora…)
```

Ver **casi todo** el sistema (muy usado):

```bash
ps aux
# a = todos los usuarios (con terminal o no, según convención BSD)
# u = formato de usuario (CPU, MEM, etc.)
# x = incluye procesos sin terminal
```

Otra forma clásica (estilo System V):

```bash
ps -ef
# -e = todos los procesos
# -f = formato completo
```

`ps aux` y `ps -ef` se parecen en objetivo; verás una u otra en tutoriales. Ambas valen para empezar.

## Leer la salida de `ps aux`

Cabecera típica:

```text
USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
```

| Columna | Qué significa (simple) |
|---------|-------------------------|
| `USER` | Quién lo lanzó |
| `PID` | Número del proceso (lo usarás con `kill`) |
| `%CPU` | Uso de procesador |
| `%MEM` | Uso de memoria |
| `TTY` | Terminal asociada (`?` = ninguna) |
| `STAT` | Estado (`R` running, `S` sleeping, `Z` zombie…) |
| `COMMAND` | Qué comando es |

## Ejemplos prácticos

Filtrar con `grep` (muy habitual):

```bash
ps aux | grep <texto>
# líneas que contienen <texto>

ps aux | grep firefox
ps aux | grep <usuario>
ps aux | grep python
```

> Nota: a veces aparece también el propio `grep` en la lista. No te preocupes;
> o ignóralo, o usa: `ps aux | grep <texto> | grep -v grep`

Solo tus procesos:

```bash
ps -u <usuario>
ps -u $(whoami)
# procesos de tu usuario
```

Ver un PID concreto:

```bash
ps -p <PID>
ps -fp <PID>
# más detalle de ese proceso
```

Árbol (padre → hijos), si está disponible:

```bash
ps auxf
# la 'f' (forest) dibuja jerarquía en muchas versiones

pstree
# comando aparte, más visual (puede hacer falta instalarlo)
```

Contar procesos:

```bash
ps aux | wc -l
# aproximación (incluye la cabecera)

ps -e --no-headers | wc -l
# número de procesos (GNU ps)
```

## Opciones útiles (mapa rápido)

| Forma | Idea |
|-------|------|
| `ps` | Solo tu sesión de terminal |
| `ps aux` | Vista amplia “tipo BSD” |
| `ps -ef` | Vista amplia “tipo System V” |
| `ps -u <usuario>` | De un usuario |
| `ps -p <PID>` | De un PID |
| `ps -f` | Más columnas |

## Errores frecuentes

- Buscar un proceso “en vivo” con solo `ps`: es una foto; si nace y muere rápido, puede no salir. Repite el comando o usa `top`.
- Confundir el **PID** con el nombre: para `kill` necesitas el número (o usa `pkill` con el nombre).
- Asustarse por cientos de líneas en `ps aux`: es normal en un sistema moderno.

## Siguiente

[top, htop y nvtop](top-htop.md)

## Relacionados

- [top-htop](top-htop.md) — lista que se actualiza sola
- [kill](kill.md) — terminar usando el `<PID>`
- [pkill-killall](pkill-killall.md) — terminar por nombre



