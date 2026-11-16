# Conceptos rápidos (antes de `run`)

Repaso práctico de **imagen vs contenedor** y dos comandos para comprobar que el Engine responde: `docker version` y `docker info`.  
Si acabas de instalar, esta página confirma que todo habla con el demonio antes de bajar imágenes.

> **Cómo leer la sintaxis:** `<algo>` = lo sustituyes tú · `[algo]` = opcional.  
> Detalle: [Cómo leer la sintaxis](../../README.md#cómo-leer-la-sintaxis-muy-importante).

## Ideas previas

| Concepto | En pocas palabras |
|----------|-------------------|
| Imagen | Plantilla **solo lectura** (el “molde”). Ej.: `hello-world`, `nginx` |
| Contenedor | Instancia de una imagen: puede estar **en marcha** o **parada** |
| Cliente (`docker`) | Lo que escribes en la terminal |
| Servidor / demonio | El Engine que escucha (suele ser el servicio `docker`) |
| Socket | Canal local por el que el cliente habla con el demonio (`/var/run/docker.sock`) |
| Host | Tu Linux donde está instalado Docker |

```text
Imagen  ──docker run──►  Contenedor (en marcha o parado)
   ▲                           │
   │                           │ docker stop / start / rm
   └── (sigue en el disco) ◄───┘
```

Una misma imagen sirve para **muchos** contenedores. Borrar un contenedor **no** borra la imagen.

## Sintaxis

```bash
docker version
docker info
```

Opcional (salida más corta / filtrada):

```bash
docker version --format '{{.Server.Version}}'
docker info --format '{{.ServerVersion}}'
```

## Ejemplos

### Comprobar cliente y servidor

```bash
docker version
```

Salida típica (resumida; los números cambian según tu instalación):

```text
Client: Docker Engine - Community
 Version:           27.x.x
 ...
Server: Docker Engine - Community
 Engine:
  Version:          27.x.x
  ...
```

Debes ver **Client** y **Server**.  
Si solo aparece Client y Server falla → el demonio no está accesible (servicio parado o permisos).

```bash
docker info
```

Resumen del motor: almacenamiento, contenedores, imágenes, runtime…  
Al principio verás muchos ceros (`Containers: 0`, `Images: 0`): es normal si aún no has lanzado nada.

### Solo la versión del servidor (si te gusta el formato)

```bash
docker version --format '{{.Client.Version}}'
# 27.x.x

docker version --format '{{.Server.Version}}'
# 27.x.x
```

### Recordatorio: imagen ≠ contenedor

Todavía no hace falta `run` aquí; solo la idea:

| Quieres… | Piensa en… |
|----------|------------|
| El paquete / plantilla | Imagen (`nginx:latest`) |
| Un proceso concreto con nombre | Contenedor (`mi-nginx`) |
| Varias copias del mismo servicio | Varios contenedores de la **misma** imagen |

Flujo del siguiente tema:

```text
1. docker pull <imagen>     → baja la plantilla (si no la tienes)
2. docker run …             → crea y arranca un contenedor
3. docker ps / logs / stop  → gestionar ese contenedor
```

### Si usas `sudo` vs grupo `docker`

```bash
# Si aún no estás en el grupo docker (o no has cerrado sesión):
sudo docker version
sudo docker info

# Tras añadir tu usuario al grupo docker y volver a entrar en la sesión:
docker version
docker info
```

Detalle del grupo: [instalacion-engine](../00-introduccion/instalacion-engine.md) (post-instalación).

## Errores frecuentes

- `Cannot connect to the Docker daemon` / `Is the docker daemon running?` → servicio parado:

  ```bash
  sudo systemctl status docker
  sudo systemctl start docker
  ```

- `permission denied` al socket `/var/run/docker.sock` → tu usuario no está en el grupo `docker`, o no has **cerrado sesión / reiniciado** tras añadirlo. Mientras tanto: `sudo docker …`.
- Confundir **versión del cliente** con “Docker no está instalado”: a veces el CLI está, pero el demonio no.
- Pensar que `docker info` “arregla” algo: solo **informa**; no descarga ni arranca contenedores.
- Mezclar mentalmente imagen y contenedor: `docker images` lista plantillas; `docker ps` lista contenedores (eso va en las siguientes páginas).


## Siguiente

[mismo “Hello from Docker!”; al terminar el contenedor se borra solo](docker-pull-run.md)

## Relacionados

- Anterior: [que-es-docker](../00-introduccion/que-es-docker.md) · [instalacion-engine](../00-introduccion/instalacion-engine.md)  
- Siguiente: [docker-pull-run](docker-pull-run.md) — bajar imágenes y arrancar contenedores  
- [README de la carpeta](README.md)  
- Doc oficial: [What is a container?](https://docs.docker.com/get-started/docker-concepts/the-basics/what-is-a-container/) · [docker version](https://docs.docker.com/reference/cli/docker/version/) · [docker info](https://docs.docker.com/reference/cli/docker/system/info/)  
