# du y ncdu

Sirven para responder: **¿qué está comiendo el disco?**  
`du` viene en casi todos los Linux; `ncdu` es interactivo y más cómodo para explorar.

> **Cómo leer la sintaxis:** `<algo>` = lo sustituyes tú · `[algo]` = opcional.  
> Detalle: [Cómo leer la sintaxis](../../README.md#cómo-leer-la-sintaxis-muy-importante).

## Ideas previas

| Concepto | En pocas palabras |
|----------|-------------------|
| Espacio usado | Bytes que ocupan ficheros/carpetas |
| `du` | *Disk usage* — resume tamaños por ruta |
| `df` | Espacio de **particiones** (disco montado), no el detalle por carpeta |
| `ncdu` | Navegas carpetas por tamaño con teclado (como un “du interactivo”) |

`df` = “¿cuánto me queda en el disco?” · `du`/`ncdu` = “¿qué carpeta es la gorda?”

## Instalación

`du` suele venir instalado (`coreutils`).

```bash
# Debian / Ubuntu / Linux Mint
sudo apt update
sudo apt install ncdu

# Fedora
sudo dnf install ncdu

# Arch
sudo pacman -S ncdu
```

```bash
# Flatpak (si aparece en Flathub)
flatpak search ncdu
flatpak install flathub <id_de_la_app>
```

En Mint, para esta herramienta de terminal lo habitual es `apt`.

## Sintaxis

```bash
du [opciones] [<ruta>...]
df [opciones] [<ruta>]
ncdu [opciones] [<ruta>]
```

## Ejemplos — df (contexto rápido)

```bash
df -h
# particiones y espacio libre (human)

df -h /
df -h "$HOME"
```

## Ejemplos — du

Resumen de una carpeta:

```bash
du -h <directorio>
# puede listar TODO el árbol (largo)

du -sh <directorio>
# s = summary (un total) · h = human (G, M…)

du -sh <directorio1> <directorio2> <directorio3>
```

Un nivel de profundidad (muy útil):

```bash
du -h --max-depth=1 <directorio>
# GNU du

du -h -d 1 <directorio>
# en BSD/mac a veces es -d; en Linux GNU suele ser --max-depth
```

Ordenar de mayor a menor (GNU):

```bash
du -h --max-depth=1 <directorio> | sort -h
du -h --max-depth=1 <directorio> | sort -hr
# -hr = human sort, reverse (gordos primero)
```

Home y sospechosos típicos:

```bash
du -sh ~/*
du -sh ~/.cache ~/.local/share/Trash 2>/dev/null
sudo du -sh /var/log /var/cache/apt 2>/dev/null
# apt cache (Debian/Ubuntu/Mint)
```

Excluir carpetas:

```bash
du -sh --exclude='<patrón>' <directorio>
du -sh --exclude='node_modules' <directorio>
```

## Ejemplos — ncdu (recomendado para explorar)

```bash
ncdu <directorio>
ncdu ~
ncdu /home/<usuario>
```

Con permisos de root (todo el sistema; tarda):

```bash
sudo ncdu /
# o más seguro empezar por:
sudo ncdu /var
sudo ncdu /home
```

Dentro de `ncdu`:

| Tecla | Acción |
|-------|--------|
| `↑` `↓` | Mover |
| `Enter` | Entrar en carpeta |
| `←` / `Backspace` | Subir |
| `d` | Borrar (pide confirmación; ¡cuidado!) |
| `g` | Porcentaje / gráfico |
| `n` | Ordenar por nombre |
| `s` | Ordenar por tamaño |
| `q` | Salir |
| `?` | Ayuda |

Escanear y guardar el resultado (para mirarlo después):

```bash
ncdu -o <fichero_scan> <directorio>
ncdu -f <fichero_scan>
# reabre el análisis guardado
```

Excluir:

```bash
ncdu --exclude <directorio_a_saltar> <ruta>
ncdu --exclude .git --exclude node_modules ~
```

## Mini laboratorio

```bash
df -h
du -sh ~
du -h --max-depth=1 ~ | sort -hr | head
ncdu ~
```

## Errores frecuentes

```text
ncdu: command not found
```

```bash
sudo apt install ncdu
```

```text
Permission denied
```

- Carpetas de otros usuarios o del sistema: `sudo ncdu <ruta>` o ignora esas líneas en `du`.

- Confundir `du` con `df`: `du` suma ficheros; `df` mira el sistema de ficheros montado (puede haber diferencias por reserved blocks, borrados abiertos, etc.).  
- Borrar desde `ncdu` con `d` sin leer: puedes cargar la papelera… o borrar de verdad según opciones; confirma siempre.  
- `du -sh /*` como root puede ser eterno y ruidoso: mejor `ncdu` por zonas (`/var`, `/home`).

## Relacionados

- [ls](../../01-fundamentos/01-basicos/ls.md)  
- [find](../../01-fundamentos/02-ficheros-directorios/find.md)  
- [sort](../../01-fundamentos/03-lectura-texto/sort.md) — `sort -h`  
- [iotop](iotop.md) — actividad de disco en vivo (no el tamaño acumulado)  
