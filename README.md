# UOC — Generador DNCP CSV

Base inicial del generador universal de CSV para DNCP.

## Objetivo

El Excel de origen puede cambiar completamente de diseño: múltiples hojas, encabezados desplazados, columnas en distinto orden y distintas formas de presentar la información. El sistema analiza el contenido y normaliza los datos antes de generar la salida.

La plantilla oficial DNCP es el contrato de salida y no debe alterarse. La siguiente etapa incorpora la plantilla CSV exacta como recurso maestro para que el generador produzca exactamente sus columnas, orden, textos y formato.

## Archivos

- `index.html`: interfaz.
- `src/app.js`: lectura, detección y normalización del Excel.
- `src/styles.css`: interfaz.

## Nota

La generación actual es una base de análisis y exportación. No se considera la versión final de producción hasta integrar la plantilla DNCP proporcionada como plantilla maestra de salida.
