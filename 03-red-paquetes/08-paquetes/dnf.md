# dnf

Gestor de paquetes de **Fedora** (y familia reciente RHEL/CentOS Stream, Alma, Rocky…).

Antes se usaba mucho `yum`; en Fedora actual el comando habitual es `dnf` (a menudo `yum` es un alias).

## Sintaxis

```bash
sudo dnf [opciones] <acción> [<paquete>...]
dnf [opciones] <acción> [<paquete>...]
```

> Recuerda: `[ ]` = opcional · `<paquete>` = nombre del paquete.  
> Detalle: [Cómo leer la sintaxis](../../README.md#cómo-leer-la-sintaxis-muy-importante).

## Actualizar el sistema

```bash
sudo dnf upgrade
# o, en muchas versiones:
sudo dnf update
# en dnf moderno update y upgrade suelen ser equivalentes
```

Comprobar sin instalar:

```bash
dnf check-update
# lista qué habría que actualizar
```

## Buscar e información

```bash
dnf search <texto>
dnf search htop

dnf info <paquete>
dnf info htop

dnf list installed
dnf list installed | grep <texto>
```

## Instalar y quitar

```bash
sudo dnf install <paquete>
sudo dnf install htop curl wget tree

sudo dnf install -y <paquete>
# -y confirma solo
```

```bash
sudo dnf remove <paquete>
sudo dnf remove htop

sudo dnf autoremove
# limpia dependencias huérfanas (según caso)
```

## Historial (muy útil en dnf)

```bash
dnf history
# operaciones recientes

sudo dnf history undo <número>
# deshacer una transacción (con cuidado)
```

## Grupos de paquetes

Fedora organiza a veces “grupos” (conjuntos):

```bash
dnf group list
sudo dnf group install "<nombre_grupo>"
```

## Caché

```bash
sudo dnf clean all
# limpia caché de metadatos/paquetes
```

## Equivalencias rápidas con apt

| Objetivo | apt (Debian/Ubuntu/Mint) | dnf (Fedora) |
|----------|---------------------------|--------------|
| Refrescar + actualizar | `sudo apt update && sudo apt upgrade` | `sudo dnf upgrade` |
| Buscar | `apt search <texto>` | `dnf search <texto>` |
| Instalar | `sudo apt install <paquete>` | `sudo dnf install <paquete>` |
| Quitar | `sudo apt remove <paquete>` | `sudo dnf remove <paquete>` |
| Info | `apt show <paquete>` | `dnf info <paquete>` |

## Flatpak en Fedora

Fedora también usa mucho Flatpak. La idea es la misma que en [flatpak](flatpak.md):

```bash
flatpak search <texto>
flatpak install flathub <id_de_la_app>
flatpak update
```

## Errores frecuentes

```text
No match for argument: <paquete>
```

- Nombre incorrecto o no está en los repos activos. Prueba `dnf search <texto>`.

- Confundir el nombre del **comando** con el del **paquete** (a veces coinciden, a veces no).
- Ejecutar sin `sudo` acciones que escriben en el sistema.

## Siguiente

[pacman](pacman.md)

## Relacionados

- [apt](apt.md) — si vienes de Ubuntu/Mint  
- [flatpak](flatpak.md) — apps adicionales  
- [pacman](pacman.md) — el de Arch



