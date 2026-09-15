---
Fuente1: https://jhfarfan.wixsite.com/datamining/clustering
aliases:
  - Clustering Jerarquico
  - Clustering Jerárquico
  - Algoritmo: Clustering Jerárquico
tags:
  - facultad/abd2
  - data-mining/clustering
---
# Clustering Jerárquico

> Pertenece a: [[Clustering]]

## 1. Definición
Los algoritmos jerárquicos producen como resultado una estructura en forma de árbol llamada **dendrograma**.
A partir del dendrograma se pueden obtener distintas particiones (estructuras de clusters) de los datos según el nivel de corte que se elija.

- [Dendrograma - Wikipedia, la enciclopedia libre](https://es.wikipedia.org/wiki/Dendrograma)

![[Pasted image 20260913212848.png]]

---

## 2. Tipos de Clustering Jerárquicos

### 2.1 Tipo Aglomerativo (Bottom-Up)
Se agrupa desde los elementos individuales:
1. Al inicio, cada punto o dato está en un clúster separado.
2. A cada paso, los dos clústers más cercanos se fusionan.
3. Estas fusiones de clústers se siguen produciendo de forma sucesiva produciendo una jerarquía de resultados de clustering.
4. Al final del proceso solo queda un único clúster que aglutina todos los elementos.

![[Pasted image 20260913213224.png]]

### 2.2 Tipo Divisible / Divisivo (Top-Down)
Empieza a la inversa:
1. Se parte desde un único clúster que aglomera todos los datos.
2. Sucesivamente se va dividiendo en clústers más pequeños hasta que cada dato forma su propio clúster o se alcanza un criterio de parada.

![[Pasted image 20260913213232.png]]

---

## 3. Algoritmos Jerárquicos Destacados
Dentro de la clasificación de algoritmos jerárquicos (según planteamiento de Fernando Berzal) se encuentran:
- **AGNES** (*Agglomerative Nesting*): Enfoque aglomerativo estándar.
- **DIANA** (*Divisive Analysis*): Enfoque divisivo estándar.
- **BIRCH** (*Balanced Iterative Reducing and Clustering using Hierarchies*): Diseñado para grandes volúmenes de datos.
- **CURE** (*Clustering Using Representatives*): Utiliza múltiples puntos representativos para manejar clústers de formas no esféricas.
- **Chameleon**: Algoritmo jerárquico adaptativo basado en grafos de k-vecinos más cercanos.
- **ROCK** (*Robust Clustering using Links*): Diseñado para variables categóricas o booleanas.

---

## 🔗 Relacionados
- [[Clustering]] (Nota principal)
- [[Clustering Partitivos]] (Enfoque alternativo de partición única)
