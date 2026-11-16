# less

Visor de ficheros con scroll: adelante, atrás y búsqueda. Ideal para textos largos.

## Sintaxis

```bash
less [opciones] <fichero>
```

También puedes recibir datos por tubería:

```bash
comando | less
```

## Ejemplos

```bash
less <fichero>
# no “imprime y listo”: ocupa la pantalla con el contenido
# abajo suele verse el nombre del fichero (o :)
# para salir: q  → recuperas el prompt normal
```

Ejemplo de aspecto (idea; el texto es el de tu fichero):

```text
primera línea del fichero…
…
<fichero>
```

```bash
less /var/log/<fichero>
# típico con logs del sistema (si tienes permiso)
# si no puedes leerlo: Permission denied y vuelves al prompt

less <directorio>/<fichero>
```

Desde otro comando:

```bash
ls -la <directorio> | less
# mismo visor, pero con la salida de ls (útil si es muy larga)
# al salir con q no se “guarda” nada: solo dejabas de mirar

man <comando>
# el manual suele usar less por debajo (mismas teclas: /, n, q…)
```

## Teclas dentro de less

| Tecla | Acción |
|-------|--------|
| `Espacio` / `f` / `AvPág` | Página siguiente |
| `b` / `RePág` | Página anterior |
| `↓` / `Enter` | Una línea abajo |
| `↑` | Una línea arriba |
| `g` | Ir al inicio |
| `G` | Ir al final |
| `/<texto>` | Buscar hacia delante |
| `?<texto>` | Buscar hacia atrás |
| `n` | Siguiente coincidencia |
| `N` | Coincidencia anterior |
| `h` | Ayuda |
| `q` | Salir |

Ejemplo de búsqueda (dentro de `less`):

```text
/<patrón>          ← escribes / y el texto; Enter
… líneas con el hallazgo resaltado (si hay) …
Pattern not found  ← si no hay coincidencias
n                  ← siguiente
N                  ← anterior
q                  ← salir
```

## Opciones útiles

| Opción | Qué hace |
|--------|----------|
| `-N` | Numera líneas |
| `-S` | No parte líneas largas (scroll horizontal) |
| `-i` | Búsqueda sin distinguir mayúsculas |
| `+F` | Modo “follow” (parecido a `tail -f`; salir del follow con `Ctrl+C` y luego `q`) |

```bash
less -N <fichero>
# cada línea empieza con su número:  1  texto…  /  2  texto…

less -i <fichero>
# luego /<patrón> ignora mayúsculas/minúsculas al buscar
```

## Errores frecuentes

- Quedarse “atrapado”: pulsa `q`.
- Confundir con `more`: `less` permite ir atrás; `more` es más limitado.
- Buscar `/<texto>` y no encontrar nada: prueba `-i` o revisa mayúsculas/acentos.

## Siguiente

[head](head.md)

## Relacionados

- [cat](cat.md) — volcar todo de golpe
- [head](head.md) / [tail](tail.md) — un fragmento sin visor
- [grep](grep.md) — filtrar antes de leer



