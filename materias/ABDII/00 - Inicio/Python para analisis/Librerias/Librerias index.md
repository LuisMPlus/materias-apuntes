# Librerías de Python para Análisis y Minería de Datos

> **Materia:** Aplicaciones de Bases de Datos II y Data Mining  
> **Directorio:** `00 - Inicio / Python para analisis / Librerias`

### Índice de Librerías
- [[Pandas]]: Estructuras tabulares (`DataFrame`, `Series`), limpieza, combinación y transformaciones.
- [[NumPy]]: Cómputo numérico de alto rendimiento, matrices contiguas (`ndarray`) y álgebra lineal.
- [[Seaborn]]: Visualización estadística de alto nivel (distribuciones, correlaciones, boxplots, pairplots).
- [[Matplotlib]]: Creación y personalización de figuras y gráficos 2D base.
- [[Scikit-Learn]]: Modelado de Machine Learning (clasificación, regresión, clustering y evaluación).
---
# numpy
Documentación:
[NumPy Reference documentation](https://numpy.org/doc/stable/reference/index.html)
```embed
title: "NumPy reference — NumPy v2.2 Manual"
image: "https://numpy.org/images/logo.svg"
description: ""
url: "https://numpy.org/doc/stable/reference/index.html"
favicon: ""
aspectRatio: "40.33333333333333"
```
## ¿Qué es?

**NumPy** (*Numerical Python*) es la librería fundacional para la computación científica y numérica en Python. Proporciona una estructura de datos de matriz multidimensional optimizada en memoria contigua en C (`ndarray`), junto con una vasta colección de rutinas matemáticas para operar sobre estas matrices a velocidad nativa sin bucles de Python.

## ¿Cuándo la uso?

En **Aplicaciones de Bases de Datos II** y **Data Mining**, NumPy se utiliza en:

- **Representación matricial:** Alimentar matrices de atributos continuos ($X$) y vectores de etiquetas ($y$) que consumen los algoritmos de [[Scikit-Learn]].
- **Operaciones vectorizadas:** Transformaciones numéricas masivas, escalados y cálculos de distancias (Euclidiana, Manhattan) para algoritmos de clustering como K-Means o DBSCAN.
- **Tratamiento numérico de faltantes:** Detección de `np.nan` y reemplazo condicional eficiente con `np.where()`.
- **Detección matemática de anomalías:** Cálculo de percentiles, rango intercuartílico ($IQR$) y umbrales de desviación estándar para aislar outliers.
- **Álgebra lineal:** Multiplicaciones de matrices con `@`, cálculo de matrices de covarianza y reducción de dimensionalidad (PCA).

---
# pandas
Documentación:
[API reference — pandas 3.0.5 documentation](https://pandas.pydata.org/docs/reference/index.html#api)
```embed
title: "API reference — pandas 3.0.5 documentation"
image: "https://pandas.pydata.org/docs/_static/pandas.svg"
description: ""
url: "https://pandas.pydata.org/docs/reference/index.html#api"
favicon: ""
aspectRatio: "40.33333333333333"
```
## ¿Qué es?

**pandas** es la librería fundamental de Python para la manipulación, limpieza y análisis de datos estructurados (tabulares y series temporales). Construida sobre [[NumPy]], introduce estructuras de datos de alto nivel con índices alineados y etiquetas, permitiendo operar con tablas al estilo de SQL o planillas de cálculo con alto rendimiento.

## ¿Cuándo la uso?

En **Aplicaciones de Bases de Datos II** y **Data Mining**, pandas se utiliza en las etapas iniciales y críticas del proceso KDD:

- **Extracción e ingesta:** Cargar datasets desde archivos (`CSV`, `Excel`, `JSON`, `Parquet`) o consultas SQL.
- **Análisis Exploratorio de Datos (EDA):** Conocer la forma del dataset, tipos de variables, estadísticas descriptivas y distribuciones preliminares.
- **[[Preprocesamiento de datos]]:** Limpieza de ruido, tratamiento de datos faltantes (*missing values*), eliminación de duplicados, filtrado y detección de anomalías u *outliers*.
- **Transformación e Ingeniería de Atributos:** Normalización, creación de nuevas variables derivadas, binarización, *one-hot encoding* y conversiones de tipo de dato.
- **Preparación para Machine Learning:** Separar matrices de variables predictoras ($X$) y vector objetivo ($y$) antes de ingresarlos a modelos de [[Scikit-Learn]].

---

# matplotlib
Documentación:
[Matplotlib Reference documentation](https://matplotlib.org/stable/api/index.html)
```embed
title: "API Reference — Matplotlib 3.10.0 documentation"
image: "https://matplotlib.org/stable/_static/logo_light.svg"
description: ""
url: "https://matplotlib.org/stable/api/index.html"
favicon: ""
aspectRatio: "40.33333333333333"
```
## ¿Qué es?

**Matplotlib** es la librería fundacional de visualización y trazado 2D en Python. Ofrece control milimétrico sobre cada componente de una figura (lienzos, ejes, marcas, leyendas y textos), sirviendo como el motor gráfico sobre el cual se construyen herramientas como [[Seaborn]] y los métodos gráficos de [[Pandas]].

## ¿Cuándo la uso?

En **Aplicaciones de Bases de Datos II** y **Data Mining**, Matplotlib se utiliza en:

- **Estructuras multipanel personalizadas:** Crear cuadrículas y tableros complejos con `plt.subplots()` para comparar múltiples métricas en un solo vistazo.
- **Visualización de curvas de aprendizaje:** Trazar la evolución de funciones de pérdida (*Loss*) y exactitud (*Accuracy*) para diagnosticar sobreajuste (*Overfitting*).
- **Anotación y personalización fina:** Añadir umbrales de decisión (`axhline`), flechas y textos explicativos sobre gráficos estadísticos de Seaborn.
- **Exportación de reportes:** Generar imágenes en alta definición (`dpi=300`) y formatos vectoriales (PDF, SVG) para entregas académicas y presentaciones de negocio.

---

# seaborn
Documentación:
[Seaborn API reference documentation](https://seaborn.pydata.org/api.html)
```embed
title: "API reference — seaborn 0.13.2 documentation"
image: "https://seaborn.pydata.org/_images/logo-mark-lightbg.svg"
description: ""
url: "https://seaborn.pydata.org/api.html"
favicon: ""
aspectRatio: "40.33333333333333"
```
## ¿Qué es?

**Seaborn** es una librería de visualización de datos construida sobre [[Matplotlib]] y estrechamente integrada con las estructuras tabulares de [[Pandas]]. Ofrece una interfaz de alto nivel para crear gráficos estadísticos informativos y visualmente atractivos que exploran y sintetizan relaciones complejas entre variables de un dataset.

## ¿Cuándo la uso?

En **Aplicaciones de Bases de Datos II** y **Data Mining**, Seaborn se utiliza en:

- **Análisis Exploratorio de Datos (EDA):** Identificar de inmediato patrones de distribución, sesgos y concentración de datos con `histplot` y `kdeplot`.
- **Detección visual de valores atípicos (*outliers*):** Inspección de rangos intercuartílicos y anomalías multivariadas mediante `boxplot` y `violinplot`.
- **Diagnóstico de multicolinealidad:** Trazar mapas de calor (`heatmap`) de matrices de correlación para descartar atributos redundantes antes del modelado predictivo.
- **Evaluación de separabilidad de clases:** Generar matrices de dispersión cruzada (`pairplot`) segmentadas por color (`hue`) para determinar qué atributos discriminan mejor las clases objetivo.
- **Visualización de matrices de confusión:** Evaluar visualmente el rendimiento y errores de modelos de clasificación de [[Scikit-Learn]].

---

# scikit-learn
Documentación:
[Scikit-learn User Guide and API](https://scikit-learn.org/stable/api/index.html)
```embed
title: "API Reference — scikit-learn 1.6.1 documentation"
image: "https://scikit-learn.org/stable/_static/scikit-learn-logo-small.png"
description: ""
url: "https://scikit-learn.org/stable/api/index.html"
favicon: ""
aspectRatio: "40.33333333333333"
```
## ¿Qué es?

**Scikit-Learn** (`sklearn`) es la librería estándar por excelencia para modelado de Machine Learning en Python. Proporciona herramientas robustas, eficientes e integradas con [[NumPy]] y [[Pandas]] para clasificación, regresión, agrupamiento (*clustering*), reducción de dimensionalidad, preprocesamiento y evaluación métrica bajo una API consistente y unificada.

## ¿Cuándo la uso?

En **Aplicaciones de Bases de Datos II** y **Data Mining**, Scikit-Learn se utiliza en las etapas centrales de modelado y evaluación KDD:

- **Partición rigurosa de datos:** Dividir datasets en entrenamiento y prueba de forma estratificada con `train_test_split()`.
- **Preprocesamiento sin fuga de datos (*Data Leakage*):** Estandarización Z-score (`StandardScaler`), escalado Min-Max y codificación dummy (`OneHotEncoder`) encadenados en `Pipeline`.
- **Minería Predictiva / Supervisada:** Entrenar y evaluar modelos de clasificación (Árboles de Decisión, Random Forest, Regresión Logística) y regresión continua.
- **Minería Descriptiva / No Supervisada:** Descubrir segmentos de clientes con algoritmos de clustering (`KMeans`, `DBSCAN`) y comprimir atributos redundantes con `PCA`.
- **Evaluación y Diagnóstico:** Construir matrices de confusión (`confusion_matrix`) y reportes de métricas (*Precision*, *Recall*, *F1-Score*, *ROC-AUC*).