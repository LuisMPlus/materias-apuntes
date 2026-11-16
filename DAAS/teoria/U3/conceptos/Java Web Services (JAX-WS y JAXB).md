# Java Web Services (JAX-WS y JAXB)

Conjunto de tecnologías de Java EE / Jakarta EE para el desarrollo de servicios web [[SOAP vs REST|SOAP]]:

* **JAX-WS (*Java API for XML Web Services*):** API para construir servidores y clientes de servicios web.
* **JAXB (*Java Architecture for XML Binding*):** Realiza el mapeo automático bidireccional entre clases Java y documentos/esquemas XML (XSD).

## Anotaciones en el Servidor
* **`@WebService`:** Declara la clase o interfaz como servicio web. Si se aplica sobre una interfaz Java, define la **SEI (*Service Endpoint Interface*)**.
* **`@WebMethod`:** Expone un método específico como operación pública del servicio.
* **`@WebParam`:** Personaliza el mapeo del parámetro del método en el XML del WSDL.
* **`@WebResult`:** Personaliza el valor de retorno en la respuesta SOAP.
* **`@SOAPBinding`:** Configura el estilo (`DOCUMENT` o `RPC`) y la codificación (`LITERAL` o `ENCODED`).

## Anotaciones y Consumo en el Cliente
* **`@WebServiceClient`:** Marca la clase fábrica generada que extiende de `javax.xml.ws.Service`.
* **Uso del Proxy / Stub:** El cliente obtiene una referencia local que implementa la SEI mediante `service.getPort(...)`, realizando llamadas como si fueran objetos locales con serialización automática a SOAP.
