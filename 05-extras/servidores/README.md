# Servidores (extras)

Servicios concretos: FTP, web, shares Windows, protección ante fuerza bruta.

> **Cómo leer la sintaxis:** [guía](../../README.md#cómo-leer-la-sintaxis-muy-importante).

## Orden recomendado

1. [vsftpd](vsftpd.md) — FTP (lab)  
2. [nginx](nginx.md) o [apache](apache.md) — web básica (elige uno para el puerto 80)  
3. [samba](samba.md) — shares con Windows en LAN  
4. [fail2ban](fail2ban.md) — bloqueo tras fallos de login  

| Documento | Qué cubre | Estado |
|-----------|-----------|--------|
| [vsftpd](vsftpd.md) | Servidor FTP | Listo |
| [nginx](nginx.md) | Servidor web nginx | Listo |
| [apache](apache.md) | Servidor web Apache | Listo |
| [samba](samba.md) | Compartir con Windows | Listo |
| [fail2ban](fail2ban.md) | Ban por intentos fallidos | Listo |

## Notas

- **nginx y Apache**: no hace falta los dos a la vez en el puerto 80.  
- **Samba / vsftpd**: mejor en LAN de confianza; en Internet prioriza SSH/SFTP y buen firewall.  
- Tras exponer SSH/FTP: mira [ufw](../seguridad/ufw.md) y [ssh-endurecido](../seguridad/ssh-endurecido.md) (plantilla o siguiente carpeta a rellenar).

Relacionado: [07-red](../../03-red-paquetes/07-red/), [systemctl](../../04-automatizacion/11-avanzado/systemctl.md).
