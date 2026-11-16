## Arquitectura
La **arquitectura** es la **estructura organizativa de un sistema**, la cual abarca su descomposición en partes, su conectividad, sus mecanismos de interacción y los principios rectores que proporcionan información sobre el diseño del mismo.

- **Propósito principal**: Mientras que el diseño funcional de un sistema alberga los requisitos funcionales (como los casos de uso), la **arquitectura proporciona la infraestructura** necesaria para hacer efectivos los **requisitos no funcionales**, tales como la **escalabilidad, fiabilidad o rendimiento**.
- **Ejemplos en el material**:
    - **Arquitectura Multicapa**: Divide el sistema en capas independientes de presentación (interfaz), negocio (servicios) e integración (comunicación con recursos de datos).
    - **Arquitectura Monolítica**: Agrupa todas las funciones y servicios dentro de una base de código única y centralizada, sin dependencias externas.
    - **Arquitectura de Microservicios (MSA)**: Descompone la aplicación en un conjunto de pequeños servicios independientes que se ejecutan en su propio proceso.
    - **Arquitectura Dirigida por Modelos (MDA)**: Enfoque promovido por el OMG centrado en la integración y supervivencia de los diseños frente a los cambios tecnológicos.


## Patrón de diseño
Un **patrón de diseño** es una técnica o solución reutilizable con **éxito ya comprobado** que se aplica de forma reiterada para resolver problemas comunes en la construcción de software. Su aplicación sistemática incrementa la confiabilidad, la productividad y la calidad del producto final.

Son soluciones simples y elegantes a problemas específicos del diseño de software.
Representan soluciones que han sido desarrolladas y han ido evolucionando a través del tiempo.

Son descripciones de clases y objetos
relacionados que están particularizados para resolver un problema de diseño general en un determinado contexto.

Cada patrón de diseño identifica:
- Las clases e instancias participantes.
- Los roles y colaboraciones de dichas clases e intancias
- La distribución de responsabilidades 


## Ejemplos 
### GRASP

### Inversión de Control (IoC)
Permite escribir componentes (como JavaBeans) sin dependencias directas, delegando la entrega de los objetos requeridos al propio marco de trabajo

### DAO (Data Access Object)
Patrón enfocado en definir objetos dedicados a abstraer y gestionar el acceso a los datos

### Patrones en Arquitecturas Multicapa
Soluciones estructuradas para resolver necesidades específicas en cada nivel, como el _Controlador Frontal_ (_Front Controller_) y _Controlador de la Aplicación_ en la capa de presentación, los objetos de _Transferencia_ o _Delegado del Negocio_ en la capa de negocio, y el _Almacén del Dominio_ en la capa de integración.