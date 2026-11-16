# top, htop y nvtop

Monitores **en tiempo real** (la pantalla se va actualizando).

| Comando | Para qué | ¿Suele venir instalado? |
|---------|----------|-------------------------|
| `top` | Procesos (CPU/RAM) — clásico | Sí, en casi todas las distros |
| `htop` | Procesos, más cómodo e interactivo | A menudo hay que instalarlo |
| `nvtop` | Uso de **GPU** (NVIDIA y, según versión, otras) | Hay que instalarlo; solo útil si tienes gráfica dedicada / drivers |

## Sintaxis

```bash
top [opciones]
htop [opciones]
nvtop [opciones]
```

> Recuerda: `[ ]` = opcional (no se escriben).  
> Detalle: [Cómo leer la sintaxis](../../README.md#cómo-leer-la-sintaxis-muy-importante).

## top — ejemplos

```bash
top
# abre el monitor; para salir: q
```

Dentro de `top` (teclas útiles):

| Tecla | Qué hace |
|-------|----------|
| `q` | Salir |
| `h` | Ayuda |
| `k` | Matar un proceso (te pide el PID y la señal) |
| `M` | Ordenar por memoria |
| `P` | Ordenar por CPU |
| `N` | Ordenar por PID |
| `1` | Ver cada CPU por separado (si hay varias) |
| `Espacio` | Refrescar ahora |

Solo un usuario:

```bash
top -u <usuario>
```

Actualizar cada cierto tiempo:

```bash
top -d <número>
# <número> = segundos entre refrescos (ej. top -d 2)
```

Salir tras unas iteraciones (útil en scripts o capturas):

```bash
top -b -n 1
# modo batch: una foto y termina (no interactivo)

top -b -n 1 | head -n 20
# primeras líneas de esa foto
```

## Cómo leer la cabecera de top (idea general)

Arriba verás algo parecido a:

- carga del sistema (`load average`)
- cuántos procesos hay
- uso de CPU y de memoria

Abajo, la tabla de procesos (PID, usuario, %CPU, %MEM, comando…), parecida a `ps`.

## htop — ejemplos

Si no está instalado:

```bash
# Debian / Ubuntu / Linux Mint (apt)
sudo apt install htop

# Linux Mint (Flatpak; si lo prefieres desde Flathub)
flatpak search htop
flatpak install flathub <id_de_la_app>

# Fedora
sudo dnf install htop

# Arch
sudo pacman -S htop
```

Para herramientas de sistema como `htop`, en Mint lo habitual sigue siendo `apt`. Flatpak encaja más con apps de escritorio; aquí lo mostramos para que veas el mismo patrón.

```bash
htop
# interfaz con colores y barras; salir: q
```

Dentro de `htop`:

| Tecla / acción | Qué hace |
|----------------|----------|
| `↑` `↓` / ratón | Moverte |
| `F9` o `k` | Enviar señal / matar (menú) |
| `F4` o `/` | Filtrar por nombre |
| `F6` | Cambiar ordenación |
| `F2` | Configurar columnas / opciones |
| `q` | Salir |

```bash
htop -u <usuario>
# solo procesos de ese usuario

htop -p <PID>
# centrarse en un PID (según versión)
```

## nvtop — monitor de GPU

`nvtop` es como un `htop`, pero centrado en la **tarjeta gráfica**: uso de GPU, memoria de vídeo, procesos que la están usando, temperatura (según hardware/drivers), etc.

No sustituye a `top`/`htop`: mira **otra cosa** (la GPU). Si no tienes gráfica dedicada o no tienes drivers adecuados, puede no mostrar datos útiles o fallar al abrir.

### Instalación (ejemplos)

```bash
# Debian / Ubuntu / Linux Mint (apt; si está en los repos de tu versión)
sudo apt install nvtop

# Linux Mint / Flatpak (alternativa si está en Flathub)
flatpak search nvtop
flatpak install flathub <id_de_la_app>

# Fedora
sudo dnf install nvtop

# Arch
sudo pacman -S nvtop
```

Si no aparece en los repos, en la documentación del proyecto suelen indicar otras formas (PPA, Flatpak, compilar…). Eso lo verás mejor en [08-paquetes](../../03-red-paquetes/08-paquetes/), donde también detallamos **Mint + Flatpak**.

### Uso

```bash
nvtop
# interfaz interactiva; salir: q
```

Dentro de `nvtop` (orientativo; puede variar según versión):

| Tecla / acción | Qué hace |
|----------------|----------|
| `q` | Salir |
| `↑` `↓` | Moverte por la lista de procesos de GPU |
| `h` | Ayuda (si está disponible) |
| `F2` / menú | Opciones de interfaz (según versión) |

```bash
nvtop -h
# o: nvtop --help
# opciones de tu versión instalada
```

Cuándo tiene sentido:

```bash
# Estás renderizando, entrenando un modelo, jugando, editando vídeo…
nvtop
# y en otra terminal, si quieres CPU/RAM:
htop
```

## ¿Cuál usar?

| Situación | Recomendación |
|-----------|----------------|
| Quieres algo que siempre esté | `top` |
| Quieres comodidad con procesos CPU/RAM | `htop` |
| Quieres ver la GPU | `nvtop` |
| Solo una consulta rápida en script | `ps` o `top -b -n 1` |

## Errores frecuentes

- Quedarse atrapado: pulsa `q` (no hace falta `Ctrl+C`, aunque a veces también sale).
- En `top`, pulsar `k` y matar el PID equivocado: mira bien el número.
- Pensar que un %CPU alto un segundo es un problema: mira si se mantiene.
- Abrir `nvtop` sin drivers de GPU / sin GPU dedicada: vacío, error o poco útil; no significa que el comando “esté roto”.
- Confundir `htop` y `nvtop`: el primero es sistema (CPU/RAM); el segundo es gráfica.

## Siguiente

[kill](kill.md)

## Relacionados

- [ps](ps.md) — foto puntual
- [kill](kill.md) — matar desde fuera de top/htop
- [free-uptime](free-uptime.md) — memoria RAM y carga de otra forma
- [sensors](../../05-extras/monitorizacion/sensors.md) — temperaturas del hardware (extras)



