# Eclipse Modeling Project (EMP)

Proyecto paraguas dentro de la comunidad **Eclipse** dedicado a impulsar y albergar tecnologías basadas en desarrollo dirigido por modelos (MDD).

## Subproyectos Principales

* **Sintaxis Abstracta:**
  * **EMF (*Eclipse Modeling Framework*):** Framework central basado en el metamodelo **Ecore** (implementación de EMOF). Genera código Java de soporte, interfaces, clases de implementación y adaptadores de UI a partir de modelos estructurados.
  * **Plugin OCL:** Evaluación y validación de expresiones y restricciones OCL añadidas como anotaciones en modelos Ecore.
* **Sintaxis Concreta:**
  * **GMF (*Graphical Modeling Framework*):** Genera editores gráficos completos integrados a Eclipse combinando EMF y GEF (*Graphical Editing Framework*).
  * **TMF (*Textual Modeling Framework*):** Herramientas para la creación de sintaxis textuales y DSLs (ej. Xtext).
* **Transformaciones:**
  * **QVT:** Implementación del estándar del OMG para transformaciones M2M.
  * **JET (*Java Emitter Templates*) / MOF2Text (Acceleo):** Motores de plantillas para transformaciones modelo a texto (M2T).
* **Implementación de Estándares:**
  * UML2, OCL, XSD, OMD.

## Flujo de Trabajo en GMF
1. Definir metamodelo en Ecore (`.ecore`).
2. Derivar modelo gráfico (`.gmfgraph`) y modelo de paleta/herramientas (`.gmftool`).
3. Crear el modelo de correspondencia/mapeo (`.gmfmap`).
4. Generar el modelo de configuración de código (`.gmfgen`) y producir el plugin del editor gráfico.
