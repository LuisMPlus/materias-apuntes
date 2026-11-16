# OCL (Object Constraint Language)

Estándar del **OMG** utilizado para enriquecer y precisar modelos agregando **restricciones formales e invariantes** que no pueden expresarse mediante diagramas gráficos.

## Características Principales

* **NO es un lenguaje de programación:** No provee mecanismos para modificar estado ni sentencias de control imperativas destructivas.
* **Libre de efectos laterales (*Side-Effect Free*):** La evaluación de expresiones OCL nunca altera el estado del sistema ni de los objetos consultados.
* **Operaciones de pura CONSULTA:** Permite interrogar el modelo, calcular valores derivados y validar invariantes, precondiciones y poscondiciones.
* **Uso temprano:** Permite a los diseñadores construir especificaciones formales precisas y libres de ambigüedad desde las fases iniciales de diseño antes de generar código o ejecutar transformaciones.
