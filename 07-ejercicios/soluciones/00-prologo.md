# Solución — Prólogo

## Ejercicio 1

```bash
whoami
# <usuario>

echo "$HOME"
# /home/<usuario>

cat /etc/os-release
# NAME=…  ID=ubuntu|linuxmint|debian|fedora|arch …
```

## Ejercicio 2 (idea)

| Ruta | Idea |
|------|------|
| `/` | Raíz de todo el árbol |
| `/home` | Carpetas de usuarios |
| `/etc` | Configuración del sistema |
| `/var/log` | Registros / logs |
| `/tmp` | Temporal (se puede vaciar) |

## Ejercicio 3 (idea)

1. Cualquiera podría romper la configuración del sistema.  
2. Usuario normal: tareas del día a día. Root/sudo: cambios de sistema (peligroso si se usa mal).  
