# clear

Limpia la pantalla de la terminal.

## Sintaxis

```bash
clear
```

> No lleva argumentos habituales.  
> Detalle de la guía: [Cómo leer la sintaxis](../../README.md#cómo-leer-la-sintaxis-muy-importante).

## Ejemplos

```bash
ls -la
# ... mucha salida ...

clear
# pantalla limpia; el historial de comandos sigue existiendo

pwd
# /home/<usuario>/...
# sigues en el mismo directorio
```

Atajo habitual en muchas terminales:

```text
Ctrl + L
```

Suele hacer un efecto muy parecido a `clear`.

### Sesión típica del primer día

```bash
cd ~
ls -la
clear
pwd
# ~ o /home/<usuario>

cd <directorio>
ls
clear
# limpias la vista; sigues dentro de <directorio>
pwd
```

### clear no borra el historial

```bash
ls
clear
history | tail -n 5
# siguen apareciendo los comandos anteriores (incluido ls y clear)
```

### Cuando la terminal se “estropea”

A veces un programa imprime caracteres raros y el prompt queda ilegible:

```bash
reset
# reinicializa la terminal (más “fuerte” que clear)
```

```bash
clear
# solo limpia; no siempre arregla un terminal “roto”
```

## Qué no hace

- No borra el historial (`history` sigue igual).  
- No cierra la sesión ni mata procesos.  
- En muchas terminales solo despeja la vista; puedes hacer scroll hacia arriba y ver salida anterior.

## Errores frecuentes

- Pensar que `clear` reinicia la shell o te lleva al home: no; solo limpia la pantalla.  
- Confundir `clear` con borrar ficheros: no borra nada del disco.  
- Usar `reset` por costumbre: es más agresivo; reserva para cuando la terminal está mal.

## Siguiente

[echo](echo.md)

## Relacionados

- [history](history.md) — ver comandos anteriores  
- [pwd](pwd.md) — orientación tras limpiar la pantalla  
- [cd](cd.md) — el directorio no cambia con `clear`



