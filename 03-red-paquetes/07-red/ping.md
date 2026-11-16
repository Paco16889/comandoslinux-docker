# ping

Comprueba si un equipo o sitio **responde** en la red enviando paquetes pequeños y midiendo el tiempo.

## Sintaxis

```bash
ping [opciones] <destino>
```

> Recuerda: `[ ]` = opcional · `<destino>` = IP o nombre (sin `<` `>`).  
> Detalle: [Cómo leer la sintaxis](../../README.md#cómo-leer-la-sintaxis-muy-importante).

`<destino>` puede ser:

- una IP: `8.8.8.8`
- un nombre: `google.com`
- tu router, otro PC de casa, etc.

## Ejemplos

```bash
ping <destino>
# en Linux sigue hasta que lo pares con Ctrl+C

ping 8.8.8.8
# DNS público de Google (útil para probar “¿tengo salida a Internet?”)

ping google.com
# prueba red + resolución DNS (nombre → IP)

ping -c <número> <destino>
# solo N intentos y termina solo

ping -c 4 8.8.8.8
ping -c 3 google.com
```

Intervalo y tamaño (cuando ya te sientas cómodo):

```bash
ping -i <número> <destino>
# segundos entre pings (ej. -i 2)

ping -c 5 -i 0.5 <destino>
```

Probar la máquina local:

```bash
ping -c 2 localhost
ping -c 2 127.0.0.1
# si esto falla, el problema no es “Internet”: es local
```

## Cómo leer una respuesta típica

```text
64 bytes from 8.8.8.8: icmp_seq=1 ttl=117 time=12.3 ms
```

| Parte | Idea |
|-------|------|
| `bytes from ...` | Hubo respuesta |
| `time=... ms` | Latencia (cuánto tardó); más bajo = mejor |
| Al final (stats) | `% packet loss` — pérdida de paquetes |

Si ves `Destination Host Unreachable` o 100% loss: no hay camino útil hasta ese destino (o un firewall corta ICMP).

## Errores frecuentes

- `ping` no para solo: en Linux es normal; `Ctrl+C` o usa `-c <número>`.
- `ping google.com` falla pero `ping 8.8.8.8` va: suele ser problema de **DNS**, no de cable/Wi‑Fi.
- Algunos servidores **bloquean ping**: que no responda no siempre significa que “Internet esté caído”.
- Hace falta el paquete `iputils-ping` (casi siempre ya viene).

## Siguiente

[ip](ip.md)

## Relacionados

- [ip](ip.md) — ver tu propia IP
- [dig-nslookup](dig-nslookup.md) — si sospechas de DNS
- [traceroute](traceroute.md) — ver por dónde se pierde el camino



