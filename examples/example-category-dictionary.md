# Ejemplo de entrada del diccionario de categorías

Ejemplo ilustrativo de una entrada de `category_dictionary.md`, en el
formato definido por `schemas/category_dictionary.schema.json`, para la
subcategoría usada en `example-taxonomy-output.md`.

```json
{
  "node_id": "04.02.01",
  "nombre": "Compresores de Sistema de Frío",
  "nivel_nombre": "subcategoria",
  "definicion": "Compresores nuevos o reacondicionados (semiherméticos, de pistón, scroll) usados en sistemas de refrigeración a bordo de embarcaciones con frío.",
  "incluye": [
    "compresor semihermético",
    "compresor de pistón",
    "compresor scroll naval",
    "compresor reacondicionado para sistema de frío"
  ],
  "excluye": [
    "repuestos internos del compresor, p.ej. válvulas de descarga (ver familia Partes de Compresor)",
    "servicio de reparación o mantenimiento del compresor (ver Servicios de Mantenimiento Naval)",
    "gases refrigerantes (ver Insumos de Refrigeración)"
  ],
  "sinonimos": ["compresor frigorífico", "compresor de frío"],
  "palabras_clave": ["compresor", "semihermetico", "bitzer", "copeland", "scroll", "sist frio", "sistema de frio"],
  "reglas_clasificacion": [
    "Si la descripción menciona 'compresor' + marca/modelo de sistema de frío, clasificar aquí.",
    "Si la descripción menciona 'compresor' sin contexto de frío (ej. compresor de aire), enviar a revisión humana (ambiguedad).",
    "Si la descripción es un repuesto interno del compresor (válvula, pistón, junta), clasificar en familia Partes de Compresor, no aquí."
  ],
  "atributos_obligatorios": ["fabricante", "modelo_equipo", "numero_parte", "capacidad_btu", "tipo_refrigerante"],
  "transversalidad": "especifica_flota_con_frio",
  "owner_sugerido": "no_determinado",
  "criticidad_tipica": "alta",
  "ejemplos_descripciones": [
    "COMPRESOR SEMIHERMETICO BITZER 4FE-25Y P/SIST FRIO",
    "COMPRESOR COPELAND SCROLL ZR94 NAVAL",
    "COMPRESOR PISTON 2 CIL REACONDICIONADO SIST FRIO"
  ],
  "estado": "propuesto"
}
```

## Nota sobre `excluye`

El campo `excluye` es la pieza clave para mantener MECE: cada exclusión debe
indicar a qué otro nodo va el caso excluido, para que no quede ambigüedad ni
duplicidad entre categorías hermanas. `governance-auditor-agent` valida que
toda entrada del diccionario tenga al menos una exclusión con destino
explícito.
