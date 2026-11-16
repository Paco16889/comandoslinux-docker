# systemctl (introducción a systemd)

En la mayoría de distros modernas, **systemd** gestiona servicios (ssh, red, cron…).  
`systemctl` es el mando a distancia.

## Sintaxis

```bash
systemctl [opciones] <acción> [<unidad>]
```

> Unidades típicas: `<nombre>.service` (a veces puedes omitir `.service`).

## Ver estado

```bash
systemctl status <servicio>
systemctl status ssh
systemctl status sshd
# el nombre cambia según distro (ssh vs sshd)
```

```bash
systemctl is-active <servicio>
systemctl is-enabled <servicio>
# active/inactive · enabled/disabled
```

## Arrancar / parar / reiniciar (ahora)

```bash
sudo systemctl start <servicio>
sudo systemctl stop <servicio>
sudo systemctl restart <servicio>
sudo systemctl reload <servicio>
# reload: recarga config si el servicio lo soporta (sin cortar del todo)
```

## Al arranque del sistema

```bash
sudo systemctl enable <servicio>
# que arranque solo al boot

sudo systemctl disable <servicio>
# que no arranque solo

sudo systemctl enable --now <servicio>
# enable + start a la vez
```

## Listar

```bash
systemctl list-units --type=service
systemctl list-units --type=service --state=running

systemctl list-unit-files --type=service | less
```

## Logs del servicio (journalctl)

```bash
journalctl -u <servicio>
journalctl -u ssh -n 50
journalctl -u ssh -f
# -f = follow (como tail -f)
```

## Ejemplos reales

```bash
# Debian / Ubuntu / Linux Mint — SSH
sudo apt install openssh-server
sudo systemctl enable --now ssh
systemctl status ssh

# Fedora
sudo systemctl enable --now sshd
```

## Errores frecuentes

```text
Unit <servicio>.service could not be found
```

- Nombre incorrecto o paquete no instalado. Prueba `systemctl list-unit-files | grep <texto>`.

- Usar `start` pensando que ya quedará para siempre: `start` es ahora; `enable` es en el boot.  
- Sin `sudo` en acciones que cambian el sistema.

## Relacionados

- [ssh](../../03-red-paquetes/07-red/ssh.md)  
- [cron](cron.md)  
- [08-paquetes](../../03-red-paquetes/08-paquetes/) — instalar el servicio antes  
