# whoami

Muestra el nombre de usuario efectivo actual.

## Sintaxis

```bash
whoami
```

> Detalle: [Cómo leer la sintaxis](../../README.md#cómo-leer-la-sintaxis-muy-importante).

## Ejemplos

```bash
whoami
# <usuario>

sudo whoami
# root

sudo -u <usuario> whoami
# <usuario>

sudo -u root whoami
# root
```

Comparar con otras formas de ver la identidad:

```bash
whoami
id -un
# suelen coincidir en el caso normal

id
# más completo: uid, gid y grupos

echo "$USER"
# variable de entorno (normalmente el usuario de login)

echo "$EUID"
# 0 = root (Bash); útil en scripts
```

### Tras cambiar de usuario

```bash
whoami
# <usuario>

su - <otro_usuario>
# pides contraseña...

whoami
id
# identidad de <otro_usuario>

exit
whoami
# <usuario>  (vuelves)
```

### Con sudo -i

```bash
whoami
sudo -i
whoami
# root
pwd
# suele ser /root
exit
whoami
```

### Mini checklist de identidad

```bash
whoami
id
who am i
# sesión asociada a tu terminal (who)

hostname
# nombre del equipo (no es el usuario)
```

## Errores frecuentes

- Confundir `whoami` con `who`: `whoami` eres **tú**; `who` lista sesiones del sistema.  
- Confiar solo en `$USER` tras `sudo`/`su`: a veces el entorno no se actualiza igual; `whoami` e `id` son más fiables para el usuario efectivo.  
- Pensar que `hostname` es el usuario: es el nombre de la máquina.

## Relacionados

- [id](../../01-fundamentos/04-permisos/id.md) — UID, GID y grupos  
- [who-w-users](who-w-users.md) — quién hay conectado  
- [su](su.md) / [sudo](sudo.md) — cambiar de identidad  
