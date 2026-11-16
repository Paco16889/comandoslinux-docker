# Red avanzada (extras)

Más allá de ping/ip/ssh del núcleo: Wi‑Fi por terminal y exploración de red **en laboratorio propio**.

> **Cómo leer la sintaxis:** [guía](../../README.md#cómo-leer-la-sintaxis-muy-importante).  
> Piensa en aficionados que acaban el bloque [07-red](../../03-red-paquetes/07-red/).

## Orden recomendado

1. [nmcli](nmcli.md) — ver red, Wi‑Fi, conexiones (uso diario en Mint)  
2. [nmap](nmap.md) — ver hosts/puertos (**solo tu red / lab**)  

| Documento | Qué cubre | Estado |
|-----------|-----------|--------|
| [nmcli](nmcli.md) | NetworkManager por CLI | Listo |
| [nmap](nmap.md) | Escaneo introductorio ético | Listo |

## Avisos para novatos

- **nmcli** es seguro de practicar en tu propio PC.  
- **nmap** solo contra equipos/redes que te pertenecen o con permiso explícito.  
- Si algo de red “se rompe” con IP manual, vuelve a DHCP (`ipv4.method auto`) o al panel gráfico de Mint.

Relacionado: [07-red](../../03-red-paquetes/07-red/), [ufw](../seguridad/ufw.md).
