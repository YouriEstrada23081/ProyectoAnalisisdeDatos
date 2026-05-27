# Detección Temprana de Sepsis en UCI
### Proyecto Integrador Fase 2 — Análisis de Datos Biomédicos (BE3006)
**Universidad del Valle de Guatemala · 2026**

**Grupo 1:** Victor Soto · Luis Rodríguez · Amy Velásquez · Youri Estrada

---

## Decisión Clínica a Apoyar

> **¿Es posible predecir el onset de sepsis con 6 horas de anticipación en pacientes de UCI, usando únicamente datos disponibles en el EHR (signos vitales, laboratorios, datos demográficos)?**

**Decisión apoyada:** activar el protocolo Sepsis-3 (cultivos + antibióticos + reanimación con fluidos) antes de que el deterioro sea clínicamente evidente, reduciendo la mortalidad asociada al diagnóstico tardío.

**Justificación clínica:** por cada hora de retraso en el inicio de antibióticos en shock séptico, la mortalidad aumenta entre 3.6% y 9.9%. Un sistema de alerta temprana basado en datos reduce ese retraso al identificar patrones fisiológicos sutiles antes de que sean visibles clínicamente.

---

## Estructura del Repositorio

```
ProyectoAnalisisdeDatos/
├── notebooks/
│   └── sepsis_prediction_finalV2.ipynb   # Pipeline end-to-end completo
├── data/
│   └── (dataset no incluido — ver instrucciones de acceso abajo)
├── requirements.txt                       # Dependencias pinneadas
└── README.md
```

> **Nota:** Los datos **no están incluidos** en el repositorio por restricciones de uso del dataset PhysioNet. Ver la sección de acceso al dataset más abajo.

---

## Pipeline del Notebook

El notebook recorre las siguientes etapas en orden, cada una con evidencia explícita:

| Sección | Contenido |
|---|---|
| **1. Adquisición y Gobernanza** | Descripción de la fuente, consideraciones éticas (HIPAA), mapeo a estándares LOINC / SNOMED CT / OMOP CDM |
| **2. Preparación y Curación** | Auditoría del dataset, rangos fisiológicos válidos, análisis de faltantes, imputación forward-fill por paciente + mediana por fold de CV |
| **3. EDA** | Distribución de estadía, signos vitales por clase, evolución temporal 12h pre-onset, Mutual Information, heatmap, análisis MAR/MNAR, baseline NEWS |
| **4. Feature Engineering** | Label de 6h de anticipación (vectorizado), tendencia 6h (`trend6`), variabilidad 1h (`delta1`) sin data leakage |
| **5. Modelado** | Regresión Logística (baseline) + XGBoost con `scale_pos_weight`, `StratifiedGroupKFold` por paciente |
| **6. Validación** | Curvas ROC y Precision-Recall, matriz de confusión, calibración de threshold, importancias promedio por fold |
| **7. Cierre del Loop Clínico** | Trazabilidad pregunta → decisión, propuesta de implementación, limitaciones |

---

## Resultados Principales

| Modelo | AUROC | AUPRC | Recall (th=0.30) |
|---|---|---|---|
| NEWS Score (baseline clínico) | ~0.65 | ~0.08 | — |
| Regresión Logística | ~0.78 ±0.02 | ~0.18 ±0.02 | ~0.68 ±0.03 |
| **XGBoost** | **~0.84 ±0.01** | **~0.28 ±0.02** | **~0.75 ±0.02** |

> Valores aproximados. Ejecutar el notebook con el dataset completo para resultados exactos.

**Threshold elegido: 0.30** — prioriza sensibilidad (Recall) porque un falso negativo en sepsis tiene consecuencias clínicas más graves que un falso positivo.

---

## Acceso al Dataset

El dataset **no está incluido** en este repositorio por las restricciones de uso del PhysioNet Credentialed Health Data License.

**Opción A — Kaggle (Utilizada en el proyecto y recomendación):**
1. Crear cuenta o iniciar sesión en [kaggle.com](https://www.kaggle.com)
2. Descargar el dataset: [https://www.kaggle.com/datasets/salikhussaini49/prediction-of-sepsis](https://www.kaggle.com/datasets/salikhussaini49/prediction-of-sepsis)
3. Colocar el archivo CSV en `data/Dataset.csv` o colocarlo donde usted desee puesto que se va a cambiar la ruta de manera manual.Donde sea que haya colocado el archivo solo tiene que hacer click derecho y seleccionar la opción de copy as a path
4. En el notebook (Cell 4), actualizar la variable `DATA_PATH` con la ruta a tu archivo copiada, se encuentran más comentarios en el notebook sobre esto.

**Opción B — PhysioNet (fuente oficial):**
1. Crear cuenta en [physionet.org](https://physionet.org)
2. Solicitar acceso: [https://physionet.org/content/challenge-2019/1.0.0/](https://physionet.org/content/challenge-2019/1.0.0/)
3. Los datos originales son archivos `.psv` individuales por paciente; el CSV de Kaggle es una versión ya consolidada,por eso se recomienda usar la otra opción.

---

## Setup del Entorno

### Requisitos
- Python 3.11 o superior
- pip

### Instalación

```bash
# 1. Clonar el repositorio
git clone https://github.com/<tu-usuario>/sepsis-detection.git
cd sepsis-detection

# 2. Instalar dependencias
pip install -r requirements.txt

# 4. Abrir el notebook
jupyter notebook notebooks/sepsis_prediction_finalV2.ipynb
```

### Instalación rápida (sin entorno virtual)

```bash
pip install pandas==3.0.2 numpy==2.4.4 matplotlib==3.10.8 seaborn==0.13.2 scikit-learn==1.8.0 xgboost==3.2.0 jupyter ipykernel
```

### Verificación del entorno

Ejecutar la **Cell 2** del notebook. Si imprime `Librerías cargadas correctamente` sin errores, el entorno está listo.

---

## Consideraciones Éticas

- **Deidentificación:** todos los datos están deidentificados conforme a HIPAA. No contienen nombres, fechas absolutas ni identificadores directos
- **Consentimiento:** publicados con autorización institucional de los hospitales de origen para uso en investigación
- **Restricciones:** el dataset no puede redistribuirse sin credenciales PhysioNet
- **Sesgo potencial:** datos de UCI de EE.UU. — puede no generalizar directamente a contextos guatemaltecos o latinoamericanos sin reentrenamiento local

---

## Fuente de Datos

Reyna, M., Josef, C., Seyedi, S., Jeter, R., Shashikumar, S. P., Westover, M. B., ... & Clifford, G. (2019).
**Early Prediction of Sepsis from Clinical Data — the PhysioNet Computing in Cardiology Challenge 2019.**
*PhysioNet.* [https://doi.org/10.13026/v64v-d857](https://doi.org/10.13026/v64v-d857)
