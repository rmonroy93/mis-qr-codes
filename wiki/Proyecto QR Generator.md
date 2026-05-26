---
type: source
title: Proyecto QR Generator
date_ingested: 2026-05-26
source: CLAUDE.md
tags: proyecto, qr, github-pages, visión-general
---

## Resumen

App web estática (HTML + JS puro) que genera códigos QR y los sube automáticamente a un repositorio de GitHub via la GitHub API. Vive completamente en GitHub Pages — sin backend, sin servidor.

## URLs en producción

| Recurso | URL |
|---|---|
| App generadora | https://rmonroy93.github.io/mis-qr-codes/ |
| Galería completa | https://rmonroy93.github.io/mis-qr-codes/gallery.html |
| Galería búsqueda | https://rmonroy93.github.io/mis-qr-codes/gallery-v2.html |
| Repositorio | https://github.com/rmonroy93/mis-qr-codes |

## Configuración del repo

- **Usuario GitHub:** rmonroy93
- **Repo:** mis-qr-codes (público)
- **Branch:** main
- **Carpeta destino de QRs:** `qr-codes/`
- **Token localStorage key:** `gh_token_rmonroy93`

## Despliegue

GitHub Pages. Los cambios tardan ~1 minuto en reflejarse. El deploy se hace via git push directo a main.

## Caso de uso principal

El usuario trabaja con ligas de SIGED: `https://siged.dgairgob.mx/{numero_folio}`. Pega una lista, la app extrae el folio como nombre del PNG.

## Versiones

| Versión | Archivo | Cambios |
|---|---|---|
| v1.0.0 | index.html | App base |
| v1.1.0 | index.html | Validación de token, badge de versión |
| v1.2.0 | index.html | Carga masiva con progreso |
| v2.0.0 | gallery-v2.html | Galería búsqueda por folios |

## Convenciones del proyecto

- Versión visible en header + comentario HTML
- Paleta: fondo `#0e0e0f`, acento `#c8f135`, fuentes DM Mono + DM Sans
- Todo en español en la UI

## Conexiones

- Relacionado con: [[Arquitectura index.html]]
- Relacionado con: [[Galerías de QRs]]
