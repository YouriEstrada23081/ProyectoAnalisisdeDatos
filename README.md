# Predicción Temprana de Sepsis en UCI

## Descripción del Problema Clínico
La sepsis es una respuesta extrema del cuerpo a una infección y constituye una emergencia médica potencialmente mortal. En la Unidad de Cuidados Intensivos (UCI), el deterioro puede ser rápido. El desafío radica en que los síntomas iniciales pueden confundirse con otras condiciones, retrasando el tratamiento. 

## Decisión Clínica a Apoyar
Este modelo de Machine Learning busca identificar patrones en los signos vitales y resultados de laboratorio (HR, O2Sat, Temp, etc.) para **predecir el riesgo de sepsis antes de 6 horas**. Esto apoya al personal médico en la decisión de **iniciar la administración temprana de antibióticos de amplio espectro**, lo cual es crítico para la supervivencia del paciente.

## Instrucciones de Acceso a los Datos
Por el tamaño y naturaleza de los datos, el dataset original no está incluido en este repositorio.
1. Crear una cuenta en [Kaggle](https://www.kaggle.com/).
2. Descargar el dataset "Prediction of Sepsis" desde este enlace: [https://www.kaggle.com/datasets/salikhussaini49/prediction-of-sepsis/data](https://www.kaggle.com/datasets/salikhussaini49/prediction-of-sepsis/data)
3. Guardar el archivo extraído como `Dataset.txt` en la carpeta `/data/` de este repositorio.

## Instrucciones de Setup
1. Clonar este repositorio.
2. Crear un entorno virtual (Python 3.9+ recomendado).
3. Ejecutar: `pip install -r requirements.txt`
4. Ejecutar el notebook `notebook_sepsis.ipynb`.
