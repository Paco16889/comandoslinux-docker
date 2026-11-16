# Seguridad básica (tips prácticos)

No hace falta ser paranoico el primer mes, pero sí evitar errores gordos: **secretos en la imagen**, contenedores siempre como root sin necesidad, publicar de más o montar el host en escritura sin pensar.

> **Cómo leer la sintaxis:** `<algo>` = lo sustituyes tú · `[algo]` = opcional.  
> Detalle: [Cómo leer la sintaxis](../../README.md#cómo-leer-la-sintaxis-muy-importante).

## Ideas previas

| Concepto | En pocas palabras |
|----------|-------------------|
| Root en el contenedor | Por defecto muchas imágenes arrancan como `root` (uid 0) |
| Secreto | Contraseña, token, clave; **no** debe quedar en capas de la imagen |
| `:ro` | Bind/volume en solo lectura |
| Superficie de ataque | Cuanto menos publiques y menos privilegios, menos daño si algo sale mal |
| Imagen mínima | Menos paquetes = menos cosas que parchear (Alpine, distroless…) |

```text
Mal:  Dockerfile con PASSWORD=1234  →  queda en la imagen para siempre
Bien: variable en runtime / fichero fuera / secret manager (más adelante)
```

## Sintaxis (herramientas que ya conoces)

```bash
# Usuario no root al ejecutar
docker run -d --user <uid>:<gid> <imagen>
docker run -d --user 1000:1000 <imagen>

# Publicar solo en localhost (no en toda la red)
docker run -d -p 127.0.0.1:8080:80 <imagen>

# Bind en solo lectura
docker run -d -v /ruta/host:/ruta/cont:ro <imagen>

# Capacidad / privilegio (evitar a ciegas)
docker run -d --privileged <imagen>   # casi nunca para novatos
```

En Dockerfile (idea):

```dockerfile
USER <usuario_no_root>
```

## Ejemplos

### 1) No meter secretos en la imagen

Mal:

```dockerfile
ENV DB_PASSWORD=supersecreto
```

Cualquiera con la imagen puede inspeccionar historial/capas.

Mejor en runtime (sigue visible en `inspect`, pero no queda horneado en Hub si no subes ese contenedor):

```bash
docker run -d --name app \
  -e DB_PASSWORD=<tu_password> \
  <imagen>
```

Aún mejor a medio plazo: fichero de entorno **no** versionado + `.dockerignore` que ignore `.env` (ver [dockerignore-contexto](dockerignore-contexto.md)).

```bash
# .env en el host (NO lo subas a git)
# DB_PASSWORD=...
docker run -d --env-file "$HOME/secreto.env" <imagen>
```

### 2) Montajes en solo lectura

```bash
mkdir -p "$HOME/mi-web-segura"
echo '<h1>Solo lectura</h1>' > "$HOME/mi-web-segura/index.html"

docker run -d --name web-ro \
  -p 127.0.0.1:8080:80 \
  -v "$HOME/mi-web-segura":/usr/share/nginx/html:ro \
  nginx:alpine

docker exec web-ro sh -c 'echo x > /usr/share/nginx/html/x.txt'
# Read-only file system
```

### 3) Publicar solo en 127.0.0.1

```bash
docker run -d --name web-local \
  -p 127.0.0.1:8082:80 \
  nginx:alpine

curl -I http://127.0.0.1:8082
# HTTP/1.1 200 OK
```

Así no abres el puerto a toda la LAN por defecto (`0.0.0.0`). En un servidor expuesto a Internet, combina con firewall ([ufw](../../05-extras/seguridad/ufw.md)).

### 4) Ejecutar como usuario no root (demo)

```bash
docker run --rm --user 1000:1000 alpine:latest id
# uid=1000 gid=1000
```

Ojo: si la app **necesita** escribir en rutas de root o abrir puertos &lt; 1024 dentro, fallará; hay que preparar la imagen (`USER` + permisos) o usar puerto alto.

### 5) USER en Dockerfile (idea)

```dockerfile
FROM alpine:latest
RUN adduser -D appuser
USER appuser
CMD ["id"]
```

```bash
# tras docker build -t demo-user:1 .
docker run --rm demo-user:1
# uid=…(appuser) …
```

### 6) Evitar `--privileged` y montar Docker socket

```bash
# Evita esto salvo que sepas exactamente por qué:
# docker run … --privileged
# docker run … -v /var/run/docker.sock:/var/run/docker.sock
```

Montar el socket de Docker = el contenedor puede controlar **todo** Docker del host (casi root).

### 7) Imágenes oficiales y tags fijos

```bash
docker pull nginx:1.27-alpine
# mejor que fiarte solo de "latest" en algo serio
```

### Checklist rápido novato

| Pregunta | Ideal |
|----------|--------|
| ¿Hay contraseñas en el Dockerfile? | No |
| ¿`.env` en `.dockerignore` / fuera de git? | Sí |
| ¿Bind de código en `:ro` si no escribe? | Sí |
| ¿`-p 127.0.0.1:…` en labs locales? | A menudo sí |
| ¿`--privileged`? | No, salvo caso muy concreto |
| ¿Límites de RAM/CPU? | Ver [limites-recursos](limites-recursos.md) |

## Errores frecuentes

- “Es solo un lab” y subir la imagen con secretos a Hub → el secreto queda público.  
- Confundir root **dentro** del contenedor con root del host: no es lo mismo, pero root en el contenedor sigue siendo peligroso si hay escapes/volumenes.  
- Abrir `0.0.0.0:5432` de una base de datos a Internet sin firewall ni clave.  
- Copiar `~/.ssh` o `docker.sock` al contenedor “para probar”.  

### Limpiar

```bash
docker rm -f web-ro web-local 2>/dev/null
```

## Relacionados

- Anterior: [restart-policies](restart-policies.md)  
- Siguiente: [registry-login](registry-login.md)  
- Volumes / bind: [bind-mounts](../03-volumes/bind-mounts.md)  
- UFW: [ufw](../../05-extras/seguridad/ufw.md)  
- [README de la carpeta](README.md)  
- Doc oficial: [Docker security](https://docs.docker.com/engine/security/) · [Dockerfile USER](https://docs.docker.com/reference/dockerfile/#user) · [docker run --user](https://docs.docker.com/reference/cli/docker/container/run/#user)  
