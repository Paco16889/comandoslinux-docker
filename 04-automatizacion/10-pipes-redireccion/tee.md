# tee

Lee de la entrada y lo escribe **a la vez** en la pantalla y en un fichero (o varios).

Útil cuando quieres ver el resultado y guardarlo.

## Sintaxis

```bash
<comando> | tee [opciones] <fichero>
```

> Recuerda: `[ ]` = opcional · `<fichero>` lo sustituyes tú.  
> Detalle: [Cómo leer la sintaxis](../../README.md#cómo-leer-la-sintaxis-muy-importante).

## Ejemplos

```bash
ls -la | tee <fichero_salida>.txt
# ves el listado Y se guarda en <fichero_salida>.txt
# en pantalla (ejemplo):
# total 48
# drwxr-xr-x  5 paco paco 4096 nov 16 22:00 .
# -rw-r--r--  1 paco paco  220 nov 16 21:50 notas.txt
# …y lo mismo queda dentro de <fichero_salida>.txt

ls -la <directorio> | tee <fichero_salida>.txt
cat <fichero_salida>.txt
# cat vuelve a mostrar el mismo listado que acabas de ver
```

```bash
dmesg | tee <fichero>.txt | less
# guarda y además puedes paginar
# less: subes/bajas con flechas; q para salir
# el fichero ya tiene una copia aunque cierres less
```

Añadir al final (`-a`):

```bash
echo "nueva linea" | tee -a <fichero_log>.log
# en pantalla: nueva linea
date | tee -a <fichero_log>.log
# en pantalla: lun 16 nov 2026 23:20:01 CET  (ejemplo)
echo "----" | tee -a <fichero_log>.log
cat <fichero_log>.log
```

```text
nueva linea
lun 16 nov 2026 23:20:01 CET
----
```

Varios ficheros:

```bash
comando | tee <fichero1>.txt <fichero2>.txt
# misma salida en pantalla y en los dos ficheros

ps aux | tee /tmp/ps_a.txt /tmp/ps_b.txt | head
# en pantalla: solo las primeras líneas (por head)
# /tmp/ps_a.txt y /tmp/ps_b.txt tienen el listado completo de ps
```

Con sudo (patrón habitual al escribir ficheros de root):

```bash
echo "<texto>" | sudo tee /etc/<fichero>
# tee corre como root y escribe el fichero
# (sudo echo > /etc/fichero NO funciona como muchos esperan)
# en pantalla: <texto>  (tee también lo muestra)
# el fichero /etc/<fichero> queda con ese contenido

echo "<texto>" | sudo tee -a /etc/<fichero>
# añadir al final como root
# otra vez ves <texto> en pantalla y se añade al fichero
```

Salida + errores al log:

```bash
comando 2>&1 | tee <fichero_sesion>.log
# ves en vivo lo normal y los errores; todo queda en el .log

bash <fichero>.sh 2>&1 | tee <fichero_sesion>.log
# igual con un script: pantalla + copia en <fichero_sesion>.log
```

Solo guardar, sin ver en pantalla:

```bash
comando | tee <fichero>.txt > /dev/null
# equivalente práctico a comando > <fichero>.txt  (casi)
# en pantalla: nada; el contenido sí está en <fichero>.txt
```

## Errores frecuentes

- Olvidar que `tee` **sobrescribe** el fichero por defecto → usa `-a` para append.  
- Pensar que `tee` sustituye a `>` en todos los casos: para “solo guardar sin ver”, con `>` basta.  
- Usar `sudo echo ... > /etc/fichero`: el `>` lo hace tu shell sin root; usa `sudo tee`.

## Relacionados

- [redireccion](redireccion.md)  
- [pipes](pipes.md)  
- [combinar-errores](combinar-errores.md)  
