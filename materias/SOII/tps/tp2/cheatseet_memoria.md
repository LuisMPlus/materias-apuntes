# 📖 Cheatsheet: Administración de Memoria y Swap en Linux

### Cátedra: Sistemas Operativos II — UNJu Facultad de Ingeniería

---

## 1. Inspección de Memoria Global del Sistema

### 📊 Comando `free`

Muestra el estado de la memoria RAM y del espacio swap en el sistema:

```bash
# Ver memoria en Megabytes (MB)
free -m

# Ver memoria en formato legible por humanos (MB, GB)
free -h

# Monitorear la memoria actualizando cada 2 segundos de forma continua
free -h -s 2

# Ver totales combinados de RAM + Swap
free -m -t
```

- **`total`**: Memoria física total instalada reconocida por el kernel.
- **`used`**: Memoria ocupada actualmente por procesos (`total - free - buffers - cache`).
- **`free`**: Memoria RAM completamente sin asignar ni utilizar.
- **`buff/cache`**: Memoria utilizada por el kernel para buffers de E/S y caché de páginas de disco.
- **`available`**: **Métrica clave**. Cantidad de memoria disponible para iniciar nuevas aplicaciones sin acudir al swap (incluye RAM libre + caché reciclable de inmediato).

---

### 🔍 Pseudo-Archivo `/proc/meminfo`

Fuente directa en tiempo real provista por el kernel desde donde utilidades como `free` obtienen sus métricas:

```bash
# Ver métricas principales de RAM
grep -E '^(MemTotal|MemFree|MemAvailable|Buffers|Cached|SwapTotal|SwapFree):' /proc/meminfo

# Ver toda la información detallada de memoria
cat /proc/meminfo
```

---

## 2. Estadísticas de Paginación y Rendimiento (`vmstat`)

### ⚡ Comando `vmstat` (_Virtual Memory Statistics_)

Informa sobre procesos en ejecución, memoria, paginación, E/S de bloques, interrupciones y actividad de la CPU:

```bash
# Muestra estadísticas de memoria virtual: 
# Informacion en tiempo real sobre procesos, memoria, paginación, E/S de bloques, interrupciones y actividad de la CPU
# Una muestra instantánea promedio desde el arranque
vmstat

# Tomar 5 muestras cada 1 segundo (la primera es el promedio histórico; las siguientes son en tiempo real)
vmstat 1 5

# Ver estadísticas acumuladas de memoria
vmstat -s
```

### 📋 Columnas Principales de `vmstat`:

- **`procs`**:
    - `r` (_runnable_): Número de procesos listos para ejecutarse o en ejecución (compitiendo por CPU).
    - `b` (_blocked_): Número de procesos en espera no interrumpible (generalmente esperando E/S de disco).
- **`memory`**:
    - `swpd`: Cantidad de memoria virtual utilizada en el swap (en KB).
    - `free`: Cantidad de memoria RAM libre (en KB).
    - `buff`: Memoria usada como buffer para operaciones de bloques.
    - `cache`: Memoria usada como caché de archivos leídos.
- **`swap`**:
    - `si` (_swap in_): Memoria leída desde el disco swap a RAM por segundo (en KB/s).
    - `so` (_swap out_): Memoria guardada desde RAM al disco swap por segundo (en KB/s). **Valores altos continuos indican hiperpaginación (_thrashing_)**.
- **`io`**:
    - `bi` (_blocks in_): Bloques recibidos desde dispositivos de almacenamiento por segundo.
    - `bo` (_blocks out_): Bloques enviados a dispositivos de almacenamiento por segundo.
- **`cpu`**:
    - `us`: Tiempo de CPU ejecutando código de usuario no privilegiado.
    - `sy`: Tiempo de CPU ejecutando código del kernel en Modo Kernel.
    - `id`: Tiempo de CPU ociosa (_idle_).
    - `wa`: Tiempo de CPU esperando que se completen operaciones de E/S (_I/O wait_).

---

## 3. Espacio de Direcciones de un Proceso (`/proc/$PID/maps` y `pmap`)

Cada proceso dispone de su propio mapa de memoria virtual:

```bash
# Inspeccionar las regiones de memoria del proceso actual de la terminal ($$)
cat /proc/$$/maps

# Ver información estructurada con la utilidad pmap
pmap -x $$

# Buscar las zonas de Heap y Stack del proceso
grep -E '\[(heap|stack)\]' /proc/$$/maps
```

- **Texto (`.text`)**: Código ejecutable (permisos `r-xp` = read, execute, private).
- **Datos (`.data` / `.bss`)**: Variables globales inicializadas y no inicializadas (permisos `rw-p`).
- **Montículo (`[heap]`)**: Memoria asignada dinámicamente (`brk`, `sbrk`, `malloc`), crece hacia arriba.
- **Bibliotecas Compartidas**: Mapeadas dinámicamente con `mmap()` (ej. `libc.so`).
- **Pila (`[stack]`)**: Variables locales y registros de activación, crece hacia abajo.

---

## 4. Gestión de Memoria de Intercambio (Swap)

### 🧐 Inspección de Áreas Swap

```bash
# Ver particiones y archivos swap activos y sus prioridades
cat /proc/swaps

# O mediante swapon
swapon --show
```

### 🛠️ Creación de un Archivo Swap Paso a Paso

```bash
# 1. Crear un archivo vacío de 2 MB (1024 bloques de 2048 bytes) lleno de ceros
# of: output file; 
# bs: block size;
#Lugar especial de flujo continuo de bytes llenos de ceros
dd if=/dev/zero of=intercambio.img bs=2048 count=1024

# 2. Asignar permisos estrictos de seguridad (solo lectura/escritura para root)
# chmod: CHange MOD
chmod 600 intercambio.img

# 3. Dar formato de área de intercambio de Linux
mkswap intercambio.img
mkswap -L SWAP_DOCENTE -f soluciones_demo/intercambio.img > soluciones_demo/intercambio.txt

# 4. Activar el archivo swap en el sistema (requiere privilegios root / sudo)
sudo swapon intercambio.img

# 5. Activar con prioridad específica (ej: prioridad 10)
sudo swapon -p 10 intercambio.img

# 6. Desactivar el área de swap
sudo swapoff intercambio.img
```

---

## 5. Algoritmos y Conceptos Teóricos del Kernel

- **Buddy Allocator:** Divide la RAM física en bloques contiguos de tamaño potencia de dos (2k2k). Rápido de asignar y liberar, pero genera **fragmentación interna**.
- **Slab Allocator:** Mecanismo de caché que pre-asigna objetos del kernel de uso recurrente (`task_struct`, `mm_struct`, `inodes`), eliminando la fragmentación interna del Buddy.
- **`vmalloc()`:** Asigna páginas contiguas en el espacio de direcciones virtuales, aunque físicamente estén dispersas en la RAM física.
- **Paginación por Demanda (_Demand Paging_):** Las páginas se cargan en memoria principal solo cuando el proceso las referencia, reduciendo el consumo de RAM.
- **Fallo de Página (_Page Fault_):** Interrupción generada por la MMU cuando el bit de presencia es 0, haciendo que el kernel traiga la página faltante de disco a RAM.
- **Demonio `kswapd`:** Proceso en segundo plano del kernel que monitorea los umbrales de memoria libre (_watermarks_) y envía páginas al swap de manera asíncrona.