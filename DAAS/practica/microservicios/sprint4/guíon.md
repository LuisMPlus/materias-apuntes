### **Tu Guión Individual — Sprint 4 (2:30 a 3:00 min)**

---

#### **[0:00 - 0:25] Introducción: Foco del Sprint**

> **En pantalla:** Pantalla de Login de LogiTrack (`http://localhost:5173/login`) o diagrama general del ecosistema.
> 
> **Speech:**
> *"Buenas tardes. En este Sprint 4 mi responsabilidad principal estuvo centrada en dos pilares transversales críticos del sistema:*
> 1. *La **seguridad e identidad centralizada con Keycloak y RBAC** tanto en backend como frontend.*
> 2. *La **plataforma de observabilidad integral** con Prometheus, Grafana, Loki y Zipkin para monitoreo en tiempo real."*

---

#### **[0:25 - 1:00] Arquitectura de Seguridad: Keycloak y Token JWT**

> **En pantalla:** Consola de administración de Keycloak (`http://localhost:8082`) mostrando el Realm `lazyhawk`, los roles del sistema (`ADMIN`, `CLIENTE`, `REPARTIDOR`, `VENDEDOR`), y opcionalmente la pestaña *Network* de DevTools mostrando la petición de login con el token.
> 
> **Speech:**
> *"En la capa de seguridad, desplegué **Keycloak** como Identity Provider centralizado bajo el realm `lazyhawk`.
> 
> Cuando el usuario inicia sesión desde el frontend a través del **API Gateway**, Keycloak valida credenciales mediante OpenID Connect y emite un token **JWT firmado con RS256**.
> 
> En el backend, los microservicios como **Shipment Service** y **Commerce Service** validan esta firma asimétrica y extraen el contexto del usuario y los roles del `realm_access`, garantizando que endpoints como el historial de envíos o la gestión de paquetes estén blindados a nivel de API."*

---

#### **[1:00 - 2:00] Casos de Prueba de RBAC en el Frontend**

> **En pantalla:** Navegador en `http://localhost:5173`. Realizas los 3 casos de prueba en vivo:

##### **Caso 1: Login como ADMIN (Backoffice Administrativo)**
> **Acción en pantalla:** Inicias sesión con usuario Administrador (`admin@logitrack.com`).
> 
> **Speech:**
> *"Ahora veamos el **control de acceso basado en roles (RBAC) en el Frontend**:
> 
> Al ingresar como **Administrador**, el `AuthContext` decodifica los roles del JWT y la aplicación activa el shell de **Backoffice**. 
> 
> Noten cómo la barra de navegación expone herramientas operativas exclusivas: **Envíos globales**, gestión de **Repartidores**, listado de **Clientes** y métricas de **Comercio**."*

##### **Caso 2: Login como CLIENTE (Portal Autónomo y Menú Dinámico)**
> **Acción en pantalla:** Cierras sesión e inicias sesión con un usuario Cliente (`cliente@logitrack.com` o usuario registrado).
> 
> **Speech:**
> *"Ahora cerramos sesión e ingresamos con el rol **Cliente**. 
> 
> Inmediatamente la interfaz se transforma en el **Portal del Cliente**: el menú administrativo desaparece por completo y se habilitan vistas exclusivas para el usuario final: **Mis Envíos**, **Crear Envío**, el **Catálogo de Productos** y el **Carrito de compras**."*

##### **Caso 3: Prueba de Protección de Rutas (Guardia contra URL Forzada)**
> **Acción en pantalla:** Estando logueado como **CLIENTE**, haces clic en la barra de direcciones del navegador e intentas escribir manualmente: `http://localhost:5173/repartidores` (o `/comercio`). Presionas Enter.
> 
> **Speech:**
> *"Aquí viene la prueba clave de seguridad en cliente: **¿qué pasa si un cliente intenta forzar una URL administrativa?**
> 
> Escribo manualmente `/repartidores` en el navegador y doy Enter. Como observan, el componente `<ProtectedRoute roles={['ADMIN']} />` intercepta la navegación, valida que el rol `CLIENTE` no tiene permisos, y **redirige automáticamente al inicio** sin exponer ningún dato ni componente administrativo."*

---

#### **[2:00 - 2:40] Observabilidad: Grafana, Métricas y Monitoreo de Servicios**

> **En pantalla:** Cambias a la pestaña de Grafana (`http://localhost:3000`) mostrando el dashboard *"LogiTrack — Salud y Rendimiento de Microservicios"* con todos los servicios en verde (**UP**).
> 
> **Speech:**
> *"Para el pilar de **Observabilidad**:
> 
> - Todos los microservicios (incluyendo Gateway, Shipment y Commerce Service) exponen métricas a través de Spring Boot Actuator y Micrometer en `/actuator/prometheus`.
> - Diseñamos este dashboard centralizado en **Grafana**: como pueden ver, todos los servicios reportan estado de salud **UP**, con métricas en vivo de consumo de memoria JVM, hilos activos, peticiones por segundo y tiempos de respuesta.
> - Además, integramos **Zipkin** para trazabilidad distribuida y **Loki** para centralización de logs de los contenedores Docker, lo que nos permite correlacionar cualquier error de punta a punta."*

---

#### **[2:40 - 2:50] Conclusión y Cierre**

> **En pantalla:** Vuelves a la vista principal de LogiTrack o plano general de servicios.
> 
> **Speech:**
> *"Con esto logramos un sistema robusto, con autenticación y autorización seguras según el perfil de usuario, y una infraestructura de telemetría lista para producción. Muchas gracias."*

---

### **Checklist de Preparación para la Grabación:**

1. **Pestaña 1 (Frontend):** `http://localhost:5173/login`
   - Usuario 1 (Admin): `admin@logitrack.com` / contraseña
   - Usuario 2 (Cliente): `cliente@logitrack.com` / contraseña
2. **Pestaña 2 (Keycloak):** `http://localhost:8082` con el Realm `lazyhawk` abierto en *Realm Roles* o *Users*.
3. **Pestaña 3 (Grafana):** `http://localhost:3000` con el Dashboard de *Salud y Rendimiento* (con todos los microservicios en **UP**).
4. **Pestaña 4 (Opcional - Zipkin):** `http://localhost:9411` listo para mostrar una traza de llamada entre servicios.
