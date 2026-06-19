# Proceso de revisión humana — Exalmar Taxonomy Council Harness

## Cuándo se activa (`requiere_revision_humana=true`)

- `score_confianza < 0.7` (umbral inicial, ajustable por el Data Council).
- Registro en `tipo_compra=mixto` sin regla clara de separación.
- Ambigüedad: dos o más categorías candidatas con score similar.
- Registro `no_clasificable` (no calza en ninguna macro categoría existente).
- Propuesta de fusión, división o baja de una macro categoría.
- Issue de gobernanza con severidad `critica` o `alta`
  (`governance-auditor-agent`).
- Hallazgo de `adversarial-taxonomy-reviewer` no resuelto tras 3 ciclos.
- Cambio estructural con impacto estimado >10% de registros ya clasificados.
- Publicación de cualquier nueva versión de la taxonomía (Fase 13 siempre
  requiere aprobación humana explícita antes de proceder).

## Flujo de revisión

1. El orquestador consolida en la Fase 12 (Human-in-the-loop): resumen de la
   propuesta, métricas de calidad, `ambiguous_categories.csv`,
   `unclassified_items.csv`, e issues de gobernanza abiertos.
2. Presenta al usuario (o al Data Council) con: qué se decidió
   automáticamente, qué quedó pendiente, y qué requiere su decisión explícita.
3. El usuario puede: aprobar, aprobar con observaciones, o rechazar/pedir
   más iteración.
4. Solo tras una decisión explícita, `final-taxonomy-arbiter-agent` procede a
   consolidar y versionar (Fase 13).

## Qué NO se hace sin revisión humana

- No se publica ninguna versión de la taxonomía.
- No se fusiona, divide o da de baja una macro categoría.
- No se elimina ningún registro ambiguo o no clasificable (se conserva y se
  marca, nunca se borra).
- No se cierra un issue de gobernanza `critica`/`alta` sin decisión humana.

## Cola de pendientes para validación humana

Cada corrida del harness debe dejar explícita una lista de pendientes
(formato libre dentro de `taxonomy_quality_report.md` o
`implementation_plan.md`), separando:

- **Hechos**: lo que la data muestra de forma directa.
- **Inferencias**: lo que el harness dedujo con cierto nivel de confianza.
- **Recomendaciones**: lo que el harness sugiere decidir, sin decidirlo por
  el usuario.

## Relación con el golden set

El golden set (ver `docs/governance-model.md`) es mantenido y actualizado por
revisión humana. Cada vez que cambie la estructura del árbol de forma que
invalide etiquetas previas del golden set, se debe revalidar antes de usarlo
para medir precisión en `taxonomy_quality_report.md`.
