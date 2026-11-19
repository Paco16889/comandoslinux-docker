# Solución — Sistema

## Ejercicio 1

```bash
whoami
id
who
# o: w
```

## Ejercicio 2

```bash
ps aux | head
ps aux | grep -i bash
# anota un PID de la columna correspondiente
```

## Ejercicio 3

```bash
sleep 300 &
# [1] 12345   ← ejemplo de PID

ps aux | grep '[s]leep'
kill 12345
# sustituye 12345 por el tuyo

ps aux | grep '[s]leep'
# vacío (o solo el grep, según filtro)
```

## Ejercicio 4

```bash
sudo whoami
# root
```

Usuario de prueba (solo en tu lab):

```bash
# sudo useradd -m prueba_ejer
# sudo passwd prueba_ejer
# … pruebas …
# sudo userdel -r prueba_ejer
```
