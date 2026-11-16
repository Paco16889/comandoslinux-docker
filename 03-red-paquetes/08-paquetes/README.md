# 08 — Paquetes

Cómo **instalar, actualizar y quitar** programas según tu distribución.

> **Cómo leer la sintaxis:** `<algo>` = lo sustituyes tú (no escribas `<` `>`).  
> `[algo]` = opcional (no escribas `[` `]`).  
> Explicación completa: [Cómo leer la sintaxis](../../README.md#cómo-leer-la-sintaxis-muy-importante).

## Ideas previas (léelas una vez)

| Concepto | En pocas palabras |
|----------|-------------------|
| Paquete | “Caja” con un programa y sus ficheros (ej. `htop`, `curl`) |
| Repositorio | Almacén oficial (o añadido) de donde se bajan paquetes |
| Dependencias | Otros paquetes que hacen falta para que uno funcione |
| Gestor de paquetes | Herramienta que instala/actualiza/resuelve dependencias |
| Flatpak | Formato/apps “en caja” (sandbox); muy usado en **Linux Mint** para apps gráficas |

## ¿Qué gestor usa mi sistema?

| Distro | Gestor principal | Documento |
|--------|------------------|-----------|
| Debian | `apt` | [apt](apt.md) |
| Ubuntu | `apt` | [apt](apt.md) |
| **Linux Mint** | `apt` + **Flatpak** | [apt](apt.md) y [flatpak](flatpak.md) |
| Fedora | `dnf` | [dnf](dnf.md) |
| Arch | `pacman` | [pacman](pacman.md) |

No hace falta memorizar todos. Aprende el de **tu** distro y mira los otros cuando cambies de sistema.

## Orden recomendado

1. [conceptos](conceptos.md) — repositorios, dependencias, qué NO hacer
2. [apt](apt.md) — Debian / Ubuntu / **Linux Mint**
3. [flatpak](flatpak.md) — Mint y apps de escritorio (Flathub)
4. [dnf](dnf.md) — Fedora
5. [pacman](pacman.md) — Arch
6. [snap](snap.md) — otra tienda de apps (Ubuntu y otras; visión general)
7. [buscar-e-info](buscar-e-info.md) — trucos comunes de búsqueda en todos
