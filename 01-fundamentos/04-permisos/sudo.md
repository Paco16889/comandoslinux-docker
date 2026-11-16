# sudo

Ejecuta un comando con privilegios de otro usuario (por defecto **root**).

Aquí es una **introducción** ligada a permisos; se amplía en `05-usuarios-sesiones`.

## Sintaxis

```bash
sudo <comando>
sudo -u <usuario> <comando>
```

## Ejemplos

```bash
sudo ls /root
# listar algo que tu usuario normal no puede

sudo cat /etc/shadow
# fichero sensible (solo lectura; no lo modifiques sin saber)

sudo chmod 644 <fichero>
sudo chown <usuario>:<grupo> <fichero>
sudo chown -R <usuario>:<grupo> <directorio>
```

Como otro usuario (no root):

```bash
sudo -u <usuario> id
sudo -u <usuario> ls /home/<usuario>
sudo -u <usuario> touch /home/<usuario>/<fichero>
```

Repetir el último comando con sudo (Bash):

```bash
!<comando_anterior_sin_privilegios>
# falló por permisos...

sudo !!
# reintenta el último comando con sudo
```

Shell interactiva como root (usar con cuidado):

```bash
sudo -i
# shell de login de root

# ... comandos ...
exit
```

Editar ficheros del sistema (según herramientas instaladas):

```bash
sudo nano <fichero>
sudoedit <fichero>
# sudoedit es más seguro: editas una copia temporal
```

## Opciones útiles

| Opción | Qué hace |
|--------|----------|
| `-u <usuario>` | Ejecutar como ese usuario |
| `-i` | Shell de login del usuario destino (env más “completo”) |
| `-s` | Shell con tu entorno (según configuración) |
| `-k` | Invalida el timestamp; pedirá contraseña otra vez |
| `-l` | Lista qué puedes hacer con sudo |
| `-v` | Renueva el timestamp sin lanzar comando |

```bash
sudo -l
# permisos de sudo de tu usuario

sudo -k
# olvidar la autenticación cacheada
```

## Buenas prácticas

- Prefiere `sudo <comando_concreto>` a quedarte en root todo el rato.
- Ten cuidado con `sudo rm -rf <ruta>` y con rutas mal escritas.
- Si un fichero acaba perteneciendo a root por error:

```bash
sudo chown <usuario>:<grupo> <ruta>
```

## Errores frecuentes

```text
<usuario> is not in the sudoers file.  This incident will be reported.
```

- Tu usuario no tiene permiso de sudo; un admin debe añadirlo a `sudo`/`wheel` según distro.

```text
Sorry, try again.
```

- Contraseña incorrecta: es la de **tu usuario**, no la de root (en la configuración habitual de Ubuntu y similares).

```text
sudo: <comando>: command not found
```

- El comando no está en el `PATH` que usa sudo, o no está instalado.

## Relacionados

- [chown](chown.md) / [chmod](chmod.md) — operaciones típicas con sudo
- [id](id.md) — quién eres antes y después (`sudo id`)
- Carpeta siguiente del temario: [05-usuarios-sesiones](../../02-sistema/05-usuarios-sesiones/)
