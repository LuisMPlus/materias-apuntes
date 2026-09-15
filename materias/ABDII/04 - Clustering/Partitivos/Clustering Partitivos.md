---
Fuente1: https://jhfarfan.wixsite.com/datamining/clustering
aliases:
  - Clustering partitivos
  - Clustering Particional
  - Agrupamiento por particiones
  - Algoritmo: Clustering Partitivos
tags:
  - facultad/abd2
  - data-mining/clustering
---
# Clustering Partitivos

> Pertenece a: [[Clustering]]

## 1. Definición
Es una técnica de partición de clustering que se enfoca en dividir $n$ observaciones iniciales en $k$ clusters, de modo que cada observación pertenece al cluster cuyo promedio (o prototipo) esté más cercano, sirviendo este como el representante o prototipo del cluster.

A diferencia de los algoritmos [[Clustering Jerárquico|jerárquicos]] (que producen un dendrograma con múltiples niveles de agrupación), en el clustering partitivo se obtiene **una única partición** de los datos.

![](../../../../Adjuntos/Pasted%20image%2020260913220208.png)

---

## 2. Algoritmos Partitivos Principales

### 2.1 [[K-Means]] (K-Medias)
El método más extendido de agrupamiento por particiones. Se basa en definir $k$ centroides (medias geométricas) y asignar cada punto al centroide más cercano minimizando la suma de distancias cuadráticas intra-cluster.
- Ver apunte completo: [[K-Means]]

### 2.2 [[K-Medoids]]
Una variante y mejora de K-Means diseñada para ser mucho más robusta ante valores atípicos (*outliers*). En lugar de promedios geométricos (centroides), utiliza observaciones reales del dataset denominadas **medoides**.
- Ver apunte completo: [[K-Medoids]]

### Diferencias entre K-Means y K-Medois
| Característica                | [[K-Means]]                                         | [[K-Medoids]]                                                 |
| :---------------------------- | :-------------------------------------------------- | :------------------------------------------------------------ |
| **Representante del cluster** | Centroide (promedio matemático o centro geométrico) | Medoide (punto real existente del conjunto de datos)          |
| **Sensibilidad a Outliers**   | Alta (los valores extremos alteran la media)        | Baja (muy resistente a ruido y valores atípicos)              |
| **Costo computacional**       | Más rápido y escalable ($O(n \cdot k \cdot i)$)     | Más costoso computacionalmente ($O(k \cdot (n-k)^2)$ en PAM)  |
| **Métrica de distancia**      | Comúnmente distancia euclídea                       | Admite cualquier métrica o matriz de disimilaridad arbitraria |

---

## 3. Otros Algoritmos Basados en Particiones
Dentro de la clasificación clásica (Berzal):
- **PAM** (*Partitioning Around Medoids*): Algoritmo fundacional de K-Medoids.
- **CLARA** (*Clustering Large Applications*): Optimización de PAM para grandes volúmenes de datos mediante sub-muestreo.
- **CLARANS** (*Clustering Large Applications based upon RANdomized Search*): Algoritmo de partición escalable que combina PAM con técnicas aleatorias.
- **BFR**: Algoritmo de agrupamiento particional escalable para datos masivos en espacios de alta dimensión.

---

## 🔗 Relacionados
- [[Clustering]] (Nota principal)
- [[Clustering Jerárquico]] (Enfoque jerárquico multinivel)
- [[K-Means]]
- [[K-Medoids]]
