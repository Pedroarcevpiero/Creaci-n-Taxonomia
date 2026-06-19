# Rúbrica: calidad MECE de la taxonomía

Usada por `taxonomy-architect-agent`, `governance-auditor-agent` y
`adversarial-taxonomy-reviewer` para evaluar si un nodo o el árbol completo es
mutuamente excluyente y colectivamente exhaustivo.

## Criterios de exclusión mutua (no solapamiento)
- Ningún registro real del dataset de referencia debe calzar en dos nodos
  hermanos del mismo nivel sin una regla de desambiguación explícita.
- Si dos categorías comparten >20% de palabras clave o descripciones típicas,
  marcar como `governance_issue` tipo `no_mece_solapamiento`.
- Cada nodo debe tener `excluye` con al menos un ejemplo concreto de qué NO
  entra y a dónde va.

## Criterios de exhaustividad (sin huecos)
- Todo registro del dataset de referencia debe poder asignarse a al menos un
  nodo o a `no_clasificable` explícito (nunca quedar sin intento de asignación).
- Si >5% de una muestra cae en `no_clasificable` de forma recurrente con un
  patrón común, proponer nueva familia o subcategoría
  (`category-discovery-agent`).

## Criterios de existencia de un nodo (sección 5 del plan original)
Una macro categoría se justifica si tiene: volumen económico relevante,
mercado proveedor propio, estrategia de sourcing distinta, riesgos propios,
atributos técnicos específicos, potencial de contrato marco/tarifario, o
necesita owner propio.

Una subcategoría se justifica si permite: comparar precios, agrupar
proveedores, crear tarifario, definir especificaciones estándar, diferenciar
criticidad, separar tecnología/equipo, o separar unidad de negocio cuando la
gestión es distinta.

## Señales de alerta (escalar a revisión humana)
- Macro categoría con <0.5% del gasto total y sin mercado proveedor propio →
  candidata a fusión.
- Categoría con un único proveedor y sin atributos definidos → revisar si es
  específica o solo está sub-poblada de data.
- Cambios de estructura (fusión/división) que afectarían >10% de los registros
  ya clasificados → requieren aprobación explícita del Data Council antes de
  aplicarse.

## Score de calidad MECE (0-1, orientativo, NO único criterio de aprobación)
- 1.0: sin solapamientos detectados, sin huecos, todos los nodos con
  `incluye`/`excluye` completos.
- 0.7-0.9: solapamientos menores documentados con regla de desambiguación.
- <0.7: requiere revisión humana antes de aprobar el nodo o la versión.
