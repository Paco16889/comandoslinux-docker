# wget

Descarga ficheros desde la red de forma sencilla (HTTP, HTTPS, FTP…).

## Sintaxis

```bash
wget [opciones] <URL>
```

> Recuerda: `[ ]` = opcional · `<URL>` = lo sustituyes tú.  
> Detalle: [Cómo leer la sintaxis](../../README.md#cómo-leer-la-sintaxis-muy-importante).

## Ejemplos

```bash
wget <URL>
# descarga en el directorio actual con el nombre remoto

wget https://example.com/archivo.zip
```

Elegir nombre o carpeta:

```bash
wget -O <fichero> <URL>
# guarda como <fichero>

wget -O paquete.tar.gz <URL>
wget -P <directorio>/ <URL>
# descarga dentro de <directorio>
```

Continuar una descarga a medias:

```bash
wget -c <URL>
# continue: muy útil si se cortó la red
```

Silencioso / con log:

```bash
wget -q <URL>
# quiet

wget -o <fichero_log> <URL>
# guarda el log en un fichero
```

Varios intentos si falla:

```bash
wget --tries=<número> <URL>
wget --tries=5 <URL>
```

Descargar una lista de URLs:

```bash
wget -i <fichero>
# <fichero> tiene una URL por línea
```

## Ejemplo de flujo

```bash
cd <directorio>
wget -c -O <fichero>.iso <URL>
ls -lh <fichero>.iso
```

## Si no está instalado

```bash
# Debian / Ubuntu / Linux Mint
sudo apt install wget

# Fedora
sudo dnf install wget

# Arch
sudo pacman -S wget

# Linux Mint / Flatpak (poco habitual para wget; suele bastar apt)
# flatpak search wget
```

## wget vs curl

```bash
wget <URL>
# se centra en descargar a disco

curl -O <URL>
# equivalente cercano; curl brilla más con APIs
```

## Errores frecuentes

```text
wget: unable to resolve host address
```

- Problema de DNS o de red (prueba `ping` / `dig`).

- Enlaces que requieren navegador/login: `wget` no “rellena” formularios complejos solo; a veces hace falta `curl` con cookies/cabeceras.
- Descargar sin `-O` y no encontrar el fichero: mira el nombre que eligió en el directorio actual (`ls -lt`).

## Relacionados

- [curl](curl.md) — más flexible con HTTP
- [scp](scp.md) — copiar desde/hacia otro equipo por SSH
- [ping](ping.md) — comprobar red antes de descargar
