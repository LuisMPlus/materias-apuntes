# WSDL (Web Services Description Language)

Estándar XML del W3C que describe la interfaz pública, contratos y formas de comunicación de un servicio web.

## Separación Abstracto / Concreto

WSDL desacopla lo que hace el servicio de cómo y dónde se expone:

### 1. Elementos Abstractos
* **`<types>`:** Contenedor de tipos de datos complejos definidos mediante **XML Schema (XSD)**.
* **`<message>`:** Definición tipada de los parámetros y datos intercambiados (partes de entrada y salida).
* **`<portType>`:** Colección abstracta de operaciones expuestas. Equivale a una `interface` en orientación a objetos.

### 2. Elementos Concretos
* **`<binding>`:** Vincula un `portType` con un protocolo concreto (ej. SOAP sobre HTTP) y formato de codificación (`style="document"`, `use="literal"`).
* **`<port>`:** Asigna una dirección de red física (URL mediante `<soap:address location="..."/>`) a un `binding`.
* **`<service>`:** Agrupa una colección de elementos `<port>`.
