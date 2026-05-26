---
type: source
title: Arquitectura index.html
date_ingested: 2026-05-26
source: index.html, CLAUDE.md
tags: arquitectura, index, flujo, autenticación, carga-masiva
---

## Resumen

La app principal `index.html` (v1.2.0) está organizada en tres pantallas/paneles intercambiables. Usa JavaScript vanilla sin frameworks. La librería `qrcode.min.js` está incluida localmente para evitar problemas con CDN en GitHub Pages.

## Pantallas / paneles

1. **panel-token** — Primera vez: ingresa y valida el token GitHub
2. **panel-main** — Panel principal con dos tabs:
   - **Tab individual:** URL + nombre manual → genera y sube 1 QR
   - **Tab carga masiva:** N ligas → extrae folio → sube todos en lote
3. **panel-success** — Confirmación con QR y link a GitHub (solo modo individual)

## Flujo de autenticación

- Token se valida contra `GET /user` de GitHub API al guardarlo
- Se almacena en `localStorage` con key `gh_token_rmonroy93`
- Nunca se sube al repositorio

## Lógica de carga masiva

- Extrae el último segmento del path de la URL (ej: `/770510338253` → `770510338253`)
- Si el segmento es solo dígitos → folio numérico (caso SIGED)
- Si no es puro dígito → limpia caracteres especiales y lo usa como nombre
- Sube secuencialmente con 400ms de pausa entre cada upload
- Muestra estado por ítem: `pendiente → ↑ subiendo... → ✓ listo / ✗ error`

## Proceso de subida a GitHub

1. Genera QR en canvas oculto (`#qr-hidden`) a 400px
2. Extrae base64 del canvas
3. `GET /repos/{owner}/{repo}/contents/{path}` — verifica si ya existe (obtiene SHA)
4. `PUT /repos/{owner}/{repo}/contents/{path}` — crea o actualiza el archivo

## Conexiones

- Relacionado con: [[Proyecto QR Generator]]
- Relacionado con: [[Galerías de QRs]]
