# sensors (lm-sensors)

Muestra **temperaturas**, ventiladores y a veces voltajes del hardware.  
No sustituye a `htop`/`nvtop`: ellos miran procesos/GPU; `sensors` mira sensores del chipset/CPU/placa.

> **Cómo leer la sintaxis:** `<algo>` = lo sustituyes tú · `[algo]` = opcional.  
> Detalle: [Cómo leer la sintaxis](../../README.md#cómo-leer-la-sintaxis-muy-importante).

## Ideas previas

| Concepto | En pocas palabras |
|----------|-------------------|
| Sensor | Chip que mide temperatura, rpm del ventilador, voltaje… |
| lm-sensors | Paquete/herramientas para leer esos sensores en Linux |
| `sensors-detect` | Asistente que busca qué sensores tienes (una vez) |

## Instalación

```bash
# Debian / Ubuntu / Linux Mint
sudo apt update
sudo apt install lm-sensors

# Fedora
sudo dnf install lm_sensors

# Arch
sudo pacman -S lm_sensors
```

En Mint, Flatpak no es la vía habitual para esto: usa `apt`.

## Configurar (primera vez)

```bash
sudo sensors-detect
```

Te hará preguntas. Para empezar, en la mayoría de PCs de escritorio/portátil puedes aceptar los valores por defecto (pulsar Enter).  
Al final suele preguntar si añadir módulos automáticamente → responde `yes` si te lo pide y confías en el equipo.

Cargar módulos (si hace falta):

```bash
sudo service kmod start
# o en sistemas systemd a veces basta un reinicio / modprobe
```

En la práctica, tras `sensors-detect`, prueba:

```bash
sensors
```

Si no sale nada útil, reinicia y vuelve a ejecutar `sensors`.

## Uso

### Sintaxis

```bash
sensors [opciones]
```

### Ejemplos

```bash
sensors
# listado de sensores detectados (temps, fans…)

sensors -A
# sin cabeceras de adaptador (salida más compacta, según versión)

sensors | less
# si la salida es larga
```

Actualizar en bucle (cada 2 segundos):

```bash
watch -n 2 sensors
# Ctrl+C para salir

watch -n 1 sensors -A
```

Filtrar con lo que ya sabes:

```bash
sensors | grep -i temp
sensors | grep -i fan
sensors | grep -E 'Core|Tctl|Package'
# nombres típicos de CPU (varían según hardware)
```

Guardar una captura:

```bash
sensors > <fichero>_temps.txt
date | tee <fichero>_temps.txt
sensors | tee -a <fichero>_temps.txt
```

### ¿Qué mirar como principiante?

| Si ves… | Idea |
|---------|------|
| `Core 0`, `Tctl`, `Package id 0` | Temperaturas de CPU (nombres cambian) |
| `fan1`, `fan2`, rpm | Velocidad de ventiladores |
| Valores a `--` o `N/A` | Sensor no conectado / no aplicable |
| Temps muy altas en reposo | Polvo, pasta térmica, ventilador parado… (diagnóstico físico) |

No hay un único “número correcto”: depende de CPU, portátil vs sobremesa, carga (`htop` a la vez ayuda).

## Combinar con otros monitores

```bash
# Terminal 1
htop

# Terminal 2
watch -n 2 sensors

# Si tienes GPU NVIDIA/AMD compatible
nvtop
```

## Desinstalación (si quieres quitarlo)

```bash
# Debian / Ubuntu / Linux Mint
sudo apt remove lm-sensors
```

## Errores frecuentes

```text
sensors: command not found
```

```bash
sudo apt install lm-sensors
# Mint/Ubuntu/Debian
```

```text
No sensors found!
```

- Ejecuta `sudo sensors-detect` y reinicia.  
- En algunas máquinas virtuales **no hay** sensores reales: es normal que no aparezca nada.  
- Portátiles/OEM a veces necesitan sensores concretos o no exponen todo.

- Comparar temps entre dos PCs distintos como si fueran la misma escala: cada modelo es diferente.  
- Asustarse por un pico corto bajo carga: mira si baja al dejar de cargar la CPU.

## Siguiente

[Debian / Ubuntu / Linux Mint](ncdu-du.md)

## Relacionados

- [top/htop/nvtop](../../02-sistema/06-procesos/top-htop.md) — uso de CPU/GPU  
- [free/uptime](../../02-sistema/06-procesos/free-uptime.md) — RAM y carga  
- [apt](../../03-red-paquetes/08-paquetes/apt.md) — instalación en Mint



