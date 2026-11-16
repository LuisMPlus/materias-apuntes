# QVT (Query / View / Transformation)

Estándar del **OMG** para la descripción formal de transformaciones entre metamodelos definidos en [[MOF (Meta Object Facility)]].

## Arquitectura de QVT

Se compone de tres lenguajes:

1. **QVT Relations (Declarativo de alto nivel):** Especifica relaciones complejas entre patrones de modelos candidatos. Soporta ejecución unidireccional y bidireccional, así como sincronización incremental.
2. **QVT Core (Declarativo de bajo nivel):** Núcleo formal y simplificado de correspondencias entre patrones de grafos. Es el destino de compilación de Relations.
3. **QVT Operational Mappings (Imperativo / Procedural):** Añade construcciones algorítmicas de control de flujo clásicas y permite integrar código de caja negra (*black-box*). Funciona únicamente de forma unidireccional.

## Dependencias
Todos los paquetes de QVT dependen de **EssentialOCL** (definido en OCL 2.0) y, a través de él, de **EMOF**.

## Escenarios de Ejecución
* **Chequeo / Control (*Check-only*):** Valida si modelos existentes cumplen con la relación especificada sin modificarlos.
* **Unidireccional y Bidireccional:** Generación y propagación de cambios en uno o ambos sentidos.
* **Actualización incremental:** Modifica solo los elementos necesarios tras cambios en los modelos vinculados.
* **Creación y borrado controlado:** Posibilidad de especificar qué elementos se pueden crear, actualizar o preservar inmutables.
