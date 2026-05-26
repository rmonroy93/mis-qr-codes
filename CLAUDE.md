# QR Generator — Contexto del proyecto

## ¿Qué es este proyecto?

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
- **Repo:** mis-qr-codes (público, requerido para GitHub Pages gratuito)
- **Branch:** main
- **Carpeta destino de QRs:** `qr-codes/`
- **Token localStorage key:** `gh_token_rmonroy93`

## Archivos del proyecto

```
/
├── index.html          # App principal (generador de QRs)
├── gallery.html        # Galería dinámica — lista todos los QRs del repo
├── gallery-v2.html     # Galería búsqueda — filtra por lista de folios pegada
├── generador-codigos.html  # Generador de folios + sellos digitales (v1.1.0)
├── qrcode.min.js       # Librería QR local (descargada de jsdelivr, NO CDN)
├── qr-codes/
│   ├── .gitkeep        # Mantiene la carpeta en git
│   └── *.png           # QRs generados por la app
└── CLAUDE.md           # Este archivo
```

> **Importante:** `qrcode.min.js` está incluido localmente porque GitHub Pages tenía problemas cargando la librería desde CDN externo.

## Versiones

| Versión | Archivo | Cambios |
|---|---|---|
| v1.0.0 | index.html | App base: token, generador individual, panel de éxito |
| v1.1.0 | index.html | Validación de token contra GitHub API `/user`, badge de versión en header |
| v1.2.0 | index.html | Tab "Carga masiva": pega N ligas, extrae folio del final de la URL, sube todos con progreso en tiempo real |
| v2.0.0 | gallery-v2.html | Galería búsqueda por folios: textarea con lista, verifica existencia en paralelo, cards encontrados/no encontrados |
| v1.0.0 | generador-codigos.html | Generador de folios (12 dígitos) + sellos digitales (15×15 caracteres), salida tabulada para Excel |
| v1.1.0 | generador-codigos.html | Validación de unicidad: nunca genera códigos repetidos en un mismo lote |

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

## gallery-v2.html

- Sin carga inicial — solo busca cuando el usuario pega folios
- Textarea acepta folios separados por salto de línea, coma o espacio
- Verifica existencia via HEAD request a `raw.githubusercontent.com` (sin GitHub API, sin rate limit)
- Verificaciones en paralelo (Promise.all) — más rápido que secuencial
- Cards verdes = encontrado (imagen + raw URL + GitHub URL + botones copiar)
- Cards rojos = no encontrado
- Barra de resumen: X encontrados / Y no encontrados
- Sin botones de navegación (diseño limpio y enfocado)

## Servidor local (Windows, sin Python)

```bash
node -e "const h=require('http'),fs=require('fs'),path=require('path');h.createServer((req,res)=>{let f=path.join('C:/Users/ricar/OneDrive/Documentos/Claude/Creador de qr',req.url==='/'?'index.html':req.url);try{const d=fs.readFileSync(f);const ext=path.extname(f);const t={'html':'text/html','js':'application/javascript','png':'image/png'}[ext.slice(1)]||'text/plain';res.writeHead(200,{'Content-Type':t});res.end(d);}catch(e){res.writeHead(404);res.end('404');}}).listen(8080,()=>console.log('OK'));"
```
Acceso: http://localhost:8080/

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
git pull https://rmonroy93:TOKEN@github.com/rmonroy93/mis-qr-codes.git main --no-edit
git push https://rmonroy93:TOKEN@github.com/rmonroy93/mis-qr-codes.git main
```

> **Nota:** El token está en `.claude/settings.local.json` como `GITHUB_TOKEN`, pero la interpolación `${GITHUB_TOKEN}` no funciona en URLs de git en este entorno. Usar el valor directamente en el comando.

GitHub Pages tarda ~1 minuto en reflejar los cambios.

## Convenciones

- Versión visible en el header (`v1.2.0`) y en comentario HTML (`<!-- APP_VERSION: 1.2.0 -->`)
- Subir versión menor en cambios de features, patch en fixes
- Paleta de colores: fondo `#0e0e0f`, acento `#c8f135` (verde lima), fuentes DM Mono + DM Sans
- Todo en español en la UI
