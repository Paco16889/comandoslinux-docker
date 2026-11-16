# 06 — Procesos

Un **proceso** es un programa en ejecución. Aquí aprendes a verlos, pausarlos, reanudarlos y terminarlos.

> **Cómo leer la sintaxis:** `<algo>` = lo sustituyes tú (no escribas `<` `>`).  
> `[algo]` = opcional (no escribas `[` `]`).  
> Explicación completa: [Cómo leer la sintaxis](../../README.md#cómo-leer-la-sintaxis-muy-importante).

## Ideas previas (léelas una vez)

| Concepto | En pocas palabras |
|----------|-------------------|
| Proceso | Programa que se está ejecutando ahora |
| PID | Número de identidad del proceso (como un DNI) |
| Primer plano | Ocupa la terminal; esperas a que termine |
| Segundo plano | Sigue corriendo y te deja usar la terminal (`&`) |
| Señal | Aviso al proceso (`kill` manda señales; la más conocida termina el proceso) |

## Orden recomendado

1. [ps](ps.md) — listar procesos
2. [top-htop](top-htop.md) — ver procesos en vivo (`top`, `htop`, `nvtop` para GPU)
3. [kill](kill.md) — terminar o señalar un proceso
4. [pkill-killall](pkill-killall.md) — terminar por nombre
5. [jobs](jobs.md) — trabajos de esta terminal
6. [bg-fg](bg-fg.md) — segundo plano / primer plano
7. [nohup](nohup.md) — que siga aunque cierres la terminal
8. [nice-renice](nice-renice.md) — prioridad de CPU
9. [free-uptime](free-uptime.md) — memoria y tiempo encendido (complemento)
