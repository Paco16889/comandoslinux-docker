# Solución — Docker

## Ejercicio 1

```bash
docker run --rm hello-world
# Hello from Docker!

# si lo lanzaste sin --rm:
# docker ps -a
# docker rm <id_o_nombre>
```

## Ejercicio 2

```bash
docker run -d --name ejer-web -p 8080:80 nginx:alpine
curl -I http://127.0.0.1:8080
# HTTP/1.1 200 OK

docker logs ejer-web
docker stop ejer-web
docker rm ejer-web
```

## Ejercicio 3

```bash
docker volume create ejer-datos
docker run --rm -v ejer-datos:/data alpine:latest \
  sh -c 'echo hola > /data/nota.txt'

docker run --rm -v ejer-datos:/data alpine:latest cat /data/nota.txt
# hola
```

## Ejercicio 4

```bash
mkdir -p ~/practica-linux/compose-ejer
cd ~/practica-linux/compose-ejer

cat > compose.yaml << 'EOF'
services:
  web:
    image: nginx:alpine
    ports:
      - "8080:80"
EOF

docker compose up -d
curl -I http://127.0.0.1:8080
docker compose down
```

## Ejercicio 5 (opcional)

```bash
docker context ls
# default / desktop-linux  (* = activo)
```
