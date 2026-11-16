# nvtop (ampliación)

Monitor en tiempo real de la **GPU** (uso, memoria de vídeo, procesos, temps según drivers).  
Es el “htop de la gráfica”. La intro corta está en el núcleo; aquí ampliamos instalación, drivers y trucos.

> **Cómo leer la sintaxis:** `<algo>` = lo sustituyes tú · `[algo]` = opcional.  
> Detalle: [Cómo leer la sintaxis](../../README.md#cómo-leer-la-sintaxis-muy-importante).

## Ideas previas

| Concepto | En pocas palabras |
|----------|-------------------|
| GPU | Tarjeta gráfica (NVIDIA, AMD, a veces Intel) |
| VRAM | Memoria de la GPU (no es la RAM del sistema) |
| Driver | Software que hace hablar a Linux con la GPU |
| `nvtop` | Interfaz tipo htop centrada en la GPU |

Sin driver adecuado (o en una VM sin GPU passthrough), `nvtop` puede abrir vacío o fallar: no siempre es “culpa” del comando.

## Instalación

```bash
# Debian / Ubuntu / Linux Mint
sudo apt update
sudo apt install nvtop

# Fedora
sudo dnf install nvtop

# Arch
sudo pacman -S nvtop
```

Flatpak (si está en Flathub; útil en Mint para apps, menos crítico aquí):

```bash
flatpak search nvtop
flatpak install flathub <id_de_la_app>
```

Si `apt` no encuentra el paquete en tu versión:

```bash
apt search nvtop
# a veces hace falta un PPA o compilar; mira la doc del proyecto
```

## Drivers (visión práctica)

| GPU | Enfoque habitual en escritorio |
|-----|--------------------------------|
| NVIDIA | Drivers propietarios (`nvidia-driver-…`) o nouveau (limitado) |
| AMD | `mesa` + a menudo funciona “out of the box” |
| Intel | i915 / xe según generación |

Comprobar si el sistema “ve” la GPU:

```bash
lspci | grep -iE 'vga|3d|display'
# qué hardware hay

lsmod | grep -iE 'nvidia|amdgpu|i915'
# módulos cargados (orientativo)
```

NVIDIA (ejemplos; el nombre del paquete cambia según distro/versión):

```bash
# Debian / Ubuntu / Linux Mint — buscar el metapaquete
apt search nvidia-driver
# luego, con cuidado y tras leer qué recomienda tu versión:
# sudo ubuntu-drivers autoinstall     # Ubuntu/Mint a menudo
# sudo apt install nvidia-driver-<número>
```

```bash
nvidia-smi
# si tienes driver NVIDIA propietario: resumen de GPU/procesos
```

Tras instalar/cambiar drivers suele hacer falta **reiniciar**.

## Sintaxis

```bash
nvtop [opciones]
```

## Ejemplos

```bash
nvtop
# interfaz interactiva; salir: q
```

Dentro de `nvtop` (orientativo; puede variar según versión):

| Tecla | Acción típica |
|-------|----------------|
| `q` | Salir |
| `↑` `↓` | Mover por procesos de GPU |
| `h` / `F1` | Ayuda (si está) |
| `F2` | Setup / opciones |
| `k` | Matar proceso (con cuidado; según build) |

```bash
nvtop -h
nvtop --help
# opciones de TU versión
```

Varias GPUs / máquina con iGPU + dedicada:

```bash
nvtop
# arriba suele listar dispositivos; mira cuál se pone a trabajar
```

Flujo típico de diagnosis:

```bash
# Terminal 1 — CPU/RAM
htop

# Terminal 2 — GPU
nvtop

# Terminal 3 — sensores placa/CPU
watch -n 2 sensors
```

Solo consultar sin interfaz (si tienes NVIDIA y `nvidia-smi`):

```bash
nvidia-smi
nvidia-smi -l 2
# refresco cada 2 s
```

Guardar una “foto” de ayuda al pedir soporte:

```bash
nvidia-smi > <fichero>_gpu.txt
lspci | grep -iE 'vga|3d' | tee -a <fichero>_gpu.txt
```

## Errores frecuentes

```text
nvtop: command not found
```

```bash
sudo apt install nvtop
```

```text
No GPU to monitor / vacío
```

- Sin driver, GPU desconectada, o VM sin GPU. Prueba `lspci` y `nvidia-smi` / `glxinfo` según caso.

- Confundir uso de **RAM** (`htop`/`free`) con **VRAM** (`nvtop`): son memorias distintas.  
- Matar el proceso gráfico del escritorio desde nvtop: puedes quedarte sin sesión gráfica; ten cuidado con PIDs del compositor/`Xorg`/`gnome-shell`.


## Siguiente

[vsftpd (servidor FTP)](../servidores/vsftpd.md)

## Relacionados

- Intro en el núcleo: [top/htop/nvtop](../../02-sistema/06-procesos/top-htop.md)  
- [sensors](sensors.md) — temperaturas de placa/CPU  
- [btop](btop.md) — monitor general “bonito”  
- [apt](../../03-red-paquetes/08-paquetes/apt.md)  
