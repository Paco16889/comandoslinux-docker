# 06 — Avanzado (Docker)

Capas siguientes cuando el núcleo (fundamentos → compose) ya está sólido.

> **Estado:** Listo.

## Orden de estudio

### Pack prioridad alta

1. [dockerignore-contexto](dockerignore-contexto.md) — contexto de build y `.dockerignore`  
2. [multi-stage](multi-stage.md) — builds en varias etapas  
3. [healthcheck](healthcheck.md) — salud del contenedor + Compose  
4. [limites-recursos](limites-recursos.md) — `--memory`, `--cpus`  

### Pack prioridad media + visión

5. [restart-policies](restart-policies.md) — `--restart` / Compose  
6. [seguridad-basica](seguridad-basica.md) — tips prácticos  
7. [registry-login](registry-login.md) — `docker login` / Hub  
8. [rootless-vision](rootless-vision.md) — visión (no camino principal)  

Antes: [05-compose](../05-compose/) · [02-imagenes-dockerfile](../02-imagenes-dockerfile/).  
Desktop: [07-desktop](../07-desktop/) (bloque aparte).  

Doc oficial (atajos): [Multi-stage](https://docs.docker.com/build/building/multi-stage/) · [HEALTHCHECK](https://docs.docker.com/reference/dockerfile/#healthcheck) · [Resources](https://docs.docker.com/engine/containers/resource_constraints/) · [Restart](https://docs.docker.com/engine/containers/start-containers-automatically/) · [Security](https://docs.docker.com/engine/security/) · [Rootless](https://docs.docker.com/engine/security/rootless/).
