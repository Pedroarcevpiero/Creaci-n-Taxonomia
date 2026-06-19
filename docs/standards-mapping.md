# Mapeo a estándares externos — Exalmar Taxonomy Council Harness

Los estándares externos son **capas de apoyo**, nunca la base de la
taxonomía propia de Exalmar. Este documento resume el uso permitido de cada
uno y cómo se materializa en el harness.

| Estándar / referencia | Uso recomendado | Dónde se aplica en el harness |
|---|---|---|
| UNSPSC | Mapping externo / benchmark | `standards-mapper-agent` → `mapping_rule` tipo `unspsc`, campo `unspsc_sugerido` en `taxonomy_record` |
| eCl@ss | Diccionario de atributos técnicos | `attribute-modeler-agent` → `category_attribute.fuente_referencia=eclass`, campo `eclass_sugerido` |
| ISO 14224 / SAP PM | Taxonomía de equipos en paralelo | `equipment-mapper-agent` → `equipo_relacionado`, `functional_location` (solo mapping) |
| IMPA Marine Stores Guide | Referencia para flota/naval | `standards-mapper-agent` para categorías específicas de flota |
| HACCP / SANIPES | Atributos de compliance alimentario | `attribute-modeler-agent` → atributos de empaque/insumos productivos |
| CPV / NSN / NIGP | Evaluar; probablemente no usar como base | `standards-mapper-agent` documenta evaluación y descarte si aplica |

## Reglas duras

1. Ningún estándar externo determina la estructura del árbol propio
   (`schemas/taxonomy_node.schema.json`).
2. Todo mapping a estándar externo se registra como `mapping_rule` con
   `direccion: salida` y `confianza` explícita.
3. Si no hay correspondencia razonable, el mapping queda vacío y se declara
   — no se fuerza un código aproximado de baja calidad.
4. La taxonomía de equipos (ISO 14224/SAP PM) nunca se fusiona con la
   taxonomía de compras; viven en paralelo y se conectan solo vía
   `equipo_relacionado`/`functional_location`.

## Proceso de mapeo (Fase 8 del workflow)

1. `standards-mapper-agent` recibe los nodos aprobados/propuestos.
2. Para cada nodo relevante, propone candidatos de mapping con evidencia y
   nivel de confianza.
3. `governance-auditor-agent` verifica que ningún mapping implique fusión
   jerárquica indebida (issue `mezcla_taxonomia_equipos` si se detecta).
4. Los mapping aprobados alimentan `mapping_rules.md` y
   `mapping_5y_spend.csv`.
