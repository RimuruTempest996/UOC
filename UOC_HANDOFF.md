# UOC — CONTEXTO DE CONTINUIDAD

Este archivo permite continuar UOC desde otra instancia o cuenta de ChatGPT sin depender del historial de esta conversación.

## Objetivo
UOC transforma Excel de origen con diseños variables en un CSV con la estructura exacta del modelo DNCP. Regla central: **entrada variable, salida fija**. No modificar la estructura DNCP.

## Repositorio
GitHub: https://github.com/RimuruTempest996/UOC
Rama: `main`

## Archivos principales
- `index.html`
- `src/app.js`
- `src/styles.css`
- `templates/dncp_template.csv`
- `.github/workflows/validate.yml`
- `README.md`
- `UOC_HANDOFF.md`

## Referencias analizadas
Excel de origen: `Caudro_de_Adj_N°_Mantenimiento_de_Edificios_de_las_Facultades_UNI.xlsx`
CSV DNCP: `462740_Mantenimiento_de_Edificios_en_el_Campus_y_en_las_Filiales(1).csv`

El CSV DNCP de referencia tiene 49 filas, 15 columnas, separador `;` y codificación Windows-1252. La fila 6 es el encabezado; las filas 1-4 son metadatos y la fila 5 está vacía.

## Contrato de salida DNCP
Filas iniciales:
1. `LICITACIÓN: {LICITACION}`
2. `SISTEMA DE ADJUDICACIÓN: {SISTEMA_ADJUDICACION}`
3. `PROVEEDOR: {PROVEEDOR}`
4. `OBSERVACIÓN: En caso de que una fila no sera cargada eliminar la fila y los atributos de la lista. `
5. vacía

Encabezados exactos:
1. `"Número Lote"`
2. `Número Grupo`
3. `Número Item`
4. `Código de Producto`
5. `Grupo Descripción`
6. `Nombre del Producto`
7. `Cantidad`
8. `Contrato Abierto`
9. `Abastecimiento Simultáneo`
10. `Tipo Contrato Abierto`
11. `Cantidad Adjudicada`
12. `Porcentaje de Distribución`
13. `Precio Unitario Adjudicado`
14. `Atributo`
15. `Descripción Atributo`

Los atributos se representan en filas independientes con las primeras 13 columnas vacías y las columnas 14-15 con atributo y valor, por ejemplo `procedencia;Paraguay`.

## Excel de referencia
Hojas y dimensiones:
- `Lote I`: 141 x 8
- `Lotes II y III`: 446 x 11
- `Lote III`: 414 x 11
- `Ítems (3)`: 569 x 8

El diseño no debe tratarse como fijo. Hay hojas con posiciones diferentes, filas vacías, fórmulas y estructuras distintas.

## Estado técnico actual
`src/app.js` ya fue actualizado para generar las 15 columnas DNCP, con separador `;`, metadatos iniciales y filas de atributos. Commit actual de la actualización del generador: `55cc8890ce0c48899de90f8f7c111d5b35ba4d7a`.

`templates/dncp_template.csv` contiene el contrato estructural de salida.

## Mapeo semántico
- Número Lote ← lote
- Número Grupo ← grupo/número de grupo
- Número Item ← ítem
- Código de Producto ← código catálogo/código producto
- Grupo Descripción ← descripción del lote/grupo
- Nombre del Producto ← descripción/nombre del ítem
- Cantidad ← cantidad
- Contrato Abierto ← valor existente; no inventar
- Abastecimiento Simultáneo ← valor existente; no inventar
- Tipo Contrato Abierto ← valor existente; no inventar
- Cantidad Adjudicada ← valor existente; no inventar
- Porcentaje de Distribución ← valor existente; no inventar
- Precio Unitario Adjudicado ← precio unitario
- Atributo ← atributo
- Descripción Atributo ← valor del atributo

## Reglas obligatorias
1. Analizar todas las hojas.
2. Detectar encabezados por similitud semántica.
3. No depender de celdas fijas.
4. Normalizar antes de exportar.
5. Detectar metadatos fuera de las tablas.
6. Conservar atributos como filas independientes.
7. Salida siempre de 15 columnas y separador `;`.
8. Mantener textos y orden del contrato DNCP.
9. No inventar datos ausentes.
10. Validar estructura antes de descargar.
11. `templates/dncp_template.csv` es el contrato maestro y no debe alterarse sin volver a verificar el modelo oficial.

## Próximas mejoras
- Validación estricta de las 15 columnas y filas de atributos.
- Conversión real a Windows-1252 si el navegador no conserva esa codificación.
- Mejor detección de fórmulas y celdas combinadas.
- Mejor inferencia de grupo/lote cuando el Excel no tenga encabezados explícitos.
- Pruebas automáticas con el Excel de referencia.
- Comparación automática de estructura contra `templates/dncp_template.csv`.

## Instrucción para otra instancia
Leer este archivo primero. Después revisar `index.html`, `src/app.js`, `templates/dncp_template.csv` y el historial actual de GitHub. Continuar desde el estado real del repositorio; no reconstruir el proyecto desde cero.
