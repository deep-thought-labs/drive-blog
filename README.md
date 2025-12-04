# Infinite Improbability Drive - Blog

Blog oficial de Infinite Improbability Drive donde compartimos actualizaciones, comunicados y el progreso de nuestros proyectos.

## Sitio Principal

- 🌐 **Sitio Web**: [infinitedrive.xyz](https://infinitedrive.xyz)
- 📦 **Repositorio Frontend**: [github.com/WizardLatino/Infinitedrivefront](https://github.com/WizardLatino/Infinitedrivefront)

## Desarrollo Local

Este blog está construido con [Hugo](https://gohugo.io/) y utiliza el tema [Geekdoc](https://geekdocs.de/).

### Requisitos

- Hugo (versión 0.124 o superior)

### Instalación y Ejecución

```bash
# Instalar dependencias del tema (si es necesario)
cd themes/hugo-geekdoc
npm install
npm run build

# Volver al directorio raíz y ejecutar el servidor de desarrollo
cd ../..
hugo server
```

El sitio estará disponible en `http://localhost:1313`

### Construcción para Producción

```bash
hugo
```

Los archivos estáticos se generarán en el directorio `public/`.

## Despliegue

Este proyecto está configurado para desplegarse en Cloudflare Workers usando Wrangler.

```bash
# Construir el sitio
hugo

# Desplegar
wrangler deploy
```

## Estructura del Contenido

- `content/posts/` - Artículos del blog
- `content/_index.md` - Página de inicio
- `hugo.toml` - Configuración de Hugo
