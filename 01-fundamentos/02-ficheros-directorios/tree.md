# tree

Muestra el contenido de un directorio en forma de árbol.

**Nota:** no viene instalado en todas las distros. Si falla, instálalo con el gestor de paquetes (ver bloque `08-paquetes`).

## Sintaxis

```bash
tree [opciones] [<directorio>]
```

Sin `<directorio>`, usa el directorio actual.

## Ejemplos

```bash
tree
# árbol del directorio actual

tree <directorio>
# árbol de esa carpeta

tree /home/<usuario>/<directorio>
# con ruta absoluta
```

Limitar y filtrar:

```bash
tree -L 2 <directorio>
# solo 2 niveles de profundidad

tree -a <directorio>
# incluye ocultos (empiezan por .)

tree -d <directorio>
# solo directorios, sin ficheros

tree -h <directorio>
# tamaños en formato legible (según versión)

tree -f <directorio>
# muestra la ruta completa de cada entrada
```

Salida a fichero:

```bash
tree <directorio> > <fichero>
# guarda el árbol en <fichero>
```

## Opciones útiles

| Opción | Qué hace |
|--------|----------|
| `-L <número>` | Máxima profundidad |
| `-a` | Incluye ocultos |
| `-d` | Solo directorios |
| `-f` | Rutas completas |
| `-h` | Tamaños legibles (si está disponible) |
| `-C` | Colores (si la terminal lo soporta) |

## Errores frecuentes

```text
tree: command not found
```

- No está instalado. Ejemplos (según distro):

```bash
# Debian / Ubuntu / Linux Mint (apt)
sudo apt install tree

# Linux Mint (Flatpak, si aplica)
flatpak search tree
flatpak install flathub <id_de_la_app>

# Fedora
sudo dnf install tree

# Arch
sudo pacman -S tree
```

  En Mint, para utilidades de terminal como `tree`, lo normal es `apt`; Flatpak se usa más para apps gráficas (lo veremos en `08-paquetes`).

- Árbol enorme e ilegible: usa `-L <número>` (por ejemplo `-L 2`).

## Siguiente

[cat](../03-lectura-texto/cat.md)

## Relacionados

- [ls](../01-basicos/ls.md) — listado clásico (`ls -R` es más tosco)
- [find](find.md) — buscar en lugar de solo visualizar
- [mkdir](mkdir.md) — crear la estructura que luego ves con `tree`



