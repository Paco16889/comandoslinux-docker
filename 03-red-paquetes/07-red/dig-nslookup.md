# dig y nslookup

Consultan el **DNS**: traducen nombres (`example.com`) a IPs y viceversa (según registros).

Si `ping google.com` falla pero `ping 8.8.8.8` funciona, mira aquí.

## Sintaxis

```bash
dig [<nombre>] [tipo]
dig @<servidor_dns> <nombre>
nslookup <nombre> [<servidor_dns>]
```

> Recuerda: `[ ]` = opcional · `<nombre>`, `<servidor_dns>` = los pones tú.  
> Detalle: [Cómo leer la sintaxis](../../README.md#cómo-leer-la-sintaxis-muy-importante).

- `dig` → salida más clara y completa (preferida en administración)
- `nslookup` → clásica; a veces más simple para una prueba rápida

## dig — ejemplos

```bash
dig <nombre>
dig example.com
# respuesta con secciones; busca la línea ANSWER con la IP
```

Solo la respuesta corta (muy didáctico):

```bash
dig +short <nombre>
dig +short example.com
# 93.184.216.34   (ejemplo; el número real puede cambiar)
```

Preguntar a un DNS concreto:

```bash
dig @8.8.8.8 <nombre>
dig @1.1.1.1 example.com
dig @8.8.8.8 +short google.com
```

Tipos de registro habituales:

```bash
dig <nombre> A
# IPv4

dig <nombre> AAAA
# IPv6

dig <nombre> MX
# servidores de correo

dig <nombre> NS
# servidores DNS autoritarios del dominio
```

IP → nombre (reverse), cuando aplica:

```bash
dig -x <IP>
dig -x 8.8.8.8
```

## nslookup — ejemplos

```bash
nslookup <nombre>
nslookup example.com

nslookup <nombre> <servidor_dns>
nslookup example.com 8.8.8.8
```

Modo interactivo:

```bash
nslookup
# > example.com
# > server 8.8.8.8
# > example.com
# > exit
```

## Mini diagnosis DNS

```bash
ping -c 2 8.8.8.8
# ¿hay Internet “por IP”?

dig +short example.com
# ¿el nombre resuelve?

cat /etc/resolv.conf
# qué DNS está usando el sistema (en systemd-resolved a veces verás 127.0.0.53)
```

## Instalación si faltan

```bash
# Debian / Ubuntu / Linux Mint
sudo apt install dnsutils

# Fedora
sudo dnf install bind-utils

# Arch
sudo pacman -S bind
# (paquete que proporciona dig; el nombre puede variar ligeramente)
```

## Errores frecuentes

- No hay sección `ANSWER`: el nombre no existe o el DNS no respondió bien (`NXDOMAIN`, timeout…).
- Confiar en una IP “para siempre”: pueden cambiar (CDN, balanceo).
- En equipos con `systemd-resolved`, mirar solo `/etc/resolv.conf` puede confundir; `dig` sigue siendo una buena prueba práctica.

## Siguiente

[Conceptos de paquetes](../08-paquetes/conceptos.md)

## Relacionados

- [ping](ping.md) — separar “fallo DNS” vs “fallo de red”
- [curl](curl.md) — si el DNS va pero la web no
- [ip](ip.md) — configuración local



