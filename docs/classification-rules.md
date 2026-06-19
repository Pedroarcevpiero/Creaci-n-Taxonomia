# Reglas de clasificación — Exalmar Taxonomy Council Harness

## Principio rector

```text
cuenta contable ≠ proveedor ≠ material ≠ categoría ≠ equipo
```

Ninguna regla de clasificación puede usar `cuenta_contable` o `proveedor`
como criterio único o como identidad de la categoría. Estas dimensiones se
conectan solo vía `schemas/mapping_rule.schema.json`.

## Reglas de existencia de nodo

**Macro categoría**: debe tener volumen económico relevante, mercado
proveedor propio, estrategia de sourcing distinta, riesgos propios, atributos
técnicos específicos, potencial de contrato marco/tarifario, capacidad de
cruzar materiales y servicios bajo un mismo driver, o necesidad de owner de
categoría propio.

**Subcategoría**: debe permitir comparar precios, agrupar proveedores, crear
tarifario, definir especificaciones estándar, diferenciar criticidad, separar
tecnología/equipo, o separar unidad de negocio cuando la gestión es distinta.

Ver desarrollo completo en `rubrics/mece-quality.md`.

## Reglas de transversalidad

Ver `rubrics/transversality-kraljic.md`. Resumen: 3+ unidades con mismo
mercado/gestión → transversal total; 2 unidades → transversal parcial; mercado
y criticidad propios → específica de unidad; "Estratégico" siempre es
atributo Kraljic, nunca macro categoría; importaciones/comercio exterior van
en Logística salvo evidencia cuantitativa suficiente para macro categoría
propia.

## Reglas de tipo de compra (material/servicio/mixto)

Ver agente `material-service-split-agent`. Los registros `mixto` requieren
propuesta explícita de separación o revisión humana; nunca se fuerza una
etiqueta única sobre evidencia genuinamente mixta.

## Reglas de confianza y revisión humana

Ver `rubrics/classification-confidence.md`. Resumen de umbrales:
- `score_confianza < 0.7` → revisión humana obligatoria.
- Categoría candidata en estado `propuesto` → revisión humana obligatoria
  independientemente del score.
- El score nunca es la única validación de una decisión estructural.

## Reglas de tarifabilidad y contrato marco

Ver `rubrics/tariffability.md`. Toda marca `tarifable=true` o
`contrato_marco_candidato=true` requiere evidencia citada de mercado
proveedor (`supplier-market-agent`).

## Reglas de mapeo a estándares externos

- UNSPSC: solo mapping de benchmark/referencia (`unspsc_sugerido`), nunca
  jerarquía base.
- eCl@ss: solo referencia de atributos técnicos (`category_attribute.schema.json`,
  campo `fuente_referencia: eclass`), nunca jerarquía base.
- ISO 14224 / SAP PM: taxonomía de equipos en paralelo, vinculada solo vía
  `equipo_relacionado` / `functional_location`.
- IMPA: referencia para categorías de flota/naval.
- HACCP / SANIPES: atributos de compliance alimentario para empaque/insumos
  productivos.
- CPV / NSN / NIGP: evaluar caso por caso; documentar si se descartan y por
  qué (`standards-mapper-agent`).

## Reglas de no eliminación y trazabilidad

- Nunca eliminar registros ambiguos o no clasificables; marcarlos y enviarlos
  a `outputs/validation/`.
- Conservar siempre `descripcion_original` sin modificar.
- Separar siempre hechos, supuestos, inferencias y recomendaciones en las
  salidas de cada agente.
