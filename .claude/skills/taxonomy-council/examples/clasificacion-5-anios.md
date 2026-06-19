# Ejemplo: clasificación de 5 años de compras

## Precondición
El modo piloto (`piloto-una-unidad.md`) ya fue validado por el usuario y la
taxonomía base (v0.0 o v0.1) está aprobada como punto de partida.

## Comando de ejemplo
```text
/taxonomy-council
Crea una taxonomía de compras para Exalmar usando los archivos en data/raw/.
Usa 5 años de compras, maestros SAP, proveedores, equipos y contratos. Genera
taxonomy_master, category_dictionary, mapping_rules y governance_report. Marca
registros ambiguos y no clasificables para revisión humana.
```

## Cómo procede el orquestador
1. **Advertencia previa de costo/tiempo**: antes de iniciar, informa al
   usuario el volumen estimado de registros y el número de lotes propuesto.
   Espera confirmación si el volumen es grande (regla de seguridad:
   "preguntar antes de procesos largos o costosos").
2. **Procesamiento por lotes**: divide por año y/o unidad de negocio para
   mantener trazabilidad. Cada lote pasa por las fases 1-10 del workflow.
3. **Checkpoint de revisión humana por lote**: antes de consolidar, cada lote
   pasa por `governance-auditor-agent` y se generan
   `ambiguous_categories.csv` / `unclassified_items.csv` parciales.
4. **Consolidación**: solo después de que todos los lotes estén auditados,
   `final-taxonomy-arbiter-agent` consolida en `taxonomy_master.csv/.json` y
   ejecuta `adversarial-taxonomy-reviewer` sobre el árbol completo.
5. **Publicación**: se registra la versión en `taxonomy_version_log.md` y se
   solicita aprobación humana final antes de marcarla `publicada`.

## Outputs esperados al final
`taxonomy_master.csv`, `taxonomy_master.json`, `category_dictionary.md`,
`mapping_rules.md`, `mapping_5y_spend.csv`, `ambiguous_categories.csv`,
`unclassified_items.csv`, `cross_business_unit_categories.md`,
`mro_transversal_report.md`, `supplier_market_report.md`,
`tariff_candidates.csv`, `contract_framework_candidates.csv`,
`category_governance_model.md`, `taxonomy_version_log.md`,
`taxonomy_quality_report.md`, `implementation_plan.md`.

## Buenas prácticas
- No intentar procesar los 5 años en una sola corrida sin checkpoints: dificulta
  la auditoría y la trazabilidad de errores.
- Si la tasa de `no_clasificable` de un lote supera el umbral acordado,
  pausar y escalar a `category-discovery-agent` antes de seguir con el
  siguiente lote.
