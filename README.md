💓 CardioVision ECG
Sistema Inteligente de Clasificación de Arritmias Cardíacas
Universidad del Magdalena · Ingeniería Electrónica · Machine Learning · Proyecto 3
---
Descripción
CardioVision ECG es una aplicación de escritorio que clasifica arritmias cardíacas a partir de señales ECG usando un pipeline híbrido CNN (extractor de features) + SVM (clasificador). El sistema integra dos fuentes de datos complementarias: señales numéricas en CSV e imágenes ECG, permitiendo comparar el desempeño de ambas representaciones.
---
Requisitos
Python 3.10 o superior
Windows / macOS / Linux
---
Instalación
```bash
pip install -r requirements.txt
```
---
Estructura del proyecto
```
proyecto/
├── app.py                  ← Aplicación principal
├── requirements.txt        ← Dependencias
├── README.md               ← Este archivo
├── resultados/             ← Artefactos generados automáticamente
│   ├── features_csv.csv
│   ├── features_cnn.csv
│   ├── experimentos_log.json
│   ├── comparacion_modelos.png
│   └── confusion_matrices.png
├── ECG csv/                ← Dataset 1: señales numéricas
│   ├── mitbih_train.csv
│   ├── mitbih_test.csv
│   ├── ptbdb_normal.csv
│   └── ptbdb_abnormal.csv
└── ECG imagenes/           ← Dataset 2: imágenes ECG
    ├── train/
    │   ├── N/
    │   ├── S/
    │   ├── V/
    │   ├── F/
    │   └── Q/
    └── test/
        ├── N/
        ├── S/
        ├── V/
        ├── F/
        └── Q/
```
---
Ejecución
```bash
python app.py
```
---
Cómo usar la aplicación
1. Cargar datasets
Clic en 📁 junto a DATASET CSV → selecciona la carpeta `ECG csv/`
Clic en 📁 junto a DATASET IMÁGENES → selecciona la carpeta `ECG imagenes/`
2. Ajustar parámetros (opcional)
Muestras/clase: cantidad de señales por clase para entrenamiento (500–5000)
Épocas CNN: ciclos de entrenamiento de la red neuronal (5–30)
SMOTE: balanceo de clases desbalanceadas
3. Entrenar
Clic en ⚙️ Entrenar todo
El sistema entrena 3 modelos en secuencia:
Random Forest (línea base)
SVM sobre señales CSV
CNN + SVM sobre imágenes ECG
4. Revisar resultados
Pestaña Datos: distribución del dataset
Pestaña Evaluación: métricas comparativas, matrices de confusión, reporte por clase
5. Diagnosticar
📊 Predecir desde señal CSV: carga un .csv con una señal ECG de 187 puntos
🖼️ Predecir desde imagen ECG: carga una imagen JPG/PNG de un trazado ECG
6. Guardar modelo
Clic en 💾 Guardar modelos para reutilizar sin reentrenar
---
Datasets utilizados
Dataset	Fuente	Licencia
ECG Heartbeat Categorization (MIT-BIH)	Kaggle: shayanfazeli/heartbeat	CC0
ECG Heart Categorization Image Version	Kaggle: mohamedeldakrory8/ecg-heart-categorization-dataset-image-version	CC0
Clases de arritmia
Clase	Nombre	Descripción
N	Normal	Latido normal sin anomalía
S	Supraventricular	Puede indicar fibrilación auricular
V	Ventricular	Contracción ventricular prematura
F	Fusión	Dos impulsos eléctricos simultáneos
Q	Desconocido	Morfología no clasificable
---
Arquitectura del sistema
```
Dataset CSV (señales)          Dataset Imágenes (ECG)
       ↓                               ↓
Extracción features            Preprocesamiento
estadísticas + FFT             (resize 64×64)
       ↓                               ↓
  Normalización              CNN (3 bloques Conv2D)
       ↓                    GlobalAveragePooling → 128D
  SVM (kernel RBF)                     ↓
       ↓                        features_cnn.csv
  Predicción B                         ↓
                              SVM (kernel RBF)
                                       ↓
                                 Predicción A
                                       ↓
                    Comparación A vs B → Mejor modelo
```
---
Artefactos generados
Archivo	Descripción
`resultados/features_csv.csv`	Features estadísticas extraídas de señales
`resultados/features_cnn.csv`	Features extraídas por la CNN (128D)
`resultados/experimentos_log.json`	Registro de métricas por experimento
`resultados/comparacion_modelos.png`	Gráfico comparativo de los 3 modelos
`resultados/confusion_matrices.png`	Matrices de confusión normalizadas
`cardiovision_modelos.joblib`	Modelos entrenados (SVM + RF + scalers)
`cardiovision_modelos_cnn.keras`	Red neuronal CNN entrenada
---
Métricas de evaluación
Accuracy: proporción de predicciones correctas
F1-macro: promedio de F1 por clase (penaliza clases con bajo recall)
Balanced Accuracy: accuracy ponderado por clase
MCC: Matthews Correlation Coefficient (robusto ante desbalance)
Matriz de confusión: normalizada por clase real
---
Advertencia
Este sistema es una herramienta académica de apoyo. No reemplaza el diagnóstico de un cardiólogo certificado. Los resultados no deben usarse para decisiones clínicas reales.
---
Autores
Francisco Fragoso - Mateo Cubillo
Proyecto 3 — Machine Learning  
Ingeniería Electrónica  
Universidad del Magdalena  
Profesor: Ing. Rafael David Linero Ramos, PhD(c)
