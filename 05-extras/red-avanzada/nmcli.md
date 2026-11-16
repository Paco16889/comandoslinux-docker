# nmcli (NetworkManager)

`nmcli` controla la red desde la terminal cuando el sistema usa **NetworkManager** (lo habitual en **Linux Mint**, Ubuntu escritorio, Fedora Workstation, muchas laptops).  
Sirve para ver Wi‑Fi, conectarte, mirar tu IP y activar/desactivar conexiones **sin** el panel gráfico.

> **Cómo leer la sintaxis:** `<algo>` = lo sustituyes tú · `[algo]` = opcional.  
> Detalle: [Cómo leer la sintaxis](../../README.md#cómo-leer-la-sintaxis-muy-importante).

## Ideas previas

| Concepto | En pocas palabras |
|----------|-------------------|
| NetworkManager | Servicio que gestiona Wi‑Fi, cable, VPN… en escritorios |
| `nmcli` | Cliente en terminal de NetworkManager |
| Conexión (connection) | Perfil guardado (“Wi‑Fi casa”, “Ethernet”…) |
| Dispositivo (device) | Hardware: `wlan0`, `eth0`, `enp3s0`… |
| SSID | Nombre de la red Wi‑Fi (lo que ves en la lista) |

¿Tienes NetworkManager?

```bash
systemctl is-active NetworkManager
# active → sí

nmcli general status
```

Si usas un servidor mínimo solo con `systemd-networkd` o netplan “a pelo”, puede que `nmcli` no sea el camino. En **Mint de escritorio**, casi seguro sí.

Relacionado básico: [ip](../../03-red-paquetes/07-red/ip.md) (ver IPs); `nmcli` además **cambia** y gestiona perfiles.

## Instalación

En Mint/Ubuntu escritorio suele venir todo. Si faltara:

```bash
# Debian / Ubuntu / Linux Mint
sudo apt update
sudo apt install network-manager

# Fedora
sudo dnf install NetworkManager

# Arch
sudo pacman -S networkmanager
sudo systemctl enable --now NetworkManager
```

```bash
nmcli --version
```

## Sintaxis (mapa mental)

```bash
nmcli <objeto> <comando>
```

Objetos que más usarás:

| Objeto | Para qué |
|--------|----------|
| `general` | Estado general |
| `device` / `d` | Tarjetas de red |
| `connection` / `c` / `con` | Perfiles guardados |
| `networking` | Activar/desactivar toda la red NM |
| `radio` | Wi‑Fi / WWAN on/off |

Casi todo admite forma corta: `nmcli d` = `nmcli device`.

## Ejemplos — ver el estado (empieza aquí)

```bash
nmcli general status
# estado de NetworkManager

nmcli device status
# o: nmcli d
# DEVICE  TYPE  STATE  CONNECTION
```

```bash
nmcli connection show
# o: nmcli c
# lista de perfiles (Wi‑Fi guardadas, ethernet…)

nmcli connection show --active
# solo las activas ahora
```

Detalle de un dispositivo:

```bash
nmcli device show
nmcli device show <interfaz>
nmcli device show wlan0
# IP, DNS, puerta de enlace… (muy útil)
```

Equivalente mental a lo que ya viste:

```bash
nmcli -p device show <interfaz> | less
ip -br a
ip route
```

## Ejemplos — Wi‑Fi

### ¿Está el radio Wi‑Fi encendido?

```bash
nmcli radio wifi
# enabled / disabled

nmcli radio wifi on
nmcli radio wifi off
```

### Listar redes visibles

```bash
nmcli device wifi list
# o: nmcli d wifi list

# Si la lista está vieja:
nmcli device wifi rescan
nmcli device wifi list
```

Verás columnas tipo SSID, señal (SIGNAL), seguridad (SECURITY).

### Conectarte a una Wi‑Fi

```bash
nmcli device wifi connect "<SSID>" password "<contraseña>"
```

Ejemplo:

```bash
nmcli device wifi connect "MiCasaFibra" password "secreto123"
```

Si la red ya estaba guardada:

```bash
nmcli connection up "<nombre_conexion>"
# el nombre a menudo coincide con el SSID
```

Con interfaz concreta:

```bash
nmcli device wifi connect "<SSID>" password "<contraseña>" ifname <interfaz>
nmcli device wifi connect "MiCasaFibra" password "secreto123" ifname wlan0
```

### Desconectar Wi‑Fi / bajar un perfil

```bash
nmcli device disconnect <interfaz>
nmcli device disconnect wlan0

nmcli connection down "<nombre_conexion>"
```

### Olvidar / borrar una red guardada

```bash
nmcli connection delete "<nombre_conexion>"
```

## Ejemplos — cable (Ethernet)

```bash
nmcli device status
# mira el dispositivo ethernet (enp*, eth0…)

nmcli device connect <interfaz>
nmcli connection up "<nombre_ethernet>"
```

Muchas veces el perfil se llama `Wired connection 1` (o traducido).

```bash
nmcli connection show
nmcli connection up "Wired connection 1"
```

## Ejemplos — IP manual (solo si sabes lo que haces)

Por defecto casi todo es **DHCP** (te dan IP sola).  
IP fija en un perfil (laboratorio):

```bash
nmcli connection modify "<nombre_conexion>" \
  ipv4.addresses <IP>/<mascara> \
  ipv4.gateway <puerta> \
  ipv4.dns "<DNS>" \
  ipv4.method manual

nmcli connection up "<nombre_conexion>"
```

Ejemplo orientativo (usa valores de **tu** red):

```bash
nmcli connection modify "MiCasaFibra" \
  ipv4.addresses 192.168.1.50/24 \
  ipv4.gateway 192.168.1.1 \
  ipv4.dns "1.1.1.1" \
  ipv4.method manual

nmcli connection up "MiCasaFibra"
```

Volver a DHCP:

```bash
nmcli connection modify "<nombre_conexion>" ipv4.method auto
nmcli connection up "<nombre_conexion>"
```

## Activar / desactivar toda la red (NetworkManager)

```bash
nmcli networking off
# te quedas sin red gestionada por NM

nmcli networking on
```

Útil si “no hay Internet” y quieres reiniciar el stack a lo bruto (también puedes `sudo systemctl restart NetworkManager`).

```bash
sudo systemctl restart NetworkManager
nmcli general status
```

## Mini laboratorio Mint (Wi‑Fi)

```bash
# 1) Estado
nmcli general status
nmcli device status

# 2) Wi‑Fi on + listar
nmcli radio wifi on
nmcli device wifi rescan
nmcli device wifi list

# 3) Conectar (pon tu SSID y clave)
nmcli device wifi connect "<SSID>" password "<contraseña>"

# 4) Comprobar
nmcli connection show --active
nmcli device show <interfaz> | grep -iE 'ip4|general'
ping -c 3 8.8.8.8
ping -c 3 google.com
```

## nmcli vs `ip` (¿cuál uso?)

| Quieres… | Usa |
|----------|-----|
| Ver IP/rutas rápido | `ip a` / `ip r` o `nmcli device show` |
| Conectar a Wi‑Fi / guardar perfil | **`nmcli`** |
| Cambio puntual muy bajo nivel | `ip` (en escritorios NM puede “machacar” el cambio) |

En Mint de escritorio, para el día a día: **`nmcli`**.

## Errores frecuentes

```text
Error: No network with SSID '...' found
```

- SSID mal escrito (mayúsculas/espacios), Wi‑Fi off, o fuera de alcance. `rescan` + `list`.

```text
Error: Connection activation failed... Secrets were required
```

- Contraseña incorrecta o red con otro tipo de seguridad.

```text
Not authorized / Insufficient privileges
```

- Algunas acciones piden estar en el grupo `netdev` o usar `sudo` (según política polkit). En sesión gráfica local suele bastar tu usuario.

- Confundir **nombre del dispositivo** (`wlan0`) con **nombre de la conexión** (`MiCasaFibra`).  
- Poner IP manual mal y perder red: ten a mano otra forma de conectar (cable, panel gráfico) o `ipv4.method auto`.  
- Escribir la contraseña Wi‑Fi en el historial de bash: queda en `history`. En equipos compartidos ten cuidado (o usa el método interactivo del applet gráfico).


## Siguiente

[Debian / Ubuntu / Linux Mint](nmap.md)

## Relacionados

- [ip](../../03-red-paquetes/07-red/ip.md) · [ping](../../03-red-paquetes/07-red/ping.md) · [hostname](../../03-red-paquetes/07-red/hostname.md)  
- [nmap](nmap.md) — ver qué hay en tu LAN **propia**  
- [systemctl](../../04-automatizacion/11-avanzado/systemctl.md) — `NetworkManager`  
