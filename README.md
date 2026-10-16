# Capellanía Empresarial Uruguay

Sitio web institucional estático de Capellanía Empresarial Uruguay. Presenta la organización, sus servicios, misión, visión, valores, indicadores pendientes de confirmación, galería institucional, datos de contacto y acceso al canal de Impulso+.

La versión de desarrollo permanece excluida de buscadores hasta que se confirme el dominio definitivo y se autorice la publicación.

## Características principales

- Home responsive con Hero, presentación institucional, video, servicios, misión, visión, valores, impacto, labor en imágenes y contacto.
- Galería con visor accesible mediante teclado y soporte para filtros cuando existan varias categorías.
- Menú adaptable a escritorio y dispositivos móviles.
- Botones de contacto por correo, Instagram y WhatsApp.
- Acceso desde la home al canal de WhatsApp de Impulso+.
- Metadatos SEO, Open Graph, Twitter Card, sitemap y `robots.txt` generados durante la compilación.
- Soporte para navegación por teclado y `prefers-reduced-motion`.

## Estructura del proyecto

```text
assets/
  data/       Datos de la galería.
  images/     Imágenes publicadas por la home y la galería.
  js/         Interacciones nativas del sitio.
  logos/      Identidad visual de Uruguay e Impulso+.
  video/      Video y póster de la sección institucional.
css/          Hoja de estilos global.
galeria/      Página de galería.
scripts/      Compilador estático y validación de configuración.
index.html    Página principal.
site.config.json  Dominio, indexación y rutas públicas.
vercel.json   Configuración de compilación y encabezados para Vercel.
```

## Requisitos

- Node.js con soporte para módulos ES.
- npm.

El proyecto se verificó durante la preparación institucional con Node.js `22.16.0` y npm `10.9.2`. No requiere frameworks ni servicios de backend para generar el sitio.

## Instalación y ejecución local

```bash
npm install
npm run build
npx serve dist
```

También puede servirse `dist/` con cualquier servidor HTTP estático. Abrir los archivos directamente con `file://` no reproduce correctamente las rutas absolutas del sitio.

## Configuración

La configuración pública se encuentra en `site.config.json`:

- `siteUrl`: origen utilizado en canonical, Open Graph, JSON-LD y sitemap.
- `siteUrlConfirmed`: debe ser `true` únicamente cuando el dominio sea definitivo.
- `provisionalSiteUrls`: dominios que el build de producción debe rechazar.
- `indexable`: controla `index`/`noindex` y la publicación del sitemap en `robots.txt`.
- `trailingSlash`: conserva rutas compatibles con alojamiento estático.
- `routes`: relaciona cada URL pública con su archivo fuente.

El build admite `SITE_URL`, `SITE_INDEXABLE` y `SITE_PRODUCTION` como ajustes de entorno. El sitio actual no necesita credenciales ni variables secretas.

## Publicación

Para una vista previa segura:

```bash
npm run build
```

Para producción, primero debe confirmarse el dominio en `site.config.json`, retirarse de `provisionalSiteUrls` y habilitarse la indexación cuando corresponda. Después:

```bash
npm run build:production
```

En Vercel, la configuración incluida ejecuta `npm run build` y publica `dist/`. Antes de la salida pública debe cambiarse el comando del proyecto a `npm run build:production` o definirse `SITE_PRODUCTION=true`, para que una configuración provisional haga fallar el despliegue.

### Repositorio institucional con historial nuevo

Para entregar una copia independiente sin trasladar el historial experimental:

1. Crear un repositorio vacío en la cuenta oficial de Capellanía Uruguay.
2. Exportar el árbol aprobado sin la carpeta `.git`, `dist/`, `_instagram/` ni archivos locales.
3. Inicializar Git en la copia exportada y crear un único commit raíz con identidad institucional.
4. Añadir el remoto oficial y publicar la rama `main` solamente después de revisar el contenido.
5. Activar protección de rama, autenticación de dos factores, revisión de cambios, secret scanning y push protection.

## Mantenimiento

- Textos y secciones de la home: `index.html`.
- Contenido y descripciones de la galería: `assets/data/galeria.json`.
- Imágenes de galería: `assets/images/galeria/`; conservar las variantes de 800 px y tamaño completo.
- Estilos: `css/capellania-global.css`.
- Navegación, menú móvil y animaciones de aparición: `assets/js/uy-site.js`.
- Visor de galería: `assets/js/galeria-uruguay.js`.
- Dominio e indexación: `site.config.json`.

Después de cualquier cambio, ejecutar `npm run build` y comprobar la home y `/galeria/` en escritorio y móvil. Mantener el número de versión de la hoja de estilos en las páginas cuando cambie CSS para evitar caché obsoleta.

## Enlaces externos

- Impulso+: la sección `#impulso` abre el canal compartido de WhatsApp configurado en `index.html`. No existe una página local ni una copia del contenido del canal.
- Presupuesto: el botón de contacto abre WhatsApp con un mensaje inicial.
- Contacto institucional: correo e Instagram se encuentran en el footer de la home y la galería.

Revisar periódicamente que estos destinos sigan vigentes y correspondan a cuentas autorizadas por Capellanía Empresarial Uruguay.
