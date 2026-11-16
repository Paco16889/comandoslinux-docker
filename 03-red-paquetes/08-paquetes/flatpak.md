# flatpak

Formato de aplicaciones (con sus dependencias) que corren de forma más aislada (**sandbox**).  
En **Linux Mint** es muy habitual para programas de escritorio (a menudo desde **Flathub**).

No sustituye a `apt` para el sistema: se **complementan**.

## Sintaxis

```bash
flatpak [opciones] <acción> [argumentos]...
```

> Recuerda: `[ ]` = opcional · `<acción>` = `search`, `install`, `update`…  
> Detalle: [Cómo leer la sintaxis](../../README.md#cómo-leer-la-sintaxis-muy-importante).

Los nombres de apps Flatpak suelen verse como ID largo, por ejemplo:

```text
org.gimp.GIMP
com.spotify.Client
```

## ¿Tienes Flatpak? ¿Tienes Flathub?

```bash
flatpak --version
# si responde una versión, el comando está

flatpak remotes
# lista de “tiendas” configuradas; debería aparecer flathub en Mint
```

Añadir Flathub si no está (habitual en varias distros; en Mint suele venir):

```bash
flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
```

## Buscar

```bash
flatpak search <texto>
flatpak search gimp
flatpak search vlc
flatpak search htop
```

La salida muestra nombre, ID y descripción. Para instalar necesitas el **ID** (o el nombre que indique la propia salida).

## Instalar

```bash
flatpak install <remoto> <id_de_la_app>
flatpak install flathub org.gimp.GIMP
```

A veces basta:

```bash
flatpak install <id_de_la_app>
# si solo hay un remote posible / el sistema lo resuelve
```

Confirma con `y` cuando pregunte. Puede instalar “runtimes” (plataformas base): es normal.

## Listar, ejecutar, quitar

```bash
flatpak list
# apps instaladas (y a veces runtimes)

flatpak list --app
# solo aplicaciones

flatpak run <id_de_la_app>
flatpak run org.gimp.GIMP
# muchas también aparecen en el menú gráfico de Mint
```

```bash
flatpak uninstall <id_de_la_app>
flatpak uninstall org.gimp.GIMP

flatpak uninstall --unused
# limpia runtimes que ya no usa nadie
```

## Actualizar

```bash
flatpak update
# actualiza apps Flatpak (independiente de apt upgrade)
```

En Mint conviene recordar:

```bash
sudo apt update && sudo apt upgrade
flatpak update
# sistema + apps Flatpak
```

## Información

```bash
flatpak info <id_de_la_app>
flatpak info org.gimp.GIMP
```

## apt vs Flatpak en Mint (regla práctica)

| Usa `apt` cuando… | Usa Flatpak cuando… |
|-------------------|---------------------|
| Herramientas de sistema/terminal (`curl`, `htop`, `git`) | Apps de escritorio grandes |
| Librerías y cosas “del sistema” | Quieres versiones más nuevas desde Flathub |
| Lo pide un tutorial del sistema | El Software Manager te ofrece Flatpak |

```bash
# Ejemplo mental en Mint:
sudo apt install git
flatpak install flathub org.gimp.GIMP
```

## Permisos / sandbox (idea)

Flatpak aísla apps. A veces una app “no ve” un disco o el micrófono hasta que le das permiso (desde la app de permisos Flatpak o herramientas como Flatseal).  
Si algo “no abre ficheros”, no siempre está roto: puede ser el sandbox.

## Errores frecuentes

```text
flatpak: command not found
```

```bash
# Debian / Ubuntu / Linux Mint
sudo apt install flatpak
```

```text
error: No remote refs found similar to ‘...’
```

- Falta Flathub u otro remote; o el ID está mal escrito. Revisa `flatpak remotes` y `flatpak search <texto>`.

- Instalar la misma app por `apt` y por Flatpak: puedes tener **dos** (nombres parecidos en el menú). Elige una vía para no liar actualizaciones.

## Siguiente

[dnf](dnf.md)

## Relacionados

- [apt](apt.md) — base del sistema en Mint  
- [snap](snap.md) — otro formato similar en idea  
- [conceptos](conceptos.md) — por qué hay varios canales



