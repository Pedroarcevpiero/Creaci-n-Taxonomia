# Exalmar Taxonomy Council Harness

Harness agéntico (Claude Code) para construir, validar, versionar y
mantener una **taxonomía propia de Exalmar** de compras de materiales y
servicios. Produce una taxonomía MECE de 3-4 niveles, diccionario de
categorías, modelo de atributos, reglas de mapping a estándares externos,
reportes de gobernanza y candidatos a tarifario/contrato marco.

Todo corre con el LLM de Claude y herramientas nativas (Read, Write, Grep,
Glob). No usa APIs externas ni código ejecutable (Python/JS).

## Principio rector

```text
cuenta contable ≠ proveedor ≠ material ≠ categoría ≠ equipo
```

Estas dimensiones se mantienen siempre separadas y se conectan solo
mediante `schemas/mapping_rule.schema.json`. La taxonomía principal no
copia UNSPSC, eCl@ss, CPV, ISO 14224 ni ninguna otra taxonomía externa
(ver `docs/standards-mapping.md`).

## Cómo usarlo

```text
/taxonomy-council <instrucción>
```

Ver formas de invocación y modos de uso (piloto, 5 años, auditoría, mapeo
puntual, publicación de versión) en `docs/operating-manual.md` y
`.claude/skills/taxonomy-council/SKILL.md`.

Antes de correr el harness, coloca los extractos de SAP en `data/raw/`
(ver columnas esperadas en `examples/example-input-columns.md`). La
carpeta se entrega vacía: este repo no incluye datos de Exalmar.

## Estructura del repo

```text
CLAUDE.md                          reglas de operación del harness
.claude/skills/taxonomy-council/   Skill principal, rubrics, ejemplos
.claude/workflows/                 Dynamic Workflow (13 fases)
.claude/agents/                    15 agentes de council + 2 meta-agentes
schemas/                           8 contratos JSON Schema
docs/                              arquitectura, gobernanza, manual, plan 30/60/90
examples/                          ejemplos de input/output del harness
data/raw/                          (vacío) extractos SAP del usuario
outputs/                           taxonomía, mapping, reportes, validación, logs
```

## Documentación clave

- Plan aprobado: `docs/plan-harness-taxonomy-council.md`
- Arquitectura: `docs/architecture.md`
- Manual de operación (18 outputs, modos de uso): `docs/operating-manual.md`
- Gobernanza (Data Council, RACI, versionado): `docs/governance-model.md`
- Reglas de clasificación: `docs/classification-rules.md`
- Proceso de revisión humana: `docs/human-review-process.md`
- Mapeo a estándares externos: `docs/standards-mapping.md`
- Requerimientos de datos: `docs/data-requirements.md`
- Plan de implementación 30/60/90 días: `docs/implementation-plan-30-60-90.md`

## Agentes

15 agentes de council (Fases 1-13 del workflow, tiering de costo/beneficio
Haiku/Sonnet/Opus según criticidad de la decisión) más 2 meta-agentes:

- `plan-compliance-checklist-agent` — audita el harness contra el plan
  aprobado, item por item.
- `harness-test-verifier-agent` — verifica estructuralmente que Skill,
  Workflow, Agents y Schemas son consistentes entre sí.

Ambos se corren cada vez que se modifica la estructura del harness (no
durante la clasificación de compras); reportes en `outputs/validation/`.

## Reglas no negociables

- No clasificar masivamente sin revisión humana.
- `score_confianza` nunca es la única validación.
- No eliminar registros ambiguos; se marcan y van a `outputs/validation/`.
- `descripcion_original` nunca se sobrescribe.
- Toda publicación de versión requiere aprobación humana explícita.
- Los subagentes no lanzan otros subagentes.

Ver detalle completo en `CLAUDE.md`.
