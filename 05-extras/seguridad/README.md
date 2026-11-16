# Seguridad (extras)

Firewall y endurecimiento básico de SSH. Va **después** de saber usar SSH y paquetes.

> **Cómo leer la sintaxis:** [guía](../../README.md#cómo-leer-la-sintaxis-muy-importante).

## Orden recomendado

1. [ufw](ufw.md) — firewall sencillo (Mint/Ubuntu)  
2. [ssh-endurecido](ssh-endurecido.md) — claves, sin root login, checklist  

| Documento | Qué cubre | Estado |
|-----------|-----------|--------|
| [ufw](ufw.md) | Uncomplicated Firewall | Listo |
| [ssh-endurecido](ssh-endurecido.md) | Claves SSH y sshd_config | Listo |

## Flujo típico en un VPS / lab expuesto

```bash
# 1) Entras por SSH con usuario normal
# 2) Copias tu clave (ssh-copy-id) y compruebas
# 3) Permites OpenSSH en ufw y activas el firewall
# 4) Endureces sshd_config (sin cortarte la única sesión)
# 5) Opcional: fail2ban
```

Relacionado: [ssh](../../03-red-paquetes/07-red/ssh.md), [fail2ban](../servidores/fail2ban.md).
