# 07 — Red

Comandos para comprobar si hay red, ver tu IP, descargar ficheros y conectar a otros equipos.

> **Cómo leer la sintaxis:** `<algo>` = lo sustituyes tú (no escribas `<` `>`).  
> `[algo]` = opcional (no escribas `[` `]`).  
> Explicación completa: [Cómo leer la sintaxis](../../README.md#cómo-leer-la-sintaxis-muy-importante).

## Ideas previas (léelas una vez)

| Concepto | En pocas palabras |
|----------|-------------------|
| IP | “Dirección” de un equipo en la red (ej. `192.168.1.10`) |
| Host / hostname | Nombre del equipo |
| Puerto | “Puerta” de un servicio (ej. 22 = SSH, 80 = web) |
| DNS | Traduce nombres (`google.com`) a IPs |
| Interfaz | Tarjeta/conexión de red (`eth0`, `wlan0`, `enp3s0`…) |
| Localhost | Este mismo equipo (`127.0.0.1` o `localhost`) |

## Orden recomendado

1. [ping](ping.md) — ¿hay respuesta en la red?
2. [ip](ip.md) — ver/configurar direcciones (moderno)
3. [hostname](hostname.md) — nombre del equipo
4. [ss](ss.md) — puertos y conexiones
5. [curl](curl.md) — hablar con URLs / APIs
6. [wget](wget.md) — descargar ficheros
7. [traceroute](traceroute.md) — camino hasta un destino
8. [ssh](ssh.md) — entrar en otro equipo
9. [scp](scp.md) — copiar ficheros por SSH
10. [dig-nslookup](dig-nslookup.md) — consultar DNS

## Extras relacionados (después de este bloque)

| Tema | Dónde |
|------|--------|
| FTP | [vsftpd](../../05-extras/servidores/vsftpd.md) |
| Firewall | [ufw](../../05-extras/seguridad/ufw.md) |
| SSH más seguro | [ssh-endurecido](../../05-extras/seguridad/ssh-endurecido.md) |
| Wi‑Fi / IP por CLI | [nmcli](../../05-extras/red-avanzada/nmcli.md) |
| Escaneo en lab | [nmap](../../05-extras/red-avanzada/nmap.md) |
