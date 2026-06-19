# Manual de operación — Exalmar Taxonomy Council Harness

## Cómo invocar el harness

Forma corta:
```text
/taxonomy-council <instrucción>
```

Forma estructurada: ver `.claude/skills/taxonomy-council/SKILL.md`.

## Modos de uso

1. **Modo piloto** (recomendado primero): una unidad de negocio o período
   corto. Ver `.claude/skills/taxonomy-council/examples/piloto-una-unidad.md`.
2. **Clasificación de 5 años**: solo después de validar el piloto. Ver
   `.claude/skills/taxonomy-council/examples/clasificacion-5-anios.md`.
3. **Auditoría de taxonomía vigente**: ejecuta solo fases 10-11 sobre el árbol
   ya publicado.
4. **Mapeo externo puntual**: ejecuta solo fase 8 (`standards-mapper-agent`).
5. **Publicación de nueva versión**: ejecuta fase 13 una vez aprobado por el
   usuario.

## Lista completa de outputs (18)

Todos versionados con sufijo de fecha cuando corresponda; nunca se
sobrescriben outputs publicados.

| # | Archivo | Carpeta |
|---|---|---|
| 1 | `taxonomy_master.csv` | `outputs/taxonomy/` |
| 2 | `taxonomy_master.json` | `outputs/taxonomy/` |
| 3 | `macro_category_map.md` | `outputs/taxonomy/` |
| 4 | `category_dictionary.md` | `outputs/taxonomy/` |
| 5 | `category_attribute_model.csv` | `outputs/taxonomy/` |
| 6 | `mapping_rules.md` | `outputs/mapping/` |
| 7 | `mapping_5y_spend.csv` | `outputs/mapping/` |
| 8 | `ambiguous_categories.csv` | `outputs/validation/` |
| 9 | `unclassified_items.csv` | `outputs/validation/` |
| 10 | `cross_business_unit_categories.md` | `outputs/reports/` |
| 11 | `mro_transversal_report.md` | `outputs/reports/` |
| 12 | `supplier_market_report.md` | `outputs/reports/` |
| 13 | `tariff_candidates.csv` | `outputs/reports/` |
| 14 | `contract_framework_candidates.csv` | `outputs/reports/` |
| 15 | `category_governance_model.md` | `outputs/reports/` |
| 16 | `taxonomy_version_log.md` | `outputs/taxonomy/` |
| 17 | `taxonomy_quality_report.md` | `outputs/validation/` |
| 18 | `implementation_plan.md` | `outputs/reports/` |

Adicionalmente, en `outputs/logs/` se guardan los artefactos intermedios de
cada fase (perfiles, propuestas, vereditos adversariales) y en
`outputs/validation/` los reportes de los 2 meta-agentes
(`*_plan_compliance_checklist.md`, `*_harness_test_report.md`).

## Métricas de calidad (sección 14 del plan original)

Cobertura de clasificación por nivel (macro/categoría/subcategoría/familia),
tasa de no clasificable, tasa de ambigüedad, pureza de clusters, duplicidad de
proveedores/materiales, categorías sin owner, categorías sin atributos
mínimos, categorías no MECE, tasa de revisión humana, precisión contra golden
set, estabilidad entre versiones, gasto bajo gestión por categoría, número y
concentración de proveedores por categoría, potencial de contrato
marco/tarifario. Se reportan en `taxonomy_quality_report.md`.

## Operación de los meta-agentes

- Ejecutar `plan-compliance-checklist-agent` y `harness-test-verifier-agent`
  **cada vez que se modifique la estructura del harness** (agents, skill,
  workflow, schemas, docs), no como parte de la clasificación de compras.
- Sus reportes van a `outputs/validation/` y deben revisarse antes de dar por
  cerrado cualquier cambio estructural al harness.

## Convención de nombres de outputs versionados

```text
outputs/<carpeta>/YYYY-MM-DD_taxonomy-council_<slug>.<ext>
```

Para los 18 outputs "maestros" (tabla arriba) que se actualizan en cada
publicación de versión, se mantiene además una copia "vigente" sin fecha en su
ruta canónica, y el historial versionado vive en `taxonomy_version_log.md` con
referencia a las copias fechadas.
