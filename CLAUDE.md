# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Qué es esto

Sitio web y hosting del podcast *El Tontunario* (www.eltontunario.com). Es un sitio
Jekyll 3.9 servido por GitHub Pages. Su función principal es generar el feed RSS de
podcast (`feed.xml`) que consumen iVoox, Spotify, Apple Podcasts, etc. La landing
(`index.md`) es secundaria: solo lista los episodios.

## Comandos

```bash
npm install      # = bundle install (instala las gems de Jekyll)
npm run dev      # = bundle exec jekyll serve (servidor local con recarga)
```

Requisitos: Ruby con Bundler (`gem install bundler -v "~> 2.5"`), Node 16+. El
`Gemfile` fija gems extra de stdlib (csv, logger, webrick, base64, bigdecimal)
necesarias en Ruby 3.4+ para que Jekyll 3.x arranque.

## Publicar un episodio nuevo

Cada episodio son tres piezas que hay que añadir a la vez:

1. **Post** en `_posts/YYYY-MM-DD-NxNN.md` (ej. `2026-03-04-2x07.md`). Solo front
   matter, sin cuerpo. El feed se construye a partir de estos campos:
   ```yaml
   ---
   layout: post
   season: 2
   episode: 7
   title: El Tontunario 2x07 - <tema1, tema2 y tema3>
   description: En este episodio charlamos, entre otras cosas, sobre <...>
   mp3: /mp3/2x07.mp3
   duration: "00:33:19"   # HH:MM:SS
   length: 47970556       # tamaño EXACTO del mp3 en bytes
   ---
   ```
2. **Audio** en `mp3/2x07.mp3`. El directorio `mp3/` SÍ se versiona en git.
3. **Imagen** en `img/2x07.png` (usada como `page.image` en el post).

`length` debe ser el tamaño real del fichero mp3 en bytes (`stat -f%z mp3/X.mp3`);
si no coincide, algunos reproductores cortan la descarga. La fecha del nombre del
post determina la fecha de publicación.

## Detalles que importan

- **Posts futuros ocultos**: `_config.yml` tiene `future: false`. Un episodio con
  fecha futura no aparece en el build hasta que llega su día. Así se pueden dejar
  episodios preparados con antelación.
- **Rebuild diario**: `.github/workflows/refresh.yml` dispara un rebuild de GitHub
  Pages cada día a la 1:00 UTC vía API. Esto es lo que hace que los posts
  programados (fecha futura) se publiquen solos al llegar la fecha, sin push.
  Requiere el secret `USER_TOKEN`.
- **Convención de commits**: un commit por episodio, mensaje `NxNN` o `feat: NxNN`.
- **Material en bruto sin versionar**: `mp4/`, `projects/` (grabaciones .MP4, .psd)
  y `trash/` están en `.gitignore`. No los toques ni intentes commitearlos.

## Estructura

- `feed.xml` — plantilla Liquid del RSS de podcast (namespaces itunes/googleplay).
  Es el artefacto más crítico; recorre `site.posts` generando un `<item>` por
  episodio. Los datos del canal salen de `_config.yml`.
- `index.md` — landing; lista los episodios enlazando al mp3.
- `_layouts/` — `default.html` (HTML base, usa Bulma vía CDN + `{% seo %}`),
  `post.html` (hereda de default, muestra imagen y "Otros artículos").
- `_config.yml` — metadatos del podcast (título, autor, categoría, URLs de las
  plataformas). El bloque "do not modify" fija kramdown y `future: false`.
- `CNAME` — dominio custom de GitHub Pages.
