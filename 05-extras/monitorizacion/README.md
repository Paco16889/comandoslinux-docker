# Monitorización (extras)

Ver CPU/GPU, disco y sensores de hardware.

> **Cómo leer la sintaxis:** [guía](../../README.md#cómo-leer-la-sintaxis-muy-importante).

## Orden recomendado

1. [sensors](sensors.md) — temperaturas / ventiladores  
2. [ncdu-du](ncdu-du.md) — qué come espacio en disco  
3. [iotop](iotop.md) — quién usa el disco ahora  
4. [btop](btop.md) — panel general “bonito”  
5. [nvtop](nvtop.md) — GPU en detalle (+ drivers)  

| Documento | Qué cubre | Estado |
|-----------|-----------|--------|
| [sensors](sensors.md) | lm-sensors | Listo |
| [ncdu-du](ncdu-du.md) | `du` + `ncdu` | Listo |
| [iotop](iotop.md) | I/O en tiempo real | Listo |
| [btop](btop.md) | btop / bashtop | Listo |
| [nvtop](nvtop.md) | GPU + drivers / tips | Listo |

## Combo útil (varias terminales)

```bash
btop          # visión general
sudo iotop -o # disco en vivo
nvtop         # GPU (si aplica)
watch -n 2 sensors
ncdu ~        # limpiar espacio
```

Relacionado en el núcleo: [top/htop/nvtop intro](../../02-sistema/06-procesos/top-htop.md), [free/uptime](../../02-sistema/06-procesos/free-uptime.md).
