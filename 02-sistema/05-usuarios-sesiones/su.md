# su

Cambia a otro usuario (o a root) en la terminal actual (*substitute user*).

## Sintaxis

```bash
su [opciones] [<usuario>]
su [opciones] [<usuario>] -c "<comando>"
```

Sin `<usuario>`, el destino suele ser **root**.

## Diferencia clave: `su` vs `su -`

| Comando | Qué hace |
|---------|----------|
| `su <usuario>` | Cambia de usuario; a menudo **conserva** parte del entorno (PATH, directorio actual…) |
| `su - <usuario>` | Simula **login completo**: home, PATH y entorno del usuario destino |
| `su -` | Login completo como root |

Recomendación habitual: usa `su - <usuario>` cuando quieras “ser” ese usuario de verdad.

## Ejemplos

```bash
su -
# pasas a root (pide contraseña de root, según configuración)
# sales con: exit

su - <usuario>
# login como <usuario>

su <usuario>
# cambio “ligero” a <usuario>
```

Ejecutar un solo comando:

```bash
su - <usuario> -c "whoami"
# <usuario>

su - <usuario> -c "ls /home/<usuario>/<directorio>"
su - <usuario> -c "touch /home/<usuario>/<fichero>"
```

Comprobar entorno:

```bash
whoami
pwd
echo $HOME

su - <usuario>
whoami
pwd
echo $HOME
# deberían reflejar a <usuario>

exit
```

## su y sudo

En Ubuntu y muchas distros de escritorio, root no tiene contraseña de login y se usa `sudo` en su lugar:

```bash
sudo -i
# shell de root vía sudo (contraseña de TU usuario)

sudo su -
# también habitual; equivalente práctico a root login shell
```

| Objetivo | Suele usarse |
|----------|----------------|
| Un comando como root | `sudo <comando>` |
| Shell de root | `sudo -i` |
| Ser otro usuario con su contraseña | `su - <usuario>` |
| Ser otro usuario sin su contraseña (si tienes sudo) | `sudo -u <usuario> -i` |

## Errores frecuentes

```text
su: Authentication failure
```

- Contraseña incorrecta, o root deshabilitado para `su` y debes usar `sudo`.

- Usar `su root` sin `-` y encontrar comandos “no found”: PATH incompleto → prueba `su -`.

## Siguiente

[sudo](sudo.md)

## Relacionados

- [sudo](sudo.md) — privilegios sin contraseña de root (configuración típica)
- [whoami](whoami.md) — verificar quién eres tras el cambio
- [exit-logout](exit-logout.md) — volver a la sesión anterior



