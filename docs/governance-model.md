# Modelo de gobernanza — Exalmar Taxonomy Council Harness

## Data Council

Cuerpo de decisión sobre la taxonomía. Compuesto idealmente por: líder de
Procurement/Category Management, representante de Flota, representante de
Harina y Aceite, representante de Congelado, representante de Finanzas/SAP, y
un data steward del harness. Aprueba: alta/baja/fusión/división de macro
categorías, cambios que afecten >10% de registros ya clasificados, y la
publicación de cada nueva versión.

## Category owners

Cada macro categoría y categoría aprobada debe tener un `owner_sugerido`
(persona o rol responsable de su estrategia de sourcing). Categorías con
`kraljic_riesgo=alto` no pueden quedar sin owner (regla auditada por
`governance-auditor-agent`).

## Data stewards

Responsables de mantener la calidad de los datos fuente (maestros SAP,
catálogo de centros/plantas/embarcaciones) que alimentan al harness. No
participan en decisiones de estructura de taxonomía, solo en calidad de dato.

## RACI (resumen)

| Actividad | Responsable | Aprueba | Consultado | Informado |
|---|---|---|---|---|
| Ejecutar el harness (piloto/lote) | Data steward / analista | — | Category owners afectados | Data Council |
| Diseñar/modificar estructura del árbol | `taxonomy-architect-agent` (propone) | Data Council | Category owners | Compradores |
| Resolver ambigüedades/no clasificables | Category owner | Data Council (si es estructural) | Data steward | — |
| Publicar nueva versión | `final-taxonomy-arbiter-agent` (consolida) | Data Council | Category owners | Toda la organización |
| Auditar gobernanza/MECE | `governance-auditor-agent` | — | Data Council | Category owners |

## Frecuencia de revisión

- Auditoría de gobernanza: cada vez que se publica una versión.
- Revisión completa de estabilidad y métricas de calidad: trimestral.
- Revalidación de la taxonomía semilla completa: anual o tras cambios
  operativos mayores (nueva planta, nueva flota, cambio de ERP).

## Versionado

Semver simple (`v0.0`, `v0.1`, `v1.0`...). Cada versión se registra en
`taxonomy_version_log.md` conforme a `schemas/taxonomy_version.schema.json`,
con `cambios` detallados, justificación y aprobador. Nunca se sobrescribe una
versión publicada. Toda versión permite rollback explícito.

## Proceso de alta/baja/modificación de categorías

1. `category-discovery-agent` o el equipo de negocio detecta necesidad.
2. `taxonomy-architect-agent` evalúa contra criterios de existencia
   (`rubrics/mece-quality.md`) y propone el cambio con impacto estimado.
3. Si el impacto estimado es >10% de registros, requiere aprobación explícita
   del Data Council antes de aplicarse.
4. `adversarial-taxonomy-reviewer` valida que el cambio no introduce
   solapamientos/huecos nuevos.
5. `final-taxonomy-arbiter-agent` consolida y versiona solo tras aprobación.

## Reglas para "no clasificable"

- Nunca se elimina un registro `no_clasificable`; se conserva con su
  descripción original intacta en `outputs/validation/unclassified_items.csv`.
- Si un patrón de `no_clasificable` se repite (umbral definido por el Data
  Council, sugerido >5% de una muestra), se escala a
  `category-discovery-agent` para evaluar nueva familia/categoría.
- La clase "no clasificable" es explícita desde la v0.0 (no es un estado
  transitorio oculto).

## Reglas de auditoría

- Toda publicación de versión requiere auditoría previa de
  `governance-auditor-agent` y `adversarial-taxonomy-reviewer` sin issues
  `critica` abiertos.
- Cambios al harness mismo (agents/skill/workflow/schemas) requieren correr
  `plan-compliance-checklist-agent` y `harness-test-verifier-agent`.

## Reglas para aprobar cambios masivos

Cualquier reclasificación masiva (>10% de registros, o cualquier
fusión/división de macro categoría) requiere: (1) justificación documentada,
(2) impacto estimado, (3) aprobación explícita del Data Council, (4) registro
en el log de versiones con posibilidad de rollback.

## Política de human-in-the-loop

Ver `docs/human-review-process.md`. Resumen: score de confianza bajo,
ambigüedad, "no clasificable", cambios estructurales y issues de severidad
alta/crítica siempre van a revisión humana; nunca se publica una versión sin
decisión humana explícita.

## Política de golden set

Se mantiene un golden set de registros pre-clasificados manualmente (por
categoría/unidad de negocio representativa), usado para medir precisión del
harness en cada versión (`taxonomy_quality_report.md`, métrica "precisión
contra golden set"). El golden set se actualiza cuando cambia la estructura
del árbol de forma que invalide etiquetas previas.

## Control de calidad

Combinación de: rúbricas de calidad (`rubrics/`), auditoría de gobernanza,
revisión adversarial, métricas cuantitativas (sección 14 del plan original) y
revisión humana. Ningún mecanismo es suficiente por sí solo (en particular,
`score_confianza` nunca es la única validación).
