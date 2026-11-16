# El sistema de ficheros: todo cuelga de `/`

En Linux **no hay letras de unidad** como `C:` o `D:` a la vista del usuario.  
Hay un único árbol que empieza en la raíz:

```text
/
├── home/
├── etc/
├── var/
├── usr/
└── …
```

Cualquier disco, USB o partición se **monta** en algún sitio de ese árbol (por ejemplo un USB en `/media/<usuario>/...`).

## Ideas previas

| Concepto | En pocas palabras |
|----------|-------------------|
| `/` | Raíz del sistema (“la carpeta de todas las carpetas”) |
| Ruta absoluta | Empieza por `/` → `/home/ana/Documentos` |
| Ruta relativa | Desde donde estás → `Documentos` o `../otro` |
| Montar | “Enganchar” un dispositivo en una carpeta del árbol |
| Fichero y directorio | En el día a día: archivo y carpeta |

## Absoluta vs relativa (con ejemplos)

Imagina que estás en `/home/<usuario>`:

```bash
pwd
# /home/<usuario>

ls Documentos
# relativa: busca Documentos DENTRO de donde estás

ls /home/<usuario>/Documentos
# absoluta: da igual dónde estés; siempre es esa ruta

ls /
# contenido de la raíz
```

```bash
cd /etc
pwd
# /etc

cd /
pwd
# /
```

## Discos y USB: ¿dónde están?

```bash
ls /media
ls /run/media/<usuario>
# sitios típicos donde el escritorio monta USBs

lsblk
# discos y particiones (visión “hardware”)

df -h
# sistemas de ficheros montados y espacio
```

Un pendrive no es `E:\`. Según distro/escritorio verás algo como:

```text
/media/<usuario>/USB_NOMBRE/          # muy visto en Ubuntu/Mint
/run/media/<usuario>/USB_NOMBRE/      # muy visto en Fedora (y otros)
```

```bash
# ¿dónde te lo montó a ti?
ls /media/"$USER" 2>/dev/null
ls /run/media/"$USER" 2>/dev/null
```

## Todo es un fichero (idea)

En Linux muchas cosas del sistema se representan como ficheros:

| Ejemplo | Idea |
|---------|------|
| `/dev/sda` | Un disco (dispositivo) |
| `/proc/` | Info del kernel/procesos (virtual) |
| `/sys/` | Info del sistema/hardware (virtual) |

Al principio **no hace falta entrar** a `/proc` o `/sys`. Solo recuerda: la filosofía es “un árbol de rutas”.

## Comprobar el árbol en tu máquina

```bash
ls /
# verás home, etc, var, usr, bin, …

sudo tree -L 1 /
# si tienes tree instalado (ver guía de ficheros)
```

## Errores frecuentes

- Escribir rutas con `\`: en Linux es `/`.  
- Confundir `/` (raíz) con `/home` o con “mi carpeta personal”.  
- Borrar cosas bajo `/` con `sudo` “para probar”: puedes romper el sistema. En el prólogo solo **miramos**.  
- Pensar que `C:` tiene que existir: no.

## Relacionados

- [arbol-de-directorios](arbol-de-directorios.md) — qué significa cada carpeta grande  
- [cd / pwd / ls](../01-fundamentos/01-basicos/)  
