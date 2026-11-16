# Unidad 3: Arquitectura Orientada a Servicios (SOA) - Parte I

**Materia:** DAAS  
**Origen:** [[Unidad 3 - SOA Parte I.pdf]]  
**Tags:** #DAAS #Arquitectura #SOA #ServiciosWeb

---

## 1. Evolución Arquitectónica hacia SOA

Para abordar aplicaciones empresariales de dimensión industrial, la arquitectura evoluciona en función de la complejidad y de los requisitos de distribución:

| Tipo de Aplicación             | Solución Arquitectónica | Sin Frameworks (J2EE)                                       | Con Frameworks (J2EE)                                                         |
| :----------------------------- | :---------------------- | :---------------------------------------------------------- | :---------------------------------------------------------------------------- |
| **Empresarial básica**         | Arquitectura multicapa  | JSPs + SAs POJOs + DAOs POJOs                               | JSFs + SAs POJOs + Mapeo O/R                                                  |
| **+ Lógica distribuida**       | Multicapa + RPC         | JSPs + SAs POJOs vía RMI + DAOs POJOs                       | JSFs + SAs EJBs de sesión + ODs EJBs de entidad                               |
| **+ Plataformas heterogéneas** | **SOA**                 | JSPs + SAs POJOs accesibles como servicios web + DAOs POJOs | JSFs + SAs EJBs de sesión accesibles como servicios web + ODs EJBs de entidad |

### Conceptos clave de tecnologías previas:
- **SAs (*Service Aplication*)**


* **RMI (*Remote Method Invocation*):** Protocolo y API Java que permite la comunicación transparente entre objetos situados en diferentes JVMs (Java Virtual Machine) y nodos de red sobre TCP/IP.

	#### **¿Cómo abstrae RMI la complejidad de la red?**

	Para evitar que el programador escriba código de sockets de manera manual, la arquitectura de RMI utiliza componentes intermediarios:

	1. **Stub (en el cliente):** Funciona como un representante local del objeto remoto. Cuando el cliente invoca un método, el _Stub_ intercepta la llamada, empaqueta los parámetros (proceso conocido como _marshalling_) y los envía a través de la red utilizando TCP/IP[1].
	2. **Skeleton / Capa Remota (en el servidor):** Recibe la petición desde la red, desempaqueta los datos (_unmarshalling_), ejecuta el método real en el objeto destino del servidor y envía el resultado de regreso al cliente[1].

	Gracias a estos componentes, el programador invoca un método remoto con la misma sintaxis que usaría para un objeto local en Java.

| NO framework                                                        | Si Framework                                                                                                                 |
| ------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------- |
| **JSP (*Java Server Pages*)**                                       | **JSF (*Java Server Faces*)**                                                                                                |
| Tecnología que mezcla HTML estático con HTML generado dinámicamente | Tecnología y framework para aplicaciones Java que simplifica el desarrollo de interfaces de usuario en aplicaciones Java EE. |


JSP en otras tecnologías:

| Tecnología                   | Ecosistema / Lenguaje | Rol principal                               | Sintaxis típica   |
| ---------------------------- | --------------------- | ------------------------------------------- | ----------------- |
| **JSP**                      | Java                  | Motor de vistas / plantillas en el servidor | `<%= variable %>` |
| **EJS (Embedded JavaScript** | Node.js (JavaScript)  | Motor de vistas / plantillas en el servidor | `<%= variable %>` |
| **Django Templates (DTL)**   | Python (Django)       | Motor de vistas / plantillas en el servidor | `{{ variable }}`  |

* **POJO (*Plain Old Java Object*):** Objetos Java simples sin dependencias a interfaces pesadas de frameworks.
* **EJB (*Enterprise JavaBeans*):** Componentes Java responsable de ejecutar las transacciones del servidor:
  * *EJB de Entidad:* Modela el ciclo de vida de los datos persistentes (mapeado a tablas relacionales).
  * *EJB de Sesión:* Encapsula la lógica de negocio y transaccional, invocando a los EJBs de entidad.

> [!NOTE]
> **Limitación de RPC/RMI tradicional:** La invocación remota de objetos en la práctica está fuertemente ligada a la plataforma de implementación.
> **Propuesta de SOA:** Ofrecer independencia respecto a las plataformas y tecnologías subyacentes.

---

## 2. Definición de SOA

> **SOA (*Service-Oriented Architecture*):** Es un #modeloArquitectónico que busca mejorar la eficiencia, agilidad y productividad de una empresa colocando a los **servicios** como el método principal mediante el cual se representa y organiza la lógica de la solución.

*Nota:* Aunque formalmente debe diferenciarse la arquitectura en sí de los elementos tecnológicos concretos de la informática orientada a servicios, frecuentemente se emplean ambos términos de manera indistinta.

---

## 3. Objetivos y Beneficios de SOA

1. **Aumento de la Interoperabilidad:** Capacidad nativa de compartir datos entre aplicaciones distintas al exponerlas a través de servicios estandarizados.
2. **Aumento de la Federación:** Permite unificar recursos y aplicaciones dispares conservando su autonomía y autogobierno. Favoreciendo la federación de funcionalidades.
3. **Diversificación de Plataformas:** Facilita la coexistencia e integración de múltiples tecnologías heterogéneas (ej. Java con DB2/Oracle y .NET con SQL Server).
4. **Mayor Alineamiento Negocio-Tecnología:** Al diseñar el sistema en torno a servicios de grano fino, las TI pueden adaptarse con mayor rapidez a los cambios en los requerimientos del negocio.
5. **Aumento del Retorno de la Inversión (ROI):** Si bien el costo inicial es más elevado (~30% adicional en el desarrollo base), el ROI en los años posteriores crece exponencialmente gracias a la reutilización.
6. **Aumento de la Agilidad Organizacional (*Time to Market*):** Reduce drásticamente el tiempo de desarrollo al reutilizar bloques existentes (ej. reutilizar un 65% de servicios del catálogo y codificar solo un 35% de nueva lógica).
7. **Reducción de la Carga de TICs:** Permite contar con un departamento de informática más ágil y con menores costos operativos en comparación con el mantenimiento de sistemas monolíticos o integraciones punto a punto.

---

## 4. Los 8 Principios de Diseño SOA

1. **Contrato de Servicio Estándar (*El más importante*):**
   * Los servicios declaran formalmente su propósito y capacidades mediante un contrato explícito (ej. WSDL (Web Services Description Language, esquemas XML, Descripción WS-Policy).
1. **Bajo Acoplamiento de Servicios:**
   * Garantiza la independencia entre las responsabilidades que ofrece el servicio y su implementación interna.
3. **Abstracción del Servicio:**
   * Oculta los detalles internos de implementación hacia el consumidor; promueve interfaces mínimas necesarias.
4. **Reusabilidad de Servicios:**
   * Los servicios se definen como recursos empresariales agnósticos a procesos específicos, permitiendo su uso en múltiples contextos.
   * O recursos empresariales independientes de contextos funcionales concretos. Siendo los servicios útiles para más de un contexto
5. **Autonomía de Servicios:**
   * El servicio posee control suficiente sobre sus propios recursos y entorno de ejecución para garantizar un comportamiento(funcionalidad) fiable y consistente.
   * La implementación de los servicios debe tener un importatnte grado de control sobre su entorno y recursos.
6. **Componibilidad de servicios**
   - Capacidad de componer servicios es fundamental en aplicaciones avanzadas según crezca la sofisticación de soluciones SOA.   



1. **Falta de Estado (*Statelessness*):**
   * Se delega o minimiza la gestión del estado para favorecer la disponibilidad y escalabilidad de la infraestructura.
7. **Capacidad de ser Encontrado (*Discoverability*):**
   * Metadatos y mecanismos estandarizados que facilitan la localización y consumo del servicio en el ecosistema.
8. **Componibilidad de Servicios (*Composability*):**
   * Capacidad esencial para combinar múltiples servicios atómicos y orquestar soluciones de mayor nivel.

> [!NOTE]
> La **interoperabilidad** no se enumera como un principio separado porque constituye el **requisito fundamental implícito** de la arquitectura y de cada uno de los 8 principios.

---

## 5. Problemas de Arquitecturas No-SOA vs. Ventajas de SOA

### Problemas comunes en arquitecturas tradicionales
* **Funcionalidad redundante:** Duplicación de lógica en diferentes sistemas de la empresa.
* **Procesos de desarrollo ineficientes:** Reinvención de la rueda constante.
* **Arquitecturas enrevesadas:** Crecimiento caótico de dependencias punto a punto ("código espagueti" a nivel integración).
* **Integración compleja:** Un reto constante ante cualquier cambio.

### Ventajas de adoptar SOA
* **Mayor cantidad de lógica independiente de la solución:** Gran parte de los servicios encapsulan funciones genéricas y reutilizables para toda la organización.
	* Estos elementos son elementos TICs reutilizables e independientes de la solución
* **Reducción de la lógica específica por aplicación:** En una arquitectura tradicional, el 100% de la lógica se programa para una aplicación puntual; en SOA, la lógica específica puede reducirse a un ~40% (el resto son servicios comunes).
* **Reducción del volumen global de lógica:** Al reutilizar componentes, el volumen total de código en la organización decrece (p. ej., a un ~65% respecto a aplicaciones aisladas).
* **Interoperabilidad inherente:** Basada en contratos estandarizados y modelos de datos alineados.

---

## 6. Desafíos de la Arquitectura SOA

* Incremento en la complejidad del diseño global y la arquitectura.
* Necesidad estricta de gobernanza, políticas y estándares corporativos. Estándares de diseño y de arquitectura.
* Exige una planificación y enumeración exhaustiva de servicios previa a la implementación.
* Puede generar fricción o dificultar la adopción directa de metodologías ágiles convencionales.
* Demanda una autoridad central de gestión para el catálogo y ciclo de vida de los servicios.

> [!CAUTION]
> **Conclusión:** Debido a la complejidad que implica, una solución SOA **solo debe implementarse cuando el tamaño del ecosistema y los requisitos de heterogeneidad lo justifiquen estrictamente**.

---

## Enlaces y Notas Relacionadas
* [[02 - Resumen - Unidad 3 - SOA Parte II]]
* [[Servicios Web (Web Services)]]
* [[SOAP vs REST]]
* [[WSDL (Web Services Description Language)]]
