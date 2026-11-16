# ssh

Abre una sesión segura en **otro equipo** (*Secure Shell*). Es la forma habitual de administrar servidores Linux a distancia.

## Sintaxis

```bash
ssh [opciones] <usuario>@<destino>
ssh [opciones] <destino>
```

> Recuerda: `[ ]` = opcional · `<usuario>` y `<destino>` (IP o hostname) los pones tú.  
> Detalle: [Cómo leer la sintaxis](../../README.md#cómo-leer-la-sintaxis-muy-importante).

Si no pones `<usuario>`, usa tu usuario local actual.

## Ejemplos

```bash
ssh <usuario>@<destino>
ssh ana@192.168.1.20
ssh ana@servidor.local
```

Primer uso: te preguntará si confías en la huella del servidor (`yes/no`). Es normal.

```bash
ssh <usuario>@<destino>
# The authenticity of host '...' can't be established.
# ... Are you sure you want to continue connecting (yes/no)?
# escribes: yes
```

Puerto distinto del 22:

```bash
ssh -p <puerto> <usuario>@<destino>
ssh -p 2222 ana@192.168.1.20
```

Ejecutar un solo comando remoto (sin quedar dentro):

```bash
ssh <usuario>@<destino> "<comando>"
ssh ana@192.168.1.20 "hostname; uptime"
ssh ana@192.168.1.20 "ls <directorio>"
```

Con clave (cuando ya tengas pares de claves; intro):

```bash
ssh -i <fichero_clave_privada> <usuario>@<destino>
ssh -i ~/.ssh/id_rsa ana@192.168.1.20
```

Salir de la sesión remota:

```bash
exit
# o Ctrl+D
```

## Generar claves (recomendado frente a solo contraseña)

```bash
ssh-keygen -t ed25519 -C "<texto>"
# Enter varias veces si aceptas rutas/passphrase por defecto

ls ~/.ssh
# verás id_ed25519 (privada) e id_ed25519.pub (pública)
```

Copiar tu clave pública al servidor (si tienes `ssh-copy-id`):

```bash
ssh-copy-id <usuario>@<destino>
# después podrás entrar sin escribir la contraseña del usuario (usa la passphrase de la clave si la pusiste)
```

## Comprobar que el puerto SSH escucha (en el servidor)

```bash
sudo ss -tlnp | grep 22
```

## Instalación del cliente / servidor

Cliente (para conectar **desde** tu PC):

```bash
# Debian / Ubuntu / Linux Mint
sudo apt install openssh-client

# Fedora
sudo dnf install openssh-clients

# Arch
sudo pacman -S openssh
```

Servidor (para **recibir** conexiones en una máquina):

```bash
# Debian / Ubuntu / Linux Mint
sudo apt install openssh-server
sudo systemctl enable --now ssh

# Fedora
sudo dnf install openssh-server
sudo systemctl enable --now sshd

# Arch
sudo pacman -S openssh
sudo systemctl enable --now sshd
```

## Errores frecuentes

```text
Connection refused
```

- En el destino no hay servidor SSH, está apagado, o el puerto no es 22.

```text
Connection timed out
```

- Firewall, IP incorrecta, o el equipo no es alcanzable (`ping` primero).

```text
Permission denied
```

- Usuario/contraseña/clave incorrectos, o el servidor no acepta ese método de login.

- Escribir `ssh <destino>@<usuario>` al revés: el orden es `<usuario>@<destino>`.

## Siguiente

[scp](scp.md)

## Relacionados

- [scp](scp.md) — copiar ficheros con el mismo canal
- [ss](ss.md) — ver el puerto 22
- [exit-logout](../../02-sistema/05-usuarios-sesiones/exit-logout.md) — salir de la sesión



