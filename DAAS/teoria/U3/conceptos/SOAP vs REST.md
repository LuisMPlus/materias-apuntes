# Comparativa: SOAP vs. REST

Existen dos aproximaciones dominantes para implementar [[Servicios Web (Web Services)]]:

| Característica | REST (*Representational State Transfer*) | SOAP (*Simple Object Access Protocol*) |
| :--- | :--- | :--- |
| **Naturaleza** | Estilo arquitectónico (Roy Fielding, 2000) | Protocolo formal estandarizado (W3C) |
| **Formato del mensaje** | Flexible: JSON, XML, HTML, texto | Estrictamente XML dentro de `<SOAP-ENV:Envelope>` |
| **Definición de Interfaz** | No obligatoria (autodescriptivo por URIs y HTTP; opcionalmente OpenAPI) | **WSDL estricto y obligatorio** |
| **Transporte** | **HTTP / HTTPS exclusivamente** | **Independiente del transporte:** HTTP, JMS, SMTP, TCP, FTP |
| **Operaciones** | Verbos HTTP universales (`GET`, `POST`, `PUT`, `DELETE`) | Operaciones arbitrarias definidas en el contrato WSDL |
| **Gestión de Estado** | **Sin Estado (*Stateless*):** Cada petición contiene todo lo necesario para procesarse | Generalmente stateless, pero soporta sesiones y transacciones con extensiones WS-* |

## Principios Clave de REST
1. **Sin Estado (*Stateless*):** El servidor no conserva contexto de sesión entre llamadas del cliente.
2. **Interfaz Común:** Operaciones limitadas a los verbos HTTP.
3. **Identificación por URIs:** Cada URI identifica de forma unívoca un recurso o función en la red.
4. **Manipulación mediante Representaciones:** Los componentes interactúan intercambiando representaciones del estado del recurso.
