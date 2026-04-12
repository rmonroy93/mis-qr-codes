# QR Generator — Contexto del proyecto

## ¿Qué es este proyecto?

App web estática (HTML + JS puro) que genera códigos QR y los sube automáticamente a un repositorio de GitHub via la GitHub API. Vive completamente en GitHub Pages — sin backend, sin servidor.

## URLs en producción

| Recurso | URL |
|---|---|
| App generadora | https://rmonroy93.github.io/mis-qr-codes/ |
| Galería de QRs | https://rmonroy93.github.io/mis-qr-codes/gallery.html |
| Repositorio | https://github.com/rmonroy93/mis-qr-codes |

## Configuración del repo

- **Usuario GitHub:** rmonroy93
- **Repo:** mis-qr-codes (público, requerido para GitHub Pages gratuito)
- **Branch:** main
- **Carpeta destino de QRs:** `qr-codes/`
- **Token localStorage key:** `gh_token_rmonroy93`

## Archivos del proyecto

```
/
├── index.html          # App principal (generador de QRs)
├── gallery.html        # Galería dinámica de QRs generados
├── qrcode.min.js       # Librería QR local (descargada de jsdelivr, NO CDN)
├── qr-codes/
│   ├── .gitkeep        # Mantiene la carpeta en git
│   └── *.png           # QRs generados por la app
└── CLAUDE.md           # Este archivo
```

> **Importante:** `qrcode.min.js` está incluido localmente porque GitHub Pages tenía problemas cargando la librería desde CDN externo.

## Versiones

| Versión | Cambios |
|---|---|
| v1.0.0 | App base: token, generador individual, panel de éxito |
| v1.1.0 | Validación de token contra GitHub API `/user`, badge de versión en header |
| v1.2.0 | Tab "Carga masiva": pega N ligas, extrae folio del final de la URL, sube todos con progreso en tiempo real |

## Arquitectura de la app (index.html)

### Pantallas / paneles
1. **panel-token** — Primera vez: ingresa y valida el token GitHub
2. **panel-main** — Panel principal con dos tabs:
   - **Tab individual:** URL + nombre manual → genera y sube 1 QR
   - **Tab carga masiva:** N ligas → extrae folio → sube todos en lote
3. **panel-success** — Confirmación con QR y link a GitHub (solo modo individual)

### Flujo de autenticación
- Token se valida contra `GET /user` de GitHub API al guardarlo
- Se almacena en `localStorage` con key `gh_token_rmonroy93`
- Nunca se sube al repositorio

### Lógica de carga masiva
- Extrae el último segmento del path de la URL (ej: `/770510338253` → `770510338253`)
- Si el segmento es solo dígitos → folio numérico (caso SIGED)
- Si no es puro dígito → limpia caracteres especiales y lo usa como nombre
- Sube secuencialmente con 400ms de pausa entre cada upload (respeta rate limit de GitHub API)
- Muestra estado por ítem: `pendiente → ↑ subiendo... → ✓ listo / ✗ error`

### Cómo se sube un QR a GitHub
1. Genera QR en canvas oculto (`#qr-hidden`) a 400px
2. Extrae base64 del canvas
3. `GET /repos/{owner}/{repo}/contents/{path}` — verifica si ya existe (obtiene SHA)
4. `PUT /repos/{owner}/{repo}/contents/{path}` — crea o actualiza el archivo

## gallery.html

- Usa GitHub API pública (sin token, repo es público) para listar `qr-codes/`
- Muestra imagen, raw URL y link a GitHub de cada QR
- Botón copiar por cada liga
- Buscador por nombre en tiempo real
- Sin autenticación requerida

## Caso de uso principal actual

El usuario trabaja con ligas de la forma:
```
https://siged.dgairgob.mx/{numero_folio}
```
Pega una lista de ligas, la app extrae el número de folio y lo usa como nombre del archivo PNG.

## Cómo hacer deploy de cambios

```bash
cd "C:\Users\ricar\OneDrive\Documentos\Claude\Creador de qr"
git add <archivos>
git commit -m "descripción"
git pull https://rmonroy93:{TOKEN}@github.com/rmonroy93/mis-qr-codes.git main --no-edit
git push https://rmonroy93:{TOKEN}@github.com/rmonroy93/mis-qr-codes.git main
```

GitHub Pages tarda ~1 minuto en reflejar los cambios.

## Convenciones

- Versión visible en el header (`v1.2.0`) y en comentario HTML (`<!-- APP_VERSION: 1.2.0 -->`)
- Subir versión menor en cambios de features, patch en fixes
- Paleta de colores: fondo `#0e0e0f`, acento `#c8f135` (verde lima), fuentes DM Mono + DM Sans
- Todo en español en la UI
