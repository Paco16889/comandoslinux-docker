# Solución — Extras

## Ejercicio 1

```bash
du -sh ~/practica-linux
#  …K / …M

# opcional
# ncdu ~/practica-linux
```

## Ejercicio 2

```bash
sudo ufw status
# Status: inactive   o   active
```

Nota: antes de `ufw enable` en un servidor remoto, permitir SSH (`ufw allow 22/tcp` o el puerto que uses).

## Ejercicio 3 (idea)

Ejemplos de medidas: autenticación por clave; `PermitRootLogin no`; fall2ban; puerto no estándar (con cuidado); desactivar password auth cuando las claves funcionen.  
Precaución: no cierres la única sesión hasta probar otra.

## Ejercicio 4

Depende del servicio; patrón típico nginx:

```bash
# sudo apt install nginx
# sudo systemctl enable --now nginx
# curl -I http://127.0.0.1
# sudo systemctl stop nginx
```
