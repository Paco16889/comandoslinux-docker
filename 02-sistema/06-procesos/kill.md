# kill

Envía una **señal** a un proceso. Lo más habitual: pedirle que termine.

A pesar del nombre, `kill` no siempre “mata a la fuerza”: primero se suele pedir un cierre limpio.

## Sintaxis

```bash
kill [opciones] <PID>...
kill -<número_señal> <PID>
kill -s <nombre_señal> <PID>
```

> Recuerda: `[ ]` = opcional · `<PID>` = número del proceso (lo ves con `ps` o `top`).  
> No escribas `<` `>` ni `[` `]`.  
> Detalle: [Cómo leer la sintaxis](../../README.md#cómo-leer-la-sintaxis-muy-importante).

## Paso a paso (método seguro)

1. Localiza el proceso y su PID  
2. Intenta un cierre normal  
3. Si no hace caso, fuerza el cierre  

```bash
ps aux | grep <texto>
# anota el PID (columna PID)

kill <PID>
# señal por defecto: TERM (15) → “por favor, termina”

ps -p <PID>
# si ya no aparece, listo
```

Si sigue vivo:

```bash
kill -9 <PID>
# SIGKILL: termina sí o sí (el proceso no puede ignorarlo)
```

## Ejemplos

```bash
kill 1234
# pide a 1234 que termine (TERM)

kill -15 <PID>
# igual que el default (TERM)

kill -TERM <PID>
# misma idea, por nombre

kill -9 <PID>
kill -KILL <PID>
# forzar
```

Varios a la vez:

```bash
kill <PID1> <PID2> <PID3>
```

Señal de “hang up” (a veces se usa para recargar configs de demonios):

```bash
kill -HUP <PID>
# o: kill -1 <PID>
```

Pausar y continuar (menos habitual al principio):

```bash
kill -STOP <PID>
# pausa el proceso

kill -CONT <PID>
# lo reanuda
```

## Señales que conviene conocer

| Número | Nombre | Efecto habitual |
|--------|--------|------------------|
| `15` | `TERM` | Cierre limpio (default de `kill`) |
| `9` | `KILL` | Forzar fin (no se puede atrapar) |
| `1` | `HUP` | “Hang up”; a veces recarga configuración |
| `2` | `INT` | Como `Ctrl+C` |
| `19` | `STOP` | Pausar |
| `18` | `CONT` | Continuar |

Listar señales disponibles:

```bash
kill -l
```

## ¿De dónde sale el PID?

```bash
ps aux | grep firefox
pidof <comando>
# algunos sistemas: imprime PIDs de ese comando

pgrep <comando>
# PIDs que coinciden con el nombre
```

## Errores frecuentes

```text
kill: (<PID>): Operation not permitted
```

- No es tuyo: prueba `sudo kill <PID>` solo si sabes lo que haces.

```text
kill: (<PID>): No such process
```

- Ese PID ya no existe (el proceso terminó o te confundiste de número).

- Empezar siempre con `-9`: mejor `kill <PID>` primero; `-9` solo si no queda otra.
- Matar procesos del sistema al azar puede dejar la máquina inestable. Si no reconoces el comando, no lo mates.

## Relacionados

- [ps](ps.md) — encontrar el `<PID>`
- [pkill-killall](pkill-killall.md) — matar por nombre (sin copiar el PID a mano)
- [jobs](jobs.md) — procesos lanzados desde *esta* terminal
