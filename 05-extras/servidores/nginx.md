# nginx

Servidor web **ligero y muy usado**: sirve páginas HTML y puede hacer de proxy.  
Aquí montamos lo mínimo: instalar, arrancar y ver una página en el navegador.

> **Cómo leer la sintaxis:** `<algo>` = lo sustituyes tú · `[algo]` = opcional.  
> Detalle: [Cómo leer la sintaxis](../../README.md#cómo-leer-la-sintaxis-muy-importante).

## Ideas previas

| Concepto | En pocas palabras |
|----------|-------------------|
| Servidor web | Programa que responde en el puerto 80 (HTTP) o 443 (HTTPS) |
| Document root | Carpeta cuyos ficheros se sirven (`index.html`, etc.) |
| Virtual host / server block | Bloque de config para un sitio (dominio o IP) |
| `nginx` | El demonio; se gestiona con `systemctl` |

Para lab local basta con `http://localhost` o `http://<IP_de_tu_PC>`.

## Instalación

```bash
# Debian / Ubuntu / Linux Mint
sudo apt update
sudo apt install nginx

# Fedora
sudo dnf install nginx

# Arch
sudo pacman -S nginx
```

En Mint/Ubuntu el sitio por defecto suele estar en `/var/www/html/` y la config en `/etc/nginx/`.

## Sintaxis / servicio

```bash
sudo systemctl <acción> nginx
```

Acciones habituales: `start`, `stop`, `restart`, `reload`, `status`, `enable`, `disable`.

## Ejemplos

### Arrancar y comprobar

```bash
sudo systemctl enable --now nginx
systemctl status nginx

sudo ss -tlnp | grep -E ':80|:443'
# debería escuchar en 80
```

```bash
curl -I http://localhost
# HTTP/1.1 200 OK   (o similar)

curl http://localhost
# HTML de la página de bienvenida
```

En el navegador: `http://localhost` o `http://127.0.0.1`.  
Desde otro PC de la LAN: `http://<IP>` (`hostname -I`).

### Dónde están los ficheros (Debian/Ubuntu/Mint)

```bash
ls /var/www/html/
# index.nginx-debian.html  (nombre típico) u otro index

ls /etc/nginx/
ls /etc/nginx/sites-available/
ls /etc/nginx/sites-enabled/
```

### Tu primera página

```bash
sudo cp /var/www/html/index.nginx-debian.html /var/www/html/index.html.bak 2>/dev/null || true

echo '<h1>Hola desde nginx en '"$(hostname)"'</h1>' | sudo tee /var/www/html/index.html
curl http://localhost
```

Permisos: el usuario del servidor (a menudo `www-data`) debe poder **leer** esos ficheros.

```bash
ls -la /var/www/html/
sudo chown root:root /var/www/html/index.html
sudo chmod 644 /var/www/html/index.html
```

### Probar la configuración antes de recargar

```bash
sudo nginx -t
# nginx: configuration file ... test is successful

sudo systemctl reload nginx
# aplica cambios sin cortar del todo (si la config es válida)
```

### Logs

```bash
sudo tail -n 50 /var/log/nginx/access.log
sudo tail -n 50 /var/log/nginx/error.log
sudo journalctl -u nginx -n 30
```

### Firewall (si usas ufw)

```bash
sudo ufw allow 'Nginx Full'
# o: sudo ufw allow 80/tcp
sudo ufw status
```

Detalle: [ufw](../seguridad/ufw.md).

### Parar / deshabilitar

```bash
sudo systemctl stop nginx
sudo systemctl disable nginx
```

## Estructura de config (idea)

```text
/etc/nginx/nginx.conf          # principal
/etc/nginx/sites-available/    # definiciones de sitios
/etc/nginx/sites-enabled/      # enlaces a los sitios activos
/var/www/html/                 # ficheros del sitio por defecto
```

Activar un sitio (patrón Debian/Ubuntu/Mint):

```bash
sudo nano /etc/nginx/sites-available/<sitio>
sudo ln -s /etc/nginx/sites-available/<sitio> /etc/nginx/sites-enabled/
sudo nginx -t && sudo systemctl reload nginx
```

## Errores frecuentes

```text
bind() to 0.0.0.0:80 failed ... Address already in use
```

- Otro servidor (Apache, otro nginx) ya usa el 80:

```bash
sudo ss -tlnp | grep ':80'
```

```text
403 Forbidden / Permission denied
```

- Permisos de `/var/www/...` o SELinux/AppArmor en distros estrictas; en Mint suele ser permisos/`www-data`.

```text
nginx -t falla
```

- No hagas `reload` hasta corregir; mira la línea que indica el error.

- Abrir el puerto 80 a Internet sin saber qué sirves: en lab usa LAN o localhost.

## Relacionados

- [apache](apache.md) — alternativa clásica  
- [systemctl](../../04-automatizacion/11-avanzado/systemctl.md)  
- [curl](../../03-red-paquetes/07-red/curl.md)  
- [ufw](../seguridad/ufw.md)  
