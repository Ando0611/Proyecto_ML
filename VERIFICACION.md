# Revisión del Proyecto Aplicado

Se revisaron el notebook original, el CSV y `/Users/ivan/Downloads/Proyecto Aplicado - Instrucciones.docx`. El original `Proyecto.ipynb` se conservó intacto. Los archivos preparados están en `entregables/`.

## Resultado de la comprobación

La copia corregida ejecutó las 30 celdas de código desde un espacio de variables vacío, sin errores ni advertencias, y generó 12 gráficas. Se usó Python 3.12.14 y las versiones de `requirements.txt`. La ejecución fue secuencial en el mismo proceso, con captura de salidas, tablas y figuras; no se reutilizaron las salidas guardadas del original. Las comprobaciones del notebook validan separación de índices, conteos de las matrices y rango de métricas. El Word final tiene 4 páginas y fue renderizado y revisado visualmente.

| Requisito de las instrucciones | Evidencia en la entrega |
| --- | --- |
| Dataset público, objetivo categórico, al menos 200 observaciones y 4 predictores | Kaggle enlazado en Word y notebook; 5,110 registros, objetivo stroke 0/1 y 10 predictores originales |
| Fuente citable y dataset permitido | Enlace de fedesoriano en Kaggle; no aparece en la lista explícita de datasets prohibidos |
| EDA específico | Tipos, BMI faltante, tabaquismo Unknown, desbalance, distribuciones y asociaciones con el objetivo |
| LDA o QDA justificado | Histogramas y QQ plots, covarianzas, comparación CV y explicación de la elección de LDA como referencia lineal |
| KNN con K por CV | K igual a 501, cinco predictores; imputación y escalado dentro del Pipeline |
| Árbol podado por CV | Alpha 0.0000056234 y mínimo 200 registros por hoja elegidos por CV; 10 hojas, profundidad 5 |
| Ensamble | Random Forest con 300 árboles, seis variables por corte y mínimo 50 por hoja; Gradient Boosting adicional ajustado por CV |
| Mismo train/test split | 75/25 estratificado, semilla 42; 3,831/1,278 registros y 187/62 positivos |
| Precision, recall, F1 y AUC | Tablas con ambas configuraciones de umbral, matrices, ROC y precision recall; AP adicional |
| Métrica principal justificada | AUC para ordenación; análisis del punto de operación y desbalance con precision, recall y F1 |
| Generalización y supuestos | Comparación train/CV/test, sesgo y varianza, normalidad, decorrelación e incertidumbre bootstrap pareada |
| Notebook comentado y reproducible | Ejecución completa, semilla, hash de datos, Pipelines y versiones fijadas |
| Reporte de 2 a 4 páginas sin código | Word de 4 páginas con conceptos, EDA, resultados, hipótesis, conclusiones y mejoras |
| Modelos permitidos | No hay redes neuronales, SVM ni AutoML |
| Autor de los metadatos | Autor y último editor del Word: Ivan Reyes Pedroza; notebook con autor y equipo |

## Qué se corrigió

- La imputación de BMI se aprende dentro de cada fold, no sobre todo train antes de CV.
- Las categorías se fijan con train y se aplican a test con el mismo esquema.
- La rejilla de poda se fija previamente y no usa etiquetas de los folds para proponer alphas.
- La selección de LDA tiene una justificación explícita; se reconoce que QDA con tres continuas alcanza una media CV mayor.
- Se distingue la selección por CV, el mayor AUC observado en test y los ganadores por F1 y recall.
- Los umbrales se eligen con probabilidades OOF de train. Se explica que, como los hiperparámetros ya se seleccionaron en train, esto no es CV anidada.
- Se añadieron conclusiones calculadas desde la ejecución e intervalos bootstrap de AUC y diferencias pareadas.

## Resultados verificados

| Modelo | AUC CV | AUC test | Precision | Recall | F1 |
| --- | ---: | ---: | ---: | ---: | ---: |
| LDA | 0.832 | 0.836 | 0.232 | 0.516 | 0.320 |
| KNN | 0.838 | 0.829 | 0.193 | 0.710 | 0.303 |
| Árbol podado | 0.830 | 0.815 | 0.208 | 0.419 | 0.278 |
| Random Forest | 0.844 | 0.836 | 0.197 | 0.677 | 0.305 |
| Gradient Boosting | 0.842 | 0.840 | 0.199 | 0.694 | 0.309 |

Las métricas del punto de operación corresponden al umbral escogido en train para cada modelo. Gradient Boosting tiene la mayor AUC observada en test; Random Forest se selecciona por AUC CV antes de consultar test. El intervalo bootstrap pareado del 95 % para AUC de Gradient Boosting menos Random Forest es [-0.0077, 0.0174]; no se afirma superioridad definitiva.

## Condiciones que no se pueden certificar desde los archivos

Las instrucciones piden trabajo en parejas y nombres con dos apellidos. Por indicación expresa del usuario se conservaron los tres integrantes y los tres apellidos en los nombres de archivo. Esto requiere que el profesor acepte esa excepción; no se presume su autorización.

No se puede verificar si el dataset fue usado en alguna clase fuera de la lista prohibida, si los archivos representan íntegramente trabajo propio del equipo, ni que la redacción refleje su comprensión personal. Las presentaciones de las Clases 17, 19 y 21 no están en el repositorio; el reporte conecta con el temario indicado en las instrucciones. Cada integrante debe revisar el contenido y poder explicarlo. Ninguna revisión técnica garantiza una calificación.

La entrega indicada es el jueves 15 de octubre de 2026 antes de las 11:59 pm en la plataforma del curso. No se realizó envío a esa plataforma.

## Cómo reproducirlo

1. Crear un entorno Python 3.12 e instalar `pip install -r requirements.txt`.
2. Mantener `healthcare-dataset-stroke-data.csv` junto al notebook o un nivel arriba, como está en este repositorio. Fuente: https://www.kaggle.com/datasets/fedesoriano/stroke-prediction-dataset
3. Abrir el notebook corregido con un kernel del entorno y ejecutar todas las celdas en orden desde un kernel vacío.
4. Si sólo se comparten los dos archivos de entrega, descargar también el CSV para reproducir los modelos.

Subir como entregables el notebook corregido y el Word. Este archivo de revisión y `requirements.txt` son apoyo para la comprobación local.
