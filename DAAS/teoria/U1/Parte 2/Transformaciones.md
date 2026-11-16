# Transformaciones de Modelos

En MDD, las transformaciones son el motor del proceso de desarrollo de software: toman uno o más modelos fuente como entrada y generan uno o más modelos destino (o código) como salida.

## Diferencias Clave

* **Transformación:** Generación automática de un modelo destino a partir de un modelo fuente.
* **Definición de la Transformación:** Colección de reglas que en conjunto describen con exactitud cómo un modelo en el lenguaje fuente se transforma en el lenguaje destino.
* **Regla de Transformación:** Especificación no ambigua de cómo una o más construcciones en el lenguaje fuente se transforman en una o más construcciones en el lenguaje destino.

## Tipos de Transformaciones

* **Modelo a Modelo (M2M):** Convierte un modelo de un nivel de abstracción a otro (por ej. PIM a PSM).
* **Modelo a Texto (M2T):** Genera artefactos textuales o código fuente ejecutable a partir de un modelo (por ej. PSM a Java/SQL).

## Implementación

* **Lenguajes de propósito general:** Java, C++, Python (más imperativos y complejos de mantener).
* **Lenguajes específicos de transformación (DSLs):** [[QVT (Query View Transformation)]], ATL (*ATLAS Transformation Language*).
