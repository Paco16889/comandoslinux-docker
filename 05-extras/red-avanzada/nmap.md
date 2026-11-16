# nmap (introducción / laboratorio)

`nmap` (*Network Mapper*) sirve para **descubrir equipos** en una red y ver **qué puertos** tienen abiertos.  
Es una herramienta potente de administración… y también de auditoría. Aquí la usamos solo como **aficionados en un laboratorio**.

> **Cómo leer la sintaxis:** `<algo>` = lo sustituyes tú · `[algo]` = opcional.  
> Detalle: [Cómo leer la sintaxis](../../README.md#cómo-leer-la-sintaxis-muy-importante).

## Muy importante (léelo antes de nada)

| Puedes usarlo en… | No lo uses en… |
|-------------------|----------------|
| Tu PC (`localhost` / `127.0.0.1`) | Redes ajenas “por curiosidad” |
| Tu red casera / lab **propia** | Empresa, universidad o Wi‑Fi público sin permiso |
| VMs que **tú** controlas | Escanear Internet al azar |

Escanear redes sin autorización puede ser **ilegal** o contra las normas del sitio.  
Esta guía asume: **solo tu red o un lab que te pertenece**.

## Ideas previas

| Concepto | En pocas palabras |
|----------|-------------------|
| Puerto abierto | Hay un servicio escuchando (ej. 22 = SSH, 80 = web) |
| Host | Un equipo con IP (tu PC, el router, una VM…) |
| Escaneo | Preguntar a una IP/puerto “¿hay alguien?” |
| CIDR | Forma de decir “un rango de IPs” (ej. `192.168.1.0/24` = tu LAN típica) |

Antes de `nmap`, conviene haber visto: [ping](../../03-red-paquetes/07-red/ping.md), [ip](../../03-red-paquetes/07-red/ip.md), [ss](../../03-red-paquetes/07-red/ss.md).

## Instalación

```bash
# Debian / Ubuntu / Linux Mint
sudo apt update
sudo apt install nmap

# Fedora
sudo dnf install nmap

# Arch
sudo pacman -S nmap
```

```bash
nmap --version
# comprueba que está instalado
```

En Mint, Flatpak no es lo habitual para nmap: usa `apt`.

## Sintaxis

```bash
nmap [opciones] <objetivo>
```

`<objetivo>` puede ser:

- una IP: `192.168.1.1`
- un nombre: `localhost`
- un rango: `192.168.1.1-20`
- una red: `192.168.1.0/24` (**solo si es tuya**)

## Antes de escanear: ¿cuál es *mi* red?

```bash
ip -br -4 a
hostname -I
ip route
# mira la línea "default via ..." → esa es la puerta (router)
# y la interfaz (wlan0, eth0…) con una IP tipo 192.168.x.x
```

Ejemplo mental:

```text
Tu IP:     192.168.1.40
Router:    192.168.1.1
Red típica: 192.168.1.0/24
```

No copies estos números a ciegas: usa **los de tu** `ip a`.

## Ejemplos — de más suave a más completo

### 1) Escuchar solo tu propio PC (lo más seguro para aprender)

```bash
nmap localhost
nmap 127.0.0.1
```

Verás puertos locales abiertos (puede salir poco o mucho según lo que tengas instalado).

Comparar con lo que ya sabes:

```bash
sudo ss -tlnp
# “qué escucha *este* PC” desde dentro
nmap localhost
# “qué se ve” con nmap hacia localhost
```

### 2) Un solo equipo de tu LAN (ej. el router)

```bash
nmap <IP_router>
nmap 192.168.1.1
# ¡solo si ese es TU router!
```

### 3) Escaneo rápido de puertos comunes

```bash
nmap -F <IP>
# F = Fast: menos puertos, más rápido
```

### 4) Un puerto concreto

```bash
nmap -p <puerto> <IP>
nmap -p 22 <IP>
nmap -p 80,443 <IP>
nmap -p 20-25 <IP>
# rango de puertos
```

¿Tienes SSH/nginx en una VM?

```bash
nmap -p 22,80 <IP_de_la_VM>
```

### 5) Descubrir qué equipos responden en *tu* LAN

```bash
nmap -sn <red>
nmap -sn 192.168.1.0/24
# sn = “ping scan” / descubrimiento de hosts (sin escaneo profundo de puertos)
```

Esto lista quién está encendido (aprox.). Puede tardar un poco.

### 6) Más detalle en un host (lab)

```bash
nmap -sV <IP>
# intenta adivinar versión del servicio (más lento, más info)

sudo nmap -sS <IP>
# SYN scan (clásico); a menudo necesita sudo
```

Al empezar, con `nmap <IP>` y `nmap -p` te basta.

### 7) Guardar resultados

```bash
nmap -oN <fichero>.txt <IP>
# salida “normal” a fichero

nmap -oN scan_local.txt localhost
less scan_local.txt
```

### 8) Salida más legible mientras aprendes

```bash
nmap -v <IP>
# verbose: cuenta qué está haciendo

nmap --reason <IP>
# indica por qué marca un puerto como open/closed
```

## Cómo leer una salida típica (versión novato)

```text
PORT     STATE SERVICE
22/tcp   open  ssh
80/tcp   open  http
443/tcp  closed https
```

| STATE | Idea simple |
|-------|-------------|
| `open` | Algo escucha ahí |
| `closed` | El host responde, pero ese puerto no tiene servicio |
| `filtered` | No queda claro (firewall por medio, etc.) |

`open` en 22 → probablemente SSH.  
`open` en 80 → probablemente web.  
Eso **no** significa automáticamente “está hackeado”: es normal en servidores que tú instalaste.

## Mini laboratorio guiado (casa / VM)

```bash
# 1) Instalar
sudo apt install nmap

# 2) Solo tú
nmap localhost

# 3) Tu IP y router
ip -br -4 a
ip route
nmap -F <IP_router>

# 4) Si tienes nginx/apache en este PC
sudo ss -tlnp | grep ':80'
nmap -p 80 localhost

# 5) Descubrir hosts en TU /24 (opcional)
nmap -sn <tu_red>/24
```

## Lo que NO vamos a enseñar aquí

- Evasión de firewalls, scripts agresivos, ataques.  
- Escanear “a ver qué sale” en la calle o en el trabajo ajeno.  

Si más adelante profundizas, que sea con cursos/labs legales (y permiso).

## Errores frecuentes

```text
nmap: command not found
```

```bash
sudo apt install nmap
```

- Usar la IP de ejemplo del tutorial (`192.168.1.0/24`) cuando tu casa es `192.168.0.x` o `10.x` → mira `ip a`.  
- Asustarse por muchos puertos `closed`: es normal.  
- Confundir “puerto abierto” con “me pueden entrar ya”: hace falta además un servicio vulnerable o mal configurado; aun así, cierra lo que no uses ([ufw](../seguridad/ufw.md)).  
- Lanzar `-A` / scripts raros sin leer: al principio no hace falta.

```text
Note: Host seems down
```

- El equipo apagado, IP mal, o un firewall no responde a los probes. Prueba `ping <IP>` primero.


## Siguiente

[Qué es Docker (introducción)](../../06-docker/00-introduccion/que-es-docker.md)

## Relacionados

- [ping](../../03-red-paquetes/07-red/ping.md) · [ip](../../03-red-paquetes/07-red/ip.md) · [ss](../../03-red-paquetes/07-red/ss.md)  
- [nmcli](nmcli.md) — conectar Wi‑Fi / ver IPs por CLI  
- [ufw](../seguridad/ufw.md) — cerrar lo que no quieras exponer  
- [ssh](../../03-red-paquetes/07-red/ssh.md) · [nginx](../servidores/nginx.md) — servicios que verás como `open`  
