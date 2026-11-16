# tar y gzip

`tar` junta muchos ficheros en **un archivo** (tarball).  
`gzip` (y amigos) **comprimen**. Muy a menudo se usan juntos: `.tar.gz` / `.tgz`.

## Sintaxis

```bash
tar [opciones] <archivo.tar> [<ruta>...]
gzip [opciones] <fichero>
gunzip [opciones] <fichero>.gz
```

> Letras típicas de `tar`: `-c` create, `-x` extract, `-t` list, `-f` file, `-v` verbose, `-z` gzip, `-C` directorio.

## Crear un .tar.gz

```bash
tar -czvf <fichero>.tar.gz <directorio>/
tar -czvf copia-docs.tar.gz Documentos/
# con -v ves cada ruta que mete, por ejemplo:
# Documentos/
# Documentos/notas.txt
# Documentos/fotos/img1.jpg
# al terminar: vuelve el prompt; el .tar.gz ya está creado
```

| Letra | Significado |
|-------|-------------|
| `c` | create (crear) |
| `z` | filtrar con gzip |
| `v` | verbose (lista lo que mete) |
| `f` | el fichero destino va a continuación |

Varios orígenes:

```bash
tar -czvf backup.tar.gz <fichero1> <fichero2> <directorio>/
# lista en pantalla cada fichero/carpeta que entra en backup.tar.gz
```

## Listar sin extraer

```bash
tar -tzvf <fichero>.tar.gz
# no extrae; solo enseña el contenido. Ejemplo:
# -rw-r--r-- paco/paco  120 2026-11-16 21:50 Documentos/notas.txt
# drwxr-xr-x paco/paco    0 2026-11-16 22:00 Documentos/fotos/

tar -tzvf backup.tar.gz | less
# mismo listado, pero paginado (q para salir)
```

## Extraer

```bash
tar -xzvf <fichero>.tar.gz
# extrae en el directorio actual
# -v otra vez lista lo que saca: Documentos/notas.txt …

mkdir -p <directorio>
tar -xzvf <fichero>.tar.gz -C <directorio>/
# extrae dentro de <directorio>
# ls <directorio>/ → verás las carpetas/ficheros descomprimidos
```

## Solo .gz de un fichero

```bash
gzip <fichero>
# crea <fichero>.gz y quita el original (por defecto)
# ls: ya no está <fichero>; sí está <fichero>.gz

gzip -k <fichero>
# keep: deja el original (según versión)
# ls: <fichero> y <fichero>.gz a la vez

gunzip <fichero>.gz
# o: gzip -d <fichero>.gz
# vuelve el fichero sin .gz; desaparece el .gz
```

## Otros formatos que verás

```bash
tar -cjvf <fichero>.tar.bz2 <directorio>/
# bzip2 (letra j)
# salida similar a -czvf: lista rutas y crea el .tar.bz2

tar -cJvf <fichero>.tar.xz <directorio>/
# xz (letra J mayúscula)
# igual de idea; el fichero final pesa distinto según el formato
```

## Ejemplo de backup rápido

```bash
fecha=$(date +%F)
tar -czvf "backup-$fecha.tar.gz" "$HOME/<directorio>/"
ls -lh backup-$fecha.tar.gz
# ejemplo: -rw-r--r-- 1 paco paco 4,2M nov 16 23:20 backup-2026-11-16.tar.gz
# (el tamaño depende de lo que hayas metido)
```

## Errores frecuentes

- Olvidar `-f` y que `tar` se quede esperando o use un nombre raro.  
- Extraer sin mirar el contenido y llenar el directorio actual de ficheros sueltos → primero `-tzvf`.  
- Confundir `tar -xzvf` (extraer) con `tar -czvf` (crear).

## Siguiente

[cron / crontab](cron.md)

## Relacionados

- [cp](../../01-fundamentos/02-ficheros-directorios/cp.md) — copiar sin empaquetar  
- [find](../../01-fundamentos/02-ficheros-directorios/find.md) — elegir qué entrar en el backup



