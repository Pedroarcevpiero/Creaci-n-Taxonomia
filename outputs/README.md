# `outputs/` — convención de carpetas y nombres

Carpetas:

- `outputs/taxonomy/` — árbol de taxonomía, diccionario, modelo de
  atributos, log de versiones (outputs 1-5 y 16 de
  `docs/operating-manual.md`).
- `outputs/mapping/` — reglas de mapeo a estándares externos y tabla de
  mapping completa (outputs 6-7).
- `outputs/reports/` — reportes de gobernanza, transversalidad, mercado
  proveedor, tarifabilidad/contrato marco, plan de implementación
  (outputs 10-15, 18).
- `outputs/validation/` — registros ambiguos/no clasificables, reporte de
  calidad de taxonomía, y reportes de los 2 meta-agentes (outputs 8-9, 17).
- `outputs/logs/` — artefactos intermedios de cada fase del workflow
  (perfiles, propuestas, vereditos adversariales). No son outputs finales.

Cada carpeta empieza vacía (`.gitkeep`) hasta la primera corrida del
harness.

## Convención de nombres

```text
outputs/<carpeta>/YYYY-MM-DD_taxonomy-council_<slug>.<ext>
```

Los 18 outputs "maestros" listados en `docs/operating-manual.md` mantienen
además una copia vigente sin fecha en su ruta canónica (ej.
`outputs/taxonomy/taxonomy_master.csv`); el historial de cambios queda en
`taxonomy_version_log.md`, nunca se sobrescribe una versión publicada sin
dejar rastro.

Ver lista completa de los 18 outputs y su carpeta en
`docs/operating-manual.md`.
