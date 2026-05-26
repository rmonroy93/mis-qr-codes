---
type: source
title: Generador de Códigos
date_ingested: 2026-05-26
source: generador-codigos.html
tags: folio, sello-digital, generador, códigos
---

## Resumen

Página `generador-codigos.html` (v1.0.0) que genera códigos compuestos por:
- **Folio**: 12 dígitos aleatorios
- **Sello digital**: 15 segmentos de 15 caracteres alfanuméricos (A-Z, a-z, 0-9) separados por guiones

## Funcionamiento

1. Usuario ingresa una cantidad (1-5000)
2. Al presionar "Generar", crea N líneas con formato `folio\t{sello digital}`
3. La salida se muestra en un textarea y está lista para copiar y pegar en Excel (tab-separada)
4. Botones: Generar, Copiar al portapapeles, Limpiar

## Detalles técnicos

- Usa `crypto.getRandomValues()` para números aleatorios criptográficamente seguros (no `Math.random()`)
- Sin dependencias externas
- Diseño oscuro consistente con el resto del proyecto
- Atajo: Enter en el input dispara la generación
- Máximo 5000 líneas por generación

## Conexiones

- Relacionado con: [[Proyecto QR Generator]]
