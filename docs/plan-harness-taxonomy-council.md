# Plan de implementación — Exalmar Taxonomy Council Harness

> Plan aprobado por el usuario. Base de referencia: convenciones del
> `Research_council_harness`. Todo corre con el LLM nativo de Claude y
> herramientas nativas (Read/Write/Grep/Glob), **sin APIs externas ni código
> ejecutable**. Outputs versionados, revisión humana obligatoria, los subagents
> no lanzan otros subagents.

## 0. Contexto

El harness construye, valida, versiona y mantiene una **taxonomía propia de
Exalmar** de compras de materiales y servicios, base para spend analysis, spend
cube, tail spend, tarifarios, contratos marco, benchmarking, gestión de
proveedores, inventarios, MRO y clasificación automática con human-in-the-loop.

Principio rector: `cuenta contable ≠ proveedor ≠ material ≠ categoría ≠ equipo`.
Estándares externos (UNSPSC, eCl@ss, ISO 14224/SAP PM, IMPA, SANIPES/HACCP) son
capas de apoyo, nunca la base.

## 1. Árbol de archivos

```
README.md
CLAUDE.md
docs/
  plan-harness-taxonomy-council.md
  architecture.md
  operating-manual.md
  governance-model.md
  data-requirements.md
  classification-rules.md
  standards-mapping.md
  human-review-process.md
  implementation-plan-30-60-90.md
examples/
  example-input-columns.md
  example-taxonomy-output.md
  example-category-dictionary.md
schemas/
  taxonomy_record.schema.json
  taxonomy_node.schema.json
  category_dictionary.schema.json
  category_attribute.schema.json
  mapping_rule.schema.json
  classification_output.schema.json
  governance_issue.schema.json
  taxonomy_version.schema.json
.claude/skills/taxonomy-council/
  SKILL.md
  README.md
  rubrics/{mece-quality,transversality-kraljic,classification-confidence,tariffability}.md
  examples/{piloto-una-unidad,clasificacion-5-anios}.md
.claude/workflows/taxonomy-council.md
.claude/agents/  (15 del consejo + 2 meta)
outputs/{taxonomy,mapping,reports,validation,logs}/  (+ README.md)
data/raw/  (+ README.md, sin datos sintéticos)
```

## 2. Estrategia de modelos por agente (costo/beneficio)

- **Haiku** (mecánico/barato): data-profiler, description-normalizer,
  material-service-split, business-unit, equipment-mapper.
- **Sonnet** (razonamiento medio): category-discovery, supplier-market,
  attribute-modeler, standards-mapper, transversality, tariffability,
  governance-auditor, plan-compliance-checklist, harness-test-verifier.
- **Opus** (decisión crítica): taxonomy-architect, adversarial-taxonomy-reviewer,
  final-taxonomy-arbiter.

## 3. Schemas (8)

Cubren el modelo de datos de la sección 7 del plan original (`taxonomy_record`),
el árbol (`taxonomy_node`), diccionario, atributos, reglas de mapping, salida de
clasificación, issues de gobernanza y versión. JSON Schema draft-07, validables,
`additionalProperties:false`, enums en español.

## 4. Meta-agentes

- **plan-compliance-checklist-agent**: checklist derivado ítem por ítem de las 22
  secciones del plan original. Emite JSON `cumple/parcial/no_cumple` + evidencia.
- **harness-test-verifier-agent**: prueba de humo sin datos (schemas parseables,
  archivos presentes, referencias cruzadas consistentes, campos del modelo de
  datos alineados, 18 outputs y 13 fases documentados).

## 5. Reglas clave embebidas

Separación de dimensiones; UNSPSC solo mapping; eCl@ss solo atributos; ISO
14224/SAP PM equipos en paralelo; MECE 3-4 niveles; transversalidad + Kraljic
como atributo; nunca clasificar masivo sin revisión humana; conservar descripción
original; versionar/rollback; separar hechos/inferencias/recomendaciones.

## 6. Flujo de ejecución de la construcción

1. Guardar este plan.
2. Crear schemas → CLAUDE.md → Skill (+rubrics +examples) → workflow → 15 agentes
   → 2 meta-agentes → docs → examples → outputs/data → README.
3. Autoverificar con los 2 meta-agentes antes del commit.
4. Commit en `claude/plan-review-pykomi`, push, PR draft.

## 7. Decisiones del usuario

- Sin dataset sintético: el verificador valida estructura, no clasificación real.
- Agentes solo-LLM, sin código ejecutable.
- Todo en una sola entrega (un PR draft).
