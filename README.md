# Comandos Linux

Documentación de comandos de Linux, de lo más básico a lo más avanzado.
Pensada para aprender **desde cero**.

## Cómo estudiar

Sigue el orden de los números: primero el bloque, después las carpetas internas.

0. **[Prólogo](00-prologo/)** — qué es Linux, carpetas del sistema, distros *(empieza aquí si eres nuevo)*  
1. **Fundamentos** — terminal, ficheros, texto y permisos  
2. **Sistema** — usuarios, sesiones y procesos  
3. **Red y paquetes** — conectividad e instalación de software  
4. **Automatización** — scripts, pipes y herramientas avanzadas  
5. **Extras** — herramientas/servicios concretos (después del núcleo)  
6. **[Docker](06-docker/)** — Engine/CLI + Desktop opcional (fuera de extras)

Antes de teclear comandos, lee también: [Cómo leer la sintaxis](#cómo-leer-la-sintaxis-muy-importante).

## Mapa

```
comandos linux/
├── fin-proyecto.md         # nota de cierre (para agentes / cambios puntuales)
├── 00-prologo/             # qué es Linux, /, distros…
├── 01-fundamentos/
│   ├── 01-basicos/
│   ├── 02-ficheros-directorios/
│   ├── 03-lectura-texto/
│   └── 04-permisos/
│
├── 02-sistema/
│   ├── 05-usuarios-sesiones/
│   └── 06-procesos/
│
├── 03-red-paquetes/
│   ├── 07-red/
│   └── 08-paquetes/
│
├── 04-automatizacion/
│   ├── 09-scripts/
│   ├── 10-pipes-redireccion/
│   └── 11-avanzado/
│
├── 05-extras/
│   ├── monitorizacion/
│   ├── servidores/
│   ├── seguridad/
│   └── red-avanzada/
│
└── 06-docker/              # Engine/CLI + Desktop opcional
    ├── 00-introduccion/    # qué es + instalación Engine
    ├── 01-fundamentos/
    ├── 02-imagenes-dockerfile/
    ├── 03-volumes/
    ├── 04-networks/
    ├── 05-compose/
    ├── 06-avanzado/
    └── 07-desktop/         # Docker Desktop (Linux, opcional)
```

## Bloques

| Bloque | Carpetas | Nivel |
|--------|----------|--------|
| [00-prologo](00-prologo/) | qué es Linux, sistema de ficheros, árbol `/`, distros | Intro |
| [01-fundamentos](01-fundamentos/) | básicos, ficheros, lectura de texto, permisos | Básico |
| [02-sistema](02-sistema/) | usuarios/sesiones, procesos | Intermedio |
| [03-red-paquetes](03-red-paquetes/) | red, paquetes | Intermedio |
| [04-automatizacion](04-automatizacion/) | scripts, pipes/redirección, avanzado | Avanzado |
| [05-extras](05-extras/) | monitorización, servidores, seguridad, red avanzada | Extra |
| [06-docker](06-docker/) | Engine (intro→avanzado) + Desktop opcional en Linux | Contenedores |

## Distros en los ejemplos

Cuando haya que **instalar** algo, los ejemplos cubrirán (si aplica):

| Sistema | Gestor habitual | Notas |
|---------|-----------------|--------|
| Debian | `apt` | Base de muchas distros |
| Ubuntu | `apt` | Muy extendida |
| **Linux Mint** | `apt` + **Flatpak** | Basada en Ubuntu: `apt` funciona igual; para apps de escritorio Mint suele usar mucho Flatpak |
| Fedora | `dnf` | Familia RHEL |
| Arch | `pacman` | Rolling release |

En Mint puedes instalar herramientas de sistema con `apt` (como en Ubuntu) y muchas aplicaciones con `flatpak` (Software Manager / Flathub). Ambas formas aparecerán en la guía; el detalle está en `08-paquetes`.

## Formato de cada documento

Cada comando (o grupo muy relacionado) tendrá un fichero `.md` con:

- Qué hace
- Sintaxis
- Ejemplos
- Opciones útiles
- Errores frecuentes
- Comandos relacionados

---

## Cómo leer la sintaxis (muy importante)

En esta guía la línea de **Sintaxis** no se escribe tal cual: es un **molde**.
Hay símbolos que **no se teclean**; solo indican cómo montar el comando.

### 1. Texto normal → se escribe tal cual

Palabras como `ls`, `cd`, `chmod`, o opciones como `-l`, `-r`, se escriben **igual**.

```bash
ls -l
```

### 2. `<así>` → marcador (lo sustituyes tú)

Todo lo que va entre `<` y `>` es un **hueco**.  
**No escribas los símbolos `<` ni `>`.** Pon tu valor real.

| Marcador | Qué pones tú | Ejemplo real |
|----------|--------------|--------------|
| `<directorio>` | nombre o ruta de una carpeta | `Documentos` |
| `<fichero>` | nombre o ruta de un fichero | `notas.txt` |
| `<ruta>` | fichero o directorio | `/home/ana/docs` |
| `<usuario>` | nombre de usuario | `ana` |
| `<grupo>` | nombre de grupo | `developers` |
| `<comando>` | un comando | `ls` |
| `<texto>` | texto libre | `Hola` |
| `<número>` | un número | `20` |
| `<patrón>` | texto a buscar | `error` |
| `<permisos>` | modo de permisos | `755` o `u+x` |
| `<formato>` | formato (p. ej. de fecha) | `%Y-%m-%d` |

**Molde:**

```bash
cd <directorio>
```

**Tú escribes:**

```bash
cd Documentos
```

Otro ejemplo:

```bash
cp <fichero> <directorio>/
```

```bash
cp foto.jpg Documentos/
```

### 3. `[así]` → opcional (puedes ponerlo o no)

Todo lo que va entre `[` y `]` es **opcional**.  
**No escribas los corchetes.** Si no lo necesitas, omite esa parte entera.

**Molde:**

```bash
ls [opciones] [<ruta>]
```

Significa:

| Parte | ¿Obligatorio? | Qué es |
|-------|---------------|--------|
| `ls` | Sí | El comando |
| `[opciones]` | No | Cosas como `-l`, `-a`, `-la`… |
| `[<ruta>]` | No | Una carpeta/fichero; si no pones nada, usa el directorio actual |

**Tú puedes escribir cualquiera de estas:**

```bash
ls
ls -l
ls Documentos
ls -la Documentos
```

Otro ejemplo:

```bash
cd [<ruta>]
```

```bash
cd
# sin ruta → suele ir a tu home

cd /etc
# con ruta → vas a /etc
```

### 4. `...` → pueden ir más de uno

Los tres puntos significan “puedes repetir”.

```bash
mkdir <directorio>...
```

Puedes crear uno o varios:

```bash
mkdir proyecto
mkdir dir1 dir2 dir3
```

### 5. Resumen visual

```text
comando  [opciones]  <obligatorio_que_tu_rellenas>  [<opcional_que_tu_rellenas>]
   │          │                    │                           │
   │          │                    │                           └─ hueco opcional (sin [ ])
   │          │                    └─ hueco obligatorio (sin < >)
   │          └─ trozo opcional (sin [ ])
   └─ se escribe tal cual
```

| Símbolo en la guía | ¿Se teclea? | Significado |
|--------------------|-------------|-------------|
| `ls`, `-l`, `chmod`… | Sí | Literal |
| `<nombre>` | No (`<` `>` no) | Hueco: sustitúyelo por tu valor |
| `[algo]` | No (`[` `]` no) | Opcional: puedes omitirlo |
| `...` | No | Se puede repetir |

### 6. Ejemplo completo comentado

Sintaxis de `cp`:

```bash
cp [opciones] <origen> <destino>
```

Lectura en humano:

1. Escribe `cp`
2. Si quieres, añade opciones (`-r`, `-i`…) — **sin** corchetes
3. Obligatorios: origen y destino — **sin** `<` `>`

```bash
cp foto.jpg Documentos/
cp -i foto.jpg Documentos/
cp -r carpeta_origen carpeta_copia
```

---

## Nota sobre scripts (`09-scripts`)

Tres formas habituales de lanzar un script:

| Forma | Qué ocurre |
|-------|------------|
| `sh script.sh` | Lo interpreta `sh` (en Ubuntu/Mint suele ser `dash`; más limitado) |
| `bash script.sh` | Lo interpreta Bash, aunque no tenga shebang |
| `./script.sh` | Usa la primera línea (`#!/bin/bash` o `#!/bin/sh`) y necesita `chmod +x` |

Guía ampliada: [09-scripts/sh-vs-bash-vs-ejecutable](04-automatizacion/09-scripts/sh-vs-bash-vs-ejecutable.md).

## Estado del temario

- **[00-prologo](00-prologo/)**: intro a Linux, carpetas y distros.  
- Camino principal **01 → 11**: documentado.  
- Bloque **[05-extras](05-extras/)**: completo (monitorización, servidores, seguridad, red avanzada).  
- Bloque **[06-docker](06-docker/)**: Engine/CLI (intro → avanzado) + **07-desktop** (opcional, Linux).  
- Cierre / handoff agentes: [`fin-proyecto.md`](fin-proyecto.md).
