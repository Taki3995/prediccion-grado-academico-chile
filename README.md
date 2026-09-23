# Modelado Predictivo de Grado en Educación Superior en Chile

Pipeline de Machine Learning para predecir el tipo de grado académico o titulación obtenida (`nomb_grado_obtenido`) combinando el historial de egreso de Educación Media (2015–2022) y los registros de Educación Superior (2023) provenientes de los datos abiertos del Ministerio de Educación de Chile (Mineduc).

---

## Descripción del Proyecto

El objetivo es modelar la trayectoria formativa de los estudiantes chilenos identificando patrones entre su desempeño y contexto en la enseñanza secundaria (promedio de notas, ubicación geográfica del establecimiento) y sus características al titularse en educación superior.

El pipeline realiza la extracción por lotes (*chunking*), optimización del uso de memoria, limpieza de identificadores, codificación categórica y entrenamiento comparativo entre clasificadores lineales y ensambles basados en árboles.

---

## Fuentes de Datos

Los datos provienen del portal de Datos Abiertos de Mineduc:
* **Titulados de Educación Superior (2023):** Registros de títulos y grados obtenidos, modalidad, jornada y áreas de estudio.
* **Notas y Egresados de Enseñanza Media (2015–2022):** Registros históricos de egreso secundario, notas finales (`PROM_NOTAS_ALU`) y datos comunales/regionales.

> **Nota:** Por motivos de peso y privacidad, los datasets no se incluyen en el repositorio. Deben descargarse y situarse en Google Drive o el directorio local configurado.

---

## Estructura del Pipeline

1. **Optimización de memoria (`reduce_memory_usage`):** Conversión dinámica de tipos numéricos (`downcast`) para manejar múltiples archivos de gran volumen.
2. **Carga particionada:** Lectura secuencial mediante `chunksize` para prevenir desbordamientos de RAM.
3. **Preprocesamiento e Ingeniería de Características:**
   * Limpieza y normalización de calificaciones (`PROM_NOTAS_ALU`).
   * Imputación de valores ausentes en variables geográficas.
   * Codificación de etiquetas (`LabelEncoder`) en variables categóricas.
   * Eliminación de identificadores directos redundantes.
4. **Cruce de datos:** `merge` interior entre egresados de media y titulados mediante `mrun`.
5. **Entrenamiento y Evaluación:**
   * `RandomForestClassifier` (ensamble no lineal).
   * `SGDClassifier` (clasificación lineal optimizada por descenso de gradiente estocástico).

---

## Requisitos e Instalación

Clona el repositorio e instala las dependencias necesarias:

```bash
git clone [https://github.com/tu-usuario/prediccion-grado-academico-chile.git](https://github.com/tu-usuario/prediccion-grado-academico-chile.git)
cd prediccion-grado-academico-chile
pip install -r requirements.txt
