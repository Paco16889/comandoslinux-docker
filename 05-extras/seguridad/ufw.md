# ufw

**UFW** = *Uncomplicated Firewall*. Es la forma sencilla de gestionar el firewall en **Ubuntu** y **Linux Mint** (también en Debian).  
Permite o deniega tráfico de red por puertos/aplicaciones sin pelearte a mano con iptables/nftables.

> **Cómo leer la sintaxis:** `<algo>` = lo sustituyes tú · `[algo]` = opcional.  
> Detalle: [Cómo leer la sintaxis](../../README.md#cómo-leer-la-sintaxis-muy-importante).

## Ideas previas

| Concepto | En pocas palabras |
|----------|-------------------|
| Firewall | Filtro: qué conexiones entran/salen de tu PC |
| Regla allow / deny | Permitir o denegar un puerto o servicio |
| Incoming | Tráfico que **entra** a tu máquina (lo habitual a filtrar) |
| Perfil de aplicación | Nombre corto (`OpenSSH`, `Nginx Full`…) que agrupa puertos |

**Aviso en servidores remotos (VPS/SSH):** antes de activar ufw, **permite SSH**. Si activas el firewall bloqueando todo y no has abierto el 22, puedes perder el acceso.

## Instalación

```bash
# Debian / Ubuntu / Linux Mint
sudo apt update
sudo apt install ufw

# Fedora — ufw no es lo habitual; suele usarse firewalld
# sudo dnf install ufw   # posible, pero mejor aprender firewalld allí

# Arch
sudo pacman -S ufw
```

En Mint/Ubuntu a menudo ya está instalado.

## Sintaxis

```bash
sudo ufw [opciones] <acción> [argumentos]
```

Casi todas las acciones de cambio necesitan `sudo`.

## Ejemplos

### Ver estado

```bash
sudo ufw status
sudo ufw status verbose
sudo ufw status numbered
# numbered: útil para borrar por número
```

### Política por defecto (recomendable en lab/servidor)

```bash
sudo ufw default deny incoming
sudo ufw default allow outgoing
# deniega entradas no permitidas; deja salir (navegar, apt…)
```

### Abrir SSH **antes** de activar (imprescindible si administras por SSH)

```bash
sudo ufw allow OpenSSH
# o:
sudo ufw allow 22/tcp

sudo ufw status
```

### Activar / desactivar

```bash
sudo ufw enable
# pregunta confirmación; en SSH recuerda: ya debiste permitir OpenSSH

sudo ufw disable
# apaga las reglas ufw (el equipo queda sin ese filtro)
```

### Permitir servicios habituales

```bash
sudo ufw app list
# perfiles instalados (OpenSSH, Nginx*, Apache*, Samba…)

sudo ufw allow 'Nginx Full'
sudo ufw allow 'Apache Full'
sudo ufw allow Samba
sudo ufw allow 21/tcp
# FTP (vsftpd); si usas pasivo, abre también el rango que configuraste
```

Por puerto / protocolo:

```bash
sudo ufw allow <puerto>/<protocolo>
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
sudo ufw allow 40000:40100/tcp
# rango (ej. pasivo FTP)
```

Desde una IP concreta (más restrictivo):

```bash
sudo ufw allow from <IP> to any port 22 proto tcp
sudo ufw allow from 192.168.1.50 to any port 22 proto tcp
```

Denegar:

```bash
sudo ufw deny <puerto>/<protocolo>
sudo ufw deny 23/tcp
```

### Borrar reglas

```bash
sudo ufw status numbered
sudo ufw delete <número>
# ej: sudo ufw delete 3

sudo ufw delete allow 80/tcp
```

### Logs

```bash
sudo ufw logging on
sudo ufw logging medium
# low|medium|high|full

sudo tail -n 50 /var/log/ufw.log
# o según rsyslog/journal:
sudo journalctl | grep -i ufw | tail
```

### Reset (vuelve a empezar; cuidado)

```bash
sudo ufw reset
# borra reglas y desactiva; tendrás que permitir SSH otra vez si aplica
```

## Mini laboratorio (PC local Mint)

```bash
sudo ufw status
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw allow OpenSSH
sudo ufw allow 80/tcp
sudo ufw enable
sudo ufw status verbose
```

Prueba:

```bash
curl -I http://localhost
# si tienes nginx/apache

ssh <usuario>@localhost
# si tienes sshd
```

## Orden seguro en un VPS

1. `sudo ufw allow OpenSSH`  
2. Otras reglas que necesites (80, 443…)  
3. `sudo ufw enable`  
4. `sudo ufw status verbose`  
5. **No cierres** la sesión SSH hasta comprobar que puedes abrir otra  

## Errores frecuentes

- Activar ufw en un VPS **sin** permitir SSH → te quedas fuera.  
- Confundir `disable` (apaga firewall) con `delete` (quita una regla).  
- Abrir `allow 22` y luego cambiar SSH al puerto `<puerto>` sin actualizar la regla.  
- Pensar que ufw sustituye contraseñas fuertes / fail2ban / claves SSH: es una capa más.

```text
ERROR: Could not find a profile matching ...
```

```bash
sudo ufw app list
# usa el nombre exacto entre comillas si tiene espacios
```

## Relacionados

- [ssh](../../03-red-paquetes/07-red/ssh.md) · [ssh-endurecido](ssh-endurecido.md)  
- [fail2ban](../servidores/fail2ban.md)  
- [nginx](../servidores/nginx.md) · [samba](../servidores/samba.md) · [vsftpd](../servidores/vsftpd.md)  
