# ip

Herramienta moderna para ver (y cambiar) la configuración de red: interfaces, direcciones IP, rutas…

Sustituye en la práctica a los antiguos `ifconfig` / `route` (aún existen en algunos sistemas, pero `ip` es lo recomendable hoy).

## Sintaxis

```bash
ip [opciones] <objeto> [comando]...
```

> Recuerda: `[ ]` = opcional · `<objeto>` suele ser `addr`, `link`, `route`…  
> Detalle: [Cómo leer la sintaxis](../../README.md#cómo-leer-la-sintaxis-muy-importante).

Al principio casi solo **consultarás**. Cambiar IPs a mano puede cortarte la red; en escritorios suele hacerlo NetworkManager.

## Ejemplos — lo esencial

Ver tus direcciones IP:

```bash
ip addr
# o la forma corta:
ip a
```

Solo una interfaz:

```bash
ip addr show <interfaz>
ip a show eth0
ip a show wlan0
ip a show enp3s0
```

Ver solo IPv4 (más legible al empezar):

```bash
ip -4 addr
ip -4 a
```

Listar interfaces (estado up/down):

```bash
ip link
ip link show
```

Tabla de rutas (por dónde sale el tráfico):

```bash
ip route
# o:
ip r
```

La ruta por defecto suele verse así:

```text
default via 192.168.1.1 dev wlan0
```

Significa: “para salir fuera, pasa por la puerta de enlace `192.168.1.1` usando `wlan0`”.

## Ejemplos útiles para el día a día

```bash
ip -br a
# resumen corto (brief): interfaz, estado, IPs

ip -br link
# interfaces y si están UP/DOWN
```

Sacar “tu IP” de forma rápida (idea):

```bash
hostname -I
# a menudo lista IPs de este host (simple)

ip -4 -br a
# mira la de tu Wi‑Fi/cable (no la de lo = 127.0.0.1)
```

Activar / desactivar interfaz (admin; con cuidado):

```bash
sudo ip link set <interfaz> down
sudo ip link set <interfaz> up
```

## Objetos habituales

| Objeto | Para qué |
|--------|----------|
| `addr` / `a` | Direcciones IP |
| `link` | Interfaces (tarjeta/Wi‑Fi…) |
| `route` / `r` | Rutas |
| `neigh` | Vecinos ARP (quién está en la LAN, a bajo nivel) |

```bash
ip neigh
# o: ip n
```

## Errores frecuentes

- Mirar solo `lo` (`127.0.0.1`): esa es la interfaz de loopback (este equipo), no tu IP en la Wi‑Fi.
- Confundir IPv4 (`192.168…`) con IPv6 (más larga, con `:`).
- Usar `ifconfig` en tutoriales viejos: si no está, instala `net-tools` o mejor aprende `ip`.

```bash
# Debian / Ubuntu / Linux Mint
sudo apt install iproute2
# (ip suele venir ya con el sistema)
```

## Relacionados

- [ping](ping.md) — probar conectividad
- [hostname](hostname.md) — nombre del equipo
- [ss](ss.md) — conexiones y puertos
