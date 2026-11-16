# hostname

Muestra (o cambia) el **nombre del equipo** en la red.

## Sintaxis

```bash
hostname [<nombre>]
hostname [opciones]
```

> Recuerda: `[ ]` = opcional · `<nombre>` = lo sustituyes tú.  
> Detalle: [Cómo leer la sintaxis](../../README.md#cómo-leer-la-sintaxis-muy-importante).

## Ejemplos

```bash
hostname
# nombre actual del equipo (ej. mint-pc, fedora-laptop…)

hostnamectl
# en sistemas con systemd: más detalle (nombre, icono, chassis…)

hostnamectl status
```

Ver IPs asociadas de forma sencilla:

```bash
hostname -I
# lista de direcciones del host (muy práctico)
# puede haber varias: Wi‑Fi, cable, VPN, docker…

hostname -I | awk '{ print $1 }'
# primera IP (orientativo; no siempre es la que te interesa)
```

Comparar con `ip`:

```bash
hostname -I
ip -br -4 a
# identifica cuál interfaz es wlan/eth y su IP
```

Nombre completo (FQDN), si está configurado:

```bash
hostname -f
# ej. pc.casa.local  (puede fallar si no hay FQDN bien definido)

hostname -s
# nombre corto
```

Cambiar el nombre (permanente con systemd; hace falta admin):

```bash
sudo hostnamectl set-hostname <nombre>
# ej: sudo hostnamectl set-hostname portatil-mint

hostname
# en la sesión actual a veces sigue el viejo hasta reabrir terminal/reiniciar servicios
```

Comprobar:

```bash
hostnamectl
cat /etc/hostname
# en muchas distros el nombre persistente está ahí
```

## Errores frecuentes

```text
hostname: Name or service not known
```

- Con `hostname -f` si el sistema no tiene un nombre de dominio bien configurado.

- Cambiar solo con `hostname <nombre>` (sin `hostnamectl`) puede ser **temporal** hasta el próximo reinicio, según distro.
- Poner espacios o caracteres raros en el nombre: mejor letras, números y guiones (`portatil-ana`).

## Siguiente

[ss](ss.md)

## Relacionados

- [ip](ip.md) — direcciones IP
- [ping](ping.md) — `ping $(hostname)` en redes locales a veces ayuda a probar
- [ssh](ssh.md) — te conectarás a `usuario@hostname` o a una IP



