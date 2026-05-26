# Log — QR Generator

## 2026-05-26 setup | Inicialización de la wiki
Se creó la estructura de la wiki y las primeras páginas a partir del CLAUDE.md y los archivos del proyecto. Sistema de memoria también inicializado.
Páginas creadas: [[Proyecto QR Generator]], [[Arquitectura index.html]], [[Galerías de QRs]].

## 2026-05-26 feat | generador-codigos.html
Nueva página `generador-codigos.html` (v1.0.0). Genera N códigos con folio aleatorio de 12 dígitos + sello digital de 15 segmentos × 15 caracteres, separados por tabulador, listo para copiar a Excel.
Páginas creadas: [[Generador de Códigos]].

## 2026-05-26 fix | Unicidad en generador-codigos.html
Se agregó validación de unicidad (Set) para garantizar que ningún código se repita dentro del mismo lote. v1.0.0 → v1.1.0.
