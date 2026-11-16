# curl

Transfiere datos desde o hacia una URL. Sirve para **probar webs/APIs**, descargar ficheros y ver cabeceras HTTP.

Muy usado en administración y desarrollo.

## Sintaxis

```bash
curl [opciones] <URL>
```

> Recuerda: `[ ]` = opcional · `<URL>` = dirección completa (ej. `https://example.com`).  
> Detalle: [Cómo leer la sintaxis](../../README.md#cómo-leer-la-sintaxis-muy-importante).

## Ejemplos básicos

```bash
curl <URL>
# descarga el contenido y lo imprime en la terminal

curl https://example.com
curl http://127.0.0.1
```

Guardar en un fichero:

```bash
curl -O <URL>
# guarda con el nombre remoto (O mayúscula)

curl -o <fichero> <URL>
# tú eliges el nombre local

curl -o pagina.html https://example.com
curl -O https://example.com/archivo.zip
```

Ver solo cabeceras (no el cuerpo):

```bash
curl -I <URL>
# como un “¿qué responde el servidor?” sin bajar toda la página
```

Seguir redirecciones:

```bash
curl -L <URL>
# si el sitio redirige (http→https, etc.), sigue el salto
```

Mostrar progreso / silencio:

```bash
curl -# -o <fichero> <URL>
# barra de progreso simple

curl -s <URL>
# silencioso (sin medidor); útil en scripts

curl -sS <URL>
# silencioso pero sí muestra errores
```

## Ejemplos un poco más útiles

Fallos y códigos HTTP:

```bash
curl -o /dev/null -s -w "%{http_code}\n" <URL>
# imprime solo el código (200, 404, 500…)
```

Enviar cabeceras / JSON (intro APIs):

```bash
curl -H "Accept: application/json" <URL>

curl -X POST <URL> \
  -H "Content-Type: application/json" \
  -d '{"nombre":"<texto>"}'
```

Con autenticación básica (solo en entornos de prueba; no dejes passwords en historial):

```bash
curl -u <usuario>:<contraseña> <URL>
# mejor: curl -u <usuario> <URL>  (te pide la clave)
```

Probar si un servicio local responde:

```bash
curl -I http://localhost:<puerto>
curl http://127.0.0.1:8080
```

## curl vs wget (idea)

| Herramienta | Encaje típico |
|-------------|----------------|
| `curl` | APIs, cabeceras, métodos HTTP, scripts |
| `wget` | Descargar ficheros/sitios de forma simple y recursiva |

## Errores frecuentes

```text
curl: (6) Could not resolve host
```

- DNS / nombre mal escrito / sin red.

```text
curl: (7) Failed to connect
```

- El host no acepta la conexión (servicio caído, firewall, puerto mal).

- Olvidar comillas en URLs con `&` o parámetros: usa `"<URL>"`.
- Imprimir un binario enorme en la terminal: mejor `-o <fichero>`.

## Relacionados

- [wget](wget.md) — descargas “clásicas”
- [ping](ping.md) — capa más baja (no es HTTP)
- [dig-nslookup](dig-nslookup.md) — si falla la resolución del nombre
