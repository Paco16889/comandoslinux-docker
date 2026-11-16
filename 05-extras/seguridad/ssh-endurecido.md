# SSH endurecido

Buenas prácticas para dejar **SSH más seguro**: claves en lugar de (o además de) contraseña, desactivar login root, y hábitos que evitan sustos.  
Parte de [ssh](../../03-red-paquetes/07-red/ssh.md) y lo complementa con [ufw](ufw.md) y [fail2ban](../servidores/fail2ban.md).

> **Cómo leer la sintaxis:** `<algo>` = lo sustituyes tú · `[algo]` = opcional.  
> Detalle: [Cómo leer la sintaxis](../../README.md#cómo-leer-la-sintaxis-muy-importante).

## Ideas previas

| Concepto | En pocas palabras |
|----------|-------------------|
| Clave SSH | Par de ficheros: privada (tuya, secreta) + pública (en el servidor) |
| `authorized_keys` | Lista de claves públicas que pueden entrar a esa cuenta |
| `sshd_config` | Configuración del **servidor** SSH |
| root login | Entrar directamente como `root` por SSH (mejor evitarlo) |

**Regla de oro en un servidor remoto:** no desactives la autenticación por contraseña hasta comprobar que puedes entrar **con clave** en una segunda sesión abierta.

## 1) Generar un par de claves (en tu PC cliente)

```bash
ssh-keygen -t ed25519 -C "<texto>"
# ejemplo: ssh-keygen -t ed25519 -C "portatil-mint-ana"

# Si ed25519 no estuviera disponible (raro hoy):
ssh-keygen -t rsa -b 4096 -C "<texto>"
```

Te pregunta ruta (Enter = `~/.ssh/id_ed25519`) y passphrase (recomendable).

```bash
ls -la ~/.ssh/
# id_ed25519      → PRIVADA (nunca la compartas ni la subas a chats)
# id_ed25519.pub  → pública (esta va al servidor)
```

Permisos correctos:

```bash
chmod 700 ~/.ssh
chmod 600 ~/.ssh/id_ed25519
chmod 644 ~/.ssh/id_ed25519.pub
```

## 2) Copiar la clave pública al servidor

```bash
ssh-copy-id <usuario>@<destino>
ssh-copy-id -i ~/.ssh/id_ed25519.pub <usuario>@<destino>
```

A mano (si no hay `ssh-copy-id`):

```bash
cat ~/.ssh/id_ed25519.pub | ssh <usuario>@<destino> "mkdir -p ~/.ssh && chmod 700 ~/.ssh && cat >> ~/.ssh/authorized_keys && chmod 600 ~/.ssh/authorized_keys"
```

Probar:

```bash
ssh <usuario>@<destino>
# debería pedir la passphrase de la clave (si la pusiste), no la contraseña del usuario
```

## 3) Endurecer `sshd_config` (en el servidor)

```bash
sudo cp /etc/ssh/sshd_config /etc/ssh/sshd_config.bak
sudo nano /etc/ssh/sshd_config
```

Directivas habituales (descomenta/ajusta; nombres exactos pueden variar ligeramente):

```text
PermitRootLogin no
PasswordAuthentication no
PubkeyAuthentication yes
PermitEmptyPasswords no
X11Forwarding no
```

| Directiva | Idea |
|-----------|------|
| `PermitRootLogin no` | Nadie entra como root por SSH; usa un usuario + `sudo` |
| `PasswordAuthentication no` | Solo claves (cuando ya te funciona la clave) |
| `PubkeyAuthentication yes` | Permite login con clave |
| `PermitEmptyPasswords no` | Sin claves vacías |

Puerto distinto (opcional; reduce ruido de bots, no es seguridad mágica):

```text
Port <puerto>
```

Si cambias el puerto: abre ese puerto en [ufw](ufw.md) **antes** de reiniciar SSH y actualiza el cliente: `ssh -p <puerto> ...`.

### Aplicar cambios sin quedarte fuera

1. Deja una sesión SSH abierta.  
2. En otra terminal, prueba login con clave.  
3. Solo entonces:

```bash
sudo sshd -t
# prueba la config (syntax)

sudo systemctl reload ssh
# Debian/Ubuntu/Mint (servicio a veces "ssh")

sudo systemctl reload sshd
# Fedora/Arch (nombre habitual)
```

Si `reload` te echa y no puedes volver: consola física, o recupera el `.bak`.

```bash
sudo cp /etc/ssh/sshd_config.bak /etc/ssh/sshd_config
sudo systemctl reload ssh
```

## 4) Usuario con sudo (en lugar de root por SSH)

```bash
# en el servidor, como admin:
sudo adduser <usuario>
sudo usermod -aG sudo <usuario>
# Fedora/RHEL a menudo: wheel
```

Entras como `<usuario>` y elevas con `sudo` cuando haga falta.

## 5) Habitos del cliente

```bash
# ~/.ssh/config  (en tu PC)
Host <alias>
    HostName <IP_o_dominio>
    User <usuario>
    IdentityFile ~/.ssh/id_ed25519
    Port 22
```

```bash
ssh <alias>
# más cómodo y menos errores
```

Agente (para no teclear la passphrase cada vez en la sesión):

```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
```

## 6) Capas extra recomendadas

```bash
# Firewall
sudo ufw allow OpenSSH
sudo ufw enable

# Fuerza bruta
sudo apt install fail2ban
# activa jail sshd — ver fail2ban.md
```

## Mini checklist

| Paso | ¿Hecho? |
|------|---------|
| Clave ed25519 creada | |
| `ssh-copy-id` y login con clave OK | |
| Segunda sesión abierta al tocar `sshd_config` | |
| `PermitRootLogin no` | |
| `PasswordAuthentication no` (solo tras probar clave) | |
| `sshd -t` + `reload` | |
| ufw permite SSH | |
| fail2ban (opcional pero recomendable) | |

## Errores frecuentes

```text
Permission denied (publickey)
```

- La pública no está en `~/.ssh/authorized_keys` del usuario correcto, o permisos de `.ssh` demasiado abiertos (`chmod 700/600`).

```text
WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!
```

- La huella del servidor cambió (reinstalación) o hay un MITM. Verifica fuera de banda; luego `ssh-keygen -R <destino>` si confías en el cambio.

- Desactivar contraseñas **antes** de instalar la clave → lockout.  
- Guardar la clave **privada** en el servidor o en un chat.  
- Usar `PermitRootLogin yes` + contraseña débil en un VPS expuesto: los bots lo escanean en minutos.

## Relacionados

- [ssh](../../03-red-paquetes/07-red/ssh.md) · [scp](../../03-red-paquetes/07-red/scp.md)  
- [ufw](ufw.md) · [fail2ban](../servidores/fail2ban.md)  
- [sudo / usuarios](../../02-sistema/05-usuarios-sesiones/)  
