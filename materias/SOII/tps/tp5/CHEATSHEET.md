# 🧭 Cheatsheet: Redes y Sistemas Distribuidos en GNU/Linux

> **Cátedra:** Sistemas Operativos II — Ciclo Lectivo 2026  
> **Universidad Nacional de Jujuy (UNJu) — Facultad de Ingeniería**  
> **Unidad Temática N° 5:** Estructura de los Sistemas Distribuidos  

---

## 🌐 1. Inspección de Interfaces de Red y Direcciones IP

```bash
# Mostrar todas las interfaces de red con estado y direcciones IPv4/IPv6
ip addr show
ip -brief addr
```

```bash
# Visualizar solo interfaces activas (UP)
ip link show up
```

```bash
# Obtener dirección IPv4 de la interfaz principal (ej: eth0 o wlan0)
ip -4 addr show eth0 | grep -oP '(?<=inet\s)\d+(\.\d+){3}'

```

```bash
# Listar estadísticas de transmisión/recepción (TX/RX) y errores
ip -s link
```

---

## 🗺️ 2. Tablas de Encaminamiento (Routing) y Puerta de Enlace

```bash
# Mostrar la tabla de encaminamiento del kernel
ip route show
route -n

# Identificar la puerta de enlace predeterminada (default gateway)
ip route show default
ip route show default | awk '{print $3}'

# Comprobar la ruta exacta que tomará el kernel hacia un destino
ip route get 8.8.8.8
```

---

## 🔌 3. Monitoreo de Sockets, Puertos y Conexiones (`ss` y `netstat`)

```bash
# Listar todos los sockets TCP y UDP en estado LISTEN (escuchando) con puertos numéricos
ss -tuln

# Ver sockets TCP establecidos con información de proceso asociado (PID y nombre)
ss -tpe --numeric

# Monitorear estadísticas globales de protocolos de transporte
ss -s

# Ver sockets Unix domain (comunicación interproceso local en el host)
ss -xl
```

---

## 🔎 4. Nombrado y Resolución de Nombres en Red

```bash
# Archivo de resolución estática local (relación IP <-> Hostname)
cat /etc/hosts

# Consultar resolución a través del servicio Name Service Switch (NSS)
getent hosts localhost
getent hosts $(hostname)

# Configuración de servidores DNS
cat /etc/resolv.conf

# Diagnóstico de resolución DNS externa e interna
nslookup google.com
dig +short google.com
```

---

## 📡 5. Diagnóstico de Conectividad y Latencia (RTT)

```bash
# Enviar 4 paquetes ICMP Echo Request para medir Round Trip Time (RTT) y pérdida
ping -c 4 127.0.0.1
ping -c 3 8.8.8.8

# Trazar la ruta de saltos de red hacia un nodo remoto
traceroute -n 8.8.8.8
tracepath 8.8.8.8
```

---

## 🔄 6. Paso de Mensajes y Comunicación por Sockets con Netcat (`nc`)

```bash
# Levantar un servidor TCP en el puerto 9090 (modo escucha):
nc -l -p 9090

# Enviar un mensaje desde el cliente al servidor:
echo "MENSAJE_DISTRIBUIDO" | nc -N 127.0.0.1 9090

# Enviar y recibir por sockets TCP nativos de Bash (sin herramientas externas):
exec 3<>/dev/tcp/127.0.0.1/9090
echo "RPC_REQ:PING" >&3
cat <&3
exec 3<&-
exec 3>&-
```

---

## 🧵 7. Tuberías con Nombre (FIFOs) y Comunicación IPC Asíncrona

```bash
# Crear una tubería FIFO con nombre en el sistema de archivos:
mkfifo /tmp/canal_coordinador

# Proceso lector (bloqueante hasta que alguien escriba):
cat /tmp/canal_coordinador

# Proceso escritor (desbloquea al lector):
echo "RPC_TASK_PAYLOAD" > /tmp/canal_coordinador

# Eliminar el FIFO al terminar:
rm -f /tmp/canal_coordinador
```

---

## 📐 8. Fórmulas de Topologías de Red (Cátedra SO II)

| Topología | Enlaces Físicos ($L$) | Diámetro de Red | Tolerancia a Fallos |
| :--- | :---: | :---: | :--- |
| **Completamente Conectada** | $\frac{N(N-1)}{2}$ | $1$ | Máxima (soporta $N-2$ fallos de enlace por nodo) |
| **Anillo (Ring)** | $N$ | $\lfloor N/2 \rfloor$ | Soporta $1$ fallo de enlace (si es bidireccional) |
| **Estrella (Star)** | $N - 1$ | $2$ | Vulnerable a fallo del nodo central (SPOF) |
| **Bus Compartido** | $1$ medio compartido | $1$ lógico | Vulnerable a corte del cable troncal (CSMA/CD) |
| **Árbol Jerárquico** | $N - 1$ | $2 \cdot h$ | Vulnerable a partición por corte de enlace raíz |
