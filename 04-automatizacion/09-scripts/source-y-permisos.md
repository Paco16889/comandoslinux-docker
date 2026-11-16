# `source`, permisos y buenos hábitos

## chmod para scripts

```bash
chmod +x <fichero>.sh
chmod u+x <fichero>.sh
# solo tu usuario

ls -l <fichero>.sh
# -rwxr-xr-x 1 <usuario> <usuario> ... <fichero>.sh
```

Quitar ejecución:

```bash
chmod -x <fichero>.sh
ls -l <fichero>.sh
# -rw-r--r-- 1 <usuario> <usuario> ... <fichero>.sh
# (ya no hay x; ./<fichero>.sh dará Permission denied)
```

Más detalle de permisos: [chmod](../../01-fundamentos/04-permisos/chmod.md).

## `source` / `.` — ejecutar en la shell actual

`bash script.sh` y `./script.sh` lanzan un proceso **hijo**.  
Lo que haga con variables/cd **no** cambia tu shell actual (salvo que exportéis cosas raras).

`source` ejecuta el fichero **dentro de tu shell actual**:

```bash
source <fichero>.sh
. <fichero>.sh
# el punto es un sinónimo de source en Bash
```

Ejemplo:

```bash
# vars.sh
export PROYECTO="$HOME/proyectos/demo"
export PATH="$PROYECTO/bin:$PATH"
```

```bash
mkdir -p "$HOME/proyectos/demo"
source vars.sh
echo "$PROYECTO"
# /home/<usuario>/proyectos/demo
cd "$PROYECTO"
# tu shell actual queda en ese directorio (porque fue source, no un hijo)
pwd
# /home/<usuario>/proyectos/demo
```

Útil para cargar configuración; peligroso si el fichero hace `exit` o borra cosas: afecta a tu sesión.

## Buenos hábitos (desde cero)

1. Empieza con `#!/bin/bash` (o `#!/bin/sh` si buscas portable).  
2. Pon comillas: `"$variable"`, `"$1"`.  
3. Comprueba argumentos (`$#`) al inicio.  
4. Prueba primero con `bash -x <fichero>.sh` (modo traza):

```bash
bash -x hola.sh
```

```text
+ echo Hola, <usuario>
Hola, <usuario>
+ echo Hoy es AAAA-MM-DD
Hoy es AAAA-MM-DD
...
```

(`+` delante = el comando que Bash va a ejecutar; debajo, lo que imprime.)

5. No guardes contraseñas en scripts en claro.  
6. En Mint/Ubuntu, si un tutorial usa `#!/bin/bash` y tú lo lanzas con `sh`, pueden fallar cosas de Bash.

## Plantilla mínima recomendada

```bash
#!/bin/bash
set -euo pipefail
# set -e: para en error
# set -u: error si usas variable no definida
# pipefail: el código de un pipe refleja fallos (Bash)

uso() {
  echo "Uso: $0 <directorio>"
  exit 1
}

[ "$#" -eq 1 ] || uso
directorio="$1"
[ -d "$directorio" ] || { echo "No existe: $directorio"; exit 2; }

echo "OK: $directorio"
```

```bash
bash plantilla.sh
# Uso: plantilla.sh <directorio>

bash plantilla.sh /tmp/no_existe_xyz
# No existe: /tmp/no_existe_xyz

bash plantilla.sh /tmp
# OK: /tmp
```

Al principio puedes omitir `set -euo pipefail` y añadirlo cuando te sientas cómodo.

## Errores frecuentes

- Hacer `source` de un script pensando que es lo mismo que `./`.  
- `chmod 777` “para que vaya”: exceso de permisos; con `+x` suele bastar.  
- Editar en Windows y romper el shebang (`^M`).

## Relacionados

- [sh-vs-bash-vs-ejecutable](sh-vs-bash-vs-ejecutable.md)  
- [shebang](shebang.md)  
- [chmod](../../01-fundamentos/04-permisos/chmod.md)  
