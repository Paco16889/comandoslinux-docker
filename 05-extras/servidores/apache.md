# Apache (`apache2` / `httpd`)

Servidor web clásico y muy documentado. En Debian/Ubuntu/Mint el paquete/servicio se llama **`apache2`**; en Fedora/RHEL suele ser **`httpd`**.

> **Cómo leer la sintaxis:** `<algo>` = lo sustituyes tú · `[algo]` = opcional.  
> Detalle: [Cómo leer la sintaxis](../../README.md#cómo-leer-la-sintaxis-muy-importante).

## Ideas previas

| Concepto | En pocas palabras |
|----------|-------------------|
| DocumentRoot | Carpeta que se publica (en Debian/Mint: `/var/www/html`) |
| VirtualHost | Config de un sitio (IP/dominio + carpeta) |
| `apache2` vs `httpd` | Mismo software; distinto nombre según distro |
| Puerto 80 | HTTP por defecto (como nginx) |

**No hace falta tener nginx y Apache a la vez** escuchando en el 80: pelearán por el puerto.

## Instalación

```bash
# Debian / Ubuntu / Linux Mint
sudo apt update
sudo apt install apache2

# Fedora
sudo dnf install httpd

# Arch
sudo pacman -S apache
```

## Sintaxis / servicio

```bash
# Debian / Ubuntu / Linux Mint
sudo systemctl <acción> apache2

# Fedora / RHEL
sudo systemctl <acción> httpd
```

## Ejemplos (Mint / Ubuntu / Debian)

### Arrancar

```bash
sudo systemctl enable --now apache2
systemctl status apache2

sudo ss -tlnp | grep ':80'
curl -I http://localhost
```

Navegador: `http://localhost`

### Página por defecto y la tuya

```bash
ls /var/www/html/
# index.html  (a menudo)

echo '<h1>Hola desde Apache en '"$(hostname)"'</h1>' | sudo tee /var/www/html/index.html
curl http://localhost
```

```bash
ls -la /var/www/html/
# dueño típico: root:root o www-data; lectura para otros (644) suele bastar
```

### Probar config y recargar

```bash
sudo apache2ctl configtest
# Syntax OK

sudo systemctl reload apache2
```

En Fedora:

```bash
sudo apachectl configtest
sudo systemctl reload httpd
```

### Logs

```bash
# Debian/Ubuntu/Mint
sudo tail -n 50 /var/log/apache2/access.log
sudo tail -n 50 /var/log/apache2/error.log
sudo journalctl -u apache2 -n 30
```

```bash
# Fedora
sudo tail -n 50 /var/log/httpd/access_log
sudo journalctl -u httpd -n 30
```

### Módulos (idea)

```bash
apache2ctl -M
# módulos cargados (Debian/Mint)

sudo a2enmod <modulo>
sudo a2dismod <modulo>
sudo systemctl reload apache2
```

### Sitios (Debian/Ubuntu/Mint)

```bash
ls /etc/apache2/sites-available/
ls /etc/apache2/sites-enabled/

sudo a2ensite <sitio>
sudo a2dissite <sitio>
sudo apache2ctl configtest && sudo systemctl reload apache2
```

### Firewall

```bash
sudo ufw allow 'Apache Full'
# o: sudo ufw allow 80/tcp
```

### Parar

```bash
sudo systemctl stop apache2
sudo systemctl disable apache2
```

## nginx vs Apache (para decidir)

| | nginx | Apache |
|-|-------|--------|
| Enfoque | Ligero, proxy, estáticos | Muy flexible, `.htaccess`, LAMP clásico |
| Nombre servicio (Mint) | `nginx` | `apache2` |
| Document root típico | `/var/www/html` | `/var/www/html` |

Para aprender “una página HTML en local”, **cualquiera de los dos vale**. Si ya tienes uno en el puerto 80, para el otro antes de instalar el segundo.

```bash
sudo systemctl stop nginx
sudo systemctl start apache2
# o al revés
```

## Errores frecuentes

```text
Address already in use (80)
```

```bash
sudo ss -tlnp | grep ':80'
# para el otro servidor web o cambia puerto (avanzado)
```

```text
Unit apache2.service not found
```

- En Fedora prueba `httpd`. En Arch el unit puede llamarse `httpd`.

- Editar `/var/www/html` sin `sudo` y no entender el “Permission denied”.  
- Mezclar tutoriales `httpd` y `apache2` sin mirar la distro.

## Relacionados

- [nginx](nginx.md)  
- [systemctl](../../04-automatizacion/11-avanzado/systemctl.md)  
- [curl](../../03-red-paquetes/07-red/curl.md)  
- [ufw](../seguridad/ufw.md)  
