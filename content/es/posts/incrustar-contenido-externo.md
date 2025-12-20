---
title: "Incrustar Contenido Web Externo en el Blog"
date: 2025-01-26T14:00:00Z
draft: false
tags: ["tutorial", "contenido", "web", "embed"]
categories: ["Guías"]
cover: 'https://raw.githubusercontent.com/foxihd/hugo-et-hd/master/static/svg/flowlines/20.svg'
alt: 'Incrustar contenido web externo en el blog'
description: "Guía práctica sobre cómo incrustar contenido de sitios web externos en nuestros posts usando shortcodes personalizados"
toc: true
---

{{< figure src="cover" caption="alt" >}}

En este post exploraremos cómo incrustar contenido de sitios web externos directamente en nuestros posts del blog. Esta funcionalidad nos permite crear páginas que actúan como "espejo" o contenedor de otras páginas web, enriqueciendo nuestro contenido con recursos externos.

{{< marginpar >}}
Esta funcionalidad es especialmente útil para mostrar documentación externa, dashboards, o cualquier contenido web que queramos integrar en nuestro blog.
{{< /marginpar >}}

## ¿Qué es la Incrustación de Contenido?

La incrustación de contenido web externo nos permite mostrar páginas de otros sitios dentro de nuestros propios posts, creando una experiencia integrada donde el contenido externo se ve como parte de nuestro sitio.

## Shortcodes Disponibles

Hemos creado dos shortcodes personalizados para facilitar esta tarea:

### 1. Shortcode `iframe` - Control Directo

El shortcode `iframe` te permite tener control total sobre las dimensiones del contenido incrustado:

```markdown
{{</* iframe url="https://ejemplo.com" width="100%" height="600px" */>}}
```

**Parámetros disponibles:**
- `url`: URL del contenido a incrustar (requerido)
- `width`: Ancho del iframe (por defecto: "100%")
- `height`: Alto del iframe (por defecto: "600px")
- `title`: Título para accesibilidad
- `loading`: "lazy" o "eager" (por defecto: "lazy")
- `sandbox`: Restricciones de seguridad
- `allow`: Permisos especiales

**Ejemplo en acción:**

{{< iframe url="https://example.com" width="100%" height="400px" title="Ejemplo de página externa - Example.com" >}}

### 2. Shortcode `embed` - Contenedor Responsivo

El shortcode `embed` proporciona un contenedor responsivo que mantiene automáticamente la proporción 16:9:

```markdown
{{</* embed url="https://ejemplo.com" */>}}
```

**Ejemplo en acción:**

{{< embed url="https://example.com" title="Contenido embebido responsivo" >}}

## Casos de Uso Prácticos

### Caso 1: Mostrar Documentación Externa

Si queremos mostrar documentación de otro sitio dentro de nuestro post:

```markdown
## Documentación de Hugo

{{</* embed url="https://gohugo.io/documentation/" */>}}
```

### Caso 2: Dashboard o Panel de Control

Para mostrar un dashboard externo con dimensiones específicas:

```markdown
{{</* iframe url="https://dashboard.ejemplo.com" width="100%" height="800px" title="Dashboard de ejemplo" */>}}
```

### Caso 3: Página Espejo Completa

Para crear una página que sea completamente un espejo de otra URL, podemos crear un post o página dedicada:

```markdown
---
title: "Vista Espejo de Documentación"
type: page
---

{{</* iframe url="https://docs.ejemplo.com" width="100%" height="100vh" title="Documentación externa" */>}}
```

## Consideraciones de Seguridad

{{< marginpar >}}
**X-Frame-Options**: Muchos sitios web tienen políticas de seguridad que impiden ser incrustados. Si ves un error o la página no se muestra, es probable que el sitio tenga estas restricciones activadas.
{{< /marginpar >}}

### Políticas de Seguridad

No todos los sitios web permiten ser incrustados en iframes. Esto se debe a políticas de seguridad como:

- **X-Frame-Options**: Header HTTP que previene el clickjacking
- **Content-Security-Policy**: Política de seguridad de contenido más moderna

Si intentas incrustar un sitio y no se muestra, verifica:

1. Si el sitio permite iframes (algunos lo indican en su documentación)
2. Si hay restricciones específicas de dominio
3. Si necesitas autenticación o permisos especiales

### Uso de Sandbox

Para mayor seguridad, puedes usar el parámetro `sandbox`:

```markdown
{{</* iframe url="https://ejemplo.com" sandbox="allow-scripts allow-same-origin" */>}}
```

Las opciones de sandbox incluyen:
- `allow-scripts`: Permite ejecutar scripts
- `allow-same-origin`: Permite acceso al mismo origen
- `allow-forms`: Permite formularios
- `allow-popups`: Permite ventanas emergentes
- Y más opciones según tus necesidades de seguridad

## Optimización de Rendimiento

### Carga Diferida (Lazy Loading)

Por defecto, nuestros shortcodes usan `loading="lazy"`, lo que significa que el contenido solo se carga cuando el usuario hace scroll hasta él:

```markdown
{{</* iframe url="https://ejemplo.com" loading="lazy" */>}}
```

Esto mejora significativamente el tiempo de carga inicial de la página.

### Carga Inmediata

Si necesitas que el contenido se cargue inmediatamente:

```markdown
{{</* iframe url="https://ejemplo.com" loading="eager" */>}}
```

## Ejemplo Completo: Página con Múltiples Incrustaciones

Aquí tienes un ejemplo de cómo estructurar un post con múltiples incrustaciones:

```markdown
---
title: "Recursos y Documentación Externa"
date: 2025-01-26
tags: ["recursos", "documentación"]
---

## Documentación Principal

{{</* embed url="https://docs.ejemplo.com" title="Documentación principal" */>}}

## Dashboard de Estadísticas

{{</* iframe url="https://dashboard.ejemplo.com/stats" width="100%" height="600px" title="Dashboard de estadísticas" */>}}

## Herramienta Interactiva

{{</* embed url="https://tools.ejemplo.com/interactive" title="Herramienta interactiva" */>}}
```

## Accesibilidad

{{< marginpar >}}
Siempre proporciona títulos descriptivos para mejorar la accesibilidad y ayudar a los usuarios con lectores de pantalla.
{{< /marginpar >}}

Es importante proporcionar títulos descriptivos para todos los iframes:

```markdown
{{</* iframe url="https://ejemplo.com" title="Descripción clara del contenido incrustado" */>}}
```

Esto ayuda a:
- Usuarios con lectores de pantalla
- Mejorar el SEO
- Proporcionar contexto cuando el contenido no carga

## Alternativa: HTML Directo

Si necesitas más control o los shortcodes no cubren tus necesidades específicas, puedes usar HTML directo (ya que `unsafe = true` está habilitado en nuestra configuración):

```html
<iframe 
    src="https://ejemplo.com" 
    width="100%" 
    height="600px" 
    style="border: 0;"
    title="Contenido externo"
    loading="lazy"
    allowfullscreen>
</iframe>
```

## Resumen

- ✅ Usa `{{< iframe >}}` para control directo de dimensiones
- ✅ Usa `{{< embed >}}` para contenedores responsivos automáticos
- ✅ Siempre proporciona títulos descriptivos
- ✅ Considera usar `loading="lazy"` para mejor rendimiento
- ✅ Ten en cuenta las restricciones de seguridad de los sitios externos
- ✅ Usa `sandbox` cuando necesites mayor seguridad

## Próximos Pasos

Ahora que conoces cómo incrustar contenido externo, puedes:

1. Experimentar con diferentes sitios y ver cuáles permiten incrustación
2. Crear páginas espejo para documentación importante
3. Integrar dashboards o herramientas externas en tus posts
4. Combinar contenido propio con recursos externos

---

{{< marginpar >}}
¿Tienes preguntas sobre cómo incrustar contenido específico? Consulta nuestra [Guía de Decoración](/es/posts/guia-decoracion-hugo-breu/) para más información sobre todos los elementos disponibles en el tema Hugo Breu.
{{< /marginpar >}}

¡Experimenta y crea contenido rico e interactivo combinando tu contenido con recursos externos!

