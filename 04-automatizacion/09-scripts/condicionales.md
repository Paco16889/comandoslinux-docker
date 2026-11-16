# Condicionales (`if`)

Ejecutan un bloque solo si se cumple una condición.

## Sintaxis básica

```bash
if <comando>; then
  # si fue bien (código 0)
fi
```

```bash
if <comando>; then
  # sí
else
  # no
fi
```

```bash
if <comando>; then
  ...
elif <otro_comando>; then
  ...
else
  ...
fi
```

> Recuerda: en Bash, “verdadero” suele ser código de salida **0**.

## Tests con `[ ]` (portable)

```bash
#!/bin/bash
fichero="$1"

if [ -f "$fichero" ]; then
  echo "Existe el fichero $fichero"
else
  echo "No existe (o no es fichero normal)"
fi
```

```bash
touch /tmp/demo_existe.txt
bash existe.sh /tmp/demo_existe.txt
# Existe el fichero /tmp/demo_existe.txt

bash existe.sh /tmp/no_existe.txt
# No existe (o no es fichero normal)
```

Pruebas habituales:

| Test | Significado |
|------|-------------|
| `-f <fichero>` | Existe y es fichero |
| `-d <directorio>` | Existe y es directorio |
| `-e <ruta>` | Existe (cualquier tipo) |
| `-r` `-w` `-x` | Legible / escribible / ejecutable |
| `-z "$var"` | Cadena vacía |
| `-n "$var"` | Cadena no vacía |
| `"$a" = "$b"` | Texto igual (en `[ ]` a veces se usa `=` ) |
| `"$a" -eq "$b"` | Números iguales |

```bash
if [ "$#" -eq 0 ]; then
  echo "Faltan argumentos"
  exit 1
fi

if [ -d "$1" ]; then
  echo "Es un directorio"
fi
```

```bash
bash comprobar.sh
# Faltan argumentos

bash comprobar.sh /tmp
# Es un directorio
```

**Espacios** dentro de `[ ]` son obligatorios:

```bash
# bien
if [ -f "$f" ]; then

# mal
if [-f "$f"]; then
```

## `[[ ]]` (solo Bash; más cómodo)

```bash
#!/bin/bash
fichero="/etc/passwd"
nombre="ana"

if [[ -f $fichero && -r $fichero ]]; then
  echo "Fichero legible"
fi

if [[ $nombre == a* ]]; then
  echo "Empieza por a"
fi
```

```bash
bash doble_corchete.sh
```

```text
Fichero legible
Empieza por a
```

Si lanzas el script con `sh` y usas `[[ ]]`, puede fallar. Con `#!/bin/bash` + `./` o `bash script.sh`, bien.

## Comparar números

```bash
#!/bin/bash
n="$1"
if [ "$n" -gt 10 ]; then
  echo "Mayor que 10"
elif [ "$n" -eq 10 ]; then
  echo "Igual a 10"
else
  echo "Menor que 10"
fi
```

```bash
bash compara.sh 15
# Mayor que 10

bash compara.sh 10
# Igual a 10

bash compara.sh 3
# Menor que 10
```

| Operador | Significado |
|----------|-------------|
| `-eq` | igual |
| `-ne` | distinto |
| `-lt` | menor |
| `-le` | menor o igual |
| `-gt` | mayor |
| `-ge` | mayor o igual |

## if con comandos normales

```bash
#!/bin/bash
if ping -c 1 -W 2 8.8.8.8 >/dev/null; then
  echo "Hay red (ping ok)"
else
  echo "No hay respuesta de ping"
fi
```

```bash
bash hay_red.sh
# Hay red (ping ok)          ← si hay salida a Internet
# No hay respuesta de ping   ← si falla o no hay red
```

## Errores frecuentes

- Olvidar `then` / `fi`.  
- Espacios mal puestos en `[ ]`.  
- Usar `==` dentro de `[ ]` antiguo: en muchos sistemas `[` acepta `=`; en `[[ ]]` es más natural `==`.  
- No poner comillas: `[ -f $f ]` falla si `$f` está vacío o tiene espacios → `[ -f "$f" ]`.

## Siguiente

[Bucles (`for`, `while`)](bucles.md)

## Relacionados

- [bucles](bucles.md)  
- [codigos-salida](codigos-salida.md)  
- [sh-vs-bash-vs-ejecutable](sh-vs-bash-vs-ejecutable.md)



