# Extensiones WS-* (Segunda Generación de Servicios Web)

Conjunto de especificaciones y estándares abiertos creados para dotar a los servicios web básicos (WSDL, SOAP, UDDI) de capacidades empresariales de misión crítica:

* **WS-Security:** Seguridad a nivel de mensaje (independiente del transporte): autenticación (tokens SAML/Username), integridad (*XML Signature*) y confidencialidad (*XML Encryption*).
* **WS-Addressing:** Estandariza la inclusión de direcciones de endpoints y remitentes dentro de los encabezados SOAP.
* **WS-ReliableMessaging:** Garantiza la entrega ordenada y fiable de mensajes entre dos extremos, gestionando retransmisiones y acuses de recibo.
* **WS-BPEL (*WS-Business Process Execution Language*):** Especificación para la **orquestación** de procesos de negocio ejecutables mediante un coordinador central.
* **WS-CDL / WS-Choreography:** Especificación del W3C basada en XML para describir protocolos de **colaboración entre participantes** de servicios web actuando como pares (*peers*) sin un coordinador central, manejando interacciones de larga duración y con estado.
* **WS-MetadataExchange (WS-MEX):** Protocolo para consultar dinámicamente metadatos (WSDL, políticas, esquemas) desde un endpoint mediante SOAP.
