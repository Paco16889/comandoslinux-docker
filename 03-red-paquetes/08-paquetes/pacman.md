# pacman

Gestor de paquetes de **Arch Linux** (y derivadas: EndeavourOS, Manjaro tiene capas extra, etc.).

La sintaxis se basa en **letras de opción** (`-S`, `-R`, `-Q`…). Al principio cuesta; con una chuleta se hace manejable.

## Sintaxis

```bash
sudo pacman [opciones] [<paquete>...]
pacman [opciones] [<paquete>...]
```

> Recuerda: `[ ]` = opcional · `<paquete>` = nombre del paquete.  
> Detalle: [Cómo leer la sintaxis](../../README.md#cómo-leer-la-sintaxis-muy-importante).

## Letras que debes conocer

| Opción | Familia | Idea |
|--------|---------|------|
| `-S` | Sync | Instalar / sincronizar desde repos |
| `-R` | Remove | Quitar |
| `-Q` | Query | Consultar lo instalado |
| `-U` | Upgrade | Instalar desde un fichero `.pkg.tar.*` local |
| `-y` | (con -S) | Refrescar bases de datos de paquetes |
| `-u` | (con -S) | Actualizar paquetes instalados |

## Actualizar el sistema (muy habitual)

```bash
sudo pacman -Syu
# -S sync + -y refrescar DB + -u actualizar el sistema
```

Hazlo con cierta frecuencia en Arch (rolling release): el sistema cambia a menudo.

## Buscar e información

```bash
pacman -Ss <texto>
# busca en los repositorios

pacman -Ss htop

pacman -Si <paquete>
# información de un paquete en repos

pacman -Qs <texto>
# busca en lo instalado

pacman -Qi <paquete>
# información de un paquete instalado

pacman -Q
# lista todo lo instalado
```

## Instalar

```bash
sudo pacman -S <paquete>
sudo pacman -S htop
sudo pacman -S curl wget tree

sudo pacman -S <paquete1> <paquete2>
```

## Quitar

```bash
sudo pacman -R <paquete>
# quita el paquete

sudo pacman -Rs <paquete>
# quita también dependencias que ya no hagan falta (según cálculo de pacman)

sudo pacman -Rns <paquete>
# más limpio: quita configs huérfanas típicas (útil; revisa qué propone)
```

## Limpiar caché

```bash
sudo pacman -Sc
# limpia caché de paquetes no instalados (pregunta)

sudo pacman -Scc
# más agresivo: casi toda la caché
```

## AUR (solo para que sepas que existe)

Arch tiene el **AUR** (repositorio de la comunidad). No lo gestiona `pacman` solo: se usan ayudantes (`yay`, `paru`…) o instalación manual.  
Para esta guía desde cero: domina `pacman` oficial antes de meterte en AUR.

## Equivalencias rápidas

| Objetivo | apt | pacman |
|----------|-----|--------|
| Actualizar sistema | `sudo apt update && sudo apt upgrade` | `sudo pacman -Syu` |
| Buscar | `apt search <texto>` | `pacman -Ss <texto>` |
| Instalar | `sudo apt install <paquete>` | `sudo pacman -S <paquete>` |
| Quitar | `sudo apt remove <paquete>` | `sudo pacman -R <paquete>` |
| Info instalado | `apt show` / `dpkg -l` | `pacman -Qi <paquete>` |

## Errores frecuentes

```text
error: target not found: <paquete>
```

- Nombre incorrecto o hay que refrescar: `sudo pacman -Sy` y vuelve a buscar. Mejor aún el hábito ` -Syu`.

- Actualizaciones parciales (`-Sy` + instalar sin `-u`) pueden romper el sistema en Arch. La costumbre sana es **`pacman -Syu`**.
- Copiar comandos AUR sin entenderlos: deja AUR para más adelante.

## Relacionados

- [dnf](dnf.md) / [apt](apt.md) — otros gestores  
- [flatpak](flatpak.md) — también se puede usar en Arch  
- [conceptos](conceptos.md) — rolling release = actualizaciones frecuentes  
