# btop (y bashtop)

Monitores de recursos con interfaz **moderna y colorida**: CPU, memoria, disco, red y procesos.  
Son alternativas “bonitas” a `htop`/`top`.

> **Cómo leer la sintaxis:** `<algo>` = lo sustituyes tú · `[algo]` = opcional.  
> Detalle: [Cómo leer la sintaxis](../../README.md#cómo-leer-la-sintaxis-muy-importante).

## Ideas previas

| Concepto | En pocas palabras |
|----------|-------------------|
| `btop` | Reescritura moderna (C++); la opción recomendada hoy |
| `bashtop` | Antecesor en Bash (más pesado; legado) |
| `bpytop` | Variante en Python (también legado/hermanos) |
| Procesos | Igual que en htop: puedes filtrar y enviar señales |

Para aprender y usar a diario: instala **`btop`**. `bashtop` lo mencionamos por si un tutorial antiguo lo cita.

## Instalación

### btop

```bash
# Debian / Ubuntu / Linux Mint
sudo apt update
sudo apt install btop

# Fedora
sudo dnf install btop

# Arch
sudo pacman -S btop
```

Flatpak (muy habitual en Mint si no está en repos o quieres otra vía):

```bash
flatpak search btop
flatpak install flathub <id_de_la_app>
flatpak run <id_de_la_app>
```

Si `apt` no lo trae en tu versión de Mint/Ubuntu, Flatpak o el repo de la distro “backports” suelen ser el plan B.

### bashtop (opcional / legado)

```bash
# a veces:
sudo apt install bashtop
# o vía pip/git en tutoriales antiguos — preferible migrar a btop
```

## Sintaxis

```bash
btop [opciones]
bashtop
```

## Ejemplos — btop

```bash
btop
# abre el panel; salir: q
```

```bash
btop -h
btop --help
```

Preset / configuración (según versión):

```bash
btop --utf-force
# fuerza UTF-8 si los bordes se ven raros

btop -p <número>
# preset de layout (si tu build lo soporta)
```

Config del usuario (ruta típica):

```bash
ls ~/.config/btop/
# btop.conf cuando ya lo has abierto al menos una vez
nano ~/.config/btop/btop.conf
```

### Teclas útiles en btop (orientativas)

| Tecla | Acción típica |
|-------|----------------|
| `q` / `Ctrl+C` | Salir |
| `↑` `↓` / ratón | Mover por procesos |
| `f` | Filtro de procesos |
| `Enter` / menú | Opciones del proceso (señales, etc.) |
| `Esc` | Cerrar menús |
| `1` `2`… | A veces cambian vistas/presets |
| `h` / `F1` | Ayuda |
| `m` | Menú principal (según versión) |

Prueba siempre `h` dentro del programa: la ayuda local es la más fiable.

### Comparar herramientas

```bash
# clásico siempre disponible
top

# cómodo y ligero
htop

# panel completo y visual
btop

# solo GPU
nvtop
```

| Herramienta | Encaje |
|-------------|--------|
| `top` | Está en todos lados |
| `htop` | Procesos, simple y efectivo |
| `btop` | Vista rica (CPU/mem/disco/red) |
| `nvtop` | Solo GPU |
| `sensors` | Temps de hardware en texto |

### Mint: apt vs Flatpak

```bash
# Preferible para CLI “de sistema”:
sudo apt install btop
btop

# Si no está en repos:
flatpak install flathub <id_de_la_app>
```

## Errores frecuentes

```text
btop: command not found
```

```bash
sudo apt install btop
# o Flatpak
```

- Bordes/caja rotos en la terminal: prueba otra terminal, fuente con iconos/nerd font, o `btop --utf-force`.  
- Abrir `bashtop` y que vaya lento: es Bash; pasa a `btop`.  
- Buscar GPU detallada en btop: para NVIDIA/AMD a fondo usa también [nvtop](nvtop.md).


## Siguiente

[Debian / Ubuntu / Linux Mint](nvtop.md)

## Relacionados

- [top/htop/nvtop](../../02-sistema/06-procesos/top-htop.md)  
- [nvtop](nvtop.md) · [sensors](sensors.md) · [iotop](iotop.md)  
- [flatpak](../../03-red-paquetes/08-paquetes/flatpak.md)  
