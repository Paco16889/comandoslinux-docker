# pwd

Muestra el directorio de trabajo actual (*print working directory*).

## Sintaxis

```bash
pwd
```

> Detalle: [Cómo leer la sintaxis](../../README.md#cómo-leer-la-sintaxis-muy-importante).

## Ejemplos

```bash
pwd
# /home/<usuario>

cd /etc
pwd
# /etc

cd ~
pwd
# /home/<usuario>

cd /var/log
pwd
# /var/log

cd
pwd
# home otra vez
```

Variable de entorno equivalente:

```bash
echo "$PWD"
# suele coincidir con pwd

cd /tmp
echo "$PWD"
pwd
```

Volver al anterior y comprobar:

```bash
cd /etc
cd /tmp
cd -
pwd
# /etc

echo "$OLDPWD"
# directorio en el que estabas antes del último cd “efectivo”
```

### Con enlaces simbólicos

```bash
mkdir -p /tmp/demo_pwd/real
ln -s /tmp/demo_pwd/real /tmp/demo_pwd/enlace

cd /tmp/demo_pwd/enlace
pwd
# .../enlace   (ruta lógica: cómo llegaste)

pwd -P
# .../real     (ruta física: a dónde apunta de verdad)

pwd -L
# lógica (comportamiento por defecto habitual)
```

Tras un `cd` fallido:

```bash
pwd
# /home/<usuario>

cd /no/existe
# error…

pwd
# sigues en /home/<usuario>  (cd fallido no te mueve)
```

## Opciones útiles

| Opción | Qué hace |
|--------|----------|
| `-L` | Ruta lógica (por defecto; respeta enlaces simbólicos) |
| `-P` | Ruta física (resuelve enlaces simbólicos) |

## Errores frecuentes

- Confundir `pwd` con `cd`: `pwd` solo informa; no cambia de sitio.  
- Pensar que `pwd` siempre muestra el home: muestra **donde estás ahora**.  
- Ignorar `-P` cuando trabajas con symlinks y “no entiendes” en qué carpeta real estás.

## Relacionados

- [cd](cd.md) — cambiar de directorio  
- [ls](ls.md) — listar el contenido del directorio actual  
- [ln](../02-ficheros-directorios/ln.md) — crear enlaces  
