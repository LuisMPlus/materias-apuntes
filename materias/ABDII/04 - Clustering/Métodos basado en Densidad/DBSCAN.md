---
Fuente1: https://es.wikipedia.org/wiki/DBSCAN
Fuente2: https://www.youtube.com/watch?v=-7j4n3FDGK4
---
## DBSCAN (Density-Based Spatial Clustering of Applications with Noise)
 
es un algoritmo de agrupamiento de datos basado en densidad porque encuentra un número de grupos (clusters) comenzando por una estimación de la distribución de densidad de los nodos correspondientes. 

Para lograr esto, el algoritmo utiliza dos reglas o parámetros fundamentales:
1. El radio (ε): Es la distancia máxima que el algoritmo revisa desde un punto. Forma un círculo a su alrededor llamado ε-vecindario.
2. El mínimo de puntos (min): Es la cantidad de "vecinos" que debe haber dentro de ese círculo para que se considere que hay un grupo.
![](../../../../Adjuntos/Pasted%20image%2020260913225333.png)
![](../../../../Adjuntos/Pasted%20image%2020260913225345.png)
- El conjunto de elementos comprendidos dentro de un radio ε del elemento x(i) se conoce como ε-vecindario del elemento x(i) 
- Un punto núcleo es un elemento cuyo ε-vecindario contiene al menos otros min elementos. 
- Un punto frontera tiene menos de min elementos en su ε-vecindario, pero esta en elv vecindario de un punto nucleo. 
- El resto de puntos se conocen como ruido
![](../../../../Adjuntos/Pasted%20image%2020260913230041.png)
Video explicativo:
[How DBSCAN Beats K-Means Clustering](https://www.youtube.com/watch?v=-7j4n3FDGK4)

## Determinar el minPts
Se usa una regla de oro básica:
$MinPts \geq Dimensiones+1$
- Donde $Dimensiones$ es el númer de columnas del datase

Para datasets estándar / grandes
Se recomienda duplicar las dimensiones:
$MinPts=2\times Dimensiones$
## Determinar el epsilon($\epsilon$)
El Método del Codo ("Elbow" o "Knee Method")

Al graficarlo verás una curva que avanza plana (puntos que están cerca dentro de los clústeres) y de repente **se dispara verticalmente hacia arriba** (puntos que están tan lejos que ya entran en zona de aislamiento o ruido).

El punto de máxima curvatura, conocido como **el "codo" o "rodilla" (knee)** de la gráfica, es tu **`Epsilon` ideal**. 

- Si pones un `Epsilon` por debajo del codo, tus grupos se romperán en pedazos pequeños.
- Si pones un `Epsilon` muy por encima del codo, todos los grupos se fusionarán en una sola masa gigante.