---
Fuente1: https://jhfarfan.wixsite.com/datamining/clustering
aliases:
  - M-Medois
  - M-Medoids
  - K-Medoides
  - k-Medoids
  - PAM
  - Algoritmo K-Medoids
tags:
  - facultad/abd2
  - data-mining/clustering
---
# Algoritmo K-Medoids

> Pertenece a: [[Clustering Partitivos]] | [[Clustering]]

## 1. Definición y Concepto
El algoritmo [[K-Means]] es sensible a la presencia de **valores atípicos (*outliers*)**. Como una posible mejora y alternativa más robusta, se utiliza el algoritmo **K-Medoids**.

En lugar de utilizar centroides (medias aritméticas calculadas que pueden no corresponder a ningún punto real y se distorsionan con facilidad), **emplea medoides**:
> Toma como referencia un **objeto ya existente en el cluster** (idealmente, el objeto más central del cluster, con la menor distancia promedio al resto de los elementos del grupo).

![[Pasted image 20260913221915.png]]

---
## 3. Algoritmos y Variantes de K-Medoids
- **PAM** (*Partitioning Around Medoids*): Algoritmo estándar para K-Medoids. Intercambia iterativamente medoides con otros puntos si esto reduce el costo total de disimilaridad.
- **CLARA** (*Clustering Large Applications*): Variante optimizada para datasets más grandes que aplica PAM sobre subconjuntos de datos muestreados al azar.
- **CLARANS** (*Clustering Large Applications based upon RANdomized Search*): Algoritmo de partición escalable que combina muestreo y búsqueda aleatoria para explorar el espacio de soluciones eficientemente.

---

## 🔗 Relacionados
- [[Clustering Partitivos]]
- [[K-Means]]
- [[Clustering]]
