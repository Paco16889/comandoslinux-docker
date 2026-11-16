# nohup

Ejecuta un comando de forma que **no se corte** al colgar la terminal (ignora la señal SIGHUP).

Útil cuando lanzas algo largo por SSH y quieres cerrar la sesión sin matarlo.

## Sintaxis

```bash
nohup <comando> [argumentos]...
```

> Recuerda: `<comando>` lo sustituyes tú; `[argumentos]` son opcionales.  
> Detalle: [Cómo leer la sintaxis](../../README.md#cómo-leer-la-sintaxis-muy-importante).

## Idea simple

Sin `nohup`, al cerrar la terminal muchos procesos en background reciben “hang up” y mueren.  
Con `nohup`, les dices: “si cierra la terminal, tú sigue”.

## Ejemplos

```bash
nohup ./<fichero>.sh &
# lanza el script en background y resistente al cierre

nohup python3 <fichero>.py &
nohup bash -c "largo-proceso" &
```

Por defecto, la salida se guarda en `nohup.out` del directorio actual:

```bash
nohup ./<fichero>.sh &
# aviso típico: output appended to 'nohup.out'

tail -f nohup.out
# ver qué está imprimiendo
```

Elegir tú el fichero de salida:

```bash
nohup ./<fichero>.sh > <fichero_log>.log 2>&1 &
# salida normal y errores van a <fichero_log>.log
# (2>&1 se explica a fondo en 10-pipes-redireccion)
```

Descartar la salida:

```bash
nohup ./<fichero>.sh > /dev/null 2>&1 &
# silencioso; /dev/null es el “agujero negro” del sistema
```

Comprobar que sigue:

```bash
jobs -l
# a veces ya no aparece en jobs si se desliga; entonces:

ps aux | grep <fichero>
pgrep -a <patrón>
```

## Alternativas modernas (para que sepas que existen)

| Herramienta | Idea |
|-------------|------|
| `nohup ... &` | Clásica, simple |
| `tmux` / `screen` | Sesiones que puedes volver a abrir |
| `systemd` user services | Más formal en sistemas modernos |

Para esta guía, `nohup` es el primer escalón.

## Errores frecuentes

```text
nohup: ignoring input and appending output to 'nohup.out'
```

- No es un error: es un aviso normal.

- Olvidar el `&`: `nohup` sigue en primer plano y no te libera la terminal (aunque ya ignore SIGHUP).
- Llenar el disco con un `nohup.out` enorme: redirige a un log concreto o a `/dev/null` si no te importa la salida.
- Cerrar la laptop / suspender: `nohup` no evita que el hardware se duerma; solo evita el SIGHUP al cerrar la terminal.

## Relacionados

- [jobs](jobs.md) / [bg-fg](bg-fg.md) — control en la sesión actual
- [ps](ps.md) — verificar el proceso tras cerrar y abrir otra terminal
- Más adelante: [09-scripts](../../04-automatizacion/09-scripts/) y redirecciones
