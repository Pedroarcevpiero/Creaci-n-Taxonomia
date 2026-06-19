---
name: taxonomy-council
description: >-
  Dynamic Workflow del Exalmar Taxonomy Council Harness. Ejecuta las 13 fases
  de construccion/clasificacion de la taxonomia propia de Exalmar mediante
  fan-out de subagents especializados y arbitraje final. Orquestado por el
  agente principal (esta Skill/Workflow); los subagents no lanzan subagents.
---

# Workflow: taxonomy-council

Este documento describe el Dynamic Workflow que ejecuta el agente principal al
invocar `/taxonomy-council`. Si el entorno no soporta Dynamic Workflows como
mecanismo nativo, ver "Fallback" al final: el mismo orden de fases se ejecuta
manualmente desde la Skill.

Todo corre con LLM de Claude + herramientas nativas (Read, Write, Grep, Glob).
**No usar APIs externas. No generar código ejecutable.**

## Reglas de orquestación
- El agente principal lanza cada subagent con la herramienta Agent.
- Los subagents devuelven su salida en JSON conforme al schema indicado; el
  agente principal consolida.
- Cada fase escribe su resultado intermedio en `outputs/logs/` antes de avanzar
  a la siguiente, para permitir trazabilidad y reanudar si se interrumpe.
- Ninguna fase sobrescribe outputs publicados; todo output nuevo usa sufijo de
  fecha/versión.
- Si una fase detecta volumen de datos grande o costo relevante, el agente
  principal debe pausar y confirmar con el usuario antes de continuar.

## Fase 0 — Intake de datasets
**Agente:** ninguno (el orquestador).
**Acción:** leer la instrucción del usuario, identificar fuente(s) en
`data/raw/`, alcance temporal, unidades de negocio incluidas y objetivo
(crear/clasificar/auditar/publicar). Si faltan datos críticos, preguntar al
usuario antes de seguir. Registrar el intake normalizado en
`outputs/logs/<fecha>_intake.md`.

## Fase 1 — Data profiling
**Agente:** `data-profiler-agent`.
**Acción:** perfilar el/los dataset(s): columnas disponibles, completitud,
cardinalidad de proveedores/materiales/cuentas, rangos de fecha, calidad
general. Salida a `outputs/logs/<fecha>_data_profile.md`.

## Fase 2 — Normalización de descripciones
**Agente:** `description-normalizer-agent`.
**Acción:** limpiar y normalizar `descripcion_original` → `descripcion_normalizada`
sin modificar el campo original. Detectar abreviaturas, unidades, idiomas
mixtos.

## Fase 3 — Separación material / servicio / mixto
**Agente:** `material-service-split-agent`.
**Acción:** clasificar cada registro en `tipo_compra` (material/servicio/mixto)
y, para mixtos, proponer regla de separación o marcar para revisión humana.

## Fase 4 — Mapeo operativo (unidad de negocio y equipo)
**Agentes:** `business-unit-agent`, `equipment-mapper-agent`.
**Acción:** asignar `unidad_negocio` y, cuando aplique, `equipo_relacionado` /
`functional_location` como **mapping**, nunca como categoría. Mantener
separación estricta cuenta/proveedor/material/categoría/equipo.

## Fase 5 — Descubrimiento de familias
**Agente:** `category-discovery-agent`.
**Acción:** sobre la muestra normalizada, proponer agrupaciones candidatas
(familias/subcategorías) basadas en patrones reales de la data, no solo en la
taxonomía semilla. Documentar evidencia de cada propuesta.

## Fase 6 — Propuesta de taxonomía
**Agente:** `taxonomy-architect-agent`.
**Acción:** consolidar las propuestas de la fase 5 contra las 16 macro
categorías semilla, decidiendo fusionar, dividir o mantener. Producir árbol de
3-4 niveles conforme a `taxonomy_node.schema.json`, evaluado con
`rubrics/mece-quality.md`.

## Fase 7 — Transversalidad
**Agente:** `transversality-agent`.
**Acción:** clasificar cada nodo/familia según `rubrics/transversality-kraljic.md`
(transversal total/parcial, específica por unidad, corporativa) y asignar
`kraljic_valor`/`kraljic_riesgo` como atributos, nunca como macro categoría.

## Fase 8 — Mapeo externo
**Agente:** `standards-mapper-agent`.
**Acción:** generar `mapping_rule` hacia UNSPSC (benchmark), eCl@ss
(atributos), ISO 14224/SAP PM (equipos, en paralelo), IMPA (flota), SANIPES/HACCP
(compliance alimentario). Nunca usar estos estándares como base de la
taxonomía propia.

## Fase 9 — Modelo de atributos
**Agente:** `attribute-modeler-agent`.
**Acción:** definir atributos obligatorios/opcionales por categoría conforme a
`category_attribute.schema.json`, usando eCl@ss solo como referencia.

## Fase 9b — Mercado proveedor y tarifabilidad
**Agentes:** `supplier-market-agent`, `tariffability-agent`.
**Acción:** evaluar concentración de proveedores, candidatos a tarifario y a
contrato marco según `rubrics/tariffability.md`.

## Fase 10 — Auditoría de gobernanza
**Agente:** `governance-auditor-agent`.
**Acción:** verificar MECE, owners, atributos mínimos, duplicidades, generar
`governance_issue` conforme a su schema.

## Fase 11 — Auditoría adversarial
**Agente:** `adversarial-taxonomy-reviewer`.
**Acción:** desafiar activamente la propuesta: buscar solapamientos, huecos,
sesgos de muestra, categorías infladas o subpobladas, inconsistencias de
Kraljic/transversalidad. Itera con la fase 6-10 hasta máximo 3 ciclos o hasta
que no haya issues `critica`/`alta` abiertos.

## Fase 12 — Human-in-the-loop
**Agente:** ninguno especializado (el orquestador presenta al usuario).
**Acción:** presentar resumen de la propuesta, métricas de calidad,
`ambiguous_categories.csv`, `unclassified_items.csv` y issues de gobernanza
abiertos. Esperar decisión humana antes de continuar a publicación.

## Fase 13 — Publicación de versión
**Agente:** `final-taxonomy-arbiter-agent`.
**Acción:** una vez aprobado por el usuario, consolidar `taxonomy_master.csv/.json`,
registrar la entrada correspondiente en `taxonomy_version_log.md`
(`taxonomy_version.schema.json`), y producir el resto de outputs listados en
`docs/operating-manual.md`.

## Concurrencia y límites
- Fan-out de subagents de una misma fase (ej. fase 4, fase 9b) puede correr en
  paralelo si son independientes entre sí.
- Límite recomendado: no lanzar más de 6-8 subagents en paralelo por fase para
  mantener trazabilidad y facilitar la auditoría posterior.
- Las fases 6, 10, 11 y 13 son secuenciales por diseño (cada una depende del
  resultado consolidado de la anterior).

## Fallback si Dynamic Workflows no está disponible
Ejecutar las fases en el mismo orden desde `.claude/skills/taxonomy-council/SKILL.md`,
lanzando cada subagent manualmente con la herramienta Agent y escribiendo los
artefactos intermedios en `outputs/logs/` entre fases. El comportamiento y las
reglas de seguridad son idénticas; solo cambia el mecanismo de invocación.
