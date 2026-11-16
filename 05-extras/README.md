# 05 — Extras

Herramientas y servicios **concretos** que no forman parte del camino principal 01–11.  
Úsalos cuando ya tengas soltura con terminal, permisos, paquetes y (para servidores/red) lo básico de `07-red`.

> **Cómo leer la sintaxis:** `<algo>` = lo sustituyes tú · `[algo]` = opcional.  
> Guía completa: [Cómo leer la sintaxis](../README.md#cómo-leer-la-sintaxis-muy-importante).

## Carpetas

| Carpeta | Contenido | Estado |
|---------|-----------|--------|
| [monitorizacion](monitorizacion/) | sensors, nvtop, ncdu/du, iotop, btop | Listo |
| [servidores](servidores/) | vsftpd, nginx, apache, samba, fail2ban | Listo |
| [seguridad](seguridad/) | ufw, ssh endurecido | Listo |
| [red-avanzada](red-avanzada/) | nmap (lab), nmcli | Listo |

## Por qué algunos temas no están en `07-red`

| Tema | Dónde | Motivo |
|------|-------|--------|
| `ufw`, SSH endurecido | `seguridad/` | Son endurecimiento/política, no “primeros pasos de red” |
| `nmap`, `nmcli` | `red-avanzada/` | Útiles, pero confunden si se mezclan con ping/ip/ssh básicos |

Desde [07-red](../03-red-paquetes/07-red/) hay enlaces a estos extras.

## Antes de empezar

1. Paquetes: [08-paquetes](../03-red-paquetes/08-paquetes/)  
2. Red básica: [07-red](../03-red-paquetes/07-red/)  
3. Servicios: [systemctl](../04-automatizacion/11-avanzado/systemctl.md)  
4. Practica en lab/VM; no abras servicios a Internet sin saber qué haces  

## Relacionado

- Temario principal: [README raíz](../README.md)  
- Ideas pendientes: [extras-futuro](../04-automatizacion/11-avanzado/extras-futuro.md)  
