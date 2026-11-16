# cron / crontab

`cron` ejecuta comandos **a una hora o con una periodicidad**.  
Tu tabla de tareas de usuario se edita con `crontab`.

## Sintaxis

```bash
crontab -l
crontab -e
crontab -r
```

> `-l` lista, `-e` edita, `-r` **borra todas** tus tareas cron (cuidado).

## Formato de una línea cron

```text
* * * * * comando
│ │ │ │ │
│ │ │ │ └── día de la semana (0-7, 0 y 7 = domingo)
│ │ │ └──── mes (1-12)
│ │ └────── día del mes (1-31)
│ └──────── hora (0-23)
└────────── minuto (0-59)
```

Ejemplos de planificación:

```text
0 9 * * *      cada día a las 09:00
30 18 * * 1-5  laborables a las 18:30
0 */6 * * *    cada 6 horas
*/15 * * * *   cada 15 minutos
0 3 1 * *      día 1 de cada mes a las 03:00
@reboot        al arrancar (muchas crons lo admiten)
@daily         una vez al día (equivalente práctico según implementación)
```

## Ejemplos prácticos

```bash
crontab -e
# abre el editor; añade una línea al final, guarda y sal
```

Línea de ejemplo (backup diario):

```cron
0 2 * * * /usr/bin/tar -czf $HOME/backup-$(date +\%F).tar.gz $HOME/Documentos
```

> En crontab, `%` tiene significado especial: a menudo hay que escribir `\%`.

Redirigir salida a un log:

```cron
*/10 * * * * $HOME/bin/ping_check.sh >> $HOME/ping.log 2>&1
```

Al arrancar el PC:

```cron
@reboot /usr/bin/touch $HOME/cron_reboot_ok
```

```bash
# tras reiniciar (cuando puedas):
ls -l "$HOME/cron_reboot_ok"
```

Prueba cada minuto (solo para aprender; luego quítala):

```cron
* * * * * /usr/bin/date >> $HOME/cron_cada_minuto.log 2>&1
```

```bash
sleep 60
tail -n 5 "$HOME/cron_cada_minuto.log"
# si no aparece nada, mira PATH/servicio (abajo)
```

Listar / editar como otro usuario (admin):

```bash
crontab -l
sudo crontab -l -u <usuario>
sudo crontab -e -u <usuario>
```

## Dónde poner el script

```bash
mkdir -p "$HOME/bin"
nano "$HOME/bin/mi_tarea.sh"
chmod +x "$HOME/bin/mi_tarea.sh"
```

En cron conviene usar **rutas absolutas** a comandos y ficheros (`/usr/bin/tar`, `/home/<usuario>/...`), porque el PATH de cron es mínimo.

## Ver si cron corre (systemd)

```bash
systemctl status cron
# Debian/Ubuntu/Mint

systemctl status crond
# Fedora/RHEL a veces
```

## Depurar un cron que “no hace nada”

```bash
systemctl status cron
# o: systemctl status crond

journalctl -u cron -n 50
# Debian/Ubuntu/Mint; en otras: journalctl -u crond

which tar
which date
# usa esas rutas absolutas dentro del crontab

echo "$PATH"
# el PATH de cron suele ser mucho más corto
```

Añade siempre redirección a un log mientras pruebas (`>> $HOME/mi.log 2>&1`).

## Errores frecuentes

- El job “no hace nada”: mira logs, redirige a un fichero, usa rutas absolutas.  
- `crontab -r` pensando que quita una línea: borra **toda** la tabla.  
- Variables/`~/` que no se expanden como en tu terminal interactiva.  
- Olvidar `\%` en `date +%F` dentro del crontab.

## Siguiente

[systemctl (introducción a systemd)](systemctl.md)

## Relacionados

- [09-scripts](../09-scripts/) — lo que cron suele lanzar  
- [systemctl](systemctl.md) — servicio cron  
- [tar-gzip](tar-gzip.md) — backups programados



