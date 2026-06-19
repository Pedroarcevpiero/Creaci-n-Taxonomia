# Requisitos de datos — Exalmar Taxonomy Council Harness

## Fuentes esperadas (sección 17, actividad 1 del plan original)

Extracción de SAP (u otra fuente equivalente), colocada en `data/raw/`:

- Órdenes de compra (OCs) y posiciones de OC.
- SOLPED (solicitudes de pedido).
- HES (hojas de entrada de servicio).
- Maestros de materiales.
- Maestros de servicios.
- Maestros de proveedores.
- Maestros de equipos y functional locations.
- Contratos.
- Cotizaciones.
- Cuentas contables.
- Centros, plantas, embarcaciones.
- Unidades de medida.
- Solicitantes.

Ver `examples/example-input-columns.md` para columnas esperadas por archivo.

## Modelo de datos del registro clasificado

Ver `schemas/taxonomy_record.schema.json` para el contrato formal. Resumen de
las dimensiones que deben mantenerse separadas en el dato fuente:

- **Identidad del documento**: `fuente`, `orden_compra`, `posicion_oc`,
  `solped`, `hes`, `contrato`.
- **Identidad organizacional**: `sociedad`, `centro`, `planta`,
  `embarcacion`, `unidad_negocio`.
- **Identidad del proveedor**: `proveedor`, `codigo_proveedor` (nunca
  categoría).
- **Identidad del material/servicio**: `codigo_material_servicio`,
  `descripcion_original` (inmutable), `descripcion_normalizada`,
  `texto_largo`.
- **Identidad financiera**: `cuenta_contable` (nunca categoría),
  `grupo_articulo_sap`, `monto`, `moneda`, `cantidad`, `unidad_medida`.
- **Identidad de equipo (paralela)**: `equipo_relacionado`,
  `functional_location` (solo mapping, nunca fusionada con la taxonomía de
  compras).
- **Clasificación del harness**: `macro_categoria`, `categoria`,
  `subcategoria`, `familia`, `atributos_extraidos`.
- **Capas de evaluación**: `transversalidad`, `kraljic_valor`,
  `kraljic_riesgo`, `criticidad`, `tarifable`, `contrato_marco_candidato`,
  `catalogable`, `owner_sugerido`.
- **Mapping externo**: `unspsc_sugerido`, `eclass_sugerido` (siempre
  sugeridos, nunca base).
- **Trazabilidad y calidad**: `score_confianza`, `requiere_revision_humana`,
  `motivo_revision`, `estado_clasificacion`.

## Calidad mínima esperada antes de clasificar

`data-profiler-agent` (Fase 1) debe confirmar como mínimo:
- `descripcion_original` presente en >95% de los registros (si no, advertir
  al usuario antes de continuar).
- Identificación de duplicados evidentes resuelta o documentada.
- Rango de fechas y unidades de negocio cubiertas declarado explícitamente.

## Qué hacer si falta un dato maestro

Si no existe maestro de embarcaciones con/sin sistema de frío, maestro de
equipos, o catálogo de centros/plantas, los agentes correspondientes
(`business-unit-agent`, `equipment-mapper-agent`) deben usar
`no_determinado` y declarar el supuesto explícitamente — nunca inventar el
dato faltante.

## Volumen y procesamiento por lotes

Para clasificación de 5 años (sección 17, días 61-90), procesar por lotes
(año y/o unidad de negocio) según `examples/clasificacion-5-anios.md`, para
mantener trazabilidad y permitir checkpoints de revisión humana.
