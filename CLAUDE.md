# Plantilla «Inmobiliaria» (NoExpertise)

Web estática Astro 5 + Tailwind 4, derivada de AstroWind (MIT, ver `LICENSE-ASTROWIND.md`). Estilo: **profesional**
(azul marino + dorado, sans del sistema, esquinas de 4 px, cabecera blanca fija con teléfono).
Todo en castellano de España, frases cortas, sin jerga.

## Dónde está cada cosa
- **Textos, contacto, menú, SEO y secciones**: `site.yaml` (raíz). Es la única fuente de contenido. `src/data/site.ts` lo carga y lo tipa; no pongas textos fijos en los componentes.
- **Colores**: `site.yaml → colores` (primario = marino, acento = dorado, fondo, texto). `src/layouts/Base.astro` los convierte en variables CSS (`--c-primario`…) y Tailwind los expone como `bg-primario`, `text-acento`, `bg-primario/10`… Los tonos derivados (`--c-acento-texto`, `--c-suave`, `--c-borde`) se calculan con `color-mix` en `src/styles/global.css`.
- **Tipografías y aspecto general**: `src/styles/global.css` (botones `.btn` marino, `.btn-oro` dorado, `.btn-sec` contorno; `.ficha`, `.icono-cuadro`, `.etiqueta-op`, `.filtro`; cabecera `.cabecera--inmo` y pie `.pie--inmo`). Fuentes del sistema: no añadas fuentes externas.
- **Cabecera**: `src/components/Header.astro`. Muestra siempre `contacto.telefono` y el botón `boton_cabecera` («Vender mi casa»).
- **Pie**: `src/components/Footer.astro`. Columnas de zonas y servicios (salen de esas secciones) y contacto.
- **Imágenes**: en `public/`. Fotos en `public/fotos/` (las genera el coordinador desde `fotos.json`); respaldo en `public/placeholders/`. En `site.yaml` se referencian con ruta absoluta (`/fotos/inmueble-1.jpg`). Si una foto falta, `Imagen.astro` muestra su SVG de respaldo. Las fotos de personas son siempre `/placeholders/persona.svg` salvo que el cliente aporte la suya.
- **Logo**: `site.yaml → logo` (ruta en `public/`); vacío = inicial + nombre en texto.
- **Pie «Web creada con NoExpertise»**: `site.yaml → marca_pie` (se oculta solo con `false`).

## Secciones
Cada sección es un componente que solo se pinta si `site.yaml → secciones.<id>.activa` es `true`. Se pintan en el orden en que aparecen en `site.yaml`.
- `hero` → `secciones/hero.astro` · foto a todo el ancho, texto a la izquierda y tarjeta de accesos.
- `inmuebles` → `secciones/inmuebles.astro` · fichas de inmuebles con filtro.
- `servicios` → `secciones/servicios.astro` · fichas con icono.
- `zonas` → `secciones/zonas.astro` · barrios y localidades + imagen.
- `valoracion` → `secciones/valoracion.astro` · banda marina con razones y botón.
- `equipo` → `secciones/equipo.astro` · fichas horizontales (`personas[]`).
- `opiniones` → `secciones/opiniones.astro` · `testimonios[]` (no se pinta si está vacío).
- `preguntas` → `secciones/preguntas.astro` (preguntas frecuentes en acordeón `details/summary`; sin preguntas no se pinta)
- `contacto` → `secciones/contacto.astro` · datos + formulario.
- `texto` → `secciones/texto.astro` · apartado genérico para secciones nuevas (`tipo: texto`).

## Campos propios de esta plantilla (además del esquema común)
- `hero.items[]`: accesos de la tarjeta sobre la foto: `{ titulo, texto, icono, url }` (`url` opcional; sin ella no es enlace). Sin items, no hay tarjeta.
- `inmuebles.inmuebles[]`: `{ titulo, zona, precio, operacion, metros, habitaciones, banos, texto, imagen, destacado }`.
  - `precio` siempre **cadena** (`"189.000 €"`, `"850 €/mes"`); vacío = no se muestra. Nunca inventes precios.
  - `operacion`: `"Venta"` o `"Alquiler"` (el filtro se crea con las operaciones que haya; con una sola, no hay filtro).
  - `metros`, `habitaciones`, `banos`: cadenas; si son solo número se añade «m²», «hab.», «baños». Vacíos = se ocultan.
  - `destacado: true` = borde dorado y estrella.
  - Lista vacía (`inmuebles: []`) = se muestra el `texto` y el botón `cta` en un recuadro, sin rejilla.
- `inmuebles.cta`: botón bajo la rejilla (p. ej. «Busco algo concreto» → `/#contacto`).
- `servicios.items[]`: `{ titulo, texto, icono }`.
- `zonas.items[]`: `{ titulo, texto }` (`titulo` = barrio o localidad; `texto` opcional). `zonas.imagen`: mapa o vista de la ciudad.
- `valoracion`: `titulo`, `texto`, `cta`, `imagen` (opcional), `items[]` (razones `{ titulo, texto, icono? }`).
- `opiniones.nota`: frase opcional bajo las opiniones (p. ej. «Opiniones de clientes publicadas en Google.»).
- `contacto.opciones`: cadena con valores separados por «;» para el desplegable «Me interesa». Vacía = títulos de `hero.items`.
- Iconos disponibles: ver `src/components/ui/Icono.astro` (casa, llave, etiqueta, lupa, documento, edificio, superficie, cama, bano, mapa…).

## Reglas para órdenes
1. **Activar o quitar una sección**: cambia `activa: true|false` en `site.yaml`. No borres componentes.
2. **Reordenar secciones**: mueve el bloque dentro de `secciones:` en `site.yaml`.
3. **Cambiar textos, teléfonos, horarios, fotos, inmuebles**: solo en `site.yaml`. Si un texto contiene comas dentro de `{ … }`, ponlo entre comillas.
4. **Listas planas**: ningún elemento de lista puede llevar otra lista dentro. Máximo 24 elementos por lista.
5. **Página nueva**: crea `src/pages/<slug>.astro` usando `import Base from '../layouts/Base.astro'` y `<Base titulo="…" descripcion="…">…</Base>`; enlázala en `site.yaml → menu`. Ejemplo: `src/pages/aviso-legal.astro`.
6. **Sección nueva**: mejor con `tipo:` (`tipo: texto`, `tipo: servicios`, `tipo: inmuebles`…) y un id propio en `site.yaml`. Si hace falta un diseño nuevo, crea `src/components/secciones/<id>.astro` copiando la estructura de una existente (`seccion(Astro.props.id)`, `Seccion`, `Titular`). `src/pages/index.astro` la recoge sola.
7. **No tocar** `src/layouts/Base.astro`, `src/pages/index.astro`, `tailwind.config.mjs` ni `src/styles/comun.css` salvo orden explícita de diseño. Los ids `hero` y `contacto` no se cambian. Tampoco `src/lib/seo.ts` ni `src/pages/robots.txt.ts`, `sitemap.xml.ts` y `llms.txt.ts`: son el SEO de base (canónica, og:image, `noindex` si `seo.indexable: false`, JSON-LD del negocio y de las preguntas, robots, sitemap y llms.txt), salen solos de `site.yaml` y el fichero es el mismo en las 16 plantillas. `seo.url` y `seo.indexable` los pone la plataforma: no los cambies.
8. Mantén: `lang="es-ES"`, imágenes con `Imagen.astro` (carga diferida), sin JavaScript nuevo salvo necesidad real (ya hay dos scripts mínimos: cierre del menú móvil y filtro de inmuebles), 16 px de margen lateral (`.contenedor`), sin scroll horizontal en móvil. Imágenes de contenido con `alt` sacado del contenido (`altHero`, `altFicha`, `altGaleria`, `altSeccion` de `src/lib/seo.ts`, o el `titulo`/`nombre` de la ficha); las decorativas, `alt=""` (Imagen.astro les pone `aria-hidden`).
9. El formulario no tiene servidor: abre el correo (`mailto`) con el email de `site.yaml`. Para envío real, cambia `action` en `src/components/ui/FormularioContacto.astro`.

## Construir
```
npm ci && npm run build      # resultado en dist/
npx astro preview            # ver dist/ en http://localhost:4321
```
Antes de dar una orden por buena: `npm run build` sin errores.
