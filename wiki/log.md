# Log — QR Generator

## 2026-05-26 setup | Inicialización de la wiki
Se creó la estructura de la wiki y las primeras páginas a partir del CLAUDE.md y los archivos del proyecto. Sistema de memoria también inicializado.
Páginas creadas: [[Proyecto QR Generator]], [[Arquitectura index.html]], [[Galerías de QRs]].

## 2026-05-26 feat | generador-codigos.html
Nueva página `generador-codigos.html` (v1.0.0). Genera N códigos con folio aleatorio de 12 dígitos + sello digital de 15 segmentos × 15 caracteres, separados por tabulador, listo para copiar a Excel.
Páginas creadas: [[Generador de Códigos]].

## 2026-05-26 fix | Unicidad en generador-codigos.html
Se agregó validación de unicidad (Set) para garantizar que ningún código se repita dentro del mismo lote. v1.0.0 → v1.1.0.

## 2026-05-26 feat | menu.html
Nueva página `menu.html` (v1.0.0) con cards de navegación a todas las herramientas del proyecto.
Páginas creadas: [[Menú de Herramientas]].

## 2026-05-26 fix | Folios 12 dígitos exactos
Corregido `randomFolio()`: primera posición 1-9 en lugar de 0-9 para evitar que Excel elimine ceros a la izquierda. v1.1.0 → v1.1.1.

## 2026-05-26 eos | Fin de sesión
Estado actual del proyecto:
- `index.html` v1.2.0 — QR generator
- `gallery.html` v1.0.0 — Galería de QRs
- `gallery-v2.html` v2.0.0 — Búsqueda por folios
- `generador-codigos.html` v1.1.1 — Generador folios + sellos digitales
- `menu.html` v1.0.0 — Menú de navegación
- Wiki inicializada con llm-wiki
- Memoria persistente activa
