# Análisis de requerimientos de equipos de poder a partir de quejas y sugerencias (QRS)

**Sector:** dispositivos médicos · ortopedia y trauma
**Rol:** Analista de datos (proyecto de punta a punta)
**Herramientas:** Python (pandas, NumPy, Matplotlib, Seaborn) · SharePoint (API REST) · Excel · Google Colab · HTML/CSS/JavaScript (Pyodide) · IA generativa como asistente de análisis

> Los nombres de clientes, instituciones, especialistas y pacientes se omiten por confidencialidad. Las cifras se presentan en porcentajes y órdenes de magnitud.

---

## 1. Contexto y necesidad

Una empresa fabricante de dispositivos médicos para cirugía de trauma presta en cada procedimiento sus **equipos de poder**: motores, perforadores y sierras quirúrgicas. Parte del inventario sigue siendo **neumático** (funciona con manguera y bala de nitrógeno), mientras los cirujanos prefieren cada vez más los **eléctricos de batería**.

Esas inconformidades llegaban por el sistema de **Quejas, Reclamos y Sugerencias (QRS)** como texto libre, mezcladas con cientos de reportes sobre otros temas. Nadie sabía cuántas eran, dónde se concentraban ni qué tipo de equipo pedían.

**Pregunta de negocio:** ¿cuál es la demanda real de motores eléctricos, en qué tipo de equipo (Pequeños o Grandes Fragmentos) y dónde se concentra, para sustentar una decisión de inversión?

---

## 2. Paso a paso

### 2.1 Extracción de datos
- Localicé la fuente vigente: una lista de SharePoint Online que alimenta la consulta corporativa de QRS. Las copias en Excel estaban desactualizadas o eran libros con macros que no se podían leer.
- Extraje la lista completa (≈1.000 registros) por medio de la **API REST de SharePoint**, con paginación y selección de campos.
- Filtré el periodo de análisis (enero a octubre) por la fecha de reporte: **≈500 registros**.

### 2.2 Limpieza y control de calidad
- Trabajé siempre sobre una **copia**, sin modificar la fuente.
- Normalicé espacios y formatos de texto, sin alterar el contenido original.
- **Deduplicación** con una regla explícita: un registro es duplicado si coinciden la descripción, la institución y la remisión. Los registros excluidos quedaron documentados en una hoja de trazabilidad.
- Documenté los campos vacíos, por ejemplo remisiones sin dato, en lugar de imputarlos.

### 2.3 Clasificación semántica del texto libre
- Leí las ≈500 descripciones con **apoyo de IA generativa**, con un primer filtro de palabras clave y una lectura completa del texto. La búsqueda por palabras sola no basta: muchas necesidades se expresan sin términos técnicos.
- Definí **5 categorías** con criterios escritos de inclusión y exclusión: falla del equipo, preferencia por motor eléctrico, accesorios, logística y mantenimiento.
- Resultado: **≈1 de cada 3 QRS** del periodo se relacionaba con equipos de poder. El **37 %** de esos reportes eran solicitudes explícitas de motor eléctrico o rechazo del neumático.
- Creé una variable derivada, **"advertencia de escalamiento"**, con expresiones regulares (regex) sobre la descripción. Marca los reportes donde el cliente advierte cancelar la cirugía o cambiar de proveedor.

### 2.4 Enriquecimiento con un segundo sistema
- Crucé cada número de remisión con una consulta del sistema de despachos para identificar el **tipo de equipo de poder** enviado (Pequeños o Grandes Fragmentos).
- Unifiqué ambas fuentes en un solo dataset por medio de la llave de remisión.

### 2.5 Análisis exploratorio (EDA)
Formulé **9 preguntas de negocio** y elegí la visualización adecuada para cada una:

| # | Pregunta | Visualización |
|---|---|---|
| 1 | ¿Qué proporción corresponde a Pequeños vs. Grandes Fragmentos? | Barras horizontales con % |
| 2 | ¿Qué ciudades concentran los requerimientos? | Barras apiladas |
| 3 | ¿Qué instituciones son recurrentes? | Top 10 en barras apiladas |
| 4 | ¿Cómo evolucionó la demanda mes a mes? | Líneas con área del total |
| 5 | ¿En qué sistemas quirúrgicos se pide el motor eléctrico? | Barras apiladas |
| 6 | ¿Qué pagadores están detrás de esas cirugías? | Barras ordenadas |
| 7 | ¿Dónde y cuándo se concentran? | Mapa de calor ciudad × mes |
| 8 | ¿Cuántos reportes implican riesgo de perder la cirugía? | Barras 100 % apiladas |
| 9 | ¿Pocas instituciones explican la mayoría? | Diagrama de Pareto |

### 2.6 Visualización y comunicación
- Construí los gráficos con **Matplotlib y Seaborn** en la paleta corporativa, con título, subtítulo, etiquetas de valor y fuente al pie en cada uno.
- Preparé un **informe ejecutivo en PDF** para la alta dirección, con una página por pregunta: pregunta, respuesta, gráfico y detalle.
- Documenté el proceso en un **notebook de Google Colab** reproducible.
- Construí un **cuaderno interactivo en HTML/CSS/JavaScript**. Ejecuta el código Python en el navegador con Pyodide y muestra junto a cada celda su pregunta, resultado y conclusión.

---

## 3. Resultados

- El **75 %** de las solicitudes de motor eléctrico corresponde a equipos de **Grandes Fragmentos** (clavos endomedulares de fémur y tibia) y el **25 %** a Pequeños Fragmentos.
- **Dos ciudades de una misma región concentran el 58 %** de los requerimientos.
- El último mes del periodo registró la **demanda más alta**: cerca del triple del promedio mensual.
- El **27 %** de los reportes incluye una **advertencia de cancelar la cirugía o pedir el material a otro proveedor**. Ese porcentaje es mayor en Pequeños Fragmentos (36 %).
- **Dos pagadores** concentran el 45 % de las cirugías afectadas.

**Impacto:** la empresa pasó de tener comentarios dispersos en texto libre a contar con evidencia cuantificada y rastreable hasta cada registro, para priorizar dónde y en qué tipo de equipo invertir.

---

## 4. Habilidades demostradas

- Extracción de datos desde APIs empresariales (SharePoint REST)
- Limpieza, deduplicación y trazabilidad de datos
- Análisis de texto libre y clasificación con criterios documentados
- Integración de fuentes por llave común
- Análisis exploratorio orientado a preguntas de negocio
- Visualización profesional (Matplotlib, Seaborn) y elección del gráfico según la pregunta
- Comunicación de hallazgos a la alta dirección (storytelling con datos)
- Uso de IA generativa para aumentar la productividad del análisis
- Conocimiento del dominio: calidad, tecnovigilancia e ISO 13485

---

## 5. Archivos del repositorio

| Archivo | Contenido |
|---|---|
| `README.md` | Descripción del proyecto |
| `dataset_anonimizado.csv` | 55 requerimientos con datos codificados |
| `analisis_equipos_poder.ipynb` | Notebook ejecutado, con resultados y gráficos |
| `01_…png` a `09_…png` | Las 9 visualizaciones del análisis |

### Diccionario de datos (`dataset_anonimizado.csv`)

| Columna | Descripción |
|---|---|
| `ID` | Consecutivo del registro |
| `ANIO` / `MES` | Año y mes del reporte (AAAA-MM) |
| `CIUDAD` | Ciudad codificada (`Ciudad_A` = la de mayor frecuencia) |
| `INSTITUCION` | Institución de salud codificada (`Inst_01` = la de mayor frecuencia) |
| `PAGADOR` | Entidad pagadora codificada (`Pagador_01` = la de mayor frecuencia) |
| `SISTEMA` | Sistema de osteosíntesis, con nombre genérico |
| `REMISION` | Remisión codificada (`REM_001`); vacía si el registro original no la tenía |
| `TIPO_EQUIPO_PODER` | Grandes Fragmentos / Pequeños Fragmentos |
| `ADVERTENCIA` | Sí / No: el reporte advierte cancelar la cirugía, no operar o pedir el material a otro proveedor |

### Cómo ejecutarlo
- **Local:** `pip install pandas matplotlib seaborn jupyter`, dejar `dataset_anonimizado.csv` en la misma carpeta y abrir `analisis_equipos_poder.ipynb`.
- **Google Colab:** abrir el notebook desde GitHub (*Archivo → Abrir cuaderno → GitHub*) y ejecutar todas las celdas. Si el repositorio es público, el CSV se descarga solo; si es privado, súbelo cuando se solicite.

---

*Autora: Starleen Gaviria Medina · [LinkedIn](https://www.linkedin.com/in/starleen-gaviria-analytics/) · [GitHub](https://github.com/Starleen1996)*
