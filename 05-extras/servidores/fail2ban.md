# fail2ban

Vigila logs de servicios (SSH, FTP, web…) y **bloquea IPs** que fallan el login demasiadas veces.  
Es una red de seguridad muy útil en un VPS o en un lab expuesto.

> **Cómo leer la sintaxis:** `<algo>` = lo sustituyes tú · `[algo]` = opcional.  
> Detalle: [Cómo leer la sintaxis](../../README.md#cómo-leer-la-sintaxis-muy-importante).

## Ideas previas

| Concepto | En pocas palabras |
|----------|-------------------|
| Jail | “Cárcel”: regla para un servicio (sshd, vsftpd…) |
| Ban | Bloqueo temporal (o permanente) de una IP |
| Filtro | Patrón que reconoce intentos fallidos en el log |
| `ufw` / iptables | fail2ban suele añadir reglas de firewall por debajo |

Complementa a [ssh-endurecido](../seguridad/ssh-endurecido.md) y [ufw](../seguridad/ufw.md); no los sustituye.

## Instalación

```bash
# Debian / Ubuntu / Linux Mint
sudo apt update
sudo apt install fail2ban

# Fedora
sudo dnf install fail2ban

# Arch
sudo pacman -S fail2ban
```

```bash
sudo systemctl enable --now fail2ban
# Created symlink … (a veces) /  o silencio si ya estaba enabled
systemctl status fail2ban
# Active: active (running)
# … fail2ban.service - Fail2Ban Service …
# (q para salir del pager si se queda en less)
```

## Sintaxis

```bash
sudo fail2ban-client <acción> [argumentos]
sudo systemctl <acción> fail2ban
```

## Configuración segura (no edites el original a lo bruto)

En Debian/Ubuntu/Mint lo recomendable es crear un **local**:

```bash
sudo cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local
# o mejor: crear jail.local solo con lo que cambias

sudo nano /etc/fail2ban/jail.local
```

Ejemplo mínimo orientativo para SSH (ajusta a tu gusto):

```ini
[DEFAULT]
bantime  = 1h
findtime = 10m
maxretry = 5

[sshd]
enabled = true
```

```bash
sudo systemctl restart fail2ban
```

Para vsftpd (si lo usas y existe la jail):

```ini
[vsftpd]
enabled = true
```

Comprueba nombres disponibles:

```bash
ls /etc/fail2ban/jail.d/
grep -E '^\[' /etc/fail2ban/jail.conf | head
```

## Ejemplos — fail2ban-client

Estado general:

```bash
sudo fail2ban-client status
```

Salida tipo:

```text
Status
|- Number of jail:      1
`- Jail list:   sshd
```

Estado de SSH:

```bash
sudo fail2ban-client status sshd
```

```text
Status for the jail: sshd
|- Filter
|  |- Currently failed: 0
|  |- Total failed:     12
|  `- File list:        /var/log/auth.log
`- Actions
   |- Currently banned: 1
   |- Total banned:     3
   `- Banned IP list:   203.0.113.50
```

(Si aún no ha baneado a nadie: `Currently banned: 0` y lista vacía.)

Desbanear una IP a mano:

```bash
sudo fail2ban-client set sshd unbanip <IP>
# 1
# (1 = ok; luego status sshd ya no debe listar esa IP)
```

Banear a mano (raro; útil en pruebas):

```bash
sudo fail2ban-client set sshd banip <IP>
# 1
```

Recargar sin reiniciar del todo (según versión):

```bash
sudo fail2ban-client reload
# OK
sudo systemctl restart fail2ban
# silencio = reinicio pedido; comprueba con status
```

Logs:

```bash
sudo tail -n 50 /var/log/fail2ban.log
# … NOTICE  [sshd] Ban 203.0.113.50
# … NOTICE  [sshd] Unban 203.0.113.50

sudo journalctl -u fail2ban -n 40
# mismas ideas vía journald (fechas + fail2ban[…]: …)
```

## Mini laboratorio (cuidado)

1. Activa jail `sshd`.  
2. Desde **otra máquina** (o con cuidado), falla el login SSH varias veces.  
3. Mira:

```bash
sudo fail2ban-client status sshd
```

4. No te bloquees a ti mismo en un servidor remoto sin tener consola/VNC alternativa.  
   Añadir tu IP a ignoreip:

```ini
[DEFAULT]
ignoreip = 127.0.0.1/8 ::1 <tu_IP_fija>
```

## Con ufw

fail2ban puede usar el backend `ufw` o iptables/nftables según distro.  
Tras instalar ambos:

```bash
sudo ufw status
sudo fail2ban-client status sshd
```

Si algo “no banea”, mira el backend en la config y los logs.

## Parar

```bash
sudo systemctl stop fail2ban
sudo systemctl disable fail2ban
```

## Errores frecuentes

```text
Jail sshd is not active / does not exist
```

- No está `enabled = true` en `jail.local`, o el nombre del jail/filtro no coincide con tu distro. Tras cambiar: `restart`.

- Editar solo `jail.conf` y que un update del paquete pise cambios → usa `jail.local`.  
- Autobloquearte por VPN/IP dinámica: configura `ignoreip` o sube `maxretry` mientras pruebas.  
- Pensar que fail2ban sustituye contraseñas fuertes y claves SSH: es una capa más, no la única.


## Siguiente

[Debian / Ubuntu / Linux Mint](../seguridad/ufw.md)

## Relacionados

- [ssh](../../03-red-paquetes/07-red/ssh.md) · [ssh-endurecido](../seguridad/ssh-endurecido.md)  
- [vsftpd](vsftpd.md)  
- [ufw](../seguridad/ufw.md)  
- [systemctl](../../04-automatizacion/11-avanzado/systemctl.md)  
