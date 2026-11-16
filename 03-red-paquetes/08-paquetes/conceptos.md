# Conceptos de paquetes

Antes de los comandos, estas ideas evitan muchos sustos.

## Qué hace el gestor por ti

Cuando escribes algo como “instala `htop`”, el gestor:

1. Busca el paquete en los **repositorios** configurados  
2. Mira qué **dependencias** necesita  
3. Las descarga e instala en el orden correcto  
4. Deja el programa listo (o casi) en el sistema  

Por eso no sueles bajar un `.exe` a mano como en Windows: el sistema ya tiene “tiendas” de software por terminal.

## Repositorio

Es una fuente de paquetes (servidores de tu distro, o extras como Flathub).

```bash
# ejemplos de “dónde mira” cada mundo (idea):
# apt     → listas en /etc/apt/
# dnf     → /etc/yum.repos.d/
# pacman  → /etc/pacman.conf y mirrorlist
# flatpak → remotes (flathub, etc.)
```

Si un programa “no aparece”, a menudo:

- el nombre del paquete es otro, o  
- falta un repositorio / Flatpak remote, o  
- en tu versión de la distro aún no está.

## Dependencias (ejemplo mental)

Quieres el programa **A**, pero A necesita la librería **B**, y B necesita **C**.  
El gestor instala C → B → A. Tú solo pediste A.

Por eso a veces al instalar “una cosa” ves que baja muchas. Es normal.

## Actualizar ≠ solo actualizar un programa

En distros Linux suele haber dos ideas:

| Idea | Significado típico |
|------|---------------------|
| Actualizar índices / caché | “Refrescar el catálogo” de lo disponible |
| Actualizar paquetes | Instalar versiones nuevas de lo ya instalado |

En `apt` se ve muy claro: primero `update`, luego `upgrade` (lo detallamos en [apt](apt.md)).

## Varias formas de instalar en el mismo PC

En **Linux Mint** es especialmente habitual:

| Vía | Para qué |
|-----|----------|
| `apt` | Sistema, herramientas de terminal, librerías |
| Flatpak | Apps gráficas (navegador, GIMP, Discord…) desde Flathub |
| .deb suelto | A veces; mejor evitar si hay paquete oficial |
| AppImage / otros | Casos puntuales |

No es que una sea “mala”: son canales distintos. El lío empieza cuando mezclas sin saber qué instaló cada una.

## Buenas costumbres (desde cero)

1. Actualiza el catálogo antes de instalar (`apt update`, `dnf check-update` / `dnf upgrade`, etc.).  
2. Lee lo que te propone instalar/quitar antes de decir que sí.  
3. No copies comandos de foros con `curl … | sudo bash` si no entiendes qué hacen.  
4. Para aprender, prioriza el gestor **oficial** de tu distro (+ Flatpak en Mint).  

## Día 0 en Linux Mint (ejemplo guiado)

```bash
sudo apt update
# Hit:1 http://… InRelease
# Get:2 … Packages […]
# Reading package lists... Done
# (si ya estaba al día: “All packages are up to date” al hacer upgrade; update solo refresca listas)

apt search tree
# Sorting... Done
# Full Text Search... Done
# tree/… - displays an indented directory tree…

sudo apt install tree
# … The following NEW packages will be installed:
#   tree
# Do you want to continue? [Y/n]  → Y
# Setting up tree (…) …

tree -L 1 ~
# /home/<usuario>
# ├── Documentos
# ├── Descargas
# └── …

flatpak remotes
# Name    Options
# flathub system
# (si sale flathub, bien; si la tabla está vacía, falta añadir el remote)

flatpak search gimp
# Name  Description  Application ID  Version  Branch  Remotes
# GNU Image Manipulation Program  org.gimp.GIMP  …  flathub
# flatpak install flathub org.gimp.GIMP   # cuando quieras instalarlo
```

Si `apt` no encuentra algo:

```bash
sudo apt update
apt search <texto>
# si no lista nada útil → prueba otro nombre o Flatpak
flatpak search <texto>
# filas con Application ID (lo que usarás al instalar)
```

## Relacionados

- [apt](apt.md) — si usas Debian/Ubuntu/Mint  
- [flatpak](flatpak.md) — apps en Mint  
- [dnf](dnf.md) / [pacman](pacman.md) — otras familias  
