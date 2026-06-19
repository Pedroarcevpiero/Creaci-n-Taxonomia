# Rúbrica: tarifabilidad y candidatos a contrato marco

Usada por `tariffability-agent` y `supplier-market-agent`.

## Criterios para marcar `tarifable=true`
- Existen 2 o más proveedores que ofrecen el mismo material/servicio con
  especificación comparable (mismos atributos obligatorios de la categoría).
- El volumen de compra es recurrente (más de N órdenes en el período de
  análisis; N lo define el Data Council según categoría).
- La unidad de medida y especificación son estandarizables sin pérdida de
  calidad/seguridad (no aplica a CAPEX único o servicios altamente
  personalizados).
- Existe variabilidad de precio histórica entre proveedores para el mismo
  ítem, lo que indica oportunidad de negociación con tarifario.

## Criterios para marcar `contrato_marco_candidato=true`
- Categoría con `kraljic_valor=alto` (volumen económico relevante) y compra
  recurrente en el tiempo (no puntual/proyecto único).
- Mercado proveedor con concentración media (2-5 proveedores relevantes); si
  hay un solo proveedor, evaluar como `cuello_de_botella` (ver rúbrica
  Kraljic) antes de proponer contrato marco.
- Especificaciones técnicas estables en el tiempo (bajo riesgo de cambio de
  atributos obligatorios).
- Transversalidad `transversal_total` o `transversal_parcial`: mayor
  apalancamiento de volumen al consolidar contrato entre unidades.

## Criterios para marcar `catalogable=true`
- `kraljic_valor=bajo` y `kraljic_riesgo=bajo` (rutinario).
- Especificación simple, sin necesidad de cotización caso a caso.
- Candidato natural a tail spend / compra automatizada.

## Salida esperada
Cada categoría evaluada produce evidencia para
`tariff_candidates.csv` y `contract_framework_candidates.csv`:
proveedores identificados, número de proveedores, variabilidad de precio
observada, volumen/frecuencia, y la justificación de por qué cumple (o no)
cada criterio. Nunca marcar `tarifable` o `contrato_marco_candidato` en true
sin al menos un dato de evidencia citado.
