# Ejemplo: modo piloto sobre una unidad de negocio

## Objetivo
Validar el harness completo y la taxonomía semilla sobre un alcance pequeño
antes de invertir en la clasificación de 5 años completos.

## Comando de ejemplo
```text
/taxonomy-council
Fuente de datos: data/raw/oc_flota_con_frio_2024_q4.csv
Alcance temporal: último trimestre 2024
Objetivo: crear taxonomía v0.0 en modo piloto
Unidades de negocio incluidas: flota con frío (RSW/CSW)
Restricciones: usar las 16 macro categorías semilla como punto de partida, no como verdad final
Profundidad: piloto
```

## Qué hace el orquestador
1. Ejecuta las 13 fases del workflow (`taxonomy-council.md`) solo sobre el
   dataset indicado.
2. Genera una taxonomía candidata limitada a lo observado en flota con frío.
3. Marca explícitamente qué macro categorías semilla NO tuvieron evidencia en
   esta muestra (no las elimina, las deja `propuesto` pendiente de más data).
4. Produce `taxonomy_quality_report.md` con métricas de cobertura y
   ambigüedad de la muestra.
5. Presenta al usuario antes de fusionar/dividir cualquier macro categoría.

## Qué validar con el usuario antes de escalar
- ¿Las categorías y familias detectadas tienen sentido para un comprador de
  Exalmar?
- ¿Los atributos extraídos (ej. refrigerante, capacidad, presión para
  sistemas de frío) son los relevantes para negociar?
- ¿El umbral de revisión humana (rúbrica `classification-confidence.md`) es
  razonable o hay que ajustarlo?

Solo después de validar el piloto se recomienda avanzar a
`clasificacion-5-anios.md`.
