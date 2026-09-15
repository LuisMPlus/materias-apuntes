# Windows Subsystem for Linux (WSL)

El **Subsistema de Windows para Linux (WSL)** es una capa de compatibilidad desarrollada por Microsoft que permite ejecutar un entorno GNU/Linux completo directamente sobre Windows (Windows 10 y 11), sin la sobrecarga de una máquina virtual tradicional ni la necesidad de configurar un arranque dual (*Dual Boot*).

---

## 1. ¿Qué es WSL y cómo funciona?

WSL permite a desarrolladores y administradores de sistemas utilizar herramientas de línea de comandos de Linux, utilidades (`grep`, `sed`, `awk`), lenguajes (`Python`, `C/C++`, `NodeJS`, `Rust`), servidores y flujos de trabajo basados en Unix directamente en Windows, compartiendo archivos e interactuando entre ambos entornos.

### Evolución: WSL 1 vs. WSL 2
![](../../Adjuntos/Pasted%20image%2020260827064410.png)

| Característica               | WSL 1                                                                     | WSL 2                                                           |
| :--------------------------- | :------------------------------------------------------------------------ | :-------------------------------------------------------------- |
| **Arquitectura**             | Capa de traducción de llamadas al sistema (*Syscalls*)                    | Máquina virtual utilitaria liviana con **Kernel Linux real**    |
| **Kernel**                   | No tiene kernel Linux; traduce llamadas de Linux a llamadas NT de Windows | Kernel Linux real optimizado por Microsoft (open source)        |
| **Compatibilidad**           | Parcial (herramientas como Docker o FUSE no funcionaban bien)             | **100% de compatibilidad** con llamadas al sistema Linux        |
| **Rendimiento de E/S (I/O)** | Más lento en el acceso a archivos de Linux                                | Extremadamente rápido en su sistema de archivos nativo (`ext4`) |
| **Integración con Windows**  | Acceso directo a archivos Windows                                         | Integración vía red virtualizada y protocolo 9P/Hyper-V         |
| **Soporte de Docker**        | No nativo                                                                 | Nativo (a través de Docker Desktop con backend WSL 2)           |

> [!NOTE]
> **¿Cómo funciona WSL 2 internamente?**  
> WSL 2 se ejecuta sobre una versión ligera del hipervisor **Hyper-V**. Inicia en menos de 1 o 2 segundos, consume memoria RAM y CPU de forma dinámica (solo lo que los procesos de Linux requieren en el momento y la libera cuando no se usa) y permite aceleración por GPU (CUDA, DirectX, ML) y soporte de aplicaciones gráficas de Linux (**WSLg**).

---

## 2. ¿Crea particiones en el disco?

> [!IMPORTANT]
> **NO crea particiones físicas en el disco duro ni modifica la tabla de particiones (MBR o GPT).**

### ¿Cómo almacena los datos y archivos WSL?

1. **Disco Virtual (`.vhdx`)**:
   - En WSL 2, cada distribución instalada se guarda dentro de un archivo de disco duro virtual llamado `ext4.vhdx` (formato VHDX de Hyper-V).
   - Por defecto, se ubica dentro del sistema de archivos NTFS de Windows en:  
     `%LOCALAPPDATA%\Packages\<NombreDelPaquete>\LocalState\ext4.vhdx`.
   - Este disco virtual **crece dinámicamente** a medida que se instalan paquetes o guardan archivos dentro de Linux.
2. **Sistema de archivos interno**:
   - Dentro del `.vhdx`, el kernel de Linux formatea y maneja el sistema de archivos nativo **ext4**, lo que garantiza permisos POSIX, enlaces simbólicos/duros y velocidad nativa.
3. **Interoperabilidad de almacenamiento**:
   - **Desde Linux a Windows**: Las unidades de Windows se montan automáticamente en `/mnt/c/`, `/mnt/d/`, etc.
   - **Desde Windows a Linux**: Es posible acceder a los archivos de Linux abriendo el Explorador de Archivos y escribiendo `\\wsl$` o `\\wsl.localhost\`.
   - **Montaje de discos físicos (Opcional)**: Si se dispone de un disco físico secundario formateado en `ext4`, WSL 2 permite montarlo directamente mediante el comando `wsl --mount <DiskPath>`.

### ¿En qué se diferencia un archivo `.vhdx` de un `.iso`?

Aunque ambos son archivos contenedores que se montan como unidades virtuales, tienen propósitos y funcionamientos radicalmente diferentes:

| Característica               | **Archivo `.iso`**                                                                                    | **Archivo `.vhdx` (Virtual Hard Disk v2)**                                                                                        |
| :--------------------------- | :---------------------------------------------------------------------------------------------------- | :-------------------------------------------------------------------------------------------------------------------------------- |
| **¿Qué emula?**              | Un **medio óptico (CD / DVD / Blu-ray)**.                                                             | Un **disco duro o SSD físico completo**.                                                                                          |
| **Lectura / Escritura**      | **Solo Lectura (*Read-Only*)**. No permite guardar cambios ni modificar su contenido mientras se usa. | **Lectura y Escritura (*Read/Write*) en tiempo real**. Los programas pueden instalarse y crear/modificar archivos continuamente.  |
| **Uso principal**            | Instaladores de sistemas operativos, imágenes en vivo (*Live CD*) y distribución de software.         | Almacenamiento activo de máquinas virtuales (Hyper-V) y el sistema de archivos completo de **WSL 2**.                             |
| **Sistema de archivos**      | Formatos ópticos estandarizados (`ISO 9660`, `UDF`).                                                  | Cualquier sistema de archivos real (**`ext4`** en WSL 2, `NTFS`, `FAT32`, etc.).                                                  |
| **Comportamiento de tamaño** | **Fijo y estático**: Pesa exactamente el tamaño del medio o los datos originales grabados.            | **Dinámico (Sparse)**: Ocupa en Windows únicamente lo que Linux tiene en uso en el momento y crece automáticamente según demanda. |

> [!TIP]
> **En resumen:**  
> Un `.iso` es equivalente a un **DVD de instalación** (para instalar un SO o arrancar en modo prueba), mientras que el `.vhdx` de WSL 2 es el **disco duro real** donde reside la instalación completa de Linux y todos tus archivos de trabajo.

---

## 3. Distribuciones (Distros) en WSL

WSL no impone una única distribución; permite instalar y ejecutar múltiples distribuciones de Linux de forma simultánea e independiente.

### Distribuciones disponibles oficialmente:
#Ver lista de distribuciones disponibles en línea para instalar
```
wsl --list --online
```

Además, es posible importar cualquier distribución personalizada (como Arch Linux, Fedora, Void Linux, etc.) a partir de un archivo comprimido `rootfs.tar` mediante el comando `wsl --import`.

---

## 4. Comparativa: WSL 2 vs. Dual Boot vs. Máquinas Virtuales (VM)

| Criterio                         | **WSL 2**                                                                                                                 | **Dual Boot (Arranque Dual)**                                                                                              | **Máquina Virtual Tradicional** (VirtualBox / VMware)                                                                    |
| :------------------------------- | :------------------------------------------------------------------------------------------------------------------------ | :------------------------------------------------------------------------------------------------------------------------- | :----------------------------------------------------------------------------------------------------------------------- |
| **Definición**                   | Kernel Linux sobre hipervisor ligero integrado en Windows.                                                                | Instalación de Linux y Windows en particiones separadas del disco físico.                                                  | Virtualización completa del hardware mediante software emulador/hipervisor.                                              |
| **Consumo de Recursos**          | **Dinámico y ligero**: asume memoria según demanda y la libera.                                                           | Utiliza el 100% de la máquina para el SO activo.                                                                           | **Estático y pesado**: asigna núcleos de CPU y RAM fija bloqueada desde el inicio.                                       |
| **Particionado de Disco**        | **No requiere particionar**. Usa un archivo `.vhdx` dinámico.                                                             | **Obligatorio particionar** (riesgo de romper el bootloader/GRUB al actualizar Windows).                                   | **No requiere particionar**. Usa archivos de disco virtual (`.vdi`, `.vmdk`).                                            |
| **Acceso a Hardware**            | Parcial (GPU y redes soportadas; acceso a USB/PCIe requiere configuración extra como `usbipd`).                           | **Acceso total y directo (Bare-Metal)** a todo el hardware (Wi-Fi, Bluetooth, tarjetas de sonido, sensores).               | Emulado o virtualizado (acceso indirecto a través de drivers del hipervisor).                                            |
| **Interoperabilidad**            | **Total**: ejecutar ejecutables de Windows desde bash y viceversa; portapapeles compartido; carpetas compartidas fluidas. | **Nula en tiempo real**: solo se puede usar un SO a la vez. Para cambiar hay que reiniciar.                                | Media: carpetas compartidas configurables, portapapeles bidireccional.                                                   |
| **Soporte de Entornos Gráficos** | Soporte de aplicaciones individuales (GUI vía WSLg / Wayland / X11).                                                      | Escritorio completo (GNOME, KDE Plasma, XFCE, etc.) con aceleración nativa.                                                | Escritorio completo en ventana dedicada.                                                                                 |
| **Ideal para...**                | Desarrollo de software, Docker, scripts, herramientas de terminal, administración de servidores y aprendizaje.            | Gaming nativo en Linux, uso de drivers específicos, benchmarking, desarrollo de drivers o módulos del kernel a bajo nivel. | Pruebas de malware/aislamiento extremo, testeo de múltiples sistemas operativos (FreeBSD, macOS, etc.), redes complejas. |

---

## 5. Ventajas y Desventajas de WSL

### Ventajas
- **Sin fricción de cambio**: No hay necesidad de reiniciar la máquina para pasar de herramientas de Windows a Linux.
- **Integración con IDEs**: Integración de primer nivel con herramientas como **VS Code** (extensión *WSL Remote*), ejecutando el editor en Windows y el servidor de desarrollo dentro del contenedor Linux.
- **Rendimiento superior frente a VMs**: Mucho menor consumo de memoria y CPU en reposo.
- **Soporte de Docker**: Docker Desktop corre nativamente sobre el backend de WSL 2 sin necesidad de crear una pesada máquina virtual Hyper-V dedicada.

### Limitaciones
- **Init/Systemd**: Aunque en versiones recientes ya se puede activar `systemd` agregando `[boot] systemd=true` en `/etc/wsl.conf`, históricamente usaba un sistema de inicio simplificado propio de Microsoft.
- **Red en modo NAT**: Por defecto opera detrás de un adaptador de red virtualizado NAT (aunque Windows 11 ya incluye modo de red `mirrored`).
- **Desarrollo de bajo nivel**: No es adecuado para compilar o depurar controladores de hardware directo del sistema físico.

---

## 6. Comandos Básicos y Útiles de WSL

```bash
# Instalar WSL con la distribución por defecto (Ubuntu)
wsl --install

# Ver lista de distribuciones disponibles en línea para instalar
wsl --list --online

# Ver distro predeterminada
wsl --status

# Instalar una distribución específica
wsl --install -d Debian

# Listar distribuciones instaladas y versión de WSL utilizada
wsl --list --verbose
# o abreviado:
wsl -l -v

# Establecer WSL 2 como versión por defecto
wsl --set-default-version 2

# Ejecutar una distribución específica
wsl -d Ubuntu

# Apagar todas las instancias de WSL en ejecución (liberar memoria)
wsl --shutdown

# Exportar e Importar distribuciones para copias de seguridad
wsl --export Ubuntu D:\backup_ubuntu.tar
wsl --import MiUbuntu C:\WSL\MiUbuntu D:\backup_ubuntu.tar

# Ver ubicación y cuanto ocupa
Get-ChildItem "$env:LOCALAPPDATA" -Filter "ext4.vhdx" -Recurse -ErrorAction SilentlyContinue |
Select-Object FullName, @{Name="GB";Expression={[math]::Round($_.Length / 1GB, 2)}}

#Ubicacion: C:\Users\{username}\AppData\Local\wsl
```
