# MOF (Meta Object Facility)

Estándar definido por el **OMG** que ocupa la cúspide de la arquitectura de cuatro capas (**Nivel M3: Meta-metamodelo**).

## Propósito
Provee un meta-metalenguaje formal para definir metamodelos en la capa M2 (como el metamodelo de UML, CWM, o metamodelos relacionales).

## Propiedades Fundamentales

* **Arquitectura Cerrada:** El metamodelo de MOF se define en términos de sí mismo (autorreferencial).
* **Arquitectura Estricta:** Cada elemento en cualquier capa se corresponde estrictamente con un elemento de la capa superior.
* **Orientado a Objetos:** Utiliza la misma sintaxis concreta y conceptos que los diagramas de clases UML (clases, atributos, operaciones, asociaciones, paquetes, herencia múltiple).

## Subconjuntos de MOF

* **EMOF (*Essential MOF*):** Subconjunto básico con las construcciones mínimas para metamodelos simples. Fácilmente mapeable a lenguajes orientados a objetos.
* **CMOF (*Complete MOF*):** Conjunto extendido para metamodelos sofisticados y complejos (como UML 2 completo).

## Implementación en la Práctica
En el ecosistema Eclipse, MOF (EMOF) está implementado en el metamodelo **Ecore** de [[Eclipse Modeling Project (EMP)]].
* Cada elemento de MOF se mapea con el prefijo `E`: `Class` $\to$ `EClass`, `Attribute` $\to$ `EAttribute`.
