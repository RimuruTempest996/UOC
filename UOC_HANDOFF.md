# UOC — CONTEXTO DE CONTINUIDAD

Este archivo existe para continuar el proyecto desde otra instancia o cuenta de ChatGPT sin depender del historial de esta conversación.

## Objetivo

UOC transforma archivos Excel de origen, aunque tengan diseños, hojas, posiciones, encabezados, celdas combinadas o columnas diferentes, en un CSV con la estructura exacta del modelo de carga DNCP.

La regla principal es: **entrada variable, salida fija**. No se debe modificar la estructura DNCP.

## Repositorio

GitHub: https://github.com/RimuruTempest996/UOC
Rama principal: `main`

## Estado actual

Existe una interfaz web inicial en `index.html`.

Archivos actuales:
- `index.html`
- `src/app.js`
- `src/styles.css`
- `templates/dncp_template.csv`
- `.github/workflows/validate.yml`
- `README.md`
- `UOC_HANDOFF.md`

## Archivos de referencia usados durante el desarrollo

Excel de origen analizado:
`Caudro_de_Adj_N°_Mantenimiento_de_Edificios_de_las_Facultades_UNI.xlsx`

CSV DNCP de referencia analizado:
`462740_Mantenimiento_de_Edificios_en_el_Campus_y_en_las_Filiales(1).csv`

El CSV de referencia tiene 49 filas y 15 columnas separadas por `;`, con codificación Windows-1252. La fila 6 contiene los encabezados exactos y las filas 1 a 4 contienen metadatos.

## Contrato exacto de salida DNCP

Fila 1:
`LICITACIÓN: {LICITACION}`

Fila 2:
`SISTEMA DE ADJUDICACIÓN: {SISTEMA_ADJUDICACION}`

Fila 3:
`PROVEEDOR: {PROVEEDOR}`

Fila 4:
`OBSERVACIÓN: En caso de que una fila no sera cargada eliminar la fila y los atributos de la lista. `

Fila 5: vacía.

Fila 6, columnas exactas:
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

Los datos comienzan después de la fila de encabezados. Los atributos pueden aparecer en filas adicionales que mantienen vacías las primeras 13 columnas y usan las columnas 14 y 15, por ejemplo `procedencia;Paraguay`.

## Excel de referencia

El Excel de origen tiene 4 hojas:
- `Lote I`: 141 x 8
- `Lotes II y III`: 446 x 11
- `Lote III`: 414 x 11
- `Ítems (3)`: 569 x 8

No asumir posiciones fijas. Hay diferentes diseños, filas vacías, fórmulas y datos de lotes.

Campos semánticos importantes detectados:
- institución
- licitación
- adjudicado/proveedor
- lote
- ítem
- código de catálogo/producto
- descripción
- cantidad
- atributos
- características
- precio unitario
- precio total

## Reglas de implementación

1. Analizar todas las hojas.
2. Detectar encabezados por similitud semántica.
3. No depender de números de fila o columna fijos.
4. Normalizar a un modelo interno.
5. Detectar metadatos aunque estén fuera de la tabla.
6. Conservar atributos como filas independientes cuando corresponda.
7. Generar siempre exactamente 15 columnas y el separador `;`.
8. Mantener la estructura textual de la plantilla DNCP.
9. Escapar correctamente `;`, comillas y saltos de línea cuando corresponda al CSV.
10. Validar antes de permitir la descarga.
11. La plantilla `templates/dncp_template.csv` es el contrato de salida y no debe cambiarse sin verificar nuevamente el modelo DNCP.

## Próximo trabajo obligatorio

Actualizar `src/app.js` para que deje de generar la salida provisional de 8 columnas y genere el formato de 15 columnas definido arriba.

El mapeo mínimo desde el Excel debe producir:
- Número Lote ← lote
- Número Grupo ← grupo/número de grupo
- Número Item ← ítem
- Código de Producto ← código catálogo/código producto
- Grupo Descripción ← descripción del lote/grupo
- Nombre del Producto ← descripción/nombre del ítem
- Cantidad ← cantidad
- Contrato Abierto ← detectar valor existente; no inventar
- Abastecimiento Simultáneo ← detectar valor existente; no inventar
- Tipo Contrato Abierto ← detectar valor existente; no inventar
- Cantidad Adjudicada ← detectar valor existente; no inventar
- Porcentaje de Distribución ← detectar valor existente; no inventar
- Precio Unitario Adjudicado ← precio unitario
- Atributo ← atributo
- Descripción Atributo ← valor del atributo

No inventar valores que no estén disponibles en el Excel.

## Continuidad

Una nueva instancia debe leer este archivo primero y después revisar el código actual del repositorio. El trabajo debe continuar desde el estado real de GitHub, no reconstruirse desde cero.
