# Rúbrica: transversalidad y Kraljic

Usada por `transversality-agent` y `business-unit-agent`.

## Categorías de transversalidad (campo `transversalidad`)
- `transversal_total`: aparece en 3 o más unidades de negocio con mismo
  mercado proveedor y misma lógica de gestión.
- `transversal_parcial`: aparece en exactamente 2 unidades de negocio.
- `especifica_flota`, `especifica_flota_con_frio`, `especifica_flota_sin_frio`,
  `especifica_harina_aceite`, `especifica_congelado`: mercado proveedor y
  criticidad propios de una sola unidad.
- `corporativa`: compras de administración/funciones de soporte, no ligadas a
  una planta o embarcación específica.

## Reglas de decisión
1. Si una categoría comparte mismo mercado proveedor y misma lógica de gestión
   en 3+ unidades → `transversal_total`.
2. Si aparece en exactamente 2 unidades → `transversal_parcial` (no asumir
   automáticamente total).
3. Si tiene mercado proveedor y criticidad propia de una unidad → específica
   de esa unidad.
4. Importaciones y comercio exterior van dentro de Logística, salvo que la
   data demuestre volumen/complejidad suficiente para macro categoría propia
   (requiere evidencia cuantitativa, no solo percepción).
5. Ejemplos de referencia: motores de combustión → transversal si aparecen en
   flota y plantas; sistemas de frío RSW/CSW → conectan flota con frío y
   congelado; redes de cerco → específicas de flota; calderas/secadores →
   específicos de harina y aceite; túneles de congelado → específicos de
   congelado; EPP/ferretería/rodamientos/energía/servicios mecánicos genéricos
   → suelen ser transversales.

## Matriz de Kraljic (atributo, NUNCA macro categoría)
`kraljic_valor` (alto/medio/bajo) × `kraljic_riesgo` (alto/medio/bajo):
- Valor alto + riesgo alto → **estratégico**: requiere owner senior, contrato
  marco, plan de continuidad de suministro.
- Valor alto + riesgo bajo → **apalancable**: negociar agresivamente,
  consolidar volumen.
- Valor bajo + riesgo alto → **cuello de botella**: asegurar suministro,
  diversificar proveedores.
- Valor bajo + riesgo bajo → **rutinario**: candidato a tail spend,
  catálogo/automatización.

## Reglas
- "Estratégico" se registra como combinación de `kraljic_valor` y
  `kraljic_riesgo`, nunca como nombre de macro categoría o categoría.
- Toda categoría con `kraljic_riesgo=alto` debe tener `owner_sugerido` no vacío
  antes de poder marcarse como `aprobado`.
- Si la evidencia de riesgo/valor es insuficiente, usar `no_determinado` y
  marcar `requiere_revision_humana=true`.
