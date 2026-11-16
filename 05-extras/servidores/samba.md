# Samba

Comparte carpetas de Linux con equipos **Windows** (y otros) en la red local, usando el protocolo SMB/CIFS (el de “\\\\servidor\\recurso”).

> **Cómo leer la sintaxis:** `<algo>` = lo sustituyes tú · `[algo]` = opcional.  
> Detalle: [Cómo leer la sintaxis](../../README.md#cómo-leer-la-sintaxis-muy-importante).

## Ideas previas

| Concepto | En pocas palabras |
|----------|-------------------|
| SMB/CIFS | Protocolo de shares de Windows |
| Samba | Implementación libre en Linux |
| Share | Recurso compartido (nombre + carpeta) |
| Usuario Samba | Puede coincidir con un usuario Linux; la clave SMB se define aparte (`smbpasswd`) |

Pensado para **LAN de confianza**. No abras Samba a Internet sin saber mucho de seguridad.

## Instalación

```bash
# Debian / Ubuntu / Linux Mint
sudo apt update
sudo apt install samba

# Fedora
sudo dnf install samba

# Arch
sudo pacman -S samba
```

```bash
systemctl status smbd
# o: nmbd  (nombre NetBIOS; a veces separado)
```

## Sintaxis / servicio

```bash
sudo systemctl <acción> smbd
# en algunas distros también: nmbd
```

Comprobar config:

```bash
testparm
# resume y valida /etc/samba/smb.conf
```

## Ejemplos — lab mínimo en Mint

### 1) Copia de seguridad de la config

```bash
sudo cp /etc/samba/smb.conf /etc/samba/smb.conf.bak
sudo nano /etc/samba/smb.conf
```

### 2) Crear carpeta a compartir

```bash
sudo mkdir -p /srv/samba/<nombre_share>
sudo chown <usuario>:<usuario> /srv/samba/<nombre_share>
sudo chmod 755 /srv/samba/<nombre_share>
```

O compartir algo dentro del home (más simple para pruebas):

```bash
mkdir -p /home/<usuario>/<directorio_compartido>
```

### 3) Definir el share al final de `smb.conf`

Ejemplo orientativo (ajusta nombres):

```ini
[<nombre_share>]
   path = /srv/samba/<nombre_share>
   browseable = yes
   read only = no
   guest ok = no
   valid users = <usuario>
```

```bash
testparm
sudo systemctl restart smbd
```

### 4) Usuario Samba

El usuario debe existir en Linux y además tener clave SMB:

```bash
id <usuario>
sudo smbpasswd -a <usuario>
# te pide la contraseña del share (puede ser distinta de la de login)

sudo smbpasswd -e <usuario>
# enable
```

### 5) Arrancar

```bash
sudo systemctl enable --now smbd
# si existe nmbd y lo necesitas para descubrimiento de nombres:
sudo systemctl enable --now nmbd

systemctl status smbd
sudo ss -tlnp | grep -E '445|139'
```

### 6) Probar desde Linux

```bash
smbclient -L localhost -U <usuario>
# lista shares

smbclient //localhost/<nombre_share> -U <usuario>
# sesión interactiva: ls, put, get, quit
```

Desde Windows (en el Explorador):

```text
\\<IP_del_linux>\<nombre_share>
```

```bash
hostname -I
# IP a usar en la LAN
```

### 7) Firewall (ufw)

```bash
sudo ufw allow Samba
# o puertos: 445/tcp (y a veces 139)
sudo ufw status
```

### 8) Ver quién está conectado / logs

```bash
sudo smbstatus
sudo journalctl -u smbd -n 40
```

### 9) Parar

```bash
sudo systemctl stop smbd
sudo systemctl disable smbd
```

## Mini checklist

| Paso | Comando / acción |
|------|------------------|
| Instalar | `sudo apt install samba` |
| Carpeta | `mkdir` + `chown` |
| Config | share en `smb.conf` + `testparm` |
| Clave | `sudo smbpasswd -a <usuario>` |
| Servicio | `sudo systemctl enable --now smbd` |
| Probar | `smbclient` o `\\IP\share` en Windows |

## Errores frecuentes

```text
NT_STATUS_ACCESS_DENIED / login failure
```

- Clave SMB mal (`smbpasswd`), usuario no en `valid users`, o `read only = yes`.

```text
NT_STATUS_BAD_NETWORK_NAME
```

- Nombre del share mal escrito o path inexistente.

- Permisos Linux de la carpeta: Samba respeta también el sistema de ficheros (`ls -ld <path>`).  
- Windows no ve el PC: firewall, o prueba directo por IP `\\192.168.x.x\share`.  
- Editar `smb.conf` sin `testparm` y dejar el servicio roto.

## Relacionados

- [vsftpd](vsftpd.md) — otra forma de compartir ficheros (FTP)  
- [permisos](../../01-fundamentos/04-permisos/)  
- [ufw](../seguridad/ufw.md)  
- [hostname / IP](../../03-red-paquetes/07-red/hostname.md)  
