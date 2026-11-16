# history

Muestra el historial de comandos ejecutados en la shell.

## Sintaxis

```bash
history [<número>]
```

## Ejemplos

```bash
history
# lista numerada de comandos recientes

history <número>
# solo las últimas <número> entradas

history 20
# las últimas 20
```

Reutilizar comandos:

```bash
!<número>
# vuelve a ejecutar esa entrada (ej. !42)

!!
# vuelve a ejecutar el último comando

!<comando>
# último comando que empezaba por ese texto (ej. !ls)

!?<patrón>?
# último comando que contenía <patrón>
```

Búsqueda interactiva en la terminal:

```text
Ctrl + R
# escribes parte del comando y vas hacia atrás en el historial
```

Gestionar el historial:

```bash
history -c
# vacía el historial de la sesión actual

history -d <número>
# borra la entrada <número>

history -d 42
# borra la entrada 42
```

## Variables relacionadas (Bash)

| Variable | Qué controla |
|----------|--------------|
| `HISTFILE` | Fichero donde se guarda (suele ser `~/.bash_history`) |
| `HISTSIZE` | Cuántos comandos guarda en memoria |
| `HISTFILESIZE` | Cuántos guarda en el fichero |

## Errores frecuentes

- Creer que `clear` borra el historial: no; solo limpia la pantalla.
- Usar `!!` tras un comando peligroso (p. ej. `rm`): revisa antes de repetir.
- El historial puede no guardarse si cierras la terminal de forma brusca, según configuración.

## Relacionados

- [help](help.md) — `help history` para más detalle
- [clear](clear.md) — limpiar pantalla sin tocar el historial
