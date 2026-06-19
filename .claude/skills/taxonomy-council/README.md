# taxonomy-council (Skill)

Skill que orquesta el **Exalmar Taxonomy Council Harness**. Ver `SKILL.md` para
el detalle operativo completo.

## Contenido de esta carpeta
- `SKILL.md` — definición de la skill (frontmatter + workflow).
- `rubrics/` — criterios de calidad usados por los subagents y por el
  orquestador para decidir score, transversalidad, tarifabilidad y umbral de
  revisión humana.
- `examples/` — guías de uso para modo piloto y para clasificación de 5 años.

## Uso rápido
```text
/taxonomy-council
Crea una taxonomía de compras para Exalmar usando los archivos en data/raw/.
Usa 5 años de compras, maestros SAP, proveedores, equipos y contratos. Genera
taxonomy_master, category_dictionary, mapping_rules y governance_report. Marca
registros ambiguos y no clasificables para revisión humana.
```

Recomendación: ejecutar primero en modo piloto (`examples/piloto-una-unidad.md`)
antes de procesar el histórico completo.
