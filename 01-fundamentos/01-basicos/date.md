# date

Muestra (o, con permisos, cambia) la fecha y hora del sistema.

## Sintaxis

```bash
date [opciones] [+<formato>]
```

Como usuario normal, casi siempre solo la **consultas**.

## Ejemplos

```bash
date
# fecha y hora completas del sistema

date +%Y-%m-%d
# 2026-11-14

date +"%Y-%m-%d %H:%M:%S"
# 2026-11-14 18:40:00

date +%F
# igual que %Y-%m-%d

date +%T
# solo hora HH:MM:SS

date -u
# hora en UTC

date +"Hoy es %A, %d/%m/%Y"
# Hoy es Saturday, 14/11/2026
```

Combinado con otros comandos:

```bash
echo "Copia hecha el $(date +%F)"
# Copia hecha el 2026-11-14

echo "backup-$(date +%Y%m%d).tar.gz"
# backup-20261114.tar.gz  (nombre típico de archivo)
```

## Formatos habituales

| Código | Significado | Ejemplo |
|--------|-------------|---------|
| `%Y` | Año (4 dígitos) | 2026 |
| `%m` | Mes | 11 |
| `%d` | Día | 14 |
| `%H` | Hora (00–23) | 18 |
| `%M` | Minutos | 40 |
| `%S` | Segundos | 00 |
| `%A` | Día de la semana | Saturday |
| `%F` | Equivale a `%Y-%m-%d` | 2026-11-14 |
| `%T` | Equivale a `%H:%M:%S` | 18:40:00 |

## Errores frecuentes

- Olvidar el `+` en el formato: `date +%F`, no `date %F`.
- Cambiar la hora del sistema sin privilegios: hace falta `sudo` (en sistemas modernos suele usarse `timedatectl`).

## Siguiente

[man](man.md)

## Relacionados

- [echo](echo.md) — combinar texto con la fecha
- [history](history.md) — marcas de tiempo si está activado `HISTTIMEFORMAT`



