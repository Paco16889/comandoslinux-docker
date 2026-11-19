# Solución — Fundamentos

## Ejercicio 1

```bash
mkdir -p ~/practica-linux/proyecto/docs ~/practica-linux/proyecto/datos
echo "hola fundamentos" > ~/practica-linux/proyecto/docs/notas.txt
# o: touch … y editar

find ~/practica-linux/proyecto -print
cat ~/practica-linux/proyecto/docs/notas.txt
# hola fundamentos
```

## Ejercicio 2

```bash
cp ~/practica-linux/proyecto/docs/notas.txt ~/practica-linux/proyecto/datos/notas-copia.txt
mv ~/practica-linux/proyecto/datos/notas-copia.txt ~/practica-linux/proyecto/datos/backup.txt
echo x > ~/practica-linux/proyecto/datos/borrar-me.txt
rm ~/practica-linux/proyecto/datos/borrar-me.txt
ls ~/practica-linux/proyecto/datos
# backup.txt
```

## Ejercicio 3

```bash
cat > ~/practica-linux/proyecto/datos/lista.txt << 'EOF'
manzana
pera
linux mint
plátano
naranja
EOF

grep -i linux ~/practica-linux/proyecto/datos/lista.txt
# linux mint

wc -l ~/practica-linux/proyecto/datos/lista.txt
# 5 …
```

## Ejercicio 4

```bash
mkdir -p ~/practica-linux/proyecto/scripts
cat > ~/practica-linux/proyecto/scripts/hola.sh << 'EOF'
#!/usr/bin/env bash
echo "hola"
EOF

chmod u+x ~/practica-linux/proyecto/scripts/hola.sh
~/practica-linux/proyecto/scripts/hola.sh
# hola
```
