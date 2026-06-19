# `data/` — datos fuente

`data/raw/` es donde el harness espera encontrar los extractos de SAP
(OCs, SOLPED, HES, maestros, contratos, etc.) antes de correr
`/taxonomy-council`. Ver columnas esperadas por tipo de archivo en
`examples/example-input-columns.md` y el detalle de requerimiento de datos
en `docs/data-requirements.md`.

Esta carpeta se entrega **vacía** (`data/raw/.gitkeep`): el harness no
incluye ni genera datos sintéticos de Exalmar. `data-profiler-agent` se
adapta a lo que realmente exista en `data/raw/` cuando el usuario cargue
los extractos reales, y reporta como riesgo de calidad cualquier columna o
archivo esperado que falte.
