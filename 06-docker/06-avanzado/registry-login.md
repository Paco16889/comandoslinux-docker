# Registry y `docker login`

Un **registry** es un almacén de imágenes (el más famoso: **Docker Hub**).  
Hasta ahora has hecho `pull` de imágenes públicas sin cuenta. Con **`docker login`** autenticas para subir (`push`) o bajar privadas.

> **Cómo leer la sintaxis:** `<algo>` = lo sustituyes tú · `[algo]` = opcional.  
> Detalle: [Cómo leer la sintaxis](../../README.md#cómo-leer-la-sintaxis-muy-importante).

## Ideas previas

| Concepto | En pocas palabras |
|----------|-------------------|
| Registry | Servidor de imágenes (Hub, GHCR, GitLab, uno propio…) |
| Docker Hub | Registry público por defecto (`docker.io`) |
| `login` | Guardar credenciales para ese registry |
| `logout` | Olvidar esas credenciales en esta máquina |
| `push` | Subir una imagen tuya al registry |
| Imagen privada | Solo quien tiene permiso (y suele estar logueado) puede bajarla |

```text
docker build -t <usuario>/<repo>:<tag> .
docker login
docker push <usuario>/<repo>:<tag>
```

Esta página **no** monta un registry privado paso a paso; solo la idea y el login.

## Sintaxis

```bash
docker login [ <registry> ]
docker login -u <usuario> [ <registry> ]
docker logout [ <registry> ]

docker tag <imagen_local> <usuario>/<repositorio>:<tag>
docker push <usuario>/<repositorio>:<tag>
docker pull <usuario>/<repositorio>:<tag>
```

| Caso | Comando |
|------|---------|
| Hub (default) | `docker login` |
| Otro registry | `docker login ghcr.io` (ejemplo) |
| Salir | `docker logout` |

Sin `<registry>`, se asume Docker Hub.

## Ejemplos

### Comprobar que puedes usar Hub sin login (público)

```bash
docker pull hello-world
# funciona sin cuenta para imágenes públicas habituales
```

### Iniciar sesión en Docker Hub

```bash
docker login
# Username: <tu_usuario_hub>
# Password: <token_o_password>
```

Salida típica si va bien:

```text
Login Succeeded
```

Hoy Docker Hub recomienda **Access Token** en lugar de la contraseña de la cuenta (créalo en la web de Hub → Account Settings → Security).

```bash
docker login -u <tu_usuario_hub>
# cuando pida Password: pega el token
```

### Etiquetar para Hub y (concepto de) push

Las imágenes en Hub suelen llamarse `<usuario>/<nombre>:<tag>`:

```bash
docker build -t demo-web:1 .
docker tag demo-web:1 <tu_usuario_hub>/demo-web:1
docker images | grep demo-web
```

Subir (solo si tienes cuenta y quieres publicar):

```bash
docker push <tu_usuario_hub>/demo-web:1
# The push refers to repository [docker.io/<tu_usuario_hub>/demo-web]
# … layers …
```

Límites de Hub (cuotas de pulls/repos) cambian con el tiempo; si falla por rate limit, suele pedir login o esperar.

### Cerrar sesión

```bash
docker logout
# Removing login credentials for https://index.docker.io/v1/
```

### Registry distinto de Hub (idea)

```bash
docker login <registry_host>
# ejemplo conceptual: ghcr.io , registry.gitlab.com , registro.empresa.local

docker tag demo-web:1 <registry_host>/<ruta>/<nombre>:<tag>
docker push <registry_host>/<ruta>/<nombre>:<tag>
```

No configuramos aquí certificados ni un registry self-hosted: eso es otro proyecto.

### ¿Dónde se guardan las credenciales?

Habitualmente en el almacén de credenciales del sistema o en un fichero de config de Docker del usuario (`~/.docker/config.json`).  
Trátalo como **secreto**: no lo subas a git.

### Relación con imágenes locales

```bash
docker images
# lo local sigue existiendo tras logout
# logout solo quita autenticación para pull/push privados
```

## Errores frecuentes

- `denied` / `unauthorized` al `push` → no estás logueado, usuario mal, o el repo no es tuyo.  
- Etiquetar solo `demo-web:1` y hacer push → Hub espera `usuario/demo-web:1`.  
- Pegar la contraseña de la cuenta cuando Hub exige token (o al revés).  
- Confundir `login` con “instalar Docker”: el Engine ya está; login es solo registry.  
- Montar un registry privado “para probar” el primer día → fuera de alcance; usa Hub o el de tu proveedor.

## Relacionados

- Anterior: [seguridad-basica](seguridad-basica.md)  
- Siguiente: [rootless-vision](rootless-vision.md)  
- Imágenes: [imagenes](../02-imagenes-dockerfile/imagenes.md) · [docker-build](../02-imagenes-dockerfile/docker-build.md)  
- [README de la carpeta](README.md)  
- Doc oficial: [docker login](https://docs.docker.com/reference/cli/docker/login/) · [docker push](https://docs.docker.com/reference/cli/docker/push/) · [Docker Hub](https://docs.docker.com/docker-hub/)  
