# Usuarios y permisos (idea general)

Antes de `chmod` y `sudo` con detalle, basta esta foto mental.

## Ideas previas

| Concepto | En pocas palabras |
|----------|-------------------|
| Usuario | Tu identidad en el sistema (`ana`, `paco`…) |
| Grupo | Conjunto de usuarios (para compartir acceso) |
| root | El superusuario: puede casi todo |
| `sudo` | “Ejecuta este comando con poderes de admin” |
| Permisos | Quién puede leer / escribir / ejecutar un fichero o carpeta |

## Dos mundos: tu home y el sistema

```bash
whoami
# <usuario>

ls -ld "$HOME"
# suele ser de tu usuario: solo tú escribes ahí a gusto

ls -ld /etc
# dueño root: tú lees; escribir pide sudo
```

Por eso:

```bash
echo hola > "$HOME/prueba.txt"
# OK

echo hola > /etc/prueba.txt
# Permission denied  → normal
```

## Qué verás con `ls -l`

```bash
ls -l "$HOME"
```

Algo como:

```text
-rw-rw-r-- 1 ana ana 120 Nov 14 20:00 notas.txt
```

De izquierda a derecha (simplificado):

1. tipo + permisos (`-rw-rw-r--`)  
2. dueño (`ana`)  
3. grupo (`ana`)  
4. tamaño, fecha, nombre  

El detalle lo tienes en [04-permisos](../01-fundamentos/04-permisos/).

## root y sudo (sin miedo, con respeto)

```bash
# ver algo del sistema
cat /etc/hostname

# editar algo del sistema (ejemplo; no lo hagas sin motivo)
# sudo nano /etc/hostname
```

| Bien | Mal |
|------|-----|
| `sudo` para una tarea concreta | Quedarte todo el día como root “porque sí” |
| Leer el comando antes de Enter | Copiar `sudo rm -rf /` de un foro de broma |

En Ubuntu/Mint, tu usuario suele poder usar `sudo` si está en el grupo adecuado (`sudo`).

```bash
groups
# ¿apareces en sudo? (o wheel en otras distros)
```

## Por qué sale “Permission denied”

Causas típicas al aprender:

1. El fichero/carpeta no es tuyo y no tienes permiso.  
2. Intentas escribir en `/etc`, `/usr`, etc. sin `sudo`.  
3. En un directorio no tienes permiso de **entrada** (`x`) aunque creas que “deberías verlo”.  

```bash
ls /root
# a menudo: Permission denied
```

## Mini laboratorio

```bash
whoami
id
groups
ls -ld / /home "$HOME" /etc /tmp
touch "$HOME/mi_fichero_de_prueba"
ls -l "$HOME/mi_fichero_de_prueba"
```

## Relacionados

- [04-permisos](../01-fundamentos/04-permisos/) — chmod, chown, umask, sudo  
- [05-usuarios-sesiones](../02-sistema/05-usuarios-sesiones/) — cuentas, su, passwd  
- [arbol-de-directorios](arbol-de-directorios.md)  
