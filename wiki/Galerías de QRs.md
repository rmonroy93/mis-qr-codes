---
type: source
title: Galerías de QRs
date_ingested: 2026-05-26
source: gallery.html, gallery-v2.html, CLAUDE.md
tags: galería, búsqueda, folios, github-api
---

## Resumen

El proyecto tiene dos galerías para visualizar los QRs almacenados en el repositorio. Ambas son HTML estático sin dependencias externas.

## gallery.html (galería completa)

- Usa GitHub API pública (sin token, repo público) para listar `qr-codes/`
- Muestra imagen, raw URL y link a GitHub de cada QR
- Botón copiar por cada liga
- Buscador por nombre en tiempo real
- Sin autenticación requerida

## gallery-v2.html (galería búsqueda por folios — v2.0.0)

- Sin carga inicial — solo busca cuando el usuario pega folios
- Textarea acepta folios separados por salto de línea, coma o espacio
- Verifica existencia via HEAD request a `raw.githubusercontent.com` (sin GitHub API, sin rate limit)
- Verificaciones en paralelo (Promise.all)
- Cards verdes = encontrado (imagen + raw URL + GitHub URL + botones copiar)
- Cards rojos = no encontrado
- Barra de resumen: X encontrados / Y no encontrados

## Diferencias clave

| Aspecto | gallery.html | gallery-v2.html |
|---|---|---|
| Carga inicial | Sí, todos los QRs | No |
| API usada | GitHub API | raw.githubusercontent.com |
| Rate limit | Sí (GitHub API) | No (HEAD directo) |
| Búsqueda | Por nombre en tiempo real | Por lista de folios exactos |
| Autenticación | No necesita | No necesita |

## Conexiones

- Relacionado con: [[Proyecto QR Generator]]
