# pkill y killall

Terminan procesos por **nombre**, sin tener que copiar el PID a mano.

Útiles, pero con cuidado: un nombre demasiado corto puede afectar a más procesos de los que pensabas.

## Sintaxis

```bash
pkill [opciones] <patrón>
killall [opciones] <nombre>
```

> Recuerda: `[ ]` = opcional · `<patrón>` / `<nombre>` = lo sustituyes tú.  
> Detalle: [Cómo leer la sintaxis](../../README.md#cómo-leer-la-sintaxis-muy-importante).

## Antes de matar: listar

Con `pkill`/`pgrep`:

```bash
pgrep <patrón>
# muestra PIDs que coinciden

pgrep -a <patrón>
# PIDs + línea de comando (muy recomendable antes de matar)

pgrep -u <usuario> <patrón>
# solo de ese usuario
```

```bash
ps aux | grep <patrón>
# otra forma de revisar
```

## pkill — ejemplos

```bash
pkill <patrón>
# manda TERM a los procesos cuyo nombre coincide

pkill firefox
pkill -u <usuario> python
```

Forzar:

```bash
pkill -9 <patrón>
pkill -KILL <patrón>
```

Coincidencia más estricta:

```bash
pkill -x <nombre>
# nombre exacto del proceso (no un trozo)

pkill -f <patrón>
# busca en la línea de comando completa (útil pero peligroso si el patrón es vago)
```

Señal concreta:

```bash
pkill -HUP <patrón>
pkill -signal TERM <patrón>
```

## killall — ejemplos

```bash
killall <nombre>
# termina procesos con ese nombre de comando

killall firefox
killall -u <usuario> <nombre>
```

Forzar e interactivo:

```bash
killall -9 <nombre>
killall -i <nombre>
# pregunta confirmación por cada proceso (más seguro al aprender)
```

> En **Solaris** y algunos UNIX antiguos, `killall` significaba otra cosa (matar *todo*).  
> En Linux (con `psmisc`) es “matar por nombre”. Esta guía asume Linux.

## pkill vs killall (idea práctica)

| Comando | Encaje |
|---------|--------|
| `pkill` | Patrones, filtros (`-u`, `-f`, `-x`)… muy flexible |
| `killall` | Nombre de comando; opción `-i` cómoda para principiantes |

## Ejemplos de flujo seguro

```bash
pgrep -a sleep
# ¿qué voy a matar?

pkill sleep
# o, si quieres ir con cuidado:
killall -i sleep
```

```bash
pgrep -a -u <usuario> python
pkill -u <usuario> python
```

## Errores frecuentes

```text
pkill: no process found
killall: <nombre>: no process found
```

- No hay coincidencias (o el nombre no es el que creías: mira `pgrep -a` / `ps`).

- `pkill java` en un servidor con varios servicios Java: puede tumbar más de uno. Acota con `-u` o un patrón más largo, o usa PIDs concretos.
- `pkill -f .` o patrones absurdamente amplios: peligro alto.

## Relacionados

- [kill](kill.md) — por PID (más preciso)
- [ps](ps.md) — inspeccionar antes
- [jobs](jobs.md) — si el proceso lo lanzaste en *esta* terminal, a veces basta `kill %1`
