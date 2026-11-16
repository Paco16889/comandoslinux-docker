# umask

Define qué permisos se **quitan por defecto** al crear ficheros y directorios nuevos.

No cambia lo ya existente: afecta a lo que crees **a partir de ahora** en esa shell (hasta que lo cambies).

## Sintaxis

```bash
umask
umask <máscara>
```

## Idea rápida

Valores base típicos antes de aplicar umask:

| Al crear | Base habitual |
|----------|----------------|
| Fichero | `666` (`rw-rw-rw-`) — sin execute |
| Directorio | `777` (`rwxrwxrwx`) |

`umask` resta (en la práctica: desactiva bits). Ejemplo clásico:

```text
umask 022
fichero:    666 - 022 → 644  (rw-r--r--)
directorio: 777 - 022 → 755  (rwxr-xr-x)
```

## Ejemplos

Ver la máscara actual:

```bash
umask
# 0022
# (a veces sin el 0 inicial: 022; es la misma idea)

umask -S
# u=rwx,g=rx,o=rx
# (= lo que “queda permitido” en simbólico; con umask 022)
```

Probar el efecto:

```bash
umask 022
touch <fichero1>
mkdir <directorio1>
ls -l <fichero1>
ls -ld <directorio1>
```

Salida tipo:

```text
-rw-r--r-- 1 <usuario> <grupo> 0 … <fichero1>     ← ~644
drwxr-xr-x 2 <usuario> <grupo> … <directorio1>   ← ~755
```

Más restrictivo (solo tú):

```bash
umask 077
touch <fichero2>
mkdir <directorio2>
ls -l <fichero2>
ls -ld <directorio2>
```

```text
-rw------- 1 <usuario> <grupo> 0 … <fichero2>     ← ~600
drwx------ 2 <usuario> <grupo> … <directorio2>   ← ~700
```

Colaboración en el mismo grupo:

```bash
umask 002
touch <fichero3>
ls -l <fichero3>
```

```text
-rw-rw-r-- 1 <usuario> <grupo> 0 … <fichero3>     ← ~664 (grupo escribe)
```

Restaurar un valor habitual:

```bash
umask 022
```

## Máscaras habituales

| umask | Ficheros ≈ | Directorios ≈ | Uso |
|-------|------------|---------------|-----|
| `022` | `644` | `755` | Por defecto en muchos sistemas |
| `002` | `664` | `775` | Trabajo en grupo |
| `077` | `600` | `700` | Privacidad máxima |
| `027` | `640` | `750` | Grupo lee; otros nada |

## Persistencia

- `umask <máscara>` en la terminal dura la sesión (o hasta otro `umask`).
- Para hacerlo permanente, suele ir en `~/.bashrc` (o fichero de arranque de tu shell) como usuario, o en configuración del sistema.

```bash
echo 'umask 027' >> /home/<usuario>/.bashrc
# ejemplo; revisa antes de añadir líneas a tu config
```

## Errores frecuentes

- Creer que `umask 644` “pone 644”: la máscara no es el permiso final; es lo que se **quita**. Para 644 de fichero suele usarse `022`.
- Cambiar umask y no ver cambios en ficheros viejos: solo afecta a creaciones nuevas.
- Poner `umask 000` → ficheros/dirs muy permisivos; poco recomendable.

## Relacionados

- [chmod](chmod.md) — ajustar un fichero ya creado
- [touch](../02-ficheros-directorios/touch.md) / [mkdir](../02-ficheros-directorios/mkdir.md) — crear y comprobar con `ls -l`
- [leer-ls-l](leer-ls-l.md) — verificar el resultado
