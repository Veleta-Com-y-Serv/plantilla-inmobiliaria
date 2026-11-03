# Plantilla NoExpertise · Inmobiliaria

Web clara para inmobiliarias, con fichas de pisos, filtro de venta y alquiler y valoración.

- Sectores: inmobiliaria, administracion-fincas. Estilo: profesional. Plan mínimo: presencia.
- Secciones: hero, inmuebles, servicios, zonas, valoracion, equipo, opiniones, preguntas, contacto (+ `texto` genérico).
- Campos propios: `hero.items[].url`, `inmuebles[]` (titulo, zona, precio, operacion, metros, habitaciones, banos, texto, imagen, destacado), `opiniones.nota`, `contacto.opciones`.
- SEO de base incluido (`src/lib/seo.ts`): canónica, og:image, JSON-LD del negocio y de las preguntas frecuentes, `robots.txt`, `sitemap.xml` y `llms.txt`; `noindex` si `seo.indexable: false`.
- Varias páginas (opcional, `paginas:` en la raíz de `site.yaml`): Inicio + una página por entrada (`{ ruta, titulo, secciones: "a; b" }`, hasta 6) con el menú hecho solo; reparto recomendado en `template.yaml → paginas`. Sin la clave, una sola página larga.
- Contenido editable en `site.yaml`; metadatos en `template.yaml`; reglas para el agente en `CLAUDE.md`; fotos a generar en `fotos.json`.

```
npm ci
npm run dev      # desarrollo
npm run build    # web estática en dist/
```

Basada en [AstroWind](https://github.com/onwidget/astrowind) © onWidget, licencia MIT (`LICENSE-ASTROWIND.md`). Imágenes de relleno (`public/placeholders/`) dibujadas para esta plantilla, sin licencias de terceros.

## Páginas propias
- Cada elemento de `secciones.inmuebles.inmuebles` con `detalle` tiene su página en `/inmuebles/<slug>/` (`src/pages/[coleccion]/[slug].astro`).
- Cada zona de `secciones.zonas.items` con `texto` tiene su página en `/zonas/<slug>/` (`src/pages/zonas/[slug].astro`); en la sección, `titulo_pagina` y `texto_pagina` con {zona} y {negocio}.
- Campos opcionales de los elementos con ficha: `detalle` (párrafos separados por una línea en blanco), `slug` e `imagenes` («/fotos/a.jpg; /fotos/b.jpg»).
