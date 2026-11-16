# cd

Cambia el directorio de trabajo actual (*change directory*).

## Sintaxis

```bash
cd [<ruta>]
```

> Recuerda: `[ ]` = opcional (no se escriben). `<ruta>` = lo sustituyes tú (sin `<` `>`).  
> Detalle: [Cómo leer la sintaxis](../../README.md#cómo-leer-la-sintaxis-muy-importante).

Sin argumentos, suele llevarte a tu home (por ejemplo `/home/ana` si tu usuario es `ana`).

## Ejemplos

```bash
cd /etc
# ruta absoluta

cd <directorio>
# ruta relativa (busca <directorio> desde donde estás)

cd /home/<usuario>/<directorio>
# absoluta hasta una carpeta concreta

cd ..
# subir un nivel

cd ../..
# subir dos niveles

cd ../<directorio>
# subir y entrar en un hermano

cd ~
# home del usuario actual

cd ~<usuario>
# home de otro usuario (si tienes permiso para entrar)

cd -
# volver al directorio anterior

cd "<directorio con espacios>"
# comillas si el nombre tiene espacios
```

Secuencia típica:

```bash
pwd
# /home/<usuario>

cd <directorio>
pwd
# /home/<usuario>/<directorio>

cd -
pwd
# /home/<usuario>   (vuelves al anterior)
```

## Rutas absolutas vs relativas

| Tipo | Ejemplo | Significado |
|------|---------|-------------|
| Absoluta | `/home/<usuario>/<directorio>` | Empieza en la raíz `/` |
| Relativa | `<directorio>` o `../<directorio>` | Parte desde el directorio actual |

## Atajos útiles

| Destino | Comando |
|---------|---------|
| Home | `cd` o `cd ~` |
| Home de `<usuario>` | `cd ~<usuario>` |
| Directorio anterior | `cd -` |
| Subir un nivel | `cd ..` |

## Errores frecuentes

```text
bash: cd: <directorio>: No such file or directory
```

- La ruta no existe, o hay un typo.
- Espacios en el nombre: usa comillas → `cd "<directorio con espacios>"`.
- Confundir `cd /home` (absoluta) con `cd home` (relativa: busca `home` dentro del directorio actual).

## Relacionados

- [pwd](pwd.md) — comprobar dónde quedaste
- [ls](ls.md) — ver qué hay al llegar
