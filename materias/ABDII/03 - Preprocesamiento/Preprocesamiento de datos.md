---
materia: Aplicaciones de Bases de Datos II
tipo: referencia
Fuente1: "[Preprocesamiento | Preparacion Datos | KDD | Datamining | Minería de Datos](https://jhfarfan.wixsite.com/datamining/4-preprocesamiento)"
Fuente2: "[4.1 Preprocesamiento Python.ipynb - Colab](https://colab.research.google.com/drive/1jWoS61gJ3rwWplKAGD55OH_iLjtCkfKg)"
created: 2026-09-06
tags:
  - "#FaseKDD"
---
git## Segunda Fase del KDD: Preparación de los datos
![[Pasted image 20260906151324.png|562]]
- Fase más tardía

## Inconvenientes que pueden tener los dataset de origen
- <span style="color:rgb(0, 176, 240)">Datos faltante: </span>Ausencia de datos debido a algún acontecimiento. Disminuye la representatividad de la muestra
- <span style="color:rgb(0, 176, 240)">Valores extremos</span>: Diferenciar si ese valor es posible o no
	Por ejemplo: como una pelicula muy vieja(posible) o una edad de 300 años (imposible)
- <span style="color:rgb(0, 176, 240)">Incon<span style="color:rgb(0, 176, 240)">sistencias:</span></span> incongruencia de datos, cuando datos sacados de diferentes tablas deberían ser iguales pero no lo son.
- <span style="color:rgb(0, 176, 240)">Ruido:</span> Cuando los valores de un atributo son afectados por los valores de otro/s atributo/s. Generalmente representado por los outliers

## Situaciones presentadas
### Valores atípicos u outliers
No se ajustan al comportamiento general de los datos. Estos datos anómalos en algunos algoritmos se descartan o ignoran al considerarlos como ruido.
No siempre es conveniente eliminarlos puesto que pueden presentar situaciones excepcionales
![[Pasted image 20260907123145.png]]

### missing values
![[Pasted image 20260907123513.png]]
Como proceder:
- Ignorar la tupla o registro
- Rellenar la tupla manualmente
- Usar una constante global para rellenar el valor nulo
- Utilizar el valor de la media u otra medida de centralidad para rellenar el valor
- Utilizar el valor de la media u otra medida de centralidad de los objetos que pertenecen la misma clase
- Utilizar alguna herramienta de Minería de Datos para calcular el valor más probable

### Transformación de atributos
Consiste en la transformación o construcción de atributos, en la que yacen nuevos atributos aplicando alguna operación o función a los atributos originales de modo que estos atributos faciliten el proceso de minería.
### Razones de trasnformación:
- Necesidad del algoritmo a aplicar

### Reducción de dimensionalidad
Proceso que consiste en simplificar datos para volverlos mas fácil de trabajar.
Para después que veamos matrices #matrices

### Aumenta de dimensionalidad
Crear nuevas  variables que aporten información adicional
### Convertir atributos no numéricos a números

### Discretización
Convertir las variables de tipo continuas en agrupadas por intervalos(cualitativos ordinales) 
Para algunos algoritmos que operan con variables cualitativas
### Normalización
Véase [[Normalización]]