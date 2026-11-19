# Ejercicios — Extras

Tras ojear [05-extras](../05-extras/).  
Aquí las actividades son **suaves**: no montamos un servidor de producción; practicamos la idea.

> **Cómo leer la sintaxis:** `<algo>` = lo sustituyes tú · `[algo]` = opcional.  
> Detalle: [Cómo leer la sintaxis](../README.md#cómo-leer-la-sintaxis-muy-importante).

## Antes de empezar

Instala solo lo que quieras probar. En Mint/Ubuntu/Debian suele bastar `apt`; mira el temario de cada herramienta.

## Ejercicio 1 — Disco: ¿qué pesa?

### Objetivo

`du` / idea de `ncdu`.

### Tareas

1. Mide el tamaño de `~/practica-linux` con `du -sh`.  
2. (Opcional) Si tienes `ncdu`, explora esa carpeta y sal con la tecla que indique el programa.

### Comprueba

Obtienes un tamaño legible (`4.0K`, `1.2M`…).

## Ejercicio 2 — Firewall: solo lectura

### Objetivo

Entender UFW sin cerrarte fuera.

### Tareas

1. Si tienes UFW: `sudo ufw status` (¿active/inactive?).  
2. **No** actives UFW en remoto sin permitir SSH antes.  
3. Escribe en un bloc de notas: “Antes de enable, debo permitir el puerto ___”.

### Comprueba

Tu nota menciona el puerto SSH (22 o el que uses).

### Pista

[ufw](../05-extras/seguridad/ufw.md).

## Ejercicio 3 — SSH endurecido (papel)

### Objetivo

Repasar ideas de [ssh-endurecido](../05-extras/seguridad/ssh-endurecido.md) **sin** dejarte fuera.

### Tareas

Lista 3 medidas típicas (ej.: clave en vez de password, desactivar root login, cambiar puerto con cuidado…).  
Marca cuáles **no** tocarías en tu primera sesión remota.

### Comprueba

Al menos 3 medidas y una precaución (“probar otra sesión antes de cerrar esta”).

## Ejercicio 4 — (Opcional) Un servicio local

### Objetivo

Si quieres profundizar: elige **uno** entre nginx (página estática) o vsftpd **en lab**, siguiendo el temario de extras.

### Tareas

1. Instala, arranca, comprueba con `curl` o un cliente.  
2. Para el servicio al terminar si no lo necesitas.

### Comprueba

Puedes explicar “cómo lo paré” (`systemctl stop`…).

## Siguiente

[Ejercicios — Docker](06-docker.md)

## Relacionados

- [05-extras](../05-extras/)  
- Solución: [soluciones/05-extras](soluciones/05-extras.md)  
- [README ejercicios](README.md)  
