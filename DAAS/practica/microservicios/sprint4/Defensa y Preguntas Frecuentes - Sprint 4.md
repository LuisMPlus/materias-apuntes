# Guía de Defensa y Preguntas Frecuentes (FAQ) — Sprint 4

> **Materia:** DAAS — Proyecto Final LazyHawk  
> **Propósito:** Banco de respuestas técnicas y fundamentos teóricos ante posibles preguntas de la cátedra durante la presentación o defensa del Sprint 4.

---

## 🛡️ BLOQUE 1: Seguridad, Identidad y Keycloak

### 1. ¿Qué es un Realm en Keycloak y por qué crearon uno llamado `lazyhawk`?
* **Concepto:** Un *Realm* (reino o dominio) es un espacio de aislamiento lógico dentro de Keycloak. Actúa como un contenedor independiente que administra su propio conjunto de usuarios, credenciales, roles, clientes (aplicaciones) y políticas de seguridad.
* **Por qué `lazyhawk`:** El realm predeterminado `master` está reservado exclusivamente para administrar el propio servidor de Keycloak. Para la lógica de negocio se crea un realm dedicado (`lazyhawk`). Esto garantiza que los clientes, repartidores y administradores del sistema estén completamente aislados de los usuarios administradores de la infraestructura.

---

### 2. ¿Qué es el algoritmo RS256 y en qué se diferencia de HS256?
* **RS256 (RSA Signature with SHA-256):** Es un algoritmo de **criptografía asimétrica** basado en un par de claves:
  - **Clave Privada:** La custodia exclusivamente Keycloak y se utiliza para **firmar** digitalmente el token JWT al autenticar un usuario.
  - **Clave Pública:** La comparten todos los microservicios (o la descargan vía JWKS - *JSON Web Key Set*). Los microservicios la usan únicamente para **verificar** que la firma sea auténtica y que el token no haya sido adulterado.
* **Diferencia con HS256 (Criptografía Simétrica):**
  - En HS256, se usa un único *secreto compartido* tanto para firmar como para verificar.
  - **Riesgo de HS256 en microservicios:** Si 7 microservicios conocen el secreto para verificar, cualquiera de ellos podría generar tokens falsos si es vulnerado.
  - **Ventaja de RS256:** Ningún microservicio conoce la clave privada; solo pueden leer y verificar. Si un microservicio se ve comprometido, la seguridad del sistema de autenticación permanece intacta.

---

### 3. ¿Qué es RBAC y cómo viajan los roles en el JWT?
* **Concepto:** **RBAC** (*Role-Based Access Control*) es el modelo de autorización donde los permisos se asignan a roles (`ADMIN`, `CLIENTE`, `REPARTIDOR`, `VENDEDOR`) y los usuarios reciben uno o más roles.
* **Estructura en el JWT:** Keycloak inyecta los roles en el payload del token dentro del claim estándar `realm_access.roles`:
  ```json
  {
    "sub": "user-uuid-1234",
    "preferred_username": "cliente@logitrack.com",
    "realm_access": {
      "roles": ["CLIENTE", "default-roles-lazyhawk"]
    }
  }
  ```
* **En el Frontend:** La aplicación parsea este claim en el `AuthContext` para condicionar qué páginas puede ver el usuario y cómo se dibuja el menú de navegación.

---

### 4. Si el Frontend ya oculta los botones y protege las rutas, ¿por qué también protegemos el Backend?
* **Principio de "Defensa en Profundidad":** La seguridad en el Frontend es puramente para **experiencia de usuario (UX)** y prevención de navegación accidental.
* Un atacante puede saltarse el Frontend usando herramientas como `cURL`, `Postman` o modificando el código de la SPA desde la consola de desarrollador.
* La **seguridad real e inviolable** reside siempre en el Backend (API Gateway y microservicios), donde cada petición HTTP que llega debe tener un token válido; de lo contrario, el backend responde con `401 Unauthorized` o `403 Forbidden`.

---

### 5. ¿Qué es OIDC (OpenID Connect) vs OAuth 2.0?
* **OAuth 2.0:** Es un framework de **Autorización** (permite otorgar permisos para acceder a recursos, por ejemplo: *"esta app puede leer mis envíos"*).
* **OpenID Connect (OIDC):** Es una capa de **Autenticación** construida encima de OAuth 2.0. Añade el concepto de identidad del usuario y estandariza el formato del token mediante JWT (*ID Token* y *Access Token*). Keycloak implementa OIDC.

---

## 📈 BLOQUE 2: Observabilidad y Monitoreo (Épica 10)

### 6. ¿Qué es Prometheus y qué arquitectura de recolección utiliza?
* **Concepto:** Es una base de datos de series temporales (*TSDB*) y un sistema de monitoreo optimizado para métricas numéricas en tiempo real.
* **Modelo Pull (Scraping):** A diferencia de otros sistemas donde los agentes empujan datos (*Push*), Prometheus utiliza un modelo **Pull**: periódicamente (cada 15 segundos en nuestra configuración) hace peticiones HTTP `GET` al endpoint `/actuator/prometheus` de cada microservicio configurado en `prometheus.yml`.
* **Ventaja:** Si un microservicio se cae o satura, Prometheus no se satura recibiendo ráfagas de datos, y detecta inmediatamente la caída si el endpoint no responde.

---

### 7. ¿Qué es Spring Boot Actuator y Micrometer?
* **Spring Boot Actuator:** Módulo oficial de Spring Boot que provee endpoints de producción para auditar la salud (`/actuator/health`), información (`/actuator/info`) y métricas del microservicio.
* **Micrometer:** Es una fachada (*vendor-neutral metrics facade*) para Java (el equivalente a SLF4J pero para métricas). Recolecta métricas de la JVM (hilos, memoria Heap, GC, conexiones de base de datos HikariCP) y las traduce al formato específico que Prometheus entiende (`micrometer-registry-prometheus`).

---

### 8. ¿Por qué `commerce-service` figuraba en estado DOWN en Grafana y cómo se solucionó?
* **Causa:** En la configuración de Prometheus (`prometheus.yml`), la meta apuntaba a `commerce-service:8084/actuator/prometheus`. Sin embargo, en el `pom.xml` del servicio faltaba la dependencia `micrometer-registry-prometheus`. Al no existir el registro, Actuator no publicaba la ruta `/actuator/prometheus`, respondiendo con un error HTTP 500/404 ante cada scrape. Prometheus interpretaba esta falla marcando la meta como caída (`up = 0`), lo que Grafana reflejaba como **DOWN**.
* **Solución:** Se incorporó la dependencia `micrometer-registry-prometheus`, se agregó un filtro para que los scrapes de Prometheus no ensucien las trazas de Zipkin (`TracingFilterConfig`), se reconstruyó la imagen Docker y el estado de la meta pasó inmediatamente a `up = 1` (**UP**).

---

### 9. ¿Qué es Grafana Loki y cómo se complementa con Promtail?
* **Promtail:** Es el agente recolector de logs que corre como contenedor Docker en el host. Lee los streams de logs de todos los contenedores y los envía a Loki etiquetados con el nombre del servicio.
* **Grafana Loki:** Es un motor de agregación de logs inspirado en Prometheus. A diferencia de Elasticsearch, Loki **no indexa el contenido completo del texto del log**, sino únicamente los metadatos y etiquetas (`container_name`, `level`, `environment`). Esto lo hace sumamente liviano en consumo de memoria y CPU.
* **Lenguaje de Consulta:** Se utiliza **LogQL** dentro de Grafana para filtrar y buscar logs en tiempo real.

---

### 10. ¿Qué es Zipkin y qué significa Trazabilidad Distribuida?
* **Concepto:** En una arquitectura de microservicios, una sola acción de usuario (ej. crear un pedido) atraviesa múltiples servicios: Frontend $\to$ API Gateway $\to$ Commerce Service $\to$ Shipment Service $\to$ Base de datos.
* **Problema:** Si la petición falla o tarda 3 segundos, es difícil saber en qué microservicio exacto ocurrió el retraso.
* **Solución de Zipkin:** Inyecta en las cabeceras HTTP dos identificadores:
  - `traceId`: Identificador único y global de toda la transacción de inicio a fin.
  - `spanId`: Identificador de un tramo o llamada específica dentro de un microservicio.
* **Zipkin UI:** Permite graficar el árbol jerárquico de la llamada y medir el tiempo exacto que tardó cada salto.

---

### 11. ¿Qué es la correlación "Log-to-Trace"?
* Es la integración configurada en Grafana entre **Loki** (logs) y **Zipkin** (trazas).
* Spring Boot inyecta automáticamente el `traceId` en cada línea de log.
* Grafana detecta el patrón del `traceId` en los logs de Loki y genera automáticamente un botón interactivo que abre directamente la visualización de la traza completa en Zipkin sin tener que copiar y pegar identificadores manualmente.

---

## 🏗️ BLOQUE 3: Arquitectura e Infraestructura

### 12. ¿Por qué usamos Eureka Server si Docker ya tiene DNS interno?
* **Service Discovery a nivel de aplicación:** Aunque Docker resuelve nombres de contenedores, **Eureka** provee:
  1. *Heartbeats* activos y control de salud a nivel de instancia de aplicación.
  2. Balanceo de carga del lado del cliente con **Spring Cloud LoadBalancer**.
  3. Desacoplamiento dinámico: si en producción se levantan 3 réplicas de `shipment-service`, el Gateway consulta a Eureka y reparte la carga entre las instancias saludables sin reconfigurar redes.

### 13. ¿Qué hace `configserver` y cuál es la ventaja de `optional:configserver`?
* Centraliza todos los archivos de configuración (`application-dev.yml`, `commerce-service-dev.yml`, etc.) en un repositorio Git desacoplado (`configserver-ms`).
* La directiva `spring.config.import: optional:configserver:...` permite que si el Config Server está disponible, cargue las propiedades centralizadas; si se apaga en un test unitario, el servicio puede utilizar su propio `application.yml` local sin romperse.

---

## ⚡ Chuleta Rápida (Cheat Sheet de 1 Línea)

| Término | Definición en 1 Frase |
| :--- | :--- |
| **Keycloak** | Identity Provider OIDC que centraliza la autenticación y emisión de tokens. |
| **Realm** | Dominio o espacio aislado de usuarios y roles dentro de Keycloak. |
| **RS256** | Algoritmo asimétrico: Keycloak firma con clave privada, microservicios verifican con clave pública. |
| **RBAC** | Control de acceso basado en roles (`ADMIN`, `CLIENTE`, `REPARTIDOR`, `VENDEDOR`). |
| **Prometheus** | Base de datos de series temporales que extrae métricas haciendo *pull* HTTP. |
| **Micrometer** | Fachada de métricas en Java que expone datos en el formato que Prometheus lee. |
| **Grafana** | Plataforma de visualización para crear tableros de métricas y logs. |
| **Loki** | Sistema de recolección de logs que solo indexa etiquetas para ser ultraligero. |
| **Promtail** | Agente que captura los logs de los contenedores Docker y los envía a Loki. |
| **Zipkin** | Plataforma de trazabilidad distribuida para medir latencias entre microservicios usando `traceId`. |
| **Correlación Log-Trace**| Saltar de un log con error en Loki a su traza gráfica en Zipkin con un solo clic. |

