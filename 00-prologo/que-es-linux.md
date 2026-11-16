# Qué es Linux

Linux es un **sistema operativo** (como Windows o macOS): arranca el ordenador, gestiona ficheros, red, programas y usuarios.  
Cuando la gente dice “uso Linux”, casi siempre habla de una **distribución** (Mint, Ubuntu, Fedora…) que incluye:

| Pieza | Qué es |
|-------|--------|
| **Kernel Linux** | El “núcleo”: habla con el hardware (CPU, disco, red…) |
| **Herramientas GNU** y otras | Comandos, librerías, shell… |
| **Entorno de escritorio** (opcional) | Cinnamon, GNOME, KDE… lo que ves con ventanas |
| **Gestor de paquetes** | `apt`, `dnf`, `pacman`… para instalar programas |
| **Configuración y apps** | Lo que la distro elige por defecto |

Por eso a veces se dice **GNU/Linux**: el kernel es Linux; muchas herramientas clásicas vienen del proyecto GNU.

## Ideas previas

| Concepto | En pocas palabras |
|----------|-------------------|
| Distro | “Sabor” de Linux empaquetado y listo para instalar |
| Terminal / consola | Ventana donde escribes comandos |
| Shell | Programa que interpreta lo que escribes (`bash`, `zsh`…) |
| Root | Superusuario (administrador total) |
| Usuario normal | Tu cuenta del día a día (más segura) |

## Linux vs Windows (para orientarte)

| | Windows (idea) | Linux (idea) |
|--|----------------|--------------|
| Instalación de programas | Tienda / `.exe` / `.msi` | Repos (`apt`…) + a veces Flatpak |
| Disco | `C:\`, `D:\`… | Todo cuelga de `/` (una sola jerarquía) |
| Separador de rutas | `\` | `/` |
| Admin | “Ejecutar como administrador” | `sudo` / usuario `root` |
| Config | Registro + paneles | Mucho en ficheros de texto en `/etc` |

No es que uno sea “mejor” en abstracto: esta guía te enseña el **mundo Linux por terminal**.

## ¿Dónde entra la terminal?

Puedes usar Linux solo con el ratón (Mint es muy amable).  
La terminal sirve para:

- repetir tareas (scripts),  
- administrar servidores (a menudo **sin** escritorio),  
- entender qué hace el sistema por debajo,  
- copiar soluciones de documentación (casi siempre vienen en comandos).

Ejemplo mínimo (cuando tengas una terminal abierta):

```bash
whoami
# tu usuario

pwd
# carpeta actual

ls
# qué hay aquí
```

Esos comandos los verás en [01-basicos](../01-fundamentos/01-basicos/).

## Escritorio vs servidor

| Tipo | Qué esperas |
|------|-------------|
| Escritorio (Mint, Ubuntu Desktop…) | Wi‑Fi, apps gráficas, Flatpak, panel de red |
| Servidor (Ubuntu Server, Debian…) | A menudo sin interfaz gráfica; todo por SSH/terminal |

Los **comandos** de esta guía valen en ambos; lo que cambia es el entorno alrededor.

## Errores frecuentes (de concepto)

- Pensar que “Linux” es solo Ubuntu: Ubuntu es **una** distro.  
- Confundir el **kernel** con la distro completa.  
- Creer que hace falta ser programador: esta guía parte de cero.  
- Buscar `C:\` en Linux: aquí las rutas empiezan por `/`.

## Siguiente

[El sistema de ficheros: todo cuelga de `/`](sistema-de-ficheros.md)

## Relacionados

- [sistema-de-ficheros](sistema-de-ficheros.md)  
- [distros](distros.md)  
- [01-fundamentos](../01-fundamentos/)



