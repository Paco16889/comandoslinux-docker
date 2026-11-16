# chmod

Cambia los permisos de ficheros y directorios (*change mode*).

## Sintaxis

```bash
chmod [opciones] <permisos> <ruta>...
```

Hay dos formas de expresar `<permisos>`: **simbólica** (`u+x`) y **octal** (`755`).

## Forma simbólica

Quién:

| Letra | Significado |
|-------|-------------|
| `u` | user (dueño) |
| `g` | group |
| `o` | others |
| `a` | all (u+g+o) |

Qué operación:

| Símbolo | Significado |
|---------|-------------|
| `+` | Añadir permiso |
| `-` | Quitar permiso |
| `=` | Dejar exactamente esos permisos |

Permisos: `r`, `w`, `x`.

### Ejemplos simbólicos

```bash
chmod u+x <fichero>
# el dueño puede ejecutarlo

chmod g+w <fichero>
# el grupo puede escribir

chmod o-r <fichero>
# otros ya no pueden leer

chmod a+r <fichero>
# todos pueden leer

chmod u=rwx,g=rx,o= <fichero>
# dueño todo; grupo rx; otros nada

chmod +x <fichero>
# suele añadir x a quien corresponda (comportamiento habitual: a+x con matices de umask)
```

Directorios:

```bash
chmod u+x <directorio>
# poder entrar (cd) como dueño

chmod -R g+r <directorio>
# recursivo: lectura para el grupo en todo el árbol
```

Scripts (muy habitual):

```bash
chmod u+x <fichero>.sh
./<fichero>.sh
# sin x, ./fichero falla aunque el contenido sea correcto
```

## Forma octal (números)

Cada permiso vale: `r=4`, `w=2`, `x=1`. Se suman por bloque (dueño, grupo, otros).

| Octal | Equivale | Uso típico |
|-------|----------|------------|
| `755` | `rwxr-xr-x` | programas / carpetas públicas de lectura |
| `644` | `rw-r--r--` | ficheros de texto normales |
| `600` | `rw-------` | secretos (claves, credenciales) |
| `700` | `rwx------` | carpeta privada del dueño |
| `664` | `rw-rw-r--` | colaboración en el mismo grupo |
| `777` | `rwxrwxrwx` | todo el mundo todo (casi nunca recomendable) |

### Ejemplos octales

```bash
chmod 644 <fichero>
chmod 755 <fichero>
chmod 600 <fichero>
chmod 700 <directorio>

chmod 755 <directorio>/<fichero>.sh
chmod -R 755 <directorio>
# ¡cuidado! -R 755 en un árbol mezcla ficheros y carpetas con los mismos bits
```

Referencia rápida de cálculo:

```text
rwx = 4+2+1 = 7
rw- = 4+2   = 6
r-x = 4+1   = 5
r-- = 4     = 4
--- = 0
```

## Opciones útiles

| Opción | Qué hace |
|--------|----------|
| `-R` | Recursivo |
| `-v` | Verbose |
| `-c` | Solo muestra lo que cambia |
| `--reference=<fichero>` | Copia los permisos de otro fichero |

```bash
chmod --reference=<fichero_modelo> <fichero>
ls -l <fichero_modelo> <fichero>
```

## Comprobar el resultado

```bash
ls -l <fichero>
ls -ld <directorio>
stat <fichero>
# más detalle (modo en octal, etc.)
```

## Errores frecuentes

```text
chmod: changing permissions of '<ruta>': Operation not permitted
```

- No eres el dueño (hace falta el dueño o `sudo`, según política).

- Poner `777` “para que funcione”: abre el fichero a cualquiera; busca el permiso mínimo necesario.
- `chmod -R 755` sobre datos: los ficheros quedan ejecutables; a menudo se separan reglas para dirs y ficheros.

## Siguiente

[chown](chown.md)

## Relacionados

- [leer-ls-l](leer-ls-l.md) — interpretar el resultado
- [chown](chown.md) — si el problema es el dueño, no el modo
- [umask](umask.md) — permisos iniciales al crear



