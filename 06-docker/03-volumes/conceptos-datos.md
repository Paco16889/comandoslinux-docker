# Conceptos de datos (volume vs bind mount)

Por defecto, lo que escribes **dentro** del contenedor vive en su capa escribible: si borras el contenedor, **se pierde**.  
Para guardar datos (o editar ficheros desde el host) Docker ofrece sobre todo dos formas: **volume** y **bind mount**.

> **Cómo leer la sintaxis:** `<algo>` = lo sustituyes tú · `[algo]` = opcional.  
> Detalle: [Cómo leer la sintaxis](../../README.md#cómo-leer-la-sintaxis-muy-importante).

## Ideas previas

| Concepto | En pocas palabras |
|----------|-------------------|
| Capa escribible | Disco “interno” del contenedor; desaparece con `docker rm` |
| Persistencia | Que los datos sigan tras borrar/recrear el contenedor |
| Volume (nombrado) | Almacén gestionado por Docker (`docker volume …`) |
| Bind mount | Carpeta/fichero **real de tu Linux** montado dentro del contenedor |
| Volumen anónimo | Volume sin nombre (Docker inventa un ID); fácil de olvidar |
| Mount | “Enganchar” esa ruta del host/volume en una ruta del contenedor |

```text
Sin persistencia:
  contenedor ──rm──► datos internos perdidos

Con volume o bind:
  contenedor ──rm──► los datos del volume/carpeta host siguen
```

## Volume vs bind mount (tabla clara)

| | **Volume** (Docker) | **Bind mount** (ruta del host) |
|--|---------------------|--------------------------------|
| ¿Quién controla el sitio? | Docker (suele estar bajo `/var/lib/docker/volumes/…`) | **Tú** (ej. `/home/<usuario>/datos`) |
| ¿Lo ves fácil en el explorador? | No tanto (ruta del demonio) | Sí: es tu carpeta |
| Uso típico novato | Bases de datos, datos “del servicio” | Código, configs, compartir con el host |
| Comando típico | `-v <nombre>:<ruta_en_contenedor>` | `-v /ruta/host:<ruta_en_contenedor>` |
| Portable entre máquinas | Más (lo gestiona Docker) | Depende de que exista la misma ruta en el host |
| Riesgo | Borrar el volume a propósito (`volume rm`) | Pisar/borrar ficheros del host desde el contenedor |

```text
Volume nombrado:
  Docker volume "datos-web"  ──montado──►  /usr/share/nginx/html  (dentro)

Bind mount:
  /home/<usuario>/mi-web     ──montado──►  /usr/share/nginx/html  (dentro)
```

## Sintaxis (vista previa)

Las dos se suelen pasar al crear el contenedor:

```bash
# Volume nombrado
docker run -d --name <nombre> \
  -v <nombre_volume>:<ruta_contenedor> \
  <imagen>

# Bind mount (ruta absoluta del host)
docker run -d --name <nombre> \
  -v /ruta/en/tu/linux:<ruta_contenedor> \
  <imagen>
```

Forma más explícita (misma idea; detalle en [bind-mounts](bind-mounts.md)):

```bash
docker run -d --name <nombre> \
  --mount type=volume,source=<nombre_volume>,target=<ruta_contenedor> \
  <imagen>

docker run -d --name <nombre> \
  --mount type=bind,source=/ruta/en/tu/linux,target=<ruta_contenedor> \
  <imagen>
```

## Ejemplos (idea, sin profundizar aún)

### Sin volumen: se pierde

```bash
docker run -d --name demo-tmp alpine:latest sleep 3600
# ID…

docker exec demo-tmp sh -c 'echo hola > /tmp/nota.txt'
docker exec demo-tmp cat /tmp/nota.txt
# hola

docker rm -f demo-tmp
# demo-tmp
# la nota se fue con el contenedor
```

### Con volume nombrado: sobrevive

```bash
docker volume create datos-demo
# datos-demo

docker volume ls
```

```text
DRIVER    VOLUME NAME
local     datos-demo
```

```bash
docker run -d --name demo-vol -v datos-demo:/data alpine:latest sleep 3600
docker exec demo-vol sh -c 'echo hola > /data/nota.txt'
docker rm -f demo-vol
# demo-vol

docker run --rm -v datos-demo:/data alpine:latest cat /data/nota.txt
# hola
```

### Con bind mount: editas desde el host

```bash
mkdir -p ~/docker-bind-demo
echo 'desde el host' > ~/docker-bind-demo/nota.txt

docker run --rm \
  -v ~/docker-bind-demo:/data \
  alpine:latest cat /data/nota.txt
# desde el host
```

(En la práctica, mejor ruta **absoluta** en `-v`; detalle en [bind-mounts](bind-mounts.md).)

## ¿Cuál elijo al principio?

| Situación | Suele ir mejor… |
|-----------|-----------------|
| “Quiero que la BBDD no se borre” | **Volume** nombrado |
| “Quiero editar HTML/código en mi carpeta” | **Bind mount** |
| “Solo estoy probando 5 minutos” | Ninguno, o volume anónimo (y luego limpia) |

## Errores frecuentes

- Pensar que `-p` guarda datos → eso es **red/puertos**, no disco.
- Confundir volume con imagen: la imagen es la plantilla; el volume es **datos**.
- Usar volumen anónimo sin saberlo (`-v /data` solo) y luego no encontrar “dónde están” los ficheros.
- Borrar el contenedor y creer que borraste el volume → el volume nombrado **sigue** hasta `docker volume rm`.
- Montar encima de un directorio que la imagen ya traía lleno → ves el contenido del mount, no el de la imagen (a veces confunde).

## Relacionados

- Siguiente: [volumes](volumes.md) — crear, listar, montar, borrar volumes  
- Luego: [bind-mounts](bind-mounts.md) — carpetas del host, `--mount`, permisos  
- Contenedores: [docker-stop-start-rm](../01-fundamentos/docker-stop-start-rm.md)  
- [README de la carpeta](README.md)  
- Doc oficial: [Storage](https://docs.docker.com/engine/storage/) · [Volumes vs bind mounts](https://docs.docker.com/engine/storage/volumes/)  
