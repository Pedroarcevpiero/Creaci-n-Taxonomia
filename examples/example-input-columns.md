# Ejemplo de columnas de entrada esperadas

Columnas típicas por tipo de archivo fuente que el harness espera encontrar en
`data/raw/`. No es una lista cerrada: `data-profiler-agent` se adapta a lo que
realmente exista en el dataset y reporta diferencias.

## Órdenes de compra (OC) y posiciones

```text
sociedad, centro, orden_compra, posicion_oc, fecha, proveedor, codigo_proveedor,
codigo_material_servicio, descripcion_original, texto_largo, cuenta_contable,
grupo_articulo_sap, monto, moneda, cantidad, unidad_medida, solped, contrato
```

## SOLPED

```text
solped, fecha, centro, solicitante, codigo_material_servicio,
descripcion_original, cantidad, unidad_medida, cuenta_contable
```

## HES (hoja de entrada de servicio)

```text
hes, orden_compra, posicion_oc, fecha, proveedor, descripcion_original,
monto, moneda, centro
```

## Maestro de materiales

```text
codigo_material_servicio, descripcion_original, grupo_articulo_sap,
unidad_medida, tipo_compra
```

## Maestro de servicios

```text
codigo_material_servicio, descripcion_original, grupo_articulo_sap,
unidad_medida
```

## Maestro de proveedores

```text
codigo_proveedor, proveedor, ruc_o_id_fiscal, pais, categoria_sap
```

## Maestro de equipos / functional locations

```text
equipo_relacionado, functional_location, descripcion_equipo, planta,
embarcacion, fabricante, modelo
```

## Contratos y cotizaciones

```text
contrato, proveedor, codigo_proveedor, vigencia_inicio, vigencia_fin,
condiciones, monto_marco
```

## Catálogos de apoyo

```text
centro, planta, embarcacion, unidad_negocio, sistema_frio (si/no), sociedad,
unidad_medida_codigo, unidad_medida_descripcion
```

## Nota

Si una columna esperada no existe en el dataset real, `data-profiler-agent` lo
reporta como riesgo de calidad y los agentes posteriores usan
`no_determinado` en los campos que dependan de ella, declarando el supuesto.
