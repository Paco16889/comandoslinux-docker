# Códigos de salida (`$?`, `exit`)

Cada comando termina con un **número**:

| Código | Significado habitual |
|--------|----------------------|
| `0` | Bien / verdadero |
| distinto de `0` | Error / falso |

## Ver el código del último comando

```bash
ls /tmp
echo $?
# 0

ls /no/existe
# ls: cannot access '/no/existe': No such file or directory
echo $?
# distinto de 0 (a menudo 2)
```

Prueba rápida en la terminal:

```bash
true
echo $?
# 0

false
echo $?
# 1
```

## Salir de un script a propósito

```bash
#!/bin/bash
if [ "$#" -ne 1 ]; then
  echo "Uso: $0 <directorio>"
  exit 1
fi

if [ ! -d "$1" ]; then
  echo "No es un directorio: $1"
  exit 2
fi

echo "OK: $1"
exit 0
```

```bash
bash check_dir.sh
# Uso: check_dir.sh <directorio>
echo $?
# 1

bash check_dir.sh /tmp/no_es_dir_xyz
# No es un directorio: /tmp/no_es_dir_xyz
echo $?
# 2

bash check_dir.sh /tmp
# OK: /tmp
echo $?
# 0
```

## Usar el código en otros comandos

```bash
ping -c 1 -W 2 8.8.8.8 >/dev/null
if [ $? -eq 0 ]; then
  echo "ping ok"
fi
# ping ok   (si hay red)
```

Forma más limpia (sin `$?` intermedio):

```bash
if ping -c 1 -W 2 8.8.8.8 >/dev/null; then
  echo "ping ok"
fi
# ping ok   (misma idea, sin guardar $?)
```

## `set -e` (avanzado suave)

```bash
#!/bin/bash
set -e
# el script para si un comando falla

mkdir /tmp/demo_script_ok
false
echo "Esto no se imprime si set -e está activo"
```

```bash
bash con_set_e.sh
# (crea /tmp/demo_script_ok si no existía)
# el script termina al llegar a false; no verás el echo final
echo $?
# distinto de 0
```

Útil en scripts serios; al aprender, úsalo cuando ya entiendas qué cuenta como “fallo”.

## Errores frecuentes

- Creer que `echo $?` dos veces seguidas muestra el mismo valor: el primer `echo $?` ya “consume” y el segundo es el código de ese `echo` (0).  

```bash
false
echo $?
# 1
echo $?
# 0   ← código del echo anterior, no del false
```

- Usar códigos al azar: conviene documentar en el script qué significa `1`, `2`…

## Siguiente

[`source`, permisos y buenos hábitos](source-y-permisos.md)

## Relacionados

- [condicionales](condicionales.md)  
- [primer-script](primer-script.md)  
- [10-pipes-redireccion](../10-pipes-redireccion/) — `&&` y `||` usan códigos de salida



