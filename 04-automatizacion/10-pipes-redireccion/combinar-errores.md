# Redirigir errores (`2>`, `2>&1`, `&>`)

## Sintaxis

```bash
<comando> 2> <fichero_errores>
<comando> > <fichero> 2>&1
<comando> &> <fichero>
```

> `2` es el canal stderr. `2>&1` significa “manda stderr donde esté ahora stdout”.

## Solo errores a un fichero

```bash
ls /tmp /no/existe 2> errores.txt
# el listado de /tmp sale en pantalla
# el error de /no/existe va a errores.txt

cat errores.txt
```

## Descartar errores

```bash
grep -r "<texto>" <directorio> 2> /dev/null
# oculta "Permission denied" y similares
```

## Guardar salida y errores juntos

Orden clásico (importante):

```bash
comando > todo.txt 2>&1
# 1) stdout → todo.txt
# 2) stderr → al mismo sitio que stdout (el fichero)
```

En Bash también:

```bash
comando &> todo.txt
# stdout + stderr al mismo fichero
```

## Separar en dos ficheros

```bash
comando > salida.txt 2> errores.txt
```

## Con pipes

Por defecto el pipe no lleva stderr. Para meter errores en el pipe:

```bash
comando 2>&1 | less
# ahora less ve también los errores
```

```bash
comando 2>&1 | grep "<texto>"
```

## Ejemplos prácticos

```bash
find /etc -name "*.conf" 2> /dev/null | head
# evita ruido de permisos

bash <fichero>.sh > salida.log 2>&1
# log completo de un script

curl -I <URL> > cabeceras.txt 2> curl_err.txt
```

## Errores frecuentes

```bash
comando 2>&1 > fichero
# mal orden habitual: primero unes stderr a la pantalla, luego solo mueves stdout
# usa: comando > fichero 2>&1
```

- Creer que `> fichero` guarda errores: no.  
- Mezclar `&>` (Bash) en un script lanzado con `sh`/dash: puede fallar; usa `> fichero 2>&1`.

## Relacionados

- [stdout-stderr](stdout-stderr.md)  
- [redireccion](redireccion.md)  
- [pipes](pipes.md)  
