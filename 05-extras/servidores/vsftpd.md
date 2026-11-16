# vsftpd (servidor FTP)

`vsftpd` = *Very Secure FTP Daemon*. Sirve para compartir ficheros con el protocolo **FTP**.

> FTP envía datos (y a menudo contraseñas) **sin cifrar**. Para Internet público suele ser mejor **SFTP** (via SSH) o FTPS.  
> Esta guía es para **aprender en red local / laboratorio**. No abras FTP a Internet sin saber las implicaciones.

> **Cómo leer la sintaxis:** `<algo>` = lo sustituyes tú · `[algo]` = opcional.  
> Detalle: [Cómo leer la sintaxis](../../README.md#cómo-leer-la-sintaxis-muy-importante).

## Ideas previas

| Concepto | En pocas palabras |
|----------|-------------------|
| FTP | Protocolo clásico para subir/bajar ficheros (puerto 21) |
| vsftpd | Programa servidor FTP en Linux |
| Usuario local | Cuenta del sistema Linux que también usará FTP |
| chroot | “Enjaular” al usuario en su carpeta (no sube a `/`) |
| Activo / pasivo | Dos modos de transferencia; el pasivo es el habitual detrás de NAT/firewall |

Antes de seguir: [paquetes](../../03-red-paquetes/08-paquetes/), [systemctl](../../04-automatizacion/11-avanzado/systemctl.md), [usuarios](../../02-sistema/05-usuarios-sesiones/).

## 1) Instalar

```bash
# Debian / Ubuntu / Linux Mint
sudo apt update
sudo apt install vsftpd

# Fedora
sudo dnf install vsftpd

# Arch
sudo pacman -S vsftpd
```

Comprobar el servicio:

```bash
systemctl status vsftpd
# o a veces: systemctl status vsftpd.service
```

## 2) Arrancar y habilitar

```bash
sudo systemctl enable --now vsftpd
systemctl is-active vsftpd
# active

sudo ss -tlnp | grep 21
# debería escuchar en el puerto 21
```

## 3) Copia de seguridad de la config

El fichero principal suele ser `/etc/vsftpd.conf`.

```bash
sudo cp /etc/vsftpd.conf /etc/vsftpd.conf.bak
# si algo sale mal: sudo cp /etc/vsftpd.conf.bak /etc/vsftpd.conf
```

Editar:

```bash
sudo nano /etc/vsftpd.conf
```

## 4) Configuración mínima orientativa (laboratorio)

Valores frecuentes que verás (comprueba que existan o descoméntalos en tu fichero; **no copies a ciegas** en producción):

```text
listen=YES
anonymous_enable=NO
local_enable=YES
write_enable=YES
chroot_local_user=YES
allow_writeable_chroot=YES
pam_service_name=vsftpd
```

Qué significan en lenguaje simple:

| Opción | Idea |
|--------|------|
| `anonymous_enable=NO` | Sin invitados anónimos |
| `local_enable=YES` | Usuarios del sistema pueden entrar |
| `write_enable=YES` | Pueden subir/escribir (si el FS lo permite) |
| `chroot_local_user=YES` | Quedan “dentro” de su home |
| `allow_writeable_chroot=YES` | Permite home escribible con chroot (vsftpd moderno) |

Modo pasivo (útil en muchas redes; ajusta el rango a tu gusto):

```text
pasv_enable=YES
pasv_min_port=40000
pasv_max_port=40100
```

Tras editar:

```bash
sudo systemctl restart vsftpd
systemctl status vsftpd
```

## 5) Crear un usuario para FTP (ejemplo)

```bash
sudo adduser <usuario_ftp>
# en Debian/Ubuntu/Mint: asistente interactivo
# (en Fedora a menudo: sudo useradd -m <usuario_ftp> && sudo passwd <usuario_ftp>)

sudo passwd <usuario_ftp>
# si hace falta definir/cambiar clave
```

Carpeta de trabajo:

```bash
sudo mkdir -p /home/<usuario_ftp>/ftp_archivos
sudo chown <usuario_ftp>:<usuario_ftp> /home/<usuario_ftp>/ftp_archivos
```

Según tu `chroot`, el usuario puede ver su `$HOME`. Practica subiendo ficheros a `ftp_archivos`.

## 6) Probar el servidor

Desde el **mismo** PC:

```bash
ftp -p localhost
# o: ftp 127.0.0.1
# usuario: <usuario_ftp>
# contraseña: la que pusiste
```

Comandos dentro del cliente `ftp` (clásico):

```text
ls
pwd
cd ftp_archivos
put <fichero_local>
get <fichero_remoto>
bye
```

Con un cliente gráfico (FileZilla, etc.) en otro equipo de la LAN:

```text
Host: <IP_del_servidor>
Usuario: <usuario_ftp>
Contraseña: ...
Puerto: 21
```

Tu IP:

```bash
hostname -I
ip -br -4 a
```

Si `ftp` no está instalado:

```bash
# Debian / Ubuntu / Linux Mint
sudo apt install ftp
# a veces el paquete se llama ftp o inetutils-ftp / tnftp según distro
```

## 7) Firewall (si lo tienes activo)

Hay que permitir el **21** y el rango pasivo si lo definiste.

```bash
# Ejemplo UFW (Ubuntu/Mint si usas ufw)
sudo ufw allow 21/tcp
sudo ufw allow 40000:40100/tcp
sudo ufw status
```

## 8) Logs y diagnóstico

```bash
systemctl status vsftpd
journalctl -u vsftpd -n 50
sudo ss -tlnp | grep 21
```

Probar conectividad desde otro PC:

```bash
ping -c 2 <IP_servidor>
# luego el cliente FTP
```

## 9) Parar / deshabilitar

```bash
sudo systemctl stop vsftpd
sudo systemctl disable vsftpd
```

## Buenas prácticas (aunque sea laboratorio)

1. `anonymous_enable=NO` salvo que sepas por qué lo activas.  
2. Prefiere usuarios dedicados, no tu cuenta admin del día a día.  
3. En Internet: valora **SFTP** (`scp`/`sftp` por SSH) en lugar de FTP plano.  
4. Guarda siempre `vsftpd.conf.bak` antes de tocar.  
5. Tras cada cambio: `restart` + prueba de login.

## Errores frecuentes

```text
500 OOPS: vsftpd: refusing to run with writable root inside chroot()
```

- Conflicto chroot + home escribible: revisa `allow_writeable_chroot=YES` o la estructura de carpetas (a veces se usa un home no escribible + subcarpeta escribible).

```text
Connection refused
```

- Servicio parado, puerto mal, o firewall. `systemctl status` + `ss -tlnp`.

```text
Login incorrect
```

- Usuario/clave mal; o el usuario está en lista de denegados (`/etc/ftpusers` en algunos sistemas — usuarios como `root` suelen estar bloqueados a propósito).

- Abrir FTP a Internet “para probar” sin firewall ni contraseñas fuertes: riesgo alto.

## Alternativa más segura (recordatorio)

```bash
# Con SSH instalado, desde el cliente:
sftp <usuario>@<IP>
scp <fichero> <usuario>@<IP>:<directorio>/
```

Ver [ssh](../../03-red-paquetes/07-red/ssh.md) y [scp](../../03-red-paquetes/07-red/scp.md).

## Siguiente

[Debian / Ubuntu / Linux Mint](nginx.md)

## Relacionados

- [systemctl](../../04-automatizacion/11-avanzado/systemctl.md)  
- [apt](../../03-red-paquetes/08-paquetes/apt.md)  
- [useradd / adduser](../../02-sistema/05-usuarios-sesiones/useradd-usermod-userdel.md)  
- [ss](../../03-red-paquetes/07-red/ss.md) — puerto 21



