# Cómo usar esta guía

Esta carpeta (`00-prologo`) es el punto de partida. El resto está numerado a propósito.

## Orden global

```text
00-prologo          ← estás aquí (conceptos)
01-fundamentos      ← terminal, ficheros, texto, permisos
02-sistema          ← usuarios y procesos
03-red-paquetes     ← red + apt/dnf/pacman/flatpak
04-automatizacion   ← scripts, pipes, avanzado
05-extras           ← sensors, servidores, seguridad, red avanzada
```

Sigue los números. Dentro de cada carpeta hay un `README` con el orden de los comandos.

## Cómo está escrito cada comando

Suele incluir:

1. Qué hace  
2. Sintaxis (molde)  
3. Ejemplos (muchos)  
4. Opciones útiles  
5. Errores frecuentes  
6. Relacionados  

Los símbolos `<nombre>` y `[opcional]` **no se teclean**:  
[Cómo leer la sintaxis](../README.md#cómo-leer-la-sintaxis-muy-importante).

## Consejos para novatos

1. **Escribe** los comandos; no solo leas.  
2. Usa una carpeta de pruebas en tu home (`mkdir ~/curso-linux`).  
3. Si un ejemplo lleva `sudo`, léelo dos veces.  
4. En Mint: herramientas de sistema con `apt`; apps grandes a menudo con Flatpak.  
5. Los **extras** son opcionales hasta que te hagan falta (FTP, nginx, ufw…).  

## Práctica mínima al salir del prólogo

```bash
pwd
ls /
ls "$HOME"
cd ~
mkdir -p "$HOME/curso-linux"
cd "$HOME/curso-linux"
echo "hola Linux" > notas.txt
cat notas.txt
```

Si eso te sale, pasa a [01-basicos](../01-fundamentos/01-basicos/).

## Relacionados

- [README principal](../README.md)  
- [01-fundamentos](../01-fundamentos/)  
- [05-extras](../05-extras/)  
