# apt

Gestor de paquetes de **Debian**, **Ubuntu** y **Linux Mint** (y derivadas).

Casi siempre necesitas `sudo` para instalar, quitar o actualizar.

## Sintaxis

```bash
sudo apt [opciones] <acción> [<paquete>...]
apt [opciones] <acción> [<paquete>...]
```

> Recuerda: `[ ]` = opcional · `<paquete>` = nombre del paquete (sin `<` `>`).  
> Detalle: [Cómo leer la sintaxis](../../README.md#cómo-leer-la-sintaxis-muy-importante).

Buscar e informar a veces funciona **sin** `sudo`. Instalar/actualizar/quitar, **con** `sudo`.

## El ciclo más importante

```bash
sudo apt update
# refresca la lista de paquetes disponibles (no instala actualizaciones todavía)

sudo apt upgrade
# instala actualizaciones de los paquetes ya instalados
```

En una sola sesión típica:

```bash
sudo apt update && sudo apt upgrade
```

`&&` significa: “si lo anterior fue bien, sigue” (más en `10-pipes-redireccion`).

## Buscar e información

```bash
apt search <texto>
apt search htop
apt search "image editor"

apt show <paquete>
apt show htop
# descripción, versión, dependencias, tamaño…

apt list --installed
# lo que tienes instalado (es largo)

apt list --installed | grep <texto>
```

## Instalar

```bash
sudo apt install <paquete>
sudo apt install htop
sudo apt install curl wget tree

sudo apt install <paquete1> <paquete2> <paquete3>
```

Si te pregunta `[Y/n]`, `Y` = yes (aceptar). En scripts a veces verás:

```bash
sudo apt install -y <paquete>
# -y responde “yes” solo; úsalo cuando sepas lo que instalas
```

## Quitar

```bash
sudo apt remove <paquete>
# quita el programa; a menudo deja configs

sudo apt purge <paquete>
# quita también ficheros de configuración típicos del paquete

sudo apt autoremove
# limpia dependencias que ya nadie necesita
```

Flujo limpio al desinstalar:

```bash
sudo apt purge <paquete>
sudo apt autoremove
```

## Actualizar un paquete concreto / el sistema

```bash
sudo apt update
sudo apt install --only-upgrade <paquete>
# solo ese, si hay versión nueva

sudo apt full-upgrade
# más agresivo que upgrade: puede quitar paquetes si hace falta resolver dependencias
# (en servidores se usa con más cuidado)
```

## Historial y caché (útil saber que existen)

```bash
ls /var/cache/apt/archives
# .deb descargados (caché)

sudo apt clean
# borra paquetes .deb descargados de la caché

sudo apt autoclean
# borra de la caché los que ya no se pueden descargar (obsoletos)
```

## Linux Mint + apt

En Mint, `apt` funciona como en Ubuntu:

```bash
sudo apt update
sudo apt install neofetch
sudo apt install flatpak
# (Flatpak suele venir ya listo en Mint; si no, se puede instalar)
```

Las apps grandes de escritorio a menudo se instalan mejor con [Flatpak](flatpak.md) (Software Manager / Flathub).  
Las herramientas de terminal (`htop`, `curl`, `git`…) suelen ir con `apt`.

## Errores frecuentes

```text
E: Unable to locate package <paquete>
```

- Nombre mal escrito, o no está en tus repos, o falta `sudo apt update`.

```text
E: Could not get lock /var/lib/dpkg/lock...
```

- Otro gestor está trabajando (Software Manager, otra terminal con apt). Cierra eso o espera.

```text
Do you want to continue? [Y/n]
```

- No es un error: está pidiendo confirmación.

- Confundir `update` con `upgrade`: `update` solo refresca el catálogo.

## Siguiente

[flatpak](flatpak.md)

## Relacionados

- [flatpak](flatpak.md) — el otro pilar en Mint  
- [conceptos](conceptos.md) — dependencias y repos  
- [buscar-e-info](buscar-e-info.md) — comparar búsquedas entre gestores



