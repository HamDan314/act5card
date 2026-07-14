# Actividad 5 — Entrenamiento, ajuste y registro con MLflow

## 1. Objetivo

Comparar dos algoritmos de clasificación binaria:

1. **Regresión logística**
2. **Random forest**

Ambos modelos se entrenan mediante un `Pipeline`, se ajustan con
`GridSearchCV`, se evalúan mediante validación cruzada estratificada de cinco
pliegues y se registran en MLflow.

## 2. Dataset

Se utiliza **Breast Cancer Wisconsin Diagnostic**, incluido en
`scikit-learn`. El problema consiste en clasificar tumores como malignos o
benignos a partir de 30 variables numéricas calculadas sobre imágenes
digitalizadas de aspirados con aguja fina.

- Registros: 569
- Predictores: 30
- Objetivo: `diagnostico`
- Clases: `0 = maligno`, `1 = benigno`

El archivo original se encuentra en:

```text
datos/datos_ini/cancer_mama_original.csv
```

La versión procesada se genera en:

```text
datos/datos_limp/cancer_mama_limpio.csv
```

## 3. Estructura

```text
Actividad5/
├── datos/
│   ├── datos_ini/
│   │   ├── cancer_mama_original.csv
│   │   └── diccionario_clases.csv
│   └── datos_limp/
│       └── cancer_mama_limpio.csv
├── fuentes/
│   ├── entrena.ipynb
│   ├── datos_prep.py
│   └── train.py
├── resultados/
├── mlruns/
├── .gitignore
├── requirements.txt
├── README.md
└── CHANGELOG.md
```

## 4. Métricas estandarizadas

- Accuracy
- Precision
- Recall
- F1-score
- ROC-AUC
- Tiempo de entrenamiento

El criterio principal para seleccionar hiperparámetros es **F1-score**, porque
equilibra precision y recall. La evaluación final se realiza sobre un conjunto
de prueba separado del 20 %.

## 5. Validación cruzada y ajuste

Se usa `StratifiedKFold` con cinco pliegues, mezcla aleatoria y semilla 42.
La estratificación conserva aproximadamente la proporción de clases en cada
pliegue.

### Regresión logística

- `C`: 0.1 y 1.0
- `solver`: liblinear
- `class_weight`: sin ponderación y balanced

### Random forest

- `n_estimators`: 20 y 50
- `max_depth`: sin límite y 10
- `min_samples_split`: 2 y 5
- `class_weight`: sin ponderación y balanced

## 6. Ejecución local

```bash
git clone URL_DEL_REPOSITORIO
cd Actividad5

python -m venv .venv
```

Activación en Windows:

```bash
.venv\Scripts\activate
```

Activación en Linux/macOS:

```bash
source .venv/bin/activate
```

Instalación y ejecución:

```bash
pip install -r requirements.txt
python fuentes/train.py
```

Abrir MLflow:

```bash
mlflow ui --backend-store-uri sqlite:///mlflow.db --port 5000
```

Después, abrir `http://127.0.0.1:5000`.

## 7. Ejecución en Google Colab

1. Subir el repositorio a GitHub.
2. Abrir `fuentes/entrena.ipynb` en Colab.
3. Cambiar la variable `REPO_URL`.
4. Ejecutar las celdas en orden.
5. Descargar la carpeta `resultados` y, si se desea, `mlruns`.

> Este ejercicio usa modelos clásicos de scikit-learn; no requiere GPU.
> Colab puede ejecutarlo con el entorno estándar de CPU.

## 8. Resultados generados

El script crea:

- `resultados/comparacion_modelos.csv`
- `resultados/mejor_modelo.json`
- Resultados completos de cada GridSearchCV
- Matrices de confusión
- Modelos serializados con Joblib
- Runs, parámetros, métricas y modelos en MLflow

## 9. Decisiones técnicas

- Se usa una partición de prueba independiente para reducir el optimismo de la
  validación cruzada.
- El escalamiento se incluye dentro del pipeline para evitar fuga de datos.
- Random forest se conserva en el mismo pipeline para mantener una interfaz
  uniforme, aunque el escalamiento no es indispensable para árboles.
- Se fija `random_state=42` para reproducibilidad.
- Se registran manualmente métricas y artefactos para controlar exactamente qué
  se almacena en MLflow.

## 10. Publicación en GitHub

```bash
git init
git add .
git commit -m "Actividad 5: modelos, validación cruzada y MLflow"
git branch -M main
git remote add origin URL_DEL_REPOSITORIO
git push -u origin main
```


## 11. Resultados reproducibles incluidos

El repositorio incluye `resultados/comparacion_modelos.csv`, matrices de confusión, resultados de GridSearchCV y los modelos serializados. En la ejecución validada, la regresión logística obtuvo F1=0.9861 y ROC-AUC=0.9960; random forest obtuvo F1=0.9583 y ROC-AUC=0.9939.
