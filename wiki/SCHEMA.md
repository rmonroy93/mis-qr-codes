# SCHEMA — Wiki del Proyecto QR Generator

## Estructura de directorios

```
wiki/
├── raw/         ← fuentes originales inmutables
├── wiki/        ← páginas markdown generadas
│   ├── index.md ← catálogo de todas las páginas
│   └── log.md   ← registro cronológico append-only
└── SCHEMA.md    ← este archivo
```

## Convenciones

- **Idioma:** español (como el proyecto)
- **Títulos:** descriptivos, en sustantivos
- **Enlaces internos:** con `[[Nombre exacto de página]]`
- **Frontmatter YAML:** siempre incluir `type`, `title`, `date_ingested`, `tags`
- **Tipos de página:** `source` (fuente), `concept` (concepto), `entity` (persona/organización), `synthesis` (síntesis), `query` (respuesta archivada)
- **Raw es inmutable:** nunca modificar archivos en `raw/`
