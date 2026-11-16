# echo

Escribe texto en la salida estándar (normalmente la pantalla).

## Sintaxis

```bash
echo [opciones] [<texto>...]
```

## Ejemplos

```bash
echo Hola
# Hola

echo "<texto>"
# imprime exactamente ese texto

echo "Hola <usuario>"
# Hola <usuario>  (si escribes el marcador literal; o pon tu nombre)

echo $HOME
# /home/<usuario>

echo $USER
# <usuario>

echo "Estoy en $PWD"
# Estoy en /home/<usuario>/...

echo "Hoy es $(date)"
# sustituye el resultado de date dentro del texto

echo -n "Sin salto de línea"
# no añade el salto final

echo -e "Línea 1\nLínea 2"
# interpreta \n como salto de línea

echo "Directorio: <directorio>"
# útil para mensajes en scripts
```

Crear o añadir a un fichero (adelanto de redirección):

```bash
echo "<texto>" > <fichero>
# crea/sobrescribe <fichero> con ese texto

echo "<texto>" >> <fichero>
# añade una línea al final de <fichero>
```

Comillas:

```bash
echo $USER
# expandido → <usuario>

echo '$USER'
# literal → $USER

echo "Home de $USER: $HOME"
# Home de <usuario>: /home/<usuario>
```

## Opciones útiles

| Opción | Qué hace |
|--------|----------|
| `-n` | No añade salto de línea al final |
| `-e` | Interpreta escapes (`\n`, `\t`, etc.) |
| `-E` | No interpreta escapes (por defecto en Bash) |

## Errores frecuentes

- Sin comillas, varios espacios se colapsan: `echo Hola    mundo` vs `echo "Hola    mundo"`.
- Querer imprimir `$USER` literal y usar comillas dobles: usa comillas simples.
- Para depuración seria a menudo se prefiere `printf`.

## Siguiente

[date](date.md)

## Relacionados

- [date](date.md) — fecha/hora que suele combinarse con `echo`
- [pwd](pwd.md) — o `echo $PWD`



