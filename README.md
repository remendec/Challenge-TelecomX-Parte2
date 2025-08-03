# Challenge-TelecomX-Parte2
Proyecto de Predicción de Churn en TelecomX

Propósito del Análisis

Este proyecto tiene como objetivo principal desarrollar un modelo predictivo para anticiparse a la cancelación (churn) de clientes de TelecomX, una empresa del sector de telecomunicaciones. Utilizando un conjunto de datos en formato JSON que incluye información demográfica, de servicios y de facturación, el análisis busca identificar patrones y variables relevantes que influyan en la decisión de los clientes de abandonar el servicio. El modelo Random Forest implementado proporciona una precisión del 80% y un recall del 47% para detectar abandonos, permitiendo a TelecomX implementar estrategias de retención personalizadas y proactivas.

Estructura del Proyecto y Organización de Archivos
- Telecom X_Parte2.ipynb: El cuaderno principal que contiene todo el código y las visualizaciones. Está organizado en celdas numeradas (1 a 7) que cubren la importación de datos, preparación, análisis exploratorio, modelado y conclusiones.
- data/ (opcional): Carpeta donde se puede guardar el archivo JSON original (TelecomX_Data.json) o un CSV procesado (telecomx_data_processed.csv) si se exportan los datos tratados.
- visualizations/ (opcional): Carpeta para almacenar gráficos generados (por ejemplo, matriz de correlación, distribución de churn, importancia de variables) si se desea exportarlos como imágenes.

Nota: Actualmente, los datos se cargan directamente desde la URL proporcionada, pero se recomienda guardar una copia local para evitar dependencias de internet.

Proceso de Preparación de Datos
El proceso de preparación asegura que los datos estén listos para el modelado, con las siguientes etapas:

Clasificación de Variables
- Numéricas: Incluyen tenure (tiempo de permanencia), Charges.Monthly (cargos mensuales) y Charges.Total (cargos totales), que reflejan métricas continuas.
- Categóricas: Comprenden variables como gender, Contract, PaymentMethod, TechSupport, OnlineSecurity, entre otras, que se codifican como factores.

Etapas de Normalización o Codificación
- Desanidado: El JSON anidado (columnas customer, phone, internet, account) se desanida usando pd.json_normalize para extraer todas las variables en un DataFrame plano.
- Codificación: Las variables categóricas se convierten en variables dummy con pd.get_dummies, eliminando la primera categoría (drop_first=True) para evitar multicolinealidad.
- Normalización: Las variables numéricas se estandarizan con StandardScaler para tener media 0 y desviación estándar 1, asegurando que todas contribuyan equitativamente al modelo.
- Manejo de NaN: Se eliminan filas con valores nulos en Churn y otras columnas para mantener la integridad de los datos, justificable por el tamaño suficiente del dataset (~7,000 registros).

Separación de Datos

Los datos se dividen en conjuntos de entrenamiento (80%) y prueba (20%) usando train_test_split con random_state=42 para reproducibilidad. Esta proporción balancea el aprendizaje del modelo y la evaluación externa.

Justificaciones para Decisiones en la Modelización
- Modelo Random Forest: Elegido por su capacidad para manejar variables categóricas y numéricas sin escalado previo (excepto numéricas), y por su robustez frente a sobreajuste con múltiples árboles (n_estimators=100).
- Balanceo de Clases: Se usa class_weight='balanced' debido al desbalanceo (73% no churn vs. 27% churn), mejorando el recall de la clase minoritaria (churn) de 0.43 a 0.47.
- Optimización con GridSearchCV: Se implementa con una grilla reducida (n_estimators: [100], max_depth: [10, None], min_samples_split: [2]) para ajustar hiperparámetros, seleccionando max_depth=None como óptimo, lo que permite árboles profundos dado el tamaño del dataset.

Ejemplos de Gráficos e Insights del EDA
- Distribución de Clases (Churn): Un gráfico de barras muestra que el 26.54% de los clientes abandonan, con una distribución sesgada hacia no churn, justificando el balanceo de clases.
- Matriz de Correlación: Un heatmap revela correlaciones moderadas, como una relación negativa entre tenure y Churn, indicando que clientes con menor tiempo de permanencia tienen mayor riesgo.
- Importancia de Variables: Un gráfico de barras destaca tenure (0.117) y Charges.Monthly (0.095) como los factores más influyentes, con contratos largos (Contract_Two year, 0.039) reduciendo el churn.

Instrucciones para Ejecutar el Cuaderno
Requisitos
- Entorno: Google Colab o un entorno local con Python 3.x.
- Bibliotecas: Instala las siguientes dependencias ejecutando en una celda de código:

bash

!pip install pandas numpy matplotlib seaborn scikit-learn

- Datos: El cuaderno carga los datos directamente desde la URL
https://raw.githubusercontent.com/alura-cursos/challenge2-data-science-LATAM/refs/heads/main/TelecomX_Data.json.
Para uso offline, descarga el archivo y guárdalo en la carpeta data/, ajustando la ruta en la Celda 2 (por ejemplo, df = pd.read_json('data/TelecomX_Data.json')).

Pasos para Ejecutar
- Clona o Crea el Proyecto:
    - Abre Google Colab y crea un nuevo cuaderno llamado TelecomX_LATAM_Prediction.ipynb.
    - Copia y pega el contenido de cada celda (1 a 7) desde el código proporcionado.

- Instala Dependencias:Ejecuta la celda con el comando !pip install si no están instaladas.

- Ejecuta las Celdas:
    - Ejecuta las celdas en orden (1 a 7) para importar librerías, cargar datos, preparar el dataset, analizar, modelar y obtener conclusiones.
    - La Celda 5 puede tomar unos minutos debido a GridSearchCV; si no termina, considera reducir la grilla en param_grid.

- Verifica Resultados:
    - Revisa las salidas (gráficos, métricas como precisión 0.80, recall 0.47, importancia de variables) para confirmar el funcionamiento.

- Exporta (Opcional):
    - Guarda los datos procesados como CSV con df.to_csv('data/telecomx_data_processed.csv', index=False) en la Celda 3.
    - Exporta gráficos manualmente desde las visualizaciones generadas.

Notas Adicionales
- Reproducibilidad: Usa random_state=42 para obtener resultados consistentes.
- Mejoras Futuras: Considera implementar SMOTE (imbalanced-learn) para mejorar el recall de churn, instalándolo con !pip install imbalanced-learn.

Instrucciones para Usar el README
- Crea el Archivo: Copia el contenido anterior en un archivo llamado README.md en la misma carpeta que TelecomX_LATAM_Prediction.ipynb.
- Revisa y Ajusta: Asegúrate de que las rutas y detalles (como la URL del JSON) coincidan con tu configuración.
- Publica (Opcional): Si subes el proyecto a GitHub, el README.md se mostrará automáticamente en la página principal.

Notas Finales

Este README documenta el propósito de predecir el churn, detalla el proceso técnico y proporciona instrucciones claras para ejecutar el análisis. Los insights obtenidos (por ejemplo, el impacto de tenure y Charges.Monthly) son clave para las estrategias de retención de TelecomX.
