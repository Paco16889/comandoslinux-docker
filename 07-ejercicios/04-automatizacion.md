# Ejercicios — Automatización

Tras estudiar [04-automatizacion](../04-automatizacion/) (scripts, pipes, avanzado).

> **Cómo leer la sintaxis:** `<algo>` = lo sustituyes tú · `[algo]` = opcional.  
> Detalle: [Cómo leer la sintaxis](../README.md#cómo-leer-la-sintaxis-muy-importante).

## Antes de empezar

```bash
mkdir -p ~/practica-linux/scripts
cd ~/practica-linux/scripts
```

## Ejercicio 1 — Primer script útil

### Objetivo

Shebang, permisos, argumentos.

### Tareas

1. Crea `saluda.sh` que:
   - tenga shebang `bash`  
   - reciba un nombre como `$1`  
   - si no hay argumento, muestre `uso: ./saluda.sh <nombre>` y salga con código ≠ 0  
   - si hay argumento, imprima `Hola, <nombre>`  
2. `chmod +x` y pruébalo con y sin argumento.  
3. Comprueba `echo $?` tras cada caso.

### Comprueba

Sin args → mensaje de uso y `$?` distinto de 0. Con args → saludo y `$?` 0.

## Ejercicio 2 — Pipes y redirección

### Objetivo

Encadenar comandos y guardar salida.

### Tareas

1. Lista `/etc` y cuenta **cuántas** entradas hay (pipe a `wc`).  
2. Guarda en `~/practica-linux/etc-listado.txt` el listado de `/etc` (solo nombres).  
3. Usa `tee` para mostrar por pantalla **y** guardar las primeras 10 líneas de ese listado en `etc-top10.txt`.

### Comprueba

Existen los dos ficheros y `etc-top10.txt` tiene ≤ 10 líneas.

## Ejercicio 3 — Buscar en logs / texto

### Objetivo

`grep`, `||` / `&&` básicos.

### Tareas

1. Busca la palabra `error` (sin importar mayúsculas) dentro de un fichero de texto tuyo o de `/var/log/syslog` / `journalctl` **si tienes permiso** (si no, crea un `demo.log` con 3 líneas y una con ERROR).  
2. Encadena: si `grep` encuentra algo, imprime `hay coincidencias` con `&&`.

### Comprueba

Ves el mensaje solo cuando hay match.

## Ejercicio 4 — systemctl (lectura)

### Objetivo

Consultar un servicio sin romper nada.

### Tareas

1. `systemctl status` de un servicio que exista en tu máquina (p. ej. `ssh`, `sshd`, `NetworkManager`, `docker`… el que tengas).  
2. Di si está `active` o no.  
3. **No** hagas `stop`/`disable` en servicios críticos de un PC ajeno.

### Comprueba

Sabes leer la línea `Active:` del status.

## Siguiente

[Ejercicios — Extras](05-extras.md)

## Relacionados

- [04-automatizacion](../04-automatizacion/)  
- Solución: [soluciones/04-automatizacion](soluciones/04-automatizacion.md)  
- [README ejercicios](README.md)  
