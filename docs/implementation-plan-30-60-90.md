# Plan de implementación 30/60/90 — Exalmar Taxonomy Council Harness

> Plan operativo para el despliegue del harness sobre data real de Exalmar,
> tomado de la sección 17 del plan original aprobado
> (`docs/plan-harness-taxonomy-council.md`).

## Días 0-30: Datos y diseño base

**Objetivos**: reunir 5 años de data, construir spend cube real, validar las
16 macro categorías semilla, definir Data Council/owners/RACI, crear clase
"no clasificable", crear taxonomía v0.0.

**Actividades**:
1. Extraer de SAP: OCs, posiciones de OC, SOLPED, HES, maestros de materiales
   y servicios, proveedores, equipos, functional locations, contratos,
   cotizaciones, cuentas, centros, plantas, embarcaciones, unidades de
   medida, solicitantes (ver `docs/data-requirements.md`).
2. Ejecutar el harness en **modo piloto** sobre una unidad de negocio
   (`examples/piloto-una-unidad.md`) para construir el spend cube inicial.
3. Validar la taxonomía semilla de 16 macro categorías con
   `taxonomy-architect-agent` + revisión del Data Council.
4. Definir reglas de fusión/división de macro categorías
   (`docs/governance-model.md`).
5. Definir gobernanza: Data Council, owners, RACI.
6. Definir criterios de "no clasificable" (`docs/human-review-process.md`).
7. Definir golden set inicial (`docs/governance-model.md`).

**Entregables**: `macro_category_map.md`, `category_governance_model.md`,
reporte de calidad de datos (derivado de `data-profiler-agent`).

## Días 31-60: Inducción semiautomática y diccionario

**Objetivos**: ejecutar limpieza y normalización, aplicar descubrimiento y
clasificación semiautomática, realizar talleres con Flota, Harina/Aceite,
Congelado y Compras, construir golden set, generar `taxonomy_master` v0.1.

**Actividades**:
1. Limpieza y normalización (`description-normalizer-agent`,
   `material-service-split-agent`).
2. Descubrimiento de familias (`category-discovery-agent`).
3. Clasificación semiautomática con revisión humana en los umbrales definidos
   (`rubrics/classification-confidence.md`).
4. Talleres por unidad de negocio para validar categorías y atributos.
5. Construcción del diccionario de categorías (`category_dictionary.md`).
6. Construcción del golden set.

**Entregables**: `taxonomy_master.csv`, `category_dictionary.md`,
`category_attribute_model.csv`, `cross_business_unit_categories.md`,
`mro_transversal_report.md`.

## Días 61-90: Mapping, calidad y harness en marcha

**Objetivos**: mapear a UNSPSC/eCl@ss/IMPA/SAP PM, crear tabla
categoría↔equipo↔cuenta↔proveedor, medir calidad de clasificación, crear cola
de ambiguos/no clasificables, activar el harness como copiloto.

**Actividades**:
1. Mapeo externo (`standards-mapper-agent`, ver `docs/standards-mapping.md`).
2. Construcción de la tabla de mapping completa
   (`schemas/mapping_rule.schema.json`).
3. Medición de métricas de calidad (`docs/operating-manual.md`,
   `taxonomy_quality_report.md`).
4. Puesta en marcha del harness como copiloto de clasificación continua.

**Entregables**: `mapping_rules.md`, `ambiguous_categories.csv`,
`unclassified_items.csv`, `taxonomy_quality_report.md`,
`implementation_plan.md`.

## Criterio de éxito (sección 21 del plan original)

El harness está bien implementado si permite: crear una taxonomía propia de
Exalmar; clasificar materiales y servicios; separar compras, equipos,
proveedores y cuentas; mapear a estándares externos; detectar categorías
transversales; identificar candidatos a tarifario y contrato marco; construir
spend cube; generar outputs estructurados; mantener revisión humana;
versionar la taxonomía; y alimentar futuros harnesses de tail spend,
inventarios, tarifarios, compras, contratos marco y supplier management.
