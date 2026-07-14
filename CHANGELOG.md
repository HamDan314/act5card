# CHANGELOG

## [1.0.0] - 2026-07-12

### Añadido

- Dataset original Breast Cancer Wisconsin Diagnostic.
- Diccionario de clases.
- Flujo reproducible de limpieza en `datos_prep.py`.
- Comparación de regresión logística y random forest.
- Ajuste de hiperparámetros mediante GridSearchCV.
- Validación cruzada StratifiedKFold de cinco pliegues.
- Métricas accuracy, precision, recall, F1 y ROC-AUC.
- Registro de parámetros, métricas, artefactos y modelos en MLflow.
- Notebook compatible con Google Colab.
- Documentación de instalación, ejecución y publicación en GitHub.

### Decisiones

- F1-score se utiliza como criterio de selección.
- Se reserva 20 % de los datos para evaluación final.
- Se incorpora el preprocesamiento dentro de los pipelines.
- Se fija semilla 42 para reproducibilidad.

### Resultado esperado

Al ejecutar `python fuentes/train.py`, se genera una tabla comparativa y se
registra un run por algoritmo. El algoritmo ganador se determina por el
F1-score del conjunto de prueba.


## [1.1.0] - 2026-07-13

### Actualizado

- Se integró el notebook final con auditoría de calidad, versionamiento SHA-256 y visualizaciones exploratorias.
- Se cambió el backend predeterminado de MLflow a SQLite para compatibilidad con MLflow 3.x.
- Se compactó la cuadrícula de hiperparámetros para ejecución reproducible en Google Colab.
- Se añadieron resultados reales en `resultados/comparacion_modelos.csv`.
- Se incorporaron matrices de confusión, modelos serializados y resultados detallados de GridSearchCV.
