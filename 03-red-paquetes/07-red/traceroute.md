# traceroute / tracepath

Muestran el **camino** (saltos/routers) que siguen los paquetes hasta un destino.

Útil cuando `ping` falla a medias o la red va lenta y quieres ver **dónde** se corta o se atrasa.

## Sintaxis

```bash
traceroute [opciones] <destino>
tracepath [opciones] <destino>
```

> Recuerda: `[ ]` = opcional · `<destino>` = IP o nombre.  
> Detalle: [Cómo leer la sintaxis](../../README.md#cómo-leer-la-sintaxis-muy-importante).

- `traceroute` → clásico (a veces hay que instalarlo; puede necesitar privilegios según opciones)
- `tracepath` → suele venir en muchas distros y no requiere root para lo básico

## Ejemplos con tracepath (buen comienzo)

```bash
tracepath <destino>
tracepath 8.8.8.8
tracepath google.com
```

Salida: una línea por salto, con tiempos. Los primeros suelen ser tu router casero; luego tu ISP; luego Internet.

```text
 1?: [LOCALHOST]                      pmtu 1500
 1:  192.168.1.1                       1.234ms
 2:  10.0.0.1                          12.456ms
 3:  ...
 5:  8.8.8.8                           28.100ms reached
```

```bash
# tu salida real tendrá IPs/nombres distintos; la idea es: salto → tiempo
# “reached” = llegó al destino
```

## Ejemplos con traceroute

```bash
traceroute <destino>
traceroute 8.8.8.8
traceroute google.com
# cabecera típica: traceroute to 8.8.8.8 (8.8.8.8), 30 hops max…
# luego líneas 1, 2, 3… con IP y tres tiempos (ms)
```

Ejemplo de aspecto:

```text
traceroute to 8.8.8.8 (8.8.8.8), 30 hops max, 60 byte packets
 1  192.168.1.1 (192.168.1.1)  1.120 ms  1.050 ms  0.980 ms
 2  10.0.0.1 (10.0.0.1)  15.200 ms  14.800 ms  15.010 ms
 3  * * *
 4  8.8.8.8 (8.8.8.8)  27.300 ms  26.900 ms  27.100 ms
```

Limitar saltos:

```bash
traceroute -m <número> <destino>
traceroute -m 15 8.8.8.8
# como mucho 15 saltos; si no llega antes, se corta ahí
```

Usar ICMP (a veces atraviesa mejor ciertos filtros):

```bash
sudo traceroute -I <destino>
# misma idea de listado por saltos; a veces menos `* * *` en redes filtradas
```

## Cómo interpretar (versión principiante)

| Lo que ves | Posible lectura |
|------------|-----------------|
| `* * *` | Ese salto no respondió (puede ser firewall; no siempre es “caído”) |
| Tiempo bajo al inicio y alto después | La demora está más lejos |
| Se corta siempre en el mismo salto | Sospecha de ese tramo / filtrado |

No hace falta entender cada router: mira **hasta dónde llega** y si hay un salto claramente problemático.

## Instalación si falta

```bash
# Debian / Ubuntu / Linux Mint
sudo apt install traceroute iputils-tracepath

# Fedora
sudo dnf install traceroute

# Arch
sudo pacman -S traceroute
```

## Errores frecuentes

- Asustarse por `* * *`: es habitual; muchos equipos no contestan a ese tipo de sondas.
- Confundir “no completa traceroute” con “no hay Internet”: prueba también `ping` y `curl`.
- Necesitar `sudo` en algunos modos de `traceroute` y pensar que el comando no existe.

## Relacionados

- [ping](ping.md) — ¿responde el destino?
- [ip](ip.md) — tu puerta de enlace (`ip route`)
- [dig-nslookup](dig-nslookup.md) — si el nombre no resuelve
