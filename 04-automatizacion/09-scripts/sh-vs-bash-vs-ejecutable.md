# sh vs bash vs `./fichero`

Tres formas de lanzar el mismo script. **No hacen exactamente lo mismo.**

## Sintaxis de las tres formas

```bash
sh <fichero>.sh
bash <fichero>.sh
./<fichero>.sh
```

> Recuerda: `<fichero>` lo sustituyes tú (sin `<` `>`).  
> Detalle: [Cómo leer la sintaxis](../../README.md#cómo-leer-la-sintaxis-muy-importante).

## 1) `sh <fichero>.sh`

```bash
sh mi_script.sh
sh <directorio>/<fichero>.sh
```

- Obligas a usar el intérprete **`sh`**.
- En Ubuntu / Linux Mint, `sh` suele apuntar a **`dash`**, no a Bash.
- Da igual lo que diga el shebang (`#!/bin/bash`): con este lanzamiento manda `sh`.
- Ideal para scripts muy portables y simples.
- Puede fallar si el script usa cosas “solo de Bash” (`[[ ]]`, arrays, etc.).

Comprobar qué es `sh` en tu sistema:

```bash
ls -l /bin/sh
# a menudo: /bin/sh -> dash   (Ubuntu/Mint)
# o:        /bin/sh -> bash
```

## 2) `bash <fichero>.sh`

```bash
bash mi_script.sh
bash <directorio>/<fichero>.sh
```

- Obligas a usar **Bash**.
- Tampoco necesita permiso de ejecución (`chmod +x`).
- Tampoco “obedece” al shebang: manda Bash porque tú lo pediste.
- Muy cómodo mientras aprendes y tus scripts usan sintaxis Bash.

## 3) `./<fichero>.sh` (ejecutar como programa)

```bash
chmod +x mi_script.sh
./mi_script.sh
```

Aquí ocurren **dos** cosas importantes:

1. El fichero debe tener permiso de **ejecución** (`x`).  
2. El sistema lee la **primera línea** (shebang) para saber con qué interpretarlo.

```bash
#!/bin/bash
# → se lanza con Bash

#!/bin/sh
# → se lanza con sh
```

Si no pones `./` y solo escribes el nombre:

```bash
mi_script.sh
# suele fallar: la shell busca en el PATH, no en el directorio actual
```

Por seguridad, el directorio actual (`.`) no suele estar en el `PATH`. Por eso se usa `./`.

## Tabla resumen

| Forma | Intérprete | ¿Necesita `chmod +x`? | ¿Respeta shebang? |
|-------|------------|------------------------|-------------------|
| `sh <fichero>.sh` | `sh` (dash/bash según distro) | No | No (manda `sh`) |
| `bash <fichero>.sh` | Bash | No | No (manda `bash`) |
| `./<fichero>.sh` | El del shebang | **Sí** | **Sí** |

## Ejemplos guiados

Crea un fichero de prueba:

```bash
mkdir -p <directorio>
cd <directorio>
```

Contenido de `hola.sh`:

```bash
#!/bin/bash
echo "Hola desde el script"
echo "Intérprete efectivo: $BASH_VERSION"
```

Pruebas:

```bash
sh hola.sh
# corre; $BASH_VERSION puede salir vacío si sh no es bash

bash hola.sh
# corre con Bash; debería mostrar versión

chmod +x hola.sh
./hola.sh
# corre según shebang (aquí Bash)
```

Script “solo Bash” lanzado con `sh` (para ver el fallo):

```bash
#!/bin/bash
if [[ -f /etc/passwd ]]; then
  echo "ok"
fi
```

```bash
sh script_bash.sh
# puede fallar: [[: not found   (si sh es dash)

bash script_bash.sh
# ok

chmod +x script_bash.sh
./script_bash.sh
# ok (shebang → bash)
```

## ¿Cuál uso yo?

| Situación | Recomendación |
|-----------|----------------|
| Estás aprendiendo Bash | `bash <fichero>.sh` o `./` con `#!/bin/bash` |
| Quieres un “programita” usable | `chmod +x` + `./<fichero>.sh` + shebang |
| Script muy portable / POSIX | `#!/bin/sh` y pruébalo también con `sh` |
| Te pasa un tutorial con `./` y falla | Mira si falta `chmod +x` |

## Errores frecuentes

```text
bash: ./hola.sh: Permission denied
```

- Falta permiso de ejecución: `chmod +x hola.sh`.

```text
bash: hola.sh: command not found
```

- Estás en el directorio del script pero no usaste `./hola.sh`.

```text
[[: not found
```

- Script de Bash ejecutado con `sh` (dash).

## Relacionados

- [shebang](shebang.md) — la primera línea  
- [primer-script](primer-script.md) — crear uno desde cero  
- [source-y-permisos](source-y-permisos.md) — `chmod` y `source`  
