# Arquitectura — Exalmar Taxonomy Council Harness

## Visión general

El harness es un sistema agéntico de 3 capas que opera enteramente con el LLM
de Claude y herramientas nativas (Read, Write, Grep, Glob). No usa APIs
externas ni código ejecutable.

```text
┌─────────────────────────────────────────────────────────────┐
│  Capa 1: Orquestación                                       │
│  Skill /taxonomy-council  ←→  Dynamic Workflow taxonomy-council.md │
└─────────────────────────────────────────────────────────────┘
                          │ lanza con Agent
                          ▼
┌─────────────────────────────────────────────────────────────┐
│  Capa 2: Consejo de subagents (15, no se lanzan entre sí)    │
│  Haiku: profiling, normalización, split, unidad negocio,     │
│         mapeo equipo                                         │
│  Sonnet: descubrimiento, mercado proveedor, atributos,       │
│          estándares, transversalidad, tarifabilidad,         │
│          auditoría gobernanza                                │
│  Opus: arquitecto, revisor adversarial, árbitro final         │
└─────────────────────────────────────────────────────────────┘
                          │ produce/consume
                          ▼
┌─────────────────────────────────────────────────────────────┐
│  Capa 3: Contratos de datos (schemas/) + outputs versionados │
└─────────────────────────────────────────────────────────────┘
```

## Capa de meta-gobernanza del harness mismo

Separada del consejo de clasificación, existen 2 meta-agentes que auditan el
harness como sistema (no la taxonomía de negocio):

- `plan-compliance-checklist-agent`: verifica que el repositorio cumple
  `docs/plan-harness-taxonomy-council.md` ítem por ítem.
- `harness-test-verifier-agent`: prueba de humo estructural (schemas válidos,
  referencias cruzadas consistentes, sin datos sintéticos).

## Principio rector de separación de dimensiones

```text
cuenta contable ≠ proveedor ≠ material ≠ categoría ≠ equipo
```

Estas 5 dimensiones nunca colapsan en una sola estructura. Se conectan
exclusivamente mediante `schemas/mapping_rule.schema.json`. La taxonomía de
equipos (ISO 14224/SAP PM) vive en paralelo a la taxonomía de compras y solo
se vincula vía `equipo_relacionado` / `functional_location` como mapping.

## Flujo de datos end-to-end

1. **Intake** (`data/raw/`) → `data-profiler-agent` perfila.
2. **Normalización** → `description-normalizer-agent`,
   `material-service-split-agent`, `business-unit-agent`,
   `equipment-mapper-agent` enriquecen cada registro sin perder el original.
3. **Descubrimiento y diseño** → `category-discovery-agent` propone,
   `taxonomy-architect-agent` consolida el árbol (`schemas/taxonomy_node.schema.json`).
4. **Enriquecimiento** → `transversality-agent`, `standards-mapper-agent`,
   `attribute-modeler-agent`, `supplier-market-agent`, `tariffability-agent`
   añaden las capas de apoyo y evaluación.
5. **Control de calidad** → `governance-auditor-agent` (reglas formales) y
   `adversarial-taxonomy-reviewer` (escepticismo estructurado) iteran hasta
   máx. 3 ciclos.
6. **Human-in-the-loop** → el orquestador presenta resumen y espera decisión.
7. **Publicación** → `final-taxonomy-arbiter-agent` consolida, versiona y
   habilita rollback.

## Por qué Skill + Dynamic Workflow (y no solo uno)

- La **Skill** (`SKILL.md`) es el punto de entrada (`/taxonomy-council`),
  contiene las reglas de negocio, rúbricas y árbol de decisión de modos.
- El **Dynamic Workflow** (`taxonomy-council.md`) formaliza las 13 fases y la
  concurrencia del fan-out para ejecuciones complejas (5 años, todas las
  unidades).
- Si el entorno no soporta Dynamic Workflows, la Skill ejecuta las mismas 13
  fases manualmente (ver sección "Fallback" en ambos documentos).

## Por qué esta asignación de modelos

- **Haiku** para tareas mecánicas de alto volumen (perfilar, normalizar,
  separar, mapear) donde el costo por registro debe ser bajo.
- **Sonnet** para tareas de razonamiento medio (descubrir patrones, modelar
  atributos, mapear estándares, evaluar transversalidad/tarifabilidad/
  gobernanza) donde se necesita juicio pero no máxima profundidad.
- **Opus** reservado para las 3 decisiones de mayor impacto y menor frecuencia:
  diseñar la estructura, desafiarla adversarialmente, y arbitrar la
  publicación final — donde un error se propaga a toda la taxonomía.
