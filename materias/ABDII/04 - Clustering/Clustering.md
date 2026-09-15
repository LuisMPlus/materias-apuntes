---
Fuente1: https://jhfarfan.wixsite.com/datamining/clustering
tags:
  - facultad/abd2
  - data-mining/clustering
---
# Clustering (Análisis de Agrupamiento)

## 1. Definición
El clustering o análisis de agrupamiento es una técnica de aprendizaje automático en la cual consiste en agrupar un conjunto de objetos de tal manera que los miembros del mismo grupo sean lo más similares posibles.

![](../../../Adjuntos/Pasted%20image%2020260913202156.png)

### 1.2 Características principales
- **Técnica de Aprendizaje automático** (*machine learning*): rama de la IA que permite a las computadoras aprender a partir de datos sin necesidad de ser programadas con reglas explícitas.
- **Aprendizaje sin supervisión**: O sea que se enfrenta a datos brutos sin etiquetas; no hay respuestas correctas o un "maestro" que guíe el proceso. El sistema debe analizar los datos en bruto de forma independiente para descubrir patrones y estructuras ocultas por sí mismo.
	- Porque no conocemos cómo serán esas agrupaciones ni cuántas serán, el sistema encontrará el agrupamiento más adecuado.
- **No requiere categorías** previas.

> [!NOTE]
> Se requiere de una medida de similitud o distancia para cuantificar la cercanía entre los elementos.

### 1.3 Tipos de clustering
#### 1.3.1 Hard Clustering
Cada dato pertenece a un solo grupo de forma exclusiva (ejemplo clásico: [[K-Means]]).

#### 1.3.2 Soft Clustering
Un dato puede pertenecer a varios grupos al mismo tiempo con un porcentaje de probabilidad para cada uno (ejemplo: Modelos de mezcla gaussiana). 

---

## 2. Métricas de agrupamiento
### 2.1 Distancia euclídea
Indica la separación entre dos puntos en un espacio, donde se cumplen los axiomas y teoremas de la geometría de Euclides. 
La distancia entre dos puntos A y B de un espacio euclidiano es la longitud del vector AB perteneciente a la única recta que pasa por dichos puntos.

$$distancia(X,Y) = \sqrt{\sum_{i=1}^{n}(x_i-y_i)^2}$$

- Es para múltiples dimensiones.
- Viene del teorema de Pitágoras:
	- Distancia para dos puntos en dos dimensiones:
		$$distancia=\sqrt{({x_2-x_1})^2+({y_2-y_1})^2}$$

![](../../../Adjuntos/Pasted%20image%2020260913205540.png)

### 2.2 Métricas del agrupamiento obtenido
Hay varias medidas de distancia, por eso lo que debemos buscar es un buen método de agrupamiento que produzca grupos de **alta calidad** donde se tiene que cumplir:
- **Intra-cluster (cohesión)**: El parecido entre los elementos que componen un mismo grupo es alto.
	- Alta cohesión.
	- Se busca **minimizar** la distancia intra-cluster.
- **Inter-cluster (separación)**: El parecido entre los elementos de grupos distintos es bajo.
	- Alta separación.
	- Se busca **maximizar** la distancia inter-clusters.

![](../../../Adjuntos/Pasted%20image%2020260913202836.png)

---

## 3. Proceso de Agrupamiento
Existen una serie de pasos para realizar un correcto Proceso de Agrupamiento:
1. **Seleccionar las características relevantes**.
2. **Definir una representación adecuada**.
3. **Definir la medida de similitud o utilidad (medida de distancia)**: Depende del problema.
4. **Aplicar un algoritmo de agrupamiento**.
5. **Validar los grupos obtenidos** y, de ser necesario, volver a repetir el proceso.

---

## 4. Clasificación de los Algoritmos de Clustering
Nos parece importante conocer una forma de clasificación de los algoritmos, tal cual la plantea Fernando Berzal en "[Clustering basado en particiones](https://elvex.ugr.es/idbis/dm/slides/41%20Clustering%20-%20Partitional.pdf)":

### 4.1 [[Clustering Jerárquico]] basados en conectividad
Construyen una jerarquía de clústers en forma de árbol (dendrograma):
- **Diana / Agnes**
- **BIRCH**
- **CURE**
- **Chameleon**
- **ROCK**

👉 *Ver nota completa:* [[Clustering Jerárquico]]

### 4.2 [[Clustering Partitivos]] basados en distancia
Dividen los datos directamente en $k$ grupos disjuntos en una única partición:
- **[[K-Means]]** (basado en centroides).
- **[[K-Medoids]]** (mejora de K-Means frente a outliers, basado en medoides).
- **PAM / CLARA / CLARANS**
- **BFR**

👉 *Ver nota completa:* [[Clustering Partitivos]]

### 4.3 Métodos basados en densidad
- **DBSCAN**
- **Optics**
- **DenClue**

---

## 📚 Índice de Notas de la Unidad
- [[Clustering Jerárquico]]
- [[Clustering Partitivos]]
	- [[K-Means]]
	- [[K-Medoids]]