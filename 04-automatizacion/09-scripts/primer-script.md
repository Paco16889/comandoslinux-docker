# Tu primer script (paso a paso)

Vamos a crear, ejecutar y entender un script mínimo.

## 1) Crear el fichero

```bash
cd ~
mkdir -p <directorio>
cd <directorio>
nano hola.sh
# o el editor que uses: micro, vim, code…
```

Contenido:

```bash
#!/bin/bash
# Esto es un comentario: la shell lo ignora

echo "Hola, $USER"
echo "Hoy es $(date +%F)"
echo "Estoy en: $PWD"
```

Guarda y sal del editor.

## 2) Probar sin permiso de ejecución

```bash
bash hola.sh
```

```text
Hola, <usuario>
Hoy es AAAA-MM-DD
Estoy en: /home/<usuario>/<directorio>
```

Deberías ver tu usuario, la fecha y el directorio.

También:

```bash
sh hola.sh
# misma idea de salida en este ejemplo simple
# (puede funcionar igual; sh en Ubuntu/Mint suele ser dash)
```

## 3) Convertirlo en “ejecutable”

```bash
chmod +x hola.sh
ls -l hola.sh
# -rwxr-xr-x 1 <usuario> <usuario> ... hola.sh
```

```bash
./hola.sh
```

```text
Hola, <usuario>
Hoy es AAAA-MM-DD
Estoy en: /home/<usuario>/<directorio>
```

## 4) ¿Y si quiero lanzarlo desde otro sitio?

Opción A — ruta completa o relativa:

```bash
bash /home/<usuario>/<directorio>/hola.sh
~/ <directorio>/hola.sh
# mejor:
bash ~/<directorio>/hola.sh
# misma salida de los tres echo (usuario, fecha, PWD)
```

Opción B — meterlo en un directorio del PATH (más adelante):

```bash
echo $PATH
# /usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:...
# carpetas donde la shell busca comandos
mkdir -p ~/.local/bin
cp hola.sh ~/.local/bin/
# (asegúrate de que ~/.local/bin está en tu PATH)
```

## 5) Script que falla a propósito (para aprender)

```bash
#!/bin/bash
echo "Antes"
ls /ruta/que/no/existe
echo "Después"
```

```bash
bash falla.sh
```

```text
Antes
ls: cannot access '/ruta/que/no/existe': No such file or directory
Después
```

Verás el error de `ls`, pero el script **sigue** con el siguiente `echo`, salvo que actives modos estrictos (`set -e`, más adelante).

## Mini checklist

| Paso | Comando / acción |
|------|------------------|
| Crear | editor + `#!/bin/bash` |
| Probar | `bash <fichero>.sh` |
| Ejecutable | `chmod +x <fichero>.sh` |
| Lanzar | `./<fichero>.sh` |

## Errores frecuentes

- Olvidar `./` → `command not found`.  
- Guardar el fichero sin extensión: la extensión `.sh` es costumbre, no magia; lo importante es el contenido y el shebang.  
- Escribir comandos en el editor y no guardar.

## Siguiente

[Variables en Bash](variables.md)

## Relacionados

- [variables](variables.md) — el siguiente nivel  
- [sh-vs-bash-vs-ejecutable](sh-vs-bash-vs-ejecutable.md)  
- [echo](../../01-fundamentos/01-basicos/echo.md) · [date](../../01-fundamentos/01-basicos/date.md)



