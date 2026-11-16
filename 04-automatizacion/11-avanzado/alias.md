# alias

Un **alias** es un apodo para un comando (o una frase de comando) en tu shell.

## Sintaxis

```bash
alias
alias <nombre>='<comando>'
unalias <nombre>
```

> Sin espacios alrededor del `=`.  
> Detalle: [Cómo leer la sintaxis](../../README.md#cómo-leer-la-sintaxis-muy-importante).

## Ejemplos

```bash
alias ll='ls -la'
alias la='ls -A'
alias ..='cd ..'
alias ...='cd ../..'
alias grep='grep --color=auto'
# al definirlos: no suelen imprimir nada; solo “quedan listos”
```

En Linux Mint (apt + Flatpak):

```bash
alias update-mint='sudo apt update && sudo apt upgrade && flatpak update'
alias update-apt='sudo apt update && sudo apt upgrade'
alias fps='flatpak search'
# igual: silencio al crearlos; el efecto se ve al usarlos
```

Usarlos:

```bash
ll
# equivale a ls -la
# en pantalla: listado largo (permisos, dueño, tamaño, nombre…)

ll <directorio>
# mismo estilo de listado, pero de ese directorio

update-mint
# ¡lanza actualizaciones de verdad! úsalo solo cuando quieras actualizar
# verás salida de apt update, apt upgrade y flatpak update
```

Ver aliases definidos:

```bash
alias
# lista todos los de la sesión, por ejemplo:
# alias ll='ls -la'
# alias grep='grep --color=auto'

alias ll
# alias ll='ls -la'

type ll
# ll is aliased to `ls -la'
```

Quitar (sesión actual):

```bash
unalias ll
# si luego escribes ll: bash: ll: command not found  (ya no es alias)

unalias -a
# quita todos los aliases de la sesión (radical)
```

## Hacerlos permanentes

```bash
nano ~/.bashrc
# se abre el editor; al guardar y salir, el fichero ya tiene los cambios
```

Al final del fichero, por ejemplo:

```bash
alias ll='ls -la'
alias update-mint='sudo apt update && sudo apt upgrade && flatpak update'
```

Recargar:

```bash
source ~/.bashrc
# o: . ~/.bashrc
# no imprime nada si va bien; a partir de ahora ll (etc.) ya funciona en esta terminal
```

## Evitar el alias temporalmente

```bash
\ls
command ls
/usr/bin/ls
# ejecutan ls real, sin alias
# salida: listado “normal” de ls (aunque tengas alias ls='ls --color=auto' u otro)
```

## Alias vs función (cuando el alias se queda corto)

```bash
# en ~/.bashrc — función: admite lógica
mkcd() {
  mkdir -p "$1" && cd "$1"
}
```

```bash
mkcd <directorio>
# crea y entra
# pwd → …/<directorio>  (ya estás dentro; sin mensaje extra si todo OK)
```

## Alias seguros vs peligrosos

| Idea | Ejemplo | Nota |
|------|---------|------|
| Acortar listados | `alias ll='ls -la'` | Seguro y útil |
| Colorear | `alias grep='grep --color=auto'` | Habitual |
| Borrar “suave” | `alias rm='rm -i'` | Puede salvarte; en scripts a veces estorba |
| Ocultar destrucciones | `alias rm='rm -rf'` | **Mala idea** |

## Errores frecuentes

- Espacios mal: `alias ll = 'ls -la'` falla; debe ser `alias ll='ls -la'`.  
- Poner un alias con el mismo nombre que un comando crítico y liarte.  
- Esperar que un alias funcione dentro de un script: por defecto los scripts no cargan tus aliases.  
- Crear `update-mint` y lanzarlo sin querer en una demo: actualiza el sistema de verdad.

## Relacionados

- [source-y-permisos](../09-scripts/source-y-permisos.md) — `source ~/.bashrc`  
- [08-paquetes](../../03-red-paquetes/08-paquetes/) — apt + flatpak  
