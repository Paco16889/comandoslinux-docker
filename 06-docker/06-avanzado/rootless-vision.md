# Docker rootless (visión)

**Rootless** significa ejecutar el Engine **sin** demonio root: el daemon corre como tu usuario.  
Es una opción de endurecimiento; **no** es el camino principal de esta guía (aquí usamos Engine “clásico” con grupo `docker` / `sudo`).

> **Cómo leer la sintaxis:** `<algo>` = lo sustituyes tú · `[algo]` = opcional.  
> Detalle: [Cómo leer la sintaxis](../../README.md#cómo-leer-la-sintaxis-muy-importante).

## Ideas previas

| Concepto | En pocas palabras |
|----------|-------------------|
| Engine clásico | Demonío suele ser root; tu usuario entra al grupo `docker` |
| Rootless | Demonío en espacio de usuario; menos privilegios en el host |
| Socket | En rootless no es el `/var/run/docker.sock` “de sistema” habitual |
| Limitaciones | Redes, puertos bajos (&lt; 1024), algunos drivers: más restricciones |
| Esta guía | Prioriza install oficial **no** rootless |

```text
Guía (lo que has instalado):
  systemctl → docker.service (root) → grupo docker → docker …

Rootless (visión):
  usuario → dockerd rootless → docker -H unix://…
```

## Sintaxis (solo orientación)

No lo instalamos paso a paso aquí. La documentación oficial describe scripts del tipo:

```bash
dockerd-rootless-setuptool.sh install
```

Y variables / contexto:

```bash
export PATH="$HOME/bin:$PATH"
# systemctl --user start docker
docker info
```

Comandos concretos y requisitos (uidmap, dbus, etc.) cambian: sigue siempre la doc actual.

## Ejemplos (qué esperar / qué no)

### Lo que ya usas en esta guía (no rootless)

```bash
docker info 2>/dev/null | head -n 8
# Client: …
# Server:
#  Containers: …
#  Server Version: …

systemctl is-active docker
# active
```

Post-install clásico: [instalacion-engine](../00-introduccion/instalacion-engine.md) (grupo `docker`).

### Cuándo alguien mira rootless

- Compartes máquina y no quieres usuarios en el grupo `docker` (ese grupo ≈ root vía API de Docker).  
- Políticas de seguridad de empresa / laboratorio.  
- Experimentar aislamiento extra en un portátil personal.

### Limitaciones típicas (para no sorprenderte)

| Tema | Idea |
|------|------|
| Puerto 80/443 en el host | A menudo necesitas puertos altos o config extra |
| Rendimiento / red | Puede diferir del modo clásico |
| Apps “privilegiadas” | Más cosas fallan o piden workarounds |
| Documentación de tutoriales | El 90 % asume Docker clásico |

### Comprobar si *ya* estás en rootless (por curiosidad)

```bash
docker info 2>/dev/null | grep -i rootless || true
# si no aparece, casi seguro estás en modo clásico (lo normal en esta guía)
```

### Doc oficial (fuente de verdad)

- [Run the Docker daemon as a non-root user (Rootless mode)](https://docs.docker.com/engine/security/rootless/)  
- Post-install clásico: [Linux post-install](https://docs.docker.com/engine/install/linux-postinstall/)

Cuando “renovéis” el bloque o entre Desktop/tokens, se puede ampliar; ahora basta la visión.

## Errores frecuentes

- Mezclar demonio **root** y **rootless** en la misma cabeza (`DOCKER_HOST` apuntando a otro socket) → “no veo mis contenedores”.  
- Seguir un tutorial rootless sin cumplir requisitos del distro → install a medias.  
- Pensar que rootless = “ya no hay riesgos” → reduce superficie; no elimina mala praxis (secretos, imágenes dudosas…).  
- Abandonar la guía principal para pelearse con rootless el primer día → mejor cerrar A–F clásico primero.

## Siguiente

[Qué es Docker Desktop (en Linux)](../07-desktop/que-es-desktop.md)

## Relacionados

- Anterior: [registry-login](registry-login.md)  
- Install Engine (camino de la guía): [instalacion-engine](../00-introduccion/instalacion-engine.md)  
- Tips: [seguridad-basica](seguridad-basica.md)  
- Cierre del proyecto: [fin-proyecto](../../fin-proyecto.md)  
- [README de la carpeta](README.md)  
- Doc oficial: [Rootless mode](https://docs.docker.com/engine/security/rootless/) · [Post-install](https://docs.docker.com/engine/install/linux-postinstall/)



