# Solución — Automatización

## Ejercicio 1

```bash
cat > ~/practica-linux/scripts/saluda.sh << 'EOF'
#!/usr/bin/env bash
if [[ -z "$1" ]]; then
  echo "uso: ./saluda.sh <nombre>"
  exit 1
fi
echo "Hola, $1"
EOF

chmod +x ~/practica-linux/scripts/saluda.sh
cd ~/practica-linux/scripts

./saluda.sh
# uso: …
echo $?
# 1

./saluda.sh Ana
# Hola, Ana
echo $?
# 0
```

## Ejercicio 2

```bash
ls /etc | wc -l
ls /etc > ~/practica-linux/etc-listado.txt
ls /etc | head | tee ~/practica-linux/etc-top10.txt
wc -l ~/practica-linux/etc-top10.txt
# 10 (o menos)
```

## Ejercicio 3

```bash
printf 'ok\nERROR disco\nok\n' > ~/practica-linux/demo.log
grep -i error ~/practica-linux/demo.log && echo "hay coincidencias"
# ERROR disco
# hay coincidencias
```

## Ejercicio 4

```bash
systemctl status NetworkManager
# o: ssh / sshd / docker …
# Active: active (running)   ← o inactive
```
