# Pipes (`|`)

El **pipe** conecta la salida de un comando con la entrada del siguiente.

```text
comando1 | comando2 | comando3
```

Piensa en una tubería: los datos fluyen de izquierda a derecha.

## Sintaxis

```bash
<comando1> | <comando2>
```

## Ejemplos clásicos

```bash
ls -la | less
# listado navegable
# less abre a pantalla completa; q para salir

ps aux | grep <texto>
ps aux | grep firefox
# ves solo las líneas de ps que contienen “firefox”
# (a veces también aparece la propia línea del grep)

dmesg | tail -n 20
# últimas líneas del buffer del kernel (si tienes permiso)
# ejemplo: [  12.345678] … mensajes del arranque / drivers
```

Contar:

```bash
ls | wc -l
# un número: cuántas entradas hay en el directorio actual (ej. 12)

ps aux | wc -l
# otro número: procesos listados + cabecera (ej. 185)

grep -r "<texto>" <directorio> | wc -l
# cuántas líneas coincidentes encontró grep en total
```

Ordenar y únicos:

```bash
cut -d: -f7 /etc/passwd | sort | uniq -c | sort -nr
# ejemplo típico (números inventados):
#     28 /usr/sbin/nologin
#     12 /bin/bash
#      2 /bin/false
# (shells más usadas arriba)
```

Buscar en la salida de otro comando:

```bash
apt search editor | less
# listado largo de paquetes; navegas con less

flatpak search music | head
# primeras líneas de apps Flatpak relacionadas con “music”

ss -tulpn | grep 22
# si SSH está escuchando, algo como: LISTEN … :22 … users:(("sshd",…
```

## Varios pipes encadenados

```bash
ps aux | grep <usuario> | grep -v grep | less
# procesos de ese usuario, sin la línea del propio grep; luego less
```

```bash
cat <fichero> | grep "<texto>" | sort | uniq
# (a menudo mejor: grep "<texto>" <fichero> | sort | uniq)
# resultado: líneas que coinciden, ordenadas y sin duplicados
```

## Pipe vs redirección

| Herramienta | Idea |
|-------------|------|
| `>` `>>` | Manda salida a un **fichero** |
| `\|` | Manda salida a otro **comando** |

```bash
ls > listado.txt
# a fichero
# en pantalla: nada; el listado está en listado.txt

ls | wc -l
# a otro comando
# en pantalla: un número (conteo), no el listado
```

## Errores frecuentes

- Un pipe solo pasa **stdout**. Los errores (stderr) siguen en pantalla, salvo que los combines (siguiente tema).  
- Poner espacios raros no rompe el `|`, pero un `|` mal colocado sí: cada lado debe ser un comando válido.  
- `comando1 | comando2 > fichero`: el fichero recibe la salida de **comando2** (el último del tramo redirigido).

## Siguiente

[Redirigir errores (`2>`, `2>&1`, `&>`)](combinar-errores.md)

## Relacionados

- [redireccion](redireccion.md)  
- [combinar-errores](combinar-errores.md)  
- [grep](../../01-fundamentos/03-lectura-texto/grep.md) · [less](../../01-fundamentos/03-lectura-texto/less.md)



