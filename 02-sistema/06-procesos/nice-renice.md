# nice y renice

Ajustan la **prioridad** de uso de CPU de un proceso.

En Linux, el valor “nice” va de `-20` (máxima prioridad) a `19` (mínima prioridad / más “amable” con los demás).

> No hace el programa más rápido por magia: solo influye cuando **varios** procesos compiten por la CPU.

## Sintaxis

```bash
nice [-n <número>] <comando> [argumentos]...
renice [-n] <número> -p <PID>
renice [-n] <número> -u <usuario>
```

> Recuerda: `[ ]` = opcional · `<número>`, `<comando>`, `<PID>` = los rellenas tú.  
> Detalle: [Cómo leer la sintaxis](../../README.md#cómo-leer-la-sintaxis-muy-importante).

## Idea en lenguaje cotidiano

| nice | Comportamiento |
|------|----------------|
| Bajo (p. ej. `-10`) | “Quiero más CPU si hay pelea” |
| 0 | Prioridad normal (default) |
| Alto (p. ej. `10` o `19`) | “Yo espero; que pasen los demás” |

Subir la prioridad (números negativos) suele requerir `sudo`.

## nice — al lanzar un comando

```bash
nice <comando>
# lanza con nice +10 en muchas distros (más “amable”)

nice -n 15 <comando>
# prioridad baja (buena para tareas pesadas de fondo)

nice -n 15 ./<fichero>.sh
nice -n 10 tar -czf <fichero>.tar.gz <directorio>/
```

Más prioridad (cuidado; admin):

```bash
sudo nice -n -5 <comando>
# más prioritario que lo normal
```

En segundo plano y “amable”:

```bash
nice -n 19 ./<fichero>.sh > <fichero_log>.log 2>&1 &
```

## renice — cambiar un proceso ya en marcha

```bash
ps aux | grep <texto>
# anota el PID

renice 10 -p <PID>
# o: renice -n 10 -p <PID>
# hace el proceso más amable
```

Varios PIDs / un usuario:

```bash
renice 15 -p <PID1> <PID2>
sudo renice 5 -u <usuario>
# todos los procesos de ese usuario (potente; úsalo con criterio)
```

Ver la columna NI:

```bash
ps -o pid,user,ni,comm -p <PID>
top
# columna NI = nice
```

## Ejemplos de cuándo usarlo

```bash
# Compresión grande sin ralentizar el escritorio:
nice -n 19 tar -czf copia.tar.gz <directorio>/

# Script de mantenimiento de madrugada:
nohup nice -n 15 ./<fichero>.sh &
```

## Errores frecuentes

```text
renice: failed to set priority for <PID>: Permission denied
```

- Bajar el nice (más prioridad) o tocar procesos de otros suele exigir `sudo`.
- Un usuario normal sí puede **aumentar** el nice (hacerse más amable) de sus procesos en la mayoría de sistemas.

- Poner `-20` a todo: puedes dejar el sistema menos responsive para el resto. Empieza con valores suaves (`10`, `15`).

## Siguiente

[free y uptime](free-uptime.md)

## Relacionados

- [ps](ps.md) / [top-htop](top-htop.md) — ver la columna NI y el PID
- [nohup](nohup.md) — combinar tareas largas de fondo
- [kill](kill.md) — si el proceso no debería seguir



