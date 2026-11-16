# scp

Copia ficheros y directorios entre máquinas a través de SSH (*secure copy*).

Si sabes usar `ssh` y `cp`, `scp` se entiende rápido: “un `cp` por la red, cifrado”.

## Sintaxis

```bash
scp [opciones] <origen> <destino>
```

> Recuerda: `[ ]` = opcional. Origen/destino pueden ser locales o `usuario@host:ruta`.  
> Detalle: [Cómo leer la sintaxis](../../README.md#cómo-leer-la-sintaxis-muy-importante).

Forma remota típica:

```text
<usuario>@<destino>:<ruta>
```

## Ejemplos — subir (local → remoto)

```bash
scp <fichero> <usuario>@<destino>:<directorio>/
scp notas.txt ana@192.168.1.20:/home/ana/Documentos/
# primero puede pedir la contraseña de ana
# luego una barra de progreso, por ejemplo:
# notas.txt                    100%  12KB   1.2MB/s   00:00
```

Con otro nombre en destino:

```bash
scp <fichero> <usuario>@<destino>:<directorio>/<fichero_nuevo>
# misma barra 100%; en el remoto el fichero se llama <fichero_nuevo>
```

## Ejemplos — bajar (remoto → local)

```bash
scp <usuario>@<destino>:<ruta_fichero> <directorio_local>/
scp ana@192.168.1.20:/home/ana/informe.pdf .
# el punto = directorio actual
# progreso similar; luego ls . → aparece informe.pdf
```

## Directorios enteros

```bash
scp -r <directorio> <usuario>@<destino>:<directorio_destino>/
# una línea de progreso por fichero (o resumen según versión)

scp -r <usuario>@<destino>:<directorio> ./
# baja la carpeta entera al directorio actual
# ls → ves <directorio>/ con su contenido
```

## Puerto SSH distinto de 22

Ojo: en `scp` la opción de puerto es `-P` (**mayúscula**). En `ssh` es `-p` (minúscula).

```bash
scp -P <puerto> <fichero> <usuario>@<destino>:<directorio>/
scp -P 2222 notas.txt ana@192.168.1.20:/home/ana/
# si el puerto es correcto: misma barra 100% que un scp normal
# si no: Connection refused / timed out (como en ssh)
```

## Más opciones útiles

```bash
scp -v <fichero> <usuario>@<destino>:<directorio>/
# verbose: útil si falla y no sabes por qué
# verás muchas líneas de debug (Authenticating, Sending file…)

scp -C <fichero> <usuario>@<destino>:<directorio>/
# comprime en tránsito (a veces más rápido en textos grandes)
# al final: la misma idea de 100% al completar

scp -i <fichero_clave_privada> <fichero> <usuario>@<destino>:<directorio>/
# usa esa clave en vez de (o además de) contraseña
```

## Ejemplo de sesión mental

```bash
# 1) ¿llego al servidor?
ping -c 2 192.168.1.20
# 2 packets transmitted, 2 received, 0% packet loss …

# 2) ¿entro por SSH?
ssh ana@192.168.1.20
# prompt remoto: ana@servidor:~$

# 3) copio un fichero
exit
# vuelves a tu máquina local
scp ./backup.tar.gz ana@192.168.1.20:/home/ana/
# backup.tar.gz                 100% … 00:0x
```

## Alternativas que verás más adelante

| Herramienta | Nota |
|-------------|------|
| `scp` | Simple y ubicuo |
| `rsync` | Mejor para sincronizar/reanudar (avanzado) |
| `sftp` | Sesión interactiva de transferencia |

## Errores frecuentes

- Olvidar `:` en la parte remota: `usuario@host/ruta` está mal → debe ser `usuario@host:ruta`.
- Confundir `-p` y `-P` para el puerto.
- Sin `-r` al copiar un directorio: fallará o no hará lo esperado.
- Permisos en el destino: el `<usuario>` remoto debe poder escribir en esa carpeta.

## Siguiente

[dig y nslookup](dig-nslookup.md)

## Relacionados

- [ssh](ssh.md) — misma autenticación y red
- [cp](../../01-fundamentos/02-ficheros-directorios/cp.md) — copia local
- [wget](wget.md) / [curl](curl.md) — bajar desde HTTP, no desde tu servidor SSH



