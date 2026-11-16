# Shebang (`#!`)

La **primera línea** de un script dice con qué programa interpretarlo cuando lo lanzas como `./<fichero>`.

## Forma

```bash
#!/ruta/al/interprete
```

Ejemplos habituales:

```bash
#!/bin/bash
#!/bin/sh
#!/usr/bin/env bash
#!/usr/bin/python3
```

## Ejemplos

Script Bash:

```bash
#!/bin/bash
echo "Soy un script Bash"
```

```bash
chmod +x soy_bash.sh
./soy_bash.sh
# Soy un script Bash
```

Script “posix / sh”:

```bash
#!/bin/sh
echo "Intento ser portable"
```

```bash
chmod +x soy_sh.sh
./soy_sh.sh
# Intento ser portable
```

Forma portable de pedir Bash (busca `bash` en el PATH):

```bash
#!/usr/bin/env bash
echo "Bash encontrado vía env"
```

```bash
chmod +x via_env.sh
./via_env.sh
# Bash encontrado vía env
```

## Reglas prácticas

1. Debe ser la **línea 1** (nada antes, ni espacios ni líneas en blanco).  
2. Los ficheros editados en Windows a veces traen finales `CRLF` y rompen el shebang → mejor guardar como Unix (LF).  
3. El shebang **solo aplica** cuando ejecutas `./fichero` (o desde el menú/cron, etc.).  
   Con `bash fichero` o `sh fichero`, gana el comando que escribiste tú.

```bash
bash soy_bash.sh
# Soy un script Bash   ← corre con bash aunque el shebang diga otra cosa
# (aquí el shebang se ignora porque tú pediste bash)
```

## Comprobar el intérprete

```bash
head -n 1 <fichero>.sh
# #!/bin/bash
# (o el shebang que uses)

ls -l <fichero>.sh
# -rwxr-xr-x 1 <usuario> <usuario> ... <fichero>.sh
# mira si hay x de ejecución: la x en rwx…
```

## Errores frecuentes

```text
./script.sh: /bin/bash^M: bad interpreter
```

- Finales de línea de Windows (`^M`). Convierte a LF (en muchos editores: “LF” / “Unix”).

```text
bad interpreter: No such file or directory
```

- La ruta del shebang no existe (`#!/bin/bash` en un sistema sin bash ahí).

- Poner el shebang en la línea 2: se ignora para `./`.

## Relacionados

- [sh-vs-bash-vs-ejecutable](sh-vs-bash-vs-ejecutable.md)  
- [primer-script](primer-script.md)  
