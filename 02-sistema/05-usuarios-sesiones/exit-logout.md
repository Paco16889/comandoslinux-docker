# exit y logout

Cierran la shell actual o la sesión de login.

## exit

Sale de la shell actual. Si esa shell era la de login/SSH, cierra la sesión.

### Sintaxis

```bash
exit [<código>]
```

### Ejemplos

```bash
exit
# cierra la shell actual

exit 0
# sale con código de éxito

exit 1
# sale con código de error (útil en scripts)
```

Tras `su` / `sudo -i`:

```bash
su - <usuario>
# ...
exit
# vuelves al usuario anterior

sudo -i
# ...
exit
# vuelves a tu usuario
```

Atajo de teclado en muchas terminales:

```text
Ctrl + D
# envía EOF; suele equivaler a salir de la shell si la línea está vacía
```

## logout

Pensado para **cerrar la sesión de login**. En Bash suele ser un sinónimo práctico de `exit` en shells de login.

### Sintaxis

```bash
logout
```

### Ejemplos

```bash
logout
# cierra la sesión de login
```

Si no estás en una login shell:

```text
bash: logout: not login shell: use `exit'
```

- Usa `exit` en su lugar.

## Casos típicos

| Situación | Comando |
|-----------|---------|
| Cerrar terminal / SSH | `exit` o `Ctrl+D` |
| Volver tras `su` / `sudo -i` | `exit` |
| Sesión de login explícita | `logout` o `exit` |
| Forzar fin de sesión remota de otro (admin) | más relacionado con procesos: `pkill -u <usuario>` (cuidado; ver `06-procesos`) |

Comprobar que cerraste:

```bash
who
# ya no deberías aparecer (desde otra sesión)

last -n 5 <usuario>
# verás la hora de logout cuando se registre
```

## Errores frecuentes

- Hacer `exit` demasiadas veces en shells anidadas (`su`, `bash` dentro de `bash`): sales de una capa cada vez.
- Cerrar la terminal con la X: suele matar la sesión, pero no siempre es equivalente a un logout limpio en todos los entornos.
- En scripts, `exit` termina el script (o la shell que lo interpreta); no “cierra Linux”.

## Siguiente

[ps](../06-procesos/ps.md)

## Relacionados

- [su](su.md) / [sudo](sudo.md) — shells anidadas de las que sales con `exit`
- [who-w-users](who-w-users.md) — ver sesiones activas
- [last](last.md) — historial de cierres/logins



