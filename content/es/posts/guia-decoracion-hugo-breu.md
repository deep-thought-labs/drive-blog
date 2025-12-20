---
title: "Guía de Decoración: Elementos Disponibles en Hugo Breu"
date: 2025-01-26T10:00:00Z
draft: false
tags: ["guía", "hugo-breu", "decoración", "tutorial"]
categories: ["Guías"]
cover: 'https://raw.githubusercontent.com/foxihd/hugo-et-hd/master/static/svg/flowlines/15.svg'
alt: 'Guía de decoración para el tema Hugo Breu'
description: "Una guía completa sobre todos los elementos de decoración disponibles en el tema Hugo Breu para mejorar tus posts y contenido del sitio"
toc: true
math: true
---

{{< figure src="cover" caption="alt" >}}

Esta guía identifica todas las formas de decorar posts y elementos del sitio basándose en el análisis del `exampleSite` del tema **Hugo Breu** (hugo-brewm) que utilizamos en este blog.

{{< marginpar >}}
Esta guía es especialmente útil si quieres aprovechar al máximo las capacidades visuales y de formato del tema Hugo Breu.
{{< /marginpar >}}

## Tabla de Contenidos

Esta guía cubre:

1. [Shortcodes para Decoración de Contenido](#shortcodes-para-decoración-de-contenido)
2. [Front Matter - Metadatos y Configuración](#front-matter---metadatos-y-configuración)
3. [Elementos Visuales y Multimedia](#elementos-visuales-y-multimedia)
4. [Organización y Taxonomías](#organización-y-taxonomías)
5. [Elementos de Estilo Tufte](#elementos-de-estilo-tufte)
6. [Configuración Global del Sitio](#configuración-global-del-sitio)

---

## Shortcodes para Decoración de Contenido

{{< marginpar >}}
**Nota importante**: Todos los ejemplos en esta sección están funcionando en tiempo real. Como este blog utiliza el tema Hugo Breu, puedes ver exactamente cómo se ven y funcionan cada uno de estos elementos.
{{< /marginpar >}}

En esta sección encontrarás la descripción de cada shortcode seguida de ejemplos reales en funcionamiento. Primero verás el código que necesitas escribir, y luego verás el resultado renderizado directamente en esta página.

### 1. `{{</* figure */>}}` - Figuras e Imágenes

El shortcode más versátil para insertar imágenes con diferentes estilos:

#### Uso Básico

```markdown
{{</* figure src="imagen.jpg" caption="Descripción de la imagen" */>}}
```

**Ejemplo en acción:**

{{< figure
  src="https://raw.githubusercontent.com/foxihd/hugo-et-hd/master/static/svg/flowlines/1.svg"
  title="Ejemplo de Figura"
  caption="Esta es una figura de ejemplo mostrando cómo se ve el shortcode figure en acción"
  label="fig-ejemplo-1"
  alt="Figura de ejemplo"
>}}

#### Tipos de Figuras Disponibles

**a) Figura Regular (ancho estándar)**

```markdown
{{</* figure
  src="https://ejemplo.com/imagen.png"
  title="Título de la imagen"
  caption="Esta es la descripción de la imagen"
  label="etiqueta-unica"
  attr="Atribución"
  attrlink="https://fuente.com"
  alt="Texto alternativo"
  link="https://enlace-opcional.com"
*/>}}
```

**Ejemplo en acción:**

{{< figure
  src="https://raw.githubusercontent.com/foxihd/hugo-et-hd/master/static/svg/flowlines/5.svg"
  title="Figura Regular"
  caption="Esta es una figura con ancho estándar. Observa cómo se integra con el texto del artículo."
  label="fig-regular"
  attr="Flowlines de Hugo Breu"
  attrlink="https://github.com/foxihd/hugo-et-hd"
  alt="Figura regular de ejemplo"
>}}

**b) Figura en el Margen (Margin Figure)**

```markdown
{{</* figure
  src="imagen.jpg"
  type="margin"
  caption="Imagen en el margen"
  label="mn-imagen"
*/>}}
```

**Ejemplo en acción:**

{{< figure
  src="https://raw.githubusercontent.com/foxihd/hugo-et-hd/master/static/svg/flowlines/8.svg"
  type="margin"
  caption="Figura en el margen"
  label="mn-ejemplo"
>}}

Las figuras en el margen son perfectas para imágenes complementarias que no interrumpen el flujo principal del texto. Como puedes ver, esta imagen aparece en el margen derecho (o izquierdo dependiendo del diseño) y se puede activar/desactivar haciendo clic en el indicador.

**c) Figura de Ancho Completo (Full Width)**

```markdown
{{</* figure
  src="imagen-grande.jpg"
  type="full"
  caption="Imagen de ancho completo"
  label="fig-completa"
*/>}}
```

**Ejemplo en acción:**

{{< figure
  src="https://raw.githubusercontent.com/foxihd/hugo-et-hd/master/static/svg/flowlines/12.svg"
  type="full"
  caption="Esta es una figura de ancho completo que se extiende por todo el ancho disponible del contenido"
  label="fig-full-ejemplo"
  alt="Figura de ancho completo"
>}}

**d) Usar la Imagen de Portada del Post**

```markdown
{{</* figure src="cover" caption="alt" */>}}
```

### 2. `{{</* marginpar */>}}` - Notas en el Margen

Inserta notas o comentarios en el margen del texto:

```markdown
{{</* marginpar */>}}
Este es un texto que aparecerá en el margen.
Puede contener **formato markdown** y enlaces.
{{</* /marginpar */>}}
```

**Ejemplo en acción:**

{{< marginpar >}}
Este es un ejemplo de una nota en el margen. Puedes ver cómo aparece al lado del texto principal. Puede contener **formato markdown**, *cursivas*, y [enlaces](https://example.com).
{{< /marginpar >}}

Como puedes observar, las notas en el margen son ideales para agregar información complementaria, aclaraciones o comentarios adicionales sin interrumpir el flujo principal de lectura. El texto principal continúa normalmente mientras que la nota aparece discretamente en el margen.

**Con contenido HTML seguro:**

```markdown
{{</* marginpar "safeHTML" */>}}
<p>Contenido HTML directo</p>
{{</* /marginpar */>}}
```

**Con atributo `abs` para contenido fluido:**

```markdown
{{</* marginpar "abs" */>}}
Tablas, listas y otros elementos complejos.
{{</* /marginpar */>}}
```

{{< marginpar "abs" >}}
**Ejemplo con contenido complejo:**

| Elemento | Descripción |
|----------|-------------|
| Tabla | Se puede incluir |
| Listas | También funcionan |
| HTML | Directo si usas safeHTML |

Este tipo de marginpar con `abs` permite contenido más complejo que fluye mejor con el texto.
{{< /marginpar >}}

### 3. `{{</* marginnote */>}}` - Notas con Contador

Similar a `marginpar` pero con contador automático (como notas al pie):

```markdown
{{</* marginnote */>}}
Esta nota tendrá un contador automático.
{{</* /marginnote */>}}
```

**Ejemplo en acción:**

{{< marginnote >}}
Esta es una nota con contador automático. Observa cómo tiene un número que se incrementa automáticamente.
{{< /marginnote >}}

Las notas con contador son útiles cuando necesitas referenciar información de manera secuencial, similar a las notas al pie tradicionales pero con la ventaja de aparecer en el margen.

Aquí hay otra nota para demostrar el contador:

{{< marginnote >}}
Esta es la segunda nota. Observa cómo el contador se incrementa automáticamente a 2.
{{< /marginnote >}}

### 4. `{{</* epigraph */>}}` - Epígrafes

Citas o epígrafes al inicio de secciones:

```markdown
{{</* epigraph pre="Autor, " cite="Fuente" post=", p.8" */>}}
Este es un epígrafe con matemáticas
$ \mathbb N \subseteq \mathbb R $
para comenzar una sección.
{{</* /epigraph */>}}
```

**Ejemplo en acción:**

{{< epigraph pre="Edward Tufte, " cite="The Visual Display of Quantitative Information" post=", p. 13" >}}
Excelencia en el diseño estadístico consiste en ideas claras representadas con claridad y precisión.
{{< /epigraph >}}

Los epígrafes son perfectos para comenzar secciones importantes con una cita inspiradora o relevante. Como puedes ver, el formato es elegante y ayuda a establecer el tono del contenido.

**Con enlace:**

```markdown
{{</* epigraph pre="Autor, " cite="www.ejemplo.com" link="https://www.ejemplo.com" */>}}
Contenido del epígrafe con enlace.
{{</* /epigraph */>}}
```

**Ejemplo en acción:**

{{< epigraph pre="Hugo Breu Theme, " cite="GitHub Repository" link="https://github.com/foxihd/hugo-brewm" >}}
Fine-brewed Hugo theme made open.
{{< /epigraph >}}

### 5. `{{</* section */>}}` - Secciones Personalizadas

Agrupa contenido en secciones con atributos personalizados:

```markdown
{{</* section "begin" */>}}
## Contenido de la sección
{{</* section "end" */>}}
```

**Ejemplo en acción:**

{{< section "begin" >}}

## Sección Agrupada

Este contenido está dentro de una sección agrupada usando el shortcode `section`. Esto es útil para crear bloques de contenido con estilos o comportamientos específicos.

Puedes incluir múltiples párrafos, listas, y otros elementos dentro de la sección.

- Elemento de lista 1
- Elemento de lista 2
- Elemento de lista 3

{{< section "end" >}}

**Con atributos personalizados:**

```markdown
{{</* section class="mi-clase" id="mi-id" role="complementary" */>}}
Contenido
{{</* /section */>}}
```

**Para dividir secciones:**

```markdown
{{</* section "break" */>}}
```

### 6. `{{</* theorem */>}}` - Teoremas y Lemas

Crea bloques de teoremas, lemas o proposiciones con contador:

```markdown
{{</* theorem "Lemma" */>}}
Este es un lema con contador automático.
{{</* /theorem */>}}

{{</* theorem "Teorema" */>}}
Este es un teorema con contador.
{{</* /theorem */>}}
```

**Ejemplo en acción:**

{{< theorem "Lema" >}}
Todo shortcode de Hugo Breu está diseñado para ser accesible y funcionar sin JavaScript.
{{< /theorem >}}

{{< theorem "Teorema" >}}
El tema Hugo Breu proporciona una experiencia de lectura excepcional combinando elegancia visual con funcionalidad práctica.
{{< /theorem >}}

Como puedes observar, cada teorema o lema tiene un contador automático que se incrementa según el tipo. Esto es especialmente útil para documentos técnicos o académicos donde necesitas referenciar proposiciones de manera estructurada.

### 7. `{{</* pin */>}}` - Galería Estilo Pinterest (Masonry)

Crea galerías con diseño tipo Pinterest:

```markdown
{{</* pin "begin" */>}}
{{</* pin img="https://ejemplo.com/item1.jpg" url="https://ejemplo.com/item1" label="Item 1" */>}}
{{</* pin img="https://ejemplo.com/item2.jpg" url="https://ejemplo.com/item2" label="Item 2" */>}}
{{</* pin img="https://ejemplo.com/item3.jpg" url="https://ejemplo.com/item3" label="Item 3" */>}}
{{</* pin "end" */>}}
```

**Ejemplo en acción:**

{{< pin "begin" >}}
{{< pin img="https://raw.githubusercontent.com/foxihd/hugo-et-hd/master/static/svg/flowlines/1.svg" label="Flowline 1" >}}
{{< pin img="https://raw.githubusercontent.com/foxihd/hugo-et-hd/master/static/svg/flowlines/2.svg" label="Flowline 2" >}}
{{< pin img="https://raw.githubusercontent.com/foxihd/hugo-et-hd/master/static/svg/flowlines/3.svg" label="Flowline 3" >}}
{{< pin img="https://raw.githubusercontent.com/foxihd/hugo-et-hd/master/static/svg/flowlines/4.svg" label="Flowline 4" >}}
{{< pin img="https://raw.githubusercontent.com/foxihd/hugo-et-hd/master/static/svg/flowlines/5.svg" label="Flowline 5" >}}
{{< pin img="https://raw.githubusercontent.com/foxihd/hugo-et-hd/master/static/svg/flowlines/6.svg" label="Flowline 6" >}}
{{< pin "end" >}}

**Con precios o citas:**

```markdown
{{</* pin "begin" */>}}
{{</* pin img="producto.jpg" url="#tienda" label="Producto 1" quote="<s>$299</s> $199" */>}}
{{</* pin img="producto2.jpg" url="#tienda" label="Producto 2" quote="Agotado" */>}}
{{</* pin "end" */>}}
```

**Ejemplo en acción:**

{{< pin "begin" >}}
{{< pin img="https://raw.githubusercontent.com/foxihd/hugo-et-hd/master/static/svg/flowlines/7.svg" label="Ejemplo 1" quote="<strong>Destacado</strong>" >}}
{{< pin img="https://raw.githubusercontent.com/foxihd/hugo-et-hd/master/static/svg/flowlines/8.svg" label="Ejemplo 2" quote="<em>Énfasis</em>" >}}
{{< pin img="https://raw.githubusercontent.com/foxihd/hugo-et-hd/master/static/svg/flowlines/9.svg" label="Ejemplo 3" quote="Nuevo" >}}
{{< pin "end" >}}

### 8. `{{</* rss */>}}` - Integración de Feeds RSS

Incorpora contenido de feeds RSS externos:

```markdown
{{</* rss "https://ejemplo.com/feed.xml" */>}}
```

**Con límite de elementos:**

```markdown
{{</* rss url="https://ejemplo.com/feed.xml" limit="5" */>}}
```

### 9. `{{</* network-graph */>}}` - Gráficos de Red

Crea visualizaciones de grafos de red (requiere D3.js):

```markdown
{{</* network-graph title="Mi Grafo" data="datos.json" height="600px" */>}}
```

### 10. `{{</* iframe */>}}` y `{{</* embed */>}}` - Incrustar Contenido Web Externo

Estos shortcodes permiten incrustar contenido de URLs externas en tus posts, creando páginas que actúan como "espejo" o contenedor de otras páginas web.

#### Shortcode `iframe` - Iframe Simple

```markdown
{{</* iframe url="https://ejemplo.com" width="100%" height="600px" */>}}
```

**Parámetros disponibles:**
- `url` o primer parámetro posicional: URL del contenido a incrustar
- `width`: Ancho del iframe (por defecto: "100%")
- `height`: Alto del iframe (por defecto: "600px")
- `title`: Título para accesibilidad (por defecto: "Contenido incrustado")
- `loading`: Estrategia de carga - "lazy" o "eager" (por defecto: "lazy")
- `sandbox`: Restricciones de seguridad (opcional)
- `allow`: Permisos especiales (por defecto: "fullscreen")
- `class`: Clase CSS personalizada (opcional)

**Ejemplo en acción:**

{{< iframe url="https://example.com" width="100%" height="400px" title="Ejemplo de página externa" >}}

#### Shortcode `embed` - Contenedor Responsivo

El shortcode `embed` proporciona un contenedor responsivo que mantiene la proporción 16:9:

```markdown
{{</* embed url="https://ejemplo.com" */>}}
```

**Parámetros disponibles:**
- `url` o primer parámetro posicional: URL del contenido a incrustar
- `title`: Título para accesibilidad
- `loading`: Estrategia de carga
- `sandbox`: Restricciones de seguridad
- `allow`: Permisos especiales

**Ejemplo en acción:**

{{< embed url="https://example.com" title="Contenido embebido responsivo" >}}

#### Crear una Página Espejo Completa

Para crear una página que sea completamente un espejo de otra URL, puedes usar el shortcode `iframe` con dimensiones completas:

```markdown
---
title: "Página Espejo"
type: page
---

{{</* iframe url="https://ejemplo.com" width="100%" height="100vh" */>}}
```

O usando HTML directo (requiere `unsafe = true` en `hugo.toml`):

```html
<iframe 
    src="https://ejemplo.com" 
    width="100%" 
    height="100vh" 
    style="border: 0; position: fixed; top: 0; left: 0; width: 100%; height: 100%;"
    title="Página espejo">
</iframe>
```

#### Consideraciones Importantes

{{< marginpar >}}
**Nota de seguridad**: Algunos sitios web tienen políticas que impiden ser incrustados en iframes (X-Frame-Options o Content-Security-Policy). Si una página no se muestra, es probable que tenga estas restricciones.
{{< /marginpar >}}

1. **Políticas de Seguridad**: Muchos sitios web bloquean la incrustación mediante iframes por razones de seguridad. Si el contenido no se muestra, verifica las políticas del sitio origen.

2. **Rendimiento**: Incrustar contenido externo puede afectar el tiempo de carga de tu página. Considera usar `loading="lazy"` para cargar el contenido solo cuando sea visible.

3. **Responsividad**: El shortcode `embed` es responsivo por defecto. El shortcode `iframe` requiere que especifiques dimensiones.

4. **Accesibilidad**: Siempre proporciona un `title` descriptivo para mejorar la accesibilidad.

5. **Sandbox**: Para mayor seguridad, puedes usar el parámetro `sandbox` con restricciones específicas:
   ```markdown
   {{</* iframe url="https://ejemplo.com" sandbox="allow-scripts allow-same-origin" */>}}
   ```

---

## Front Matter - Metadatos y Configuración

### Metadatos Básicos del Post

```yaml
---
title: "Título del Post"
subtitle: "Subtítulo Opcional"
description: "Descripción para SEO y metadatos"
date: 2025-01-26
author: ['Nombre del Autor']
type: post
draft: false
translationKey: clave-traduccion
---
```

### Imágenes de Portada

**Método 1: Front Matter**

```yaml
---
title: "Mi Artículo"
cover: "https://ejemplo.com/portada.jpg"
# o alternativamente:
images: "images/mi-portada.jpg"
alt: "Descripción de la imagen de portada"
coverAlt: "Descripción alternativa"
imagesAlt: "Otra descripción"
---
```

**Método 2: Page Bundle**

- Crear una carpeta para el post
- Nombrar la imagen `cover.*` (cover.jpg, cover.png, etc.)
- Colocarla en la misma carpeta que el contenido
- Configurar el alt text en el front matter

### Audio Articles

**Método 1: Front Matter**

```yaml
---
title: "Mi Artículo con Audio"
audio: "audio/mi-audio.ogg"
---
```

**Método 2: Page Bundle**

- Crear carpeta para el post
- Nombrar el audio `audio.*` (audio.mp3, audio.ogg, etc.)
- Colocarlo en la misma carpeta que el contenido
- Puedes tener múltiples formatos (mp3, ogg, etc.)

### Configuración de Matemáticas

```yaml
---
math: true  # Habilita KaTeX para matemáticas
---
```

**Uso en el contenido:**

- Inline: `$e^{i \pi} = -1$`
- Display: `$$\int_{-\infty}^{\infty} e^{-x^2} dx = \sqrt{\pi}$$`

**Ejemplo en acción (requiere `math: true` en el front matter):**

Para matemáticas inline, puedes escribir: $e^{i \pi} = -1$ directamente en el texto.

Para ecuaciones más complejas en modo display:

$$
\int_{-\infty}^{\infty} e^{-x^2} dx = \sqrt{\pi}
$$

Las matemáticas se renderizan usando KaTeX, que proporciona renderizado rápido y de alta calidad.

### Tabla de Contenidos

```yaml
---
toc: true  # Genera tabla de contenidos automática
---
```

### Numeración de Secciones

```yaml
---
secnum: true  # Habilita numeración automática de secciones
---
```

### Métricas de Café (Coffee Stat)

```yaml
---
coffee: 1  # Tiempo de lectura estimado en tazas de café
---
```

### Etapas de Desarrollo (Digital Garden)

```yaml
---
stage: "seedling"  # Opciones: seedling, budding, evergreen
---
```

**Con historial de etapas:**

```yaml
---
stage: "evergreen"
history = [
  {date = "2025-01-26", stage="seedling"},
  {date = "2025-01-27", stage="budding"},
  {date = "2025-01-28", stage="evergreen"},
]
---
```

### Historial de Redacción

```yaml
---
history = [
  {date = "2025-01-26", stage="seedling", author = "Autor", reviewer = "Revisor", note = "Primera versión"},
  {date = "2025-01-27", stage="budding", author = "Autor", editor = "Editor", note = "Correcciones"},
  {date = "2025-01-28", stage="evergreen", note = "Versión final"},
]
---
```

### Enlaces a Redes Sociales

```yaml
---
toot: "https://mastodon.ejemplo.com/@usuario/123456"  # Mastodon
bsky: "https://bsky.app/profile/usuario/post/abc123"  # Bluesky
---
```

### Meta Información

```yaml
---
meta: true  # Habilita metadatos extendidos
---
```

---

## Elementos Visuales y Multimedia

### 1. Imágenes de Portada en Taxonomías

Para categorías, tags, series y autores:

```yaml
# content/es/categories/mi-categoria/_index.md
---
title: "Mi Categoría"
description: "Descripción de la categoría"
cover: "https://ejemplo.com/cover.png"
translationKey: mi-categoria
---

Si hay texto aquí, se mostrará como sección hero.
```

### 2. Slides en la Página Principal

Crear slides para el carrusel de la homepage:

```yaml
# content/es/slide-mi-slide.md
---
type: slide
title: "Título del Slide"
cover: "https://ejemplo.com/slide-cover.jpg"
textColor: black  # o "white"
params:
    headless: true  # Previene renderizado de página
    target: "https://ejemplo.com"  # URL de destino
---
```

**Slide con RSS:**

```yaml
---
type: slide
title: "Mi Post en ejemplo.com"
---

{{</* rss "https://ejemplo.com/feed.xml" */>}}
```

**Slide con Galería Pin:**

```yaml
---
type: slide
title: "Galería"
---

{{</* pin "begin" */>}}
{{</* pin img="item1.jpg" label="Item 1" */>}}
{{</* pin "end" */>}}
```

### 3. Código con Resaltado de Sintaxis

```go {linenos=table,hl_lines=["2-5"],linenostart=199}
package main

import "log"

func add(x int, y int) int {
  return x + y
}
```

**Opciones disponibles:**

- `linenos=table`: Muestra números de línea en tabla
- `hl_lines=["2-5"]`: Resalta líneas específicas
- `linenostart=199`: Inicia numeración desde un número específico

---

## Organización y Taxonomías

### Taxonomías Personalizadas

**Registrar en `hugo.toml`:**

```toml
[taxonomies]
    stage = "stage"
    category = "categories"
    tag = "tags"
    author = "author"
    series = "series"
```

**Crear estructura:**

```
content/
└── stage/
    ├── seedling/
    │   └── _index.md
    ├── budding/
    │   └── _index.md
    └── evergreen/
        └── _index.md
```

**Configurar términos:**

```yaml
# content/stage/seedling/_index.md
---
title: 'Etapa de Desarrollo'
translationKey: seedling
emoji: '🌱'
# indicator: 'https://ejemplo.com/indicator.svg'
---

Descripción de la etapa.
```

### Etiquetas y Categorías

```yaml
---
tags: ['markdown', 'tutorial', 'decoración']
categories: ['Guías', 'Tutoriales']
---
```

### Series

```yaml
---
series: ['Mi Serie']
---
```

### Múltiples Autores

```yaml
---
author: ['Autor Principal', 'Coautor']
---
```

---

## Elementos de Estilo Tufte

El tema incluye características inspiradas en el estilo Tufte:

### 1. Matemáticas con KaTeX

**Inline:**

```markdown
Algunas matemáticas inline: $e^{i \pi} = -1$ y $\sqrt{-1} = i$.
```

**Display:**

```markdown
$$
\int_{-\infty}^{\infty} e^{-x^2} dx = \sqrt{\pi}
$$
```

**Entornos complejos:**

```html
<p>
$$
\begin{aligned}
  \mu(A) &= \iint_{I^2} \chi_A (x,y) \ d(x,y)
  = \int_I \left( \int_I  \chi_A (x,y) \ dx\right) dy
\end{aligned}
$$
</p>
```

### 2. Citas con Etiquetas HTML

```html
<cite>Esta es una cita con matemáticas: $e^x$</cite>
```

### 3. Clases CSS Especiales

```html
<span class="letterine"><i>T</i>his es un ejemplo de span con clase letterine.</span>
```

---

## Configuración Global del Sitio

### Configuración de Posts

En `hugo.toml`:

```toml
[params.posts]
    ## Habilitar justificación de texto
    justifying = false
    ## Deshabilitar indentación de párrafos
    noIndent = false
    ## Usar fuentes sans-serif por defecto
    sfdefault = false
    ## Mostrar sección colophon (incluyendo QR code)
    colophon = true
    ## Deshabilitar historial de redacción
    disableHistory = false
    ## Mostrar contenido relacionado
    related = true
    ## Mostrar botones de compartir
    share = true
    ## Habilitar numeración de secciones
    secnum = false
```

### Configuración de Tipografía

```toml
[params.typeface]
    ## Usar fuentes web seguras
    webSafe = false
    ## Fuente serif: 'Cormorant', 'EB Garamond', 'Crimson' (default)
    roman = 'crimson'
    ## Fuente sans-serif: 'Inter', 'Montserrat', 'Rosario', 'Lexica Ultralegible' (default)
    sans = 'inter'
    ## Hostear fuentes localmente
    localHost = true
```

### Configuración de Feed

```toml
[params.feed]
    ## Habilitar flowlines (ilustraciones generadas)
    flowlines = true
    ## Límite de flowlines (máximo 42)
    flowlinesLimit = 21
    ## Directorio personalizado de flowlines
    OverrideFlowlinesDir = 'https://ejemplo.com/flowlines/'
    ## Extensión de archivos
    flowlinesExt = 'svg'
```

### Configuración de Búsqueda

```toml
[params.search]
    ## Habilitar búsqueda
    enable = true
    ## Usar Pagefind
    pagefind = true
    ## Búsqueda alternativa sin JavaScript
    # fallback = 'mojeek'
```

### Configuración de Comentarios

```toml
[params.comments]
    ## Deshabilitar comentarios
    disabled = false
    ## Plataforma de comentarios
    # platform = 'giscus'
```

### Configuración de Giscus

```toml
[params.giscus]
    repo = "usuario/repositorio"
    repoID = "R_xxx"
    category = "Comentarios"
    categoryID = "DIC_xxx"
    mapping = "og:title"
    reactionsEnabled = "1"
    emitMetadata = "0"
    inputPosition = "bottom"
    theme = "light"
    loading = "lazy"
```

---

## Ejemplos de Uso Combinado

### Post Completo con Múltiples Elementos

```markdown
---
title: "Mi Post Decorado"
date: 2025-01-26
author: ['Autor']
cover: "portada.jpg"
alt: "Descripción de portada"
math: true
toc: true
secnum: true
stage: "evergreen"
tags: ['decoración', 'tutorial']
categories: ['Guías']
coffee: 2
---

{{</* section "begin" */>}}

{{</* epigraph pre="Autor, " cite="Fuente" */>}}
Este es un epígrafe para comenzar el artículo.
{{</* /epigraph */>}}

<!--more-->

## Sección Principal

{{</* marginpar */>}}
Esta es una nota en el margen que complementa el contenido.
{{</* /marginpar */>}}

Contenido principal del artículo con matemáticas inline: $E = mc^2$.

### Subsección

{{</* figure
  src="imagen.jpg"
  type="full"
  caption="Imagen de ancho completo"
  label="fig-1"
*/>}}

{{</* theorem "Lema" */>}}
Este es un lema importante con contador.
{{</* /theorem */>}}

{{</* section "end" */>}}
```

### Galería de Productos

```markdown
## Nuestros Productos

{{</* pin "begin" */>}}
{{</* pin img="producto1.jpg" url="/producto1" label="Producto 1" quote="$99" */>}}
{{</* pin img="producto2.jpg" url="/producto2" label="Producto 2" quote="$149" */>}}
{{</* pin img="producto3.jpg" url="/producto3" label="Producto 3" quote="<s>$199</s> $149" */>}}
{{</* pin "end" */>}}
```

---

## Resumen de Elementos Disponibles

### Shortcodes

- ✅ `figure` - Imágenes (regular, margin, full width)
- ✅ `marginpar` - Notas en el margen
- ✅ `marginnote` - Notas con contador
- ✅ `epigraph` - Epígrafes
- ✅ `section` - Secciones personalizadas
- ✅ `theorem` - Teoremas y lemas
- ✅ `pin` - Galería estilo Pinterest
- ✅ `rss` - Integración de feeds
- ✅ `network-graph` - Gráficos de red
- ✅ `iframe` - Incrustar contenido web externo (simple)
- ✅ `embed` - Incrustar contenido web externo (responsivo)

### Front Matter

- ✅ Imágenes de portada (cover/images)
- ✅ Audio articles
- ✅ Matemáticas (KaTeX)
- ✅ Tabla de contenidos
- ✅ Numeración de secciones
- ✅ Etapas de desarrollo (Digital Garden)
- ✅ Historial de redacción
- ✅ Enlaces sociales (Mastodon/Bluesky)
- ✅ Métricas de café

### Elementos Visuales

- ✅ Slides en homepage
- ✅ Portadas en taxonomías
- ✅ Flowlines (ilustraciones generadas)
- ✅ Código con resaltado de sintaxis

### Organización

- ✅ Taxonomías personalizadas
- ✅ Etapas de desarrollo
- ✅ Series
- ✅ Múltiples autores

---

## Notas Importantes

1. **Markdown Unsafe**: Para usar HTML directamente en markdown, necesitas configurar `markup.goldmark.renderer.unsafe = true` en `hugo.toml`, aunque no es recomendado.

2. **Matemáticas**: El tema soporta KaTeX y MathJax. Configura `math: true` en el front matter o globalmente.

3. **Page Bundle**: Para usar imágenes y audio como page bundles, crea una carpeta para cada post y coloca los archivos dentro.

4. **Flowlines**: Son ilustraciones generadas automáticamente que se usan cuando no hay imagen de portada. Puedes personalizarlas.

5. **Accesibilidad**: Todos los elementos incluyen atributos ARIA y son accesibles incluso sin JavaScript.

---

Este análisis está basado en el contenido del `exampleSite` del tema Hugo Breu (hugo-brewm) versión actual. Para más detalles, consulta la documentación oficial del tema o explora el `exampleSite` directamente.

