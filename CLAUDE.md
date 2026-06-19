# CLAUDE.md — Exalmar Taxonomy Council Harness

## Propósito
Harness agéntico reutilizable para construir, validar, versionar y mantener una
**taxonomía propia de Exalmar** de compras de materiales y servicios. Produce una
taxonomía MECE de 3-4 niveles, diccionario de categorías, modelo de atributos,
reglas de mapping a estándares externos, reportes de gobernanza y candidatos a
tarifario/contrato marco.

Todo corre con el LLM de Claude y herramientas nativas del entorno (Read, Write,
Grep, Glob). **No usar APIs externas. No crear código ejecutable** (Python/JS) a
menos que el usuario lo apruebe explícitamente.

## Principio rector
```text
cuenta contable ≠ proveedor ≠ material ≠ categoría ≠ equipo
```
Estas dimensiones se mantienen siempre separadas y se conectan solo mediante
`mapping_rule.schema.json`. La taxonomía principal **no copia** UNSPSC, eCl@ss,
CPV, ISO 14224 ni ninguna otra taxonomía externa.

| Estándar | Uso permitido |
|---|---|
| UNSPSC | Mapping externo / benchmark, nunca taxonomía base |
| eCl@ss | Diccionario de atributos técnicos, nunca taxonomía base |
| ISO 14224 / SAP PM | Taxonomía de equipos **en paralelo**, nunca fusionada |
| IMPA Marine Stores Guide | Referencia de flota/naval |
| HACCP / SANIPES | Atributos de compliance alimentario |
| CPV / NSN / NIGP | Evaluar; no usar como base |

## Cuándo usar /taxonomy-council
- Construir o actualizar la taxonomía de compras de Exalmar desde data SAP
  (OCs, SOLPED, HES, maestros de materiales/servicios, proveedores, equipos,
  contratos, cotizaciones, functional locations).
- Clasificar lotes históricos (hasta 5 años) de compras.
- Detectar categorías transversales, candidatos a tarifario o contrato marco.
- Auditar la taxonomía vigente (MECE, owners, atributos, estabilidad).
- Versionar y publicar una nueva versión de la taxonomía.

Invocación: `/taxonomy-council <instrucción>` (ver
`.claude/skills/taxonomy-council/SKILL.md` para formas corta y estructurada).

## Reglas de clasificación
- No usar `cuenta_contable` como categoría.
- No usar `proveedor` como categoría.
- No usar UNSPSC como taxonomía principal.
- No mezclar taxonomía de compras con taxonomía de equipos.
- "Estratégico" no es macro categoría; es atributo Kraljic (valor × riesgo).
- Una macro categoría debe tener volumen económico relevante, mercado proveedor
  propio, estrategia de sourcing distinta o riesgos propios.
- Una subcategoría debe habilitar comparar precios, agrupar proveedores, crear
  tarifario o diferenciar criticidad/tecnología/unidad de negocio.

## Reglas de seguridad y calidad
- No clasificar masivamente sin revisión humana.
- No usar `score_confianza` como única validación.
- No eliminar registros ambiguos; marcarlos y enviarlos a `outputs/validation/`.
- Siempre conservar `descripcion_original` sin modificar.
- Siempre generar trazabilidad de cambios (`taxonomy_version_log.md`).
- Siempre permitir rollback de versión.
- Siempre marcar supuestos explícitamente.
- Siempre separar hechos, inferencias y recomendaciones.
- Siempre generar lista de pendientes para validación humana.
- Subagents **no lanzan otros subagents**: la orquestación vive en la Skill o en
  el Dynamic Workflow (agente principal).
- No sobrescribir outputs existentes sin versionar (usar sufijo de fecha/versión).

## Outputs
Carpetas: `outputs/taxonomy/`, `outputs/mapping/`, `outputs/reports/`,
`outputs/validation/`, `outputs/logs/`. Ver convención de nombres en
`outputs/README.md`. Lista completa de entregables en
`docs/operating-manual.md`.

## Referencia
- Skill principal: `.claude/skills/taxonomy-council/SKILL.md`
- Agentes: `.claude/agents/`
- Workflow: `.claude/workflows/taxonomy-council.md`
- Schemas: `schemas/`
- Arquitectura: `docs/architecture.md`
- Manual de operación: `docs/operating-manual.md`
- Gobernanza: `docs/governance-model.md`
- Plan de implementación 30/60/90: `docs/implementation-plan-30-60-90.md`
- Plan original aprobado: `docs/plan-harness-taxonomy-council.md`
