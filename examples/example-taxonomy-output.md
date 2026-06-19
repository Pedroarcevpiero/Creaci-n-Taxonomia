# Ejemplo de salida — árbol de taxonomía y registro clasificado

Ejemplo ilustrativo (no real) de cómo se ven los outputs de
`outputs/taxonomy/` tras una corrida del harness. Usa una de las 16 macro
categorías semilla (Repuestos Navales) a modo de muestra de los 3 niveles
principales más el nivel opcional `familia`.

## Nodos del árbol (`taxonomy_master.csv`, formato `taxonomy_node`)

```json
[
  {
    "node_id": "04",
    "nivel": 0,
    "nivel_nombre": "macro_categoria",
    "nombre": "Repuestos Navales",
    "parent_id": null,
    "definicion": "Repuestos y partes de reemplazo para equipos a bordo de embarcaciones (motores, sistemas de frío, cubierta).",
    "incluye": ["repuestos de motor principal", "repuestos de sistema de frío", "repuestos de winche"],
    "excluye": ["servicios de reparación naval (ver Servicios de Mantenimiento Naval)", "combustible (ver Combustibles y Lubricantes)"],
    "driver": "mercado proveedor especializado en partes navales, criticidad operativa alta",
    "transversalidad": "especifica_flota_con_frio",
    "owner_sugerido": "no_determinado",
    "atributos_obligatorios": ["fabricante", "modelo_equipo", "numero_parte"],
    "unspsc_ref": ["25000000"],
    "eclass_ref": [],
    "estado": "aprobado",
    "version_alta": "0.1",
    "version_baja": null,
    "notas": "Macro categoría semilla validada en taller piloto."
  },
  {
    "node_id": "04.02",
    "nivel": 1,
    "nivel_nombre": "categoria",
    "nombre": "Repuestos de Sistema de Frío",
    "parent_id": "04",
    "definicion": "Partes de reemplazo para sistemas de refrigeración/frío a bordo (compresores, válvulas, evaporadores).",
    "incluye": ["compresores", "válvulas de expansión", "evaporadores"],
    "excluye": ["refrigerantes y gases (ver Insumos de Refrigeración)"],
    "driver": "tecnología distinta a repuestos de motor, proveedores especializados en frío",
    "transversalidad": "especifica_flota_con_frio",
    "owner_sugerido": "no_determinado",
    "atributos_obligatorios": ["fabricante", "modelo_equipo", "numero_parte", "capacidad_btu"],
    "estado": "aprobado",
    "version_alta": "0.1",
    "version_baja": null
  },
  {
    "node_id": "04.02.01",
    "nivel": 2,
    "nivel_nombre": "subcategoria",
    "nombre": "Compresores de Sistema de Frío",
    "parent_id": "04.02",
    "definicion": "Compresores nuevos o reacondicionados para sistemas de frío naval.",
    "incluye": ["compresor semihermético", "compresor de pistón"],
    "excluye": ["repuestos internos de compresor (ver familia Partes de Compresor)"],
    "driver": "permite comparar precios entre proveedores y crear tarifario",
    "atributos_obligatorios": ["fabricante", "modelo_equipo", "numero_parte", "capacidad_btu", "tipo_refrigerante"],
    "estado": "propuesto",
    "version_alta": "0.1",
    "version_baja": null
  }
]
```

## Registro clasificado (`spend_cube_5y.csv`, formato `taxonomy_record`)

```json
{
  "id_registro": "OC-2024-0001-010",
  "fuente": "SAP_OC",
  "sociedad": "EXALMAR_PERU",
  "centro": "C001",
  "embarcacion": "no_determinado",
  "unidad_negocio": "flota_con_frio",
  "fecha": "2024-03-12",
  "proveedor": "Frio Naval SAC",
  "codigo_proveedor": "PROV-0456",
  "codigo_material_servicio": "MAT-778899",
  "descripcion_original": "COMPRESOR SEMIHERMETICO BITZER 4FE-25Y P/SIST FRIO",
  "descripcion_normalizada": "compresor semihermético bitzer 4fe-25y para sistema de frío",
  "cuenta_contable": "60501020",
  "monto": 18500,
  "moneda": "USD",
  "cantidad": 1,
  "unidad_medida": "UND",
  "tipo_compra": "material",
  "macro_categoria": "Repuestos Navales",
  "categoria": "Repuestos de Sistema de Frío",
  "subcategoria": "Compresores de Sistema de Frío",
  "atributos_extraidos": {
    "fabricante": "Bitzer",
    "modelo_equipo": "4FE-25Y",
    "tipo_refrigerante": "no_determinado"
  },
  "equipo_relacionado": "no_determinado",
  "transversalidad": "especifica_flota_con_frio",
  "kraljic_valor": "alto",
  "kraljic_riesgo": "alto",
  "criticidad": "alta",
  "tarifable": false,
  "contrato_marco_candidato": true,
  "catalogable": true,
  "owner_sugerido": "no_determinado",
  "unspsc_sugerido": "25174001",
  "eclass_sugerido": "no_determinado",
  "score_confianza": 0.81,
  "requiere_revision_humana": false,
  "estado_clasificacion": "validado"
}
```

## Nota

`contrato_marco_candidato=true` requiere evidencia de mercado proveedor
citada por `supplier-market-agent` (ver `rubrics/tariffability.md`); en una
corrida real esa evidencia se referencia en `tariffability_candidates.csv`,
no se repite aquí por brevedad.
