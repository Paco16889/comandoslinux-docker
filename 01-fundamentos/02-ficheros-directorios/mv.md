# mv

Mueve o renombra ficheros y directorios (*move*).

## Sintaxis

```bash
mv [opciones] <origen> <destino>
mv [opciones] <origen>... <directorio_destino>/
```

## Ejemplos

Renombrar:

```bash
mv <fichero> <fichero_nuevo>
# renombra en el mismo directorio

mv <directorio> <directorio_nuevo>
# renombra una carpeta
```

Mover:

```bash
mv <fichero> <directorio>/
# mueve el fichero dentro de <directorio>

mv <fichero> /home/<usuario>/<directorio>/
# mueve a otra ruta absoluta

mv <fichero1> <fichero2> <directorio>/
# mueve varios a un directorio
```

Mover y renombrar a la vez:

```bash
mv <fichero> <directorio>/<fichero_nuevo>
```

Con confirmación:

```bash
mv -i <fichero> <destino>
# pregunta si <destino> ya existe

mv -v <fichero> <directorio>/
# muestra la operación
```

## Opciones útiles

| Opción | Qué hace |
|--------|----------|
| `-i` | Pregunta antes de sobrescribir |
| `-n` | No sobrescribe |
| `-v` | Verbose |
| `-u` | Solo mueve si el origen es más reciente o el destino no existe |

## Errores frecuentes

```text
mv: cannot move '<origen>' to '<destino>': No such file or directory
```

- El directorio destino no existe: créalo con `mkdir -p <directorio>` antes.

- Confundir `mv` con `cp`: `mv` **no** deja copia en el origen.
- Sobrescribir sin querer: usa `mv -i` mientras aprendes.

## Siguiente

[rm](rm.md)

## Relacionados

- [cp](cp.md) — copiar (deja el original)
- [rm](rm.md) — borrar
- [mkdir](mkdir.md) — crear destino si falta



