# Capellanía Empresarial Uruguay

Sitio web institucional de Capellanía Empresarial Uruguay. Está desarrollado como un sitio estático, responsive y accesible, sin frameworks de frontend ni servicios de backend.

## Arquitectura y herramientas

- **HTML5 estático** para la home y la galería.
- **CSS propio** para el sistema visual, responsive y animaciones.
- **JavaScript nativo** para navegación móvil, video, galería y mejoras de interacción.
- **JSON** para administrar el contenido de la galería.
- **Node.js y npm** únicamente para generar y validar la versión publicable.
- **Cloudflare Workers con Static Assets** como plataforma de publicación.

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
_headers      Encabezados de seguridad para los recursos estáticos de Cloudflare.
```

La carpeta `dist/` se genera automáticamente y no debe editarse ni subirse a Git.

## Requisitos

- Node.js con soporte para módulos ES.
- npm.

El proyecto fue verificado con Node.js `22.16.0` y npm `10.9.2`.

## Ejecución local

```bash
npm ci
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

El build de producción exige un dominio confirmado. `indexable` controla por separado la indexación; actualmente está habilitada para el dominio oficial.

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

## Despliegue en Cloudflare Workers con Static Assets

El flujo de publicación utiliza Workers Builds y `npx wrangler deploy`, no Cloudflare Pages. El sitio sigue siendo HTML, CSS y JavaScript estáticos; no necesita un servidor ni un archivo de entrada Worker.

`wrangler.jsonc` fija el recurso `paginaweb`, la fecha de compatibilidad `2026-10-16` y `assets.directory: "./dist"`. No ejecutar el despliegue con `--assets .` ni desde otra carpeta.

En el panel del Worker, verificar antes de autorizar la publicación:

```text
Repositorio: CapellaniaUruguay/PaginaWeb
Rama de producción: main
Directorio raíz: raíz del repositorio (dejar vacío o / según el panel)
Comando de build: npm run build:production
Comando de despliegue: npx wrangler deploy --config wrangler.jsonc
Worker esperado: paginaweb
```

El build también puede ejecutarse con `npm run build` para una vista previa. Workers Builds debe tener un comando de build explícito; la configuración de assets no genera `dist/` por sí sola.

Validación local sin publicar (Wrangler se descarga temporalmente, sin añadir dependencias al proyecto):

```bash
npm ci
npm run build
npx --yes wrangler@4.151.1 deploy --config wrangler.jsonc --dry-run
```

La versión anterior es la utilizada para esta validación. El comando automático `npx wrangler deploy` puede resolver otra versión si Cloudflare no la fija; repetir el dry run si cambia la herramienta.

Solo se publica `dist/`, generado desde una lista explícita en `scripts/build-static.js`. El build rechaza archivos superiores a 25 MiB y rutas internas o privadas. No deben copiarse a esa carpeta dependencias, documentación, credenciales ni archivos de desarrollo. `.gitignore` no sustituye esta separación.

`site.config.json` tiene `indexable: true`, autorizado para el dominio oficial. El build genera `index, follow` en ambas páginas y anuncia el sitemap en `robots.txt`.

Para vistas previas que deban permanecer fuera de buscadores, definir `SITE_INDEXABLE=false` en su entorno de build. En producción, eliminar esa variable o definirla como `true`; un valor `false` prevalece sobre el archivo y vuelve a generar `noindex`. No retirar protecciones legítimas de recursos privados.

Después de publicar esta corrección, comprobar por HTTP la home, `/galeria/`, `/robots.txt` y `/sitemap.xml` del dominio oficial. Revisar también que Cloudflare no añada un encabezado `X-Robots-Tag: noindex`. Luego enviar `https://capellaniaempresarial-uy.org/sitemap.xml` en la propiedad verificada de Google Search Console y solicitar inspección de las dos páginas. La autorización local de indexación no confirma que la versión pública haya sido actualizada.

No se modifican DNS ni dominios mediante esta configuración. Una validación local correcta no confirma un despliegue remoto exitoso.

## Enlaces externos

- Impulso+: configurado en la sección `#impulso` de `index.html`.
- Solicitud de presupuesto: enlace de WhatsApp en `index.html`.
- Correo e Instagram: footer de `index.html` y `galeria/index.html`.

Revisar periódicamente que estos destinos continúen vigentes y autorizados.
