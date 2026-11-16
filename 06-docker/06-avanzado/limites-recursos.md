# Límites de recursos (`memory`, `cpus`)

Sin límites, un contenedor puede usar **casi toda** la RAM/CPU del host.  
Con límites le pones un techo: útil en el portátil, en un VPS pequeño o cuando pruebas algo “pesado”.

> **Cómo leer la sintaxis:** `<algo>` = lo sustituyes tú · `[algo]` = opcional.  
> Detalle: [Cómo leer la sintaxis](../../README.md#cómo-leer-la-sintaxis-muy-importante).

## Ideas previas

| Concepto | En pocas palabras |
|----------|-------------------|
| `--memory` / `-m` | Tope de RAM del contenedor |
| `--cpus` | Cuánta CPU puede usar (ej. `1.5` = uno y medio) |
| OOM | *Out of memory*: el proceso puede morir si supera la RAM |
| Host | Tu Linux; los límites protegen al resto del sistema |
| Compose | Se pueden declarar límites en el YAML (ver formas abajo) |

```text
Host (8 GB RAM, 4 CPUs)
└── contenedor con -m 512m --cpus 1.0
    → como máximo ~512 MB y ~1 CPU
```

## Sintaxis

### `docker run`

```bash
docker run -d --name <nombre> \
  --memory <tamaño> \
  --cpus <número> \
  <imagen>
```

Ejemplos de tamaño: `256m`, `512m`, `1g`.

```bash
docker run … -m 512m --cpus 1.0 …
```

Inspección suave:

```bash
docker inspect --format 'mem={{.HostConfig.Memory}} cpus={{.HostConfig.NanoCpus}}' <contenedor>
```

(`Memory` en bytes; `NanoCpus` = CPUs × 1e9.)

### Compose — forma simple (Compose V2 / uso habitual en desktop/dev)

En muchos entornos con Compose V2 funciona:

```yaml
services:
  web:
    image: nginx:alpine
    mem_limit: 512m
    cpus: 1.0
```

### Compose — forma `deploy.resources` (origen Swarm; Compose la entiende en bastantes casos)

```yaml
services:
  web:
    image: nginx:alpine
    deploy:
      resources:
        limits:
          cpus: "1.0"
          memory: 512M
```

Si una clave no te hace efecto, prueba la otra forma o revisa la versión:

```bash
docker compose version
```

Para esta guía prioriza **`docker run -m` / `--cpus`** para ver el efecto claro; luego copia la idea a Compose.

## Ejemplos

### Contenedor con tope de memoria y CPU

```bash
docker run -d --name limitado \
  -m 256m \
  --cpus 0.5 \
  alpine:latest sleep 3600
```

```bash
docker inspect --format 'MemoryBytes={{.HostConfig.Memory}} NanoCpus={{.HostConfig.NanoCpus}}' limitado
# MemoryBytes=268435456
# NanoCpus=500000000
```

256 MiB ≈ `268435456` bytes; `0.5` CPU ≈ `500000000` nanoCPUs.

### Ver límites en `docker stats`

```bash
docker stats --no-stream limitado
```

Salida típica (columnas resumidas):

```text
CONTAINER   CPU %   MEM USAGE / LIMIT     MEM %
limitado    0.00%   1.2MiB / 256MiB       0.47%
```

La columna **LIMIT** debe reflejar el tope (`256MiB`).

```bash
docker ps --filter "name=limitado"
# … Up …   limitado
```

### Compose mínimo con límites

```bash
mkdir -p ~/compose-limits
cd ~/compose-limits

cat > compose.yaml << 'EOF'
services:
  web:
    image: nginx:alpine
    ports:
      - "8080:80"
    mem_limit: 256m
    cpus: 0.50
EOF

docker compose up -d
docker stats --no-stream
# … MEM USAGE / LIMIT … 256MiB …
```

### ¿Qué pasa si se pasa de RAM?

Depende del proceso y de si hay swap, pero la idea: el kernel puede matar el proceso (**OOM killer**) y el contenedor queda `Exited` con error.

No hace falta provocarlo a propósito en tu máquina; basta saber que el límite **no es un aviso amable**, es un techo.

### Reservar vs limitar (idea breve)

En `deploy.resources` a veces verás:

- `limits` — techo duro  
- `reservations` — “me gustaría al menos esto” (orquestación)

Para novatos en un solo PC: empieza solo con **limits** (`-m` / `--cpus`).

### Relación con el host

```bash
# RAM / CPU del host (recordatorio Linux)
free -h
nproc
```

No pongas límites absurdos (ej. `-m 64g` en un portátil de 8 GB): Docker no inventa RAM.

## Errores frecuentes

- Escribir `512mb` en lugar de `512m` / `512M` → formato inválido.  
- Pensar que `--cpus 1` “reserva en exclusiva” una CPU → es un **tope** de uso, no una CPU física arrancada para ti.  
- Poner límites en Compose con una sintaxis que tu versión ignora → comprueba con `docker stats` / `inspect`.  
- Olvidar unidades: `memory: 512` sin unidad puede interpretarse mal; usa `512M` / `512m`.  
- Limitar tanto que la app no arranca → healthcheck `unhealthy` o proceso que muere al inicio.

### Limpiar

```bash
docker rm -f limitado 2>/dev/null
cd ~/compose-limits 2>/dev/null && docker compose down 2>/dev/null
```

## Relacionados

- Anterior: [healthcheck](healthcheck.md)  
- Compose: [compose-basico](../05-compose/compose-basico.md) · [compose-servicios](../05-compose/compose-servicios.md)  
- Run: [docker-pull-run](../01-fundamentos/docker-pull-run.md)  
- Siguiente: [restart-policies](restart-policies.md) · [seguridad-basica](seguridad-basica.md)  
- [README de la carpeta](README.md)  
- Doc oficial: [Resource constraints](https://docs.docker.com/engine/containers/resource_constraints/)  
