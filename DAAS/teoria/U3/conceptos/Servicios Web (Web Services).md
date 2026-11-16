# Servicios Web (Web Services)

Tecnología fundamental para implementar soluciones basadas en [[01 - Resumen - Unidad 3 - SOA Parte I|SOA]], permitiendo la comunicación estandarizada entre aplicaciones heterogéneas sobre la red.

## Primera Generación de Servicios Web

Se fundamenta en cuatro estándares abiertos principales:
1. **WSDL (*Web Services Description Language*):** Especificación XML de la interfaz y operaciones públicas.
2. **SOAP (*Simple Object Access Protocol*):** Protocolo de intercambio de mensajes estructurados en XML.
3. **UDDI (*Universal Description, Discovery and Integration*):** Directorio o registro donde se publican y buscan servicios.
4. **WS-I Basic Profile:** Guías de interoperabilidad entre distintos fabricantes y plataformas.

## El Triángulo de Servicios Web
* **Proveedor del servicio (*Service Provider*):** Publica el contrato WSDL en el registro UDDI.
* **Registro de servicios (UDDI):** Directorio accesible para consulta de servicios disponibles.
* **Solicitante del servicio (*Service Requestor / Client*):** Busca y descarga el WSDL desde el registro y se comunica con el proveedor mediante mensajes SOAP.
