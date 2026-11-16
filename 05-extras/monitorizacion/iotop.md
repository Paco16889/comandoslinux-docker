# iotop

Muestra **qué procesos están usando el disco** ahora mismo (lecturas/escrituras), al estilo de `top` pero para I/O.

> **Cómo leer la sintaxis:** `<algo>` = lo sustituyes tú · `[algo]` = opcional.  
> Detalle: [Cómo leer la sintaxis](../../README.md#cómo-leer-la-sintaxis-muy-importante).

## Ideas previas

| Concepto | En pocas palabras |
|----------|-------------------|
| I/O | Entrada/salida: leer y escribir en disco (o SSD) |
| DISK READ / WRITE | Velocidad de lectura/escritura del proceso |
| SWAPIN / IO | Columnas típicas: uso de swap e I/O wait del proceso |
| Root | `iotop` casi siempre necesita privilegios de administrador |

`ncdu`/`du` = “qué ocupa sitio”.  
`iotop` = “quién está machacando el disco **ahora**”.

## Instalación

```bash
# Debian / Ubuntu / Linux Mint
sudo apt update
sudo apt install iotop

# Fedora
sudo dnf install iotop

# Arch
sudo pacman -S iotop
```

```bash
# Flatpak (poco habitual para iotop; prioriza apt)
flatpak search iotop
```

## Sintaxis

```bash
sudo iotop [opciones]
```

Sin `sudo` suele fallar o mostrar datos incompletos.

## Ejemplos

```bash
sudo iotop
# vista interactiva; salir: q
```

Solo procesos con I/O real (menos ruido):

```bash
sudo iotop -o
# only: oculta los que están a cero
```

Sin interactivo (una foto / batch):

```bash
sudo iotop -b -n 3
# b = batch · n = número de iteraciones

sudo iotop -bo -n 5
# solo activos, 5 muestras
```

Retraso entre refrescos:

```bash
sudo iotop -d <número>
# segundos (ej. -d 2)
sudo iotop -od 2
```

Acumular totales:

```bash
sudo iotop -a
# accumulated: suma desde que abriste iotop
```

Procesos de un usuario:

```bash
sudo iotop -u <usuario>
sudo iotop -ou <usuario>
```

Ordenar por columna (según versión; mira ayuda):

```bash
sudo iotop -h
# teclas en modo interactivo: izquierda/derecha suelen cambiar orden
```

### Dentro de iotop (teclas habituales)

| Tecla | Acción típica |
|-------|----------------|
| `q` | Salir |
| `o` | Alternar “solo con I/O” |
| `←` `→` | Cambiar columna de orden |
| `p` | Toggle threads / procesos (según versión) |
| `a` | Totales acumulados |
| `i` | Prioridad I/O (avanzado; ionice) |

### Flujo cuando el PC “va a tirones”

```bash
# Terminal 1
htop
# ¿CPU a tope o esperando?

# Terminal 2
sudo iotop -o
# ¿alguien escribiendo/leyendo a lo loco?

# Terminal 3 (si sospechas de espacio)
ncdu ~
```

Casos típicos que verás: indexadores, backups, `apt`/`flatpak` bajando, navegador, máquinas virtuales, `rsync`.

Guardar una muestra:

```bash
sudo iotop -bo -n 10 > <fichero>_iotop.txt
```

## Errores frecuentes

```text
iotop: command not found
```

```bash
sudo apt install iotop
```

```text
Netlink error / Permission denied / Configura el kernel…
```

- Ejecuta con `sudo`.  
- Kernels muy mínimos o contenedores pueden no exponer la info que iotop necesita.

- Confundir un pico corto (actualizar paquetes) con un problema permanente: observa unos segundos con `-o`.  
- Culpar al disco cuando el cuello de botella es CPU/red: combina con `htop` y `nvtop` si aplica.  
- En SSD los picos son normales al instalar o compilar; mira si **se mantiene** alto.


## Siguiente

[Debian / Ubuntu / Linux Mint](btop.md)

## Relacionados

- [top/htop](../../02-sistema/06-procesos/top-htop.md) — CPU/RAM  
- [ncdu-du](ncdu-du.md) — espacio ocupado  
- [ps](../../02-sistema/06-procesos/ps.md)  
- [btop](btop.md) — a menudo incluye vista de disco más amigable  
