# Rúbrica: confianza de clasificación y revisión humana

Usada por todos los agentes de clasificación y por el orquestador para decidir
`score_confianza` y `requiere_revision_humana`. **El score nunca es la única
validación** (regla irrenunciable de `CLAUDE.md`).

## Cómo se calcula `score_confianza` (0.0-1.0, orientativo)
Factores que **aumentan** confianza:
- Coincidencia exacta o casi exacta con `palabras_clave`/`sinonimos` del
  diccionario de categorías.
- Coincidencia con una `mapping_rule` de confianza `alta` ya aprobada.
- Atributos extraídos consistentes con `atributos_obligatorios` de la
  categoría candidata.
- Mismo proveedor/material clasificado de forma consistente en el histórico.

Factores que **reducen** confianza:
- Descripción corta, abreviada o ambigua sin atributos extraíbles.
- Coincidencia parcial con 2+ categorías candidatas con score similar.
- `tipo_compra=mixto` sin regla clara de split.
- Proveedor o material nuevo sin historial de clasificación.
- Categoría candidata marcada `en_revision` o `propuesto` (no `aprobado`).

## Umbrales (ajustables por el Data Council, ver `docs/governance-model.md`)
- `score_confianza >= 0.85` y categoría `aprobado` → `auto_propuesto`, sin
  bloquear pero sujeto a muestreo de auditoría.
- `0.7 <= score_confianza < 0.85` → `auto_propuesto`, incluir en cola de
  revisión muestral.
- `score_confianza < 0.7` → `requiere_revision_humana=true` obligatorio.
- Cualquier categoría candidata en estado `propuesto` → `requiere_revision_humana=true`
  independientemente del score.

## Motivos de revisión (`motivo_revision`, valores típicos)
`score_bajo`, `ambiguedad_multiple_categoria`, `categoria_no_existe`,
`tipo_compra_mixto_sin_regla`, `proveedor_material_nuevo`,
`conflicto_con_clasificacion_historica`, `cambio_estructural_propuesto`.

## Regla de oro
Nunca usar exclusivamente `score_confianza` para decidir publicar, fusionar o
dar de baja un nodo de taxonomía. Toda decisión estructural pasa por
`adversarial-taxonomy-reviewer` y revisión humana, sin excepción.
