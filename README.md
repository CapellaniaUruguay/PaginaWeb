# Capellanía Empresarial Uruguay

Sitio web institucional de Capellanía Empresarial Uruguay. Está desarrollado como un sitio estático, responsive y accesible, sin frameworks de frontend ni servicios de backend.

## Arquitectura y herramientas

- **HTML5 estático** para la home y la galería.
- **CSS propio** para el sistema visual, responsive y animaciones.
- **JavaScript nativo** para navegación móvil, video, galería y mejoras de interacción.
- **JSON** para administrar el contenido de la galería.
- **Node.js y npm** únicamente para generar y validar la versión publicable.
- **Cloudflare Pages** como plataforma prevista de alojamiento.

No utiliza React, Vue, Next.js, Tailwind, bases de datos ni APIs privadas.

## Páginas y funcionalidades

- Home institucional con Hero, presentación, misión, visión, valores, servicios, impacto, video y contacto.
- Sección Impulso+ con acceso al canal compartido de WhatsApp.
- Galería responsive con visor accesible mediante teclado.
- Navegación móvil con soporte para teclado y tecla `Escape`.
- Enlaces a WhatsApp, Instagram y correo institucional.
- Metadatos SEO, Open Graph, Twitter Card, sitemap y `robots.txt` generados durante el build.
- Soporte para `prefers-reduced-motion`.

## Estructura del proyecto

```text
assets/
  data/       Información y descripciones de la galería.
  images/     Imágenes de la home, galería y Open Graph.
  js/         Interacciones del sitio en JavaScript nativo.
  logos/      Logos, favicon e identidad visual.
  video/      Video institucional y su imagen de portada.
css/          Estilos globales y responsive.
galeria/      Página independiente de la galería.
scripts/      Build estático y validación de configuración.
index.html    Página principal.
site.config.json  Dominio, indexación y rutas públicas.
_headers      Encabezados de seguridad para Cloudflare Pages.
```

La carpeta `dist/` se genera automáticamente y no debe editarse ni subirse a Git.

## Requisitos

- Node.js con soporte para módulos ES.
- npm.

El proyecto fue verificado con Node.js `22.16.0` y npm `10.9.2`.

## Ejecución local

```bash
npm install
npm run build
npx serve dist
```

El sitio debe abrirse mediante un servidor HTTP local. Las rutas absolutas no funcionan correctamente abriendo los HTML directamente con `file://`.

## Configuración del sitio

El archivo `site.config.json` controla:

- `siteUrl`: dominio utilizado en canonical, Open Graph, JSON-LD y sitemap.
- `siteUrlConfirmed`: confirma que el dominio ya es definitivo.
- `provisionalSiteUrls`: dominios que no deben aparecer en producción.
- `indexable`: activa o bloquea la indexación en buscadores.
- `trailingSlash`: formato de las rutas públicas.
- `routes`: páginas incluidas en el build.

Mientras el dominio no esté confirmado, el sitio se genera con `noindex` y el build de producción permanece bloqueado.

## Mantenimiento de contenido

### Textos y secciones

La home se mantiene en `index.html`. Antes de publicar cambios institucionales, confirmar nombres, cifras, teléfonos, correos, enlaces y textos con Capellanía Empresarial Uruguay.

### Galería

- Datos y descripciones: `assets/data/galeria.json`.
- Imágenes: `assets/images/galeria/`.
- Cada fotografía dispone de una variante optimizada de 800 px y otra de tamaño completo.
- El visor utiliza la imagen completa y las tarjetas utilizan la variante optimizada.

### Estilos

Los estilos están en `css/capellania-global.css`. Si se modifica este archivo, actualizar el parámetro de versión del enlace CSS en `index.html` y `galeria/index.html` para evitar que el navegador conserve una versión anterior en caché.

### JavaScript

- `assets/js/uy-site.js`: menú móvil y animaciones de aparición.
- `assets/js/uy-video.js`: controles del video institucional.
- `assets/js/galeria-uruguay.js`: tarjetas, filtros y visor de galería.

### Imágenes, logos y video

Los recursos que deben publicarse se enumeran en `scripts/build-static.js`. Cuando se añada un nuevo recurso fuera de una carpeta ya incluida, incorporarlo también a esa lista.

## Validación antes de publicar

Ejecutar siempre:

```bash
npm run build
```

Después revisar:

- Home y `/galeria/`.
- Navegación en escritorio y móvil.
- Ausencia de scroll horizontal.
- Imágenes, video, botones y enlaces externos.
- Consola del navegador.
- Contenido pendiente y metadatos SEO.

Cuando el dominio sea definitivo, actualizar `site.config.json` y ejecutar:

```bash
npm run build:production
```

El build de producción se detiene si el dominio continúa marcado como provisional.

## Despliegue en Cloudflare Pages

Conectar este repositorio desde el panel de Cloudflare Pages y utilizar:

```text
Comando de build: npm run build:production
Directorio de salida: dist
Directorio raíz: /
```

Para una vista previa antes de confirmar el dominio puede utilizarse `npm run build`, que mantiene `noindex`.

Cloudflare Pages instalará las dependencias declaradas en `package-lock.json`, ejecutará el comando configurado y publicará únicamente el contenido generado en `dist/`.

## Enlaces externos

- Impulso+: configurado en la sección `#impulso` de `index.html`.
- Solicitud de presupuesto: enlace de WhatsApp en `index.html`.
- Correo e Instagram: footer de `index.html` y `galeria/index.html`.

Revisar periódicamente que estos destinos continúen vigentes y autorizados.
