# `&&` y `||`

Encadenan comandos según si el anterior **fue bien** (código 0) o **falló**.

## Sintaxis

```bash
<comando1> && <comando2>
<comando1> || <comando2>
```

## `&&` — “y entonces”

Ejecuta el segundo **solo si** el primero terminó bien.

```bash
sudo apt update && sudo apt upgrade
# si update falla, no lanza upgrade
# si va bien, verás el progreso de apt (paquetes, “Done”, etc.)

mkdir -p <directorio> && cd <directorio>
# solo hace cd si mkdir fue bien
# si todo OK: no imprime nada; el prompt ya está dentro de <directorio>

ping -c 1 -W 2 8.8.8.8 >/dev/null && echo "Red OK"
# si hay red: Red OK
# si no hay red: no escribe nada (porque el && no llega al echo)
```

## `||` — “si no”

Ejecuta el segundo **solo si** el primero falló.

```bash
ping -c 1 -W 2 8.8.8.8 >/dev/null || echo "Sin red"
# si falla el ping: Sin red
# si el ping va bien: silencio (no hace falta el echo)

ls <fichero> || echo "No existe"
# si <fichero> no está, algo así:
# ls: cannot access '<fichero>': No such file or directory
# No existe
```

## Combinación típica en scripts

```bash
#!/bin/bash
cp <fichero> <destino>/ && echo "Copia OK" || echo "Copia FALLÓ"
# éxito → Copia OK
# fallo (permisos, origen inexistente…) → Copia FALLÓ
```

Ojo: con `&&` y `||` juntos, la prioridad puede confundir. Al empezar, usa `if` si el flujo es complejo.

```bash
# más claro para humanos:
if cp "$1" "$2"; then
  echo "OK"
else
  echo "FALLÓ"
fi
# en pantalla solo verás OK o FALLÓ (más el mensaje de error de cp si falló)
```

## Ejemplos del día a día

```bash
cd <directorio> && ls -la
# si el cd funciona, ves el listado largo de ese directorio
# si el directorio no existe: error de cd y NO hace ls

sudo apt install -y <paquete> && echo "Instalado"
# al final, si apt terminó bien: Instalado

grep -q "<texto>" <fichero> && echo "Encontrado" || echo "No está"
# -q = quiet (solo importa el código de salida)
# aparece UNA de las dos frases: Encontrado  o  No está
```

## Errores frecuentes

- Usar `&&` pensando que es un pipe: no pasa datos; solo decide si ejecutar lo siguiente.  
- `comando1 && comando2 || comando3` sin claridad: cuando dude, usa `if/else`.

## Relacionados

- [codigos-salida](../09-scripts/codigos-salida.md)  
- [condicionales](../09-scripts/condicionales.md)  
- [pipes](pipes.md)  
