---
Fuente1: https://jhfarfan.wixsite.com/datamining/clustering
aliases:
  - K-Medias
  - KMeans
  - Algoritmo: K-Means o K-Medias
tags:
  - facultad/abd2
  - data-mining/clustering
---
# Algoritmo: K-Means o K-Medias

> Pertenece a: [[Clustering Partitivos]] | [[Clustering]]

## 1. Definición
K-Means es un método de agrupamiento que tiene como objetivo la partición de un conjunto de $n$ observaciones en $k$ grupos, en el que cada observación pertenece al grupo cuyo valor medio (centroide) es más cercano.

---

## 2. Características
- **Requiere conocer a priori el número $K$** de grupos a formar.
- Cada cluster tiene asociado un **centroide** (centro geométrico del cluster).
- Está basado en la **minimización de la distancia interna**, es decir, la suma de las distancias de los patrones asignados a un agrupamiento al centroide de dicho agrupamiento.
- De hecho, este algoritmo **minimiza la suma de las distancias al cuadrado** de cada patrón al centroide de su agrupamiento.
- El algoritmo es **sencillo y eficiente**.
- Procesa los patrones secuencialmente (por lo que requiere un almacenamiento mínimo).
- **Está sesgado por el orden de presentación de los patrones** (los primeros patrones determinan la configuración inicial de los agrupamientos).
- Su comportamiento **depende enormemente del parámetro $K$**.
- Los puntos se asignan al cluster cuyo centroide esté más cerca (utilizando cualquier métrica de distancia).
- Iterativamente, se van actualizando los centroides en función de las asignaciones de puntos a clusters, hasta que los centroides dejen de cambiar.

![](../../../../Adjuntos/Pasted%20image%2020260913220421.png)

---

## 3. Pasos del Algoritmo

### 3.1 Inicialización
- Escoger $k$ centroides aleatoriamente (hay métodos más sofisticados, como K-Means++).
- Formar $k$ grupos, asignando cada punto al centroide más cercano.

### 3.2 Proceso Iterativo
Mientras que los centroides sigan cambiando:
1. Calcular las distancias de todos los puntos a los $k$ centroides.
2. Formar $k$ grupos, asignando cada punto al centroide más cercano.
3. Recalcular los nuevos centroides.

---

## 4. Determinación del Valor K Óptimo

### 4.1 Elbow Method (Método del codo)
Es una forma precisa y sencilla (tal como lo menciona Joaquín Rodríguez en [RPubs - K-Means](https://rpubs.com/Joaquin_AR/310338)), cuando afirma que cuando no se dispone de información adicional y se aplica el algoritmo de K-Means, una forma de conocer el $K$ óptimo es identificar aquel valor a partir del cual la **reducción en la suma total de varianza intra-cluster deja de ser sustancial**.

A continuación se muestra un ejemplo del mismo:

![](../../../../Adjuntos/Pasted%20image%2020260913221059.png)

> De la gráfica se observa que con un **$K = 4$** la reducción en la suma total de cuadrados internos parece estabilizarse, indicando que ese número es una opción apropiada ("el punto de inflexión o codo").

---

### 4.2 Índice de Silhouette (Silueta)
Un método muy utilizado para la interpretación y validación de la coherencia dentro de un cluster de datos. La técnica proporciona una representación gráfica sucinta de lo bien que se ha clasificado cada objeto.

Luego de formar $k$ grupos, puede calcularse el índice Silhouette de cada ejemplo aplicando la siguiente fórmula:

$$s(i) = \frac{b(i) - a(i)}{\max\{a(i), b(i)\}}; \quad -1 \leq s(i) \leq 1$$

Donde:
- $a(i)$ es la distancia promedio del ejemplo $i$ a todos los otros ejemplos dentro del **mismo cluster** (medida de cohesión intra-cluster).
- $b(i)$ es la distancia promedio del ejemplo $i$ a todos los otros ejemplos del **cluster más cercano** (medida de separación inter-cluster).

Promediando todos los valores de $s(i)$ se tendrá el índice Silhouette global del agrupamiento:
- **Cercano a 1**: Los datos están muy bien agrupados y lejos de clusters vecinos.
- **Cercano a 0**: Los datos se encuentran en el límite o frontera entre dos clusters.
- **Cercano a -1**: Indica asignaciones incorrectas a ese cluster.

---

## ⚠️ Limitación y Alternativa
- K-Means es especialmente **sensible a la presencia de valores atípicos (*outliers*)**, ya que un solo dato extremo puede distorsionar fuertemente el cálculo del centroide.
- Para solucionar este problema, se suele utilizar como alternativa el algoritmo [[K-Medoids]], el cual utiliza medoides (elementos reales de la muestra) en vez de promedios aritméticos.

---

## 🔗 Relacionados
- [[Clustering Partitivos]]
- [[Clustering]]
- [[K-Medoids]]
