# Contexto de build y `.dockerignore`

Al hacer `docker build`, Docker envía al demonio una carpeta llamada **contexto** (casi siempre `.`).  
`.dockerignore` decide **qué no se envía**: builds más rápidos, menos riesgo de colar secretos o basura.

> **Cómo leer la sintaxis:** `<algo>` = lo sustituyes tú · `[algo]` = opcional.  
> Detalle: [Cómo leer la sintaxis](../../README.md#cómo-leer-la-sintaxis-muy-importante).

Intro breve ya vista en [docker-build](../02-imagenes-dockerfile/docker-build.md); aquí lo ampliamos.

## Ideas previas

| Concepto | En pocas palabras |
|----------|-------------------|
| Contexto | Carpeta que se empaqueta y manda al Engine al construir |
| `COPY` / `ADD` | Solo pueden copiar ficheros **dentro** del contexto |
| `.dockerignore` | Lista de patrones excluidos del contexto (como un `.gitignore` para build) |
| Tamaño del contexto | Cuanto más grande, más lento el “Sending build context…” |
| Secreto en contexto | Si está en la carpeta y no lo ignoras, puede acabar en una capa si haces `COPY .` |

```text
~/mi-app/
├── Dockerfile
├── .dockerignore     ← filtra
├── src/              ← suele entrar
├── .git/             ← casi siempre excluir
└── node_modules/     ← casi siempre excluir
        │
        ▼  docker build -t … .
   contexto filtrado → demonio → imagen
```

## Sintaxis

```bash
docker build -t <nombre>[:<tag>] <ruta_contexto>
```

Fichero `.dockerignore` (en la raíz del contexto), un patrón por línea:

```gitignore
# comentarios con #
.git
**/node_modules
*.log
tmp
.env
```

| Idea de patrón | Ejemplo |
|----------------|---------|
| Nombre exacto | `.git` |
| Comodín | `*.log` |
| Carpeta | `node_modules` o `node_modules/` |
| Negar (re-incluir) | `!importante.txt` (avanzado; úsalo con cuidado) |

## Ejemplos

### Ver el problema sin `.dockerignore`

```bash
mkdir -p ~/demo-context/app
cd ~/demo-context

# Simula “basura” grande en el contexto
mkdir -p .git/objects
dd if=/dev/zero of=.git/objects/peso.bin bs=1M count=20 status=none
echo 'hola' > app/index.html

cat > Dockerfile << 'EOF'
FROM alpine:latest
COPY app/index.html /index.html
CMD ["cat", "/index.html"]
EOF
```

```bash
docker build -t demo-ctx:1 .
# Verás algo tipo: transferring / transferring context … (incluye .git aunque no hagas COPY de .git)
```

La imagen final puede ser pequeña, pero **igual enviaste** el contexto gordo.

### Añadir `.dockerignore`

```bash
cat > .dockerignore << 'EOF'
.git
.gitignore
*.md
tmp
*.log
**/.DS_Store
EOF

docker build -t demo-ctx:2 .
# el contexto debería transferirse mucho más ligero
```

Comprobar:

```bash
docker run --rm demo-ctx:2
# hola
```

### Ejemplo realista (proyecto web / node mental)

```gitignore
.git
.gitignore

# dependencias (se instalan en el Dockerfile si hace falta)
node_modules
npm-debug.log*

# entornos y secretos
.env
.env.*
!.env.example

# basura de editor / SO
.idea
.vscode
*.swp
.DS_Store

# docs y tests que no van a la imagen
README.md
docs
coverage
*.log
tmp
dist
```

Ajusta a tu stack: si tu `Dockerfile` hace `COPY package*.json` y luego `npm ci`, **sí** quieres ignorar `node_modules` del host.

### `COPY .` es el caso peligroso

```dockerfile
COPY . /app
```

Sin `.dockerignore`, metes en la imagen (o al menos en el contexto) cosas que no querías.  
Con `.dockerignore`, reduces lo que `COPY .` puede ver.

### Lo que ignore no existe para `COPY`

```bash
echo secreto > ~/demo-context/secreto.txt
echo 'secreto' >> ~/demo-context/.dockerignore

# Dockerfile que intenta copiarlo:
# COPY secreto.txt /secreto.txt  → fallará en el build
```

```text
ERROR: failed to calculate checksum … "/secreto.txt": not found
```

Eso es bueno si el secreto no debía entrar; malo si lo necesitabas en la imagen (entonces no lo ignores, o cópialo con otra estrategia segura más adelante).

### Relación con multi-stage

`.dockerignore` limpia el **contexto**.  
[Multi-stage](multi-stage.md) limpia la **imagen final**.  
Las dos se complementan.

## Errores frecuentes

- Poner `.dockerignore` fuera del contexto (otra carpeta) → no aplica.  
- Ignorar el propio `Dockerfile` por un patrón demasiado amplio → el build se vuelve loco.  
- Creer que “si no hago `COPY` de `.git`, no se envía” → **sí se envía** como parte del contexto si no está en `.dockerignore`.  
- Ignorar un fichero que luego `COPY` necesita → error `not found`.  
- Meter `.env` con contraseñas y hacer `COPY .` sin ignore → riesgo alto.

## Siguiente

[Multi-stage builds](multi-stage.md)

## Relacionados

- Build básico: [docker-build](../02-imagenes-dockerfile/docker-build.md) · [dockerfile](../02-imagenes-dockerfile/dockerfile.md)  
- Siguiente: [multi-stage](multi-stage.md)  
- [README de la carpeta](README.md)  
- Doc oficial: [Build context](https://docs.docker.com/build/building/context/) · [.dockerignore](https://docs.docker.com/build/building/context/#dockerignore-files)



