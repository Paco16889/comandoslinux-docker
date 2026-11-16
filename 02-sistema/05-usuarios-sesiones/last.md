# last / lastlog

Historial de inicios de sesión (y a veces reinicios).

## last

Lee el fichero de registros de login (habitualmente `/var/log/wtmp`).

### Sintaxis

```bash
last [opciones] [<usuario>]
```

### Ejemplos

```bash
last
# últimos logins (más reciente primero)

last <usuario>
# solo ese usuario

last -n <número>
# limitar a <número> líneas

last -n 20
last -n 10 <usuario>
```

Reinicios y apagados:

```bash
last reboot
# historial de reinicios

last shutdown
# apagados (si está registrado)
```

Filtrar por origen:

```bash
last -a
# muestra el hostname/IP al final

last | grep <patrón>
# filtrar por texto (IP, tty, etc.)
```

```bash
last -s <fecha>
# desde una fecha (sintaxis según versión; en util-linux admite formas como yesterday)

last -t <fecha>
# hasta una fecha
```

Salida típica (campos):

```text
<usuario>  pts/0  192.168.1.10  Sat Nov 14 18:00   still logged in
<usuario>  pts/1  192.168.1.10  Sat Nov 14 17:00 - 17:30  (00:30)
```

## lastlog

Último login **por cada cuenta** del sistema (fichero `/var/log/lastlog`).

### Sintaxis

```bash
lastlog [opciones]
```

### Ejemplos

```bash
lastlog
# tabla de todos los usuarios

lastlog -u <usuario>
# solo un usuario

sudo lastlog -u <usuario>
# si tu sistema restringe la lectura
```

## Errores frecuentes

```text
last: /var/log/wtmp: No such file or directory
```

- El sistema no tiene ese log o se rotó/eliminó.

- `last` vacío tras rotar logs: el historial antiguo puede estar en `wtmp.*` o comprimido; a veces `last -f /var/log/wtmp.1`.

```bash
sudo last -f /var/log/wtmp.1
```

## Siguiente

[su](su.md)

## Relacionados

- [who-w-users](who-w-users.md) — sesiones activas ahora
- [whoami](whoami.md) — usuario actual
- [exit-logout](exit-logout.md) — fin de sesión que luego verás en `last`



