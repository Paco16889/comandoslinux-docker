# cp

Copia ficheros o directorios (*copy*).

## Sintaxis

```bash
cp [opciones] <origen> <destino>
cp [opciones] <origen>... <directorio_destino>/
```

> Recuerda: `[ ]` = opcional · `<origen>` / `<destino>` = los sustituyes tú · `...` = pueden ser varios.  
> No escribas los símbolos `[` `]` `<` `>`.  
> Detalle: [Cómo leer la sintaxis](../../README.md#cómo-leer-la-sintaxis-muy-importante).

## Ejemplos

```bash
cp <fichero> <fichero_copia>
# copia con otro nombre en el mismo sitio

cp <fichero> <directorio>/
# copia el fichero dentro de <directorio> (mismo nombre)

cp <fichero> <directorio>/<fichero_nuevo>
# copia dentro con otro nombre

cp <fichero1> <fichero2> <directorio>/
# copia varios ficheros a un directorio
```

Directorios (hace falta `-r` o `-R`):

```bash
cp -r <directorio> <directorio_copia>
# copia la carpeta entera

cp -r <directorio> /home/<usuario>/
# copia la carpeta al home de <usuario>
```

Opciones prácticas:

```bash
cp -i <fichero> <destino>
# pregunta antes de sobrescribir

cp -v <fichero> <directorio>/
# muestra qué está copiando

cp -a <directorio> <directorio_copia>
# archivo: conserva permisos, fechas, enlaces (muy usado en backups)
```

## Opciones útiles

| Opción | Qué hace |
|--------|----------|
| `-r` / `-R` | Copia directorios de forma recursiva |
| `-i` | Pregunta antes de sobrescribir |
| `-v` | Verbose |
| `-a` | Modo archivo (≈ `-dR --preserve=all`) |
| `-u` | Solo copia si el origen es más reciente o el destino no existe |
| `-n` | No sobrescribe (no-clobber) |

## Errores frecuentes

```text
cp: -r not specified; omitting directory '<directorio>'
```

- Intentaste copiar una carpeta sin `-r`.

```text
cp: cannot create regular file '<destino>': Permission denied
```

- No tienes permiso de escritura en el destino (a veces hace falta `sudo`, con cuidado).

- `cp <fichero> <directorio>` si `<directorio>` no existe: puede crear un fichero con ese nombre en lugar de fallar como esperabas. Comprueba con `ls`.

## Siguiente

[mv](mv.md)

## Relacionados

- [mv](mv.md) — mover o renombrar (no deja copia)
- [rm](rm.md) — borrar tras copiar si hacía falta
- [ls](../01-basicos/ls.md) — verificar el resultado



