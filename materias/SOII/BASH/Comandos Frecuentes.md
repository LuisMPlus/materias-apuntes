# Guía Rápida de Comandos Bash (Cheat Sheet)

Referencia rápida de comandos de Bash agrupados por áreas de administración y desarrollo.

---

## 📁 1. Navegación y Archivos

| Acción | Comando |
| :--- | :--- |
| Ver directorio actual | `pwd` |
| Cambiar de directorio | `cd /ruta/carpeta` (o `cd ..` para subir nivel) |
| Listado detallado y ocultos | `ls -lah` |
| Ver árbol de directorios | `tree -L 2` |
| Crear carpetas anidadas | `mkdir -p carpeta1/carpeta2` |
| Crear archivo vacío | `touch nuevo_archivo.txt` |
| Copiar archivo o carpeta | `cp archivo.txt destino.txt` / `cp -r dir1/ dir2/` |
| Mover o renombrar | `mv viejo.txt nuevo.txt` |
| Borrar archivo o carpeta | `rm archivo.txt` / `rm -rf carpeta/` |
| Buscar archivos por nombre | `find . -name "*.log"` |
| Crear enlace simbólico | `ln -s /ruta/archivo enlace.sh` |

---

## 📝 2. Visualización y Procesamiento de Texto

| Acción | Comando |
| :--- | :--- |
| Ver archivo completo | `cat archivo.txt` |
| Ver archivo paginado | `less archivo.txt` |
| Primeras / Últimas N líneas | `head -n 20 archivo.txt` / `tail -n 20 archivo.txt` |
| Monitorear log en vivo | `tail -f /var/log/syslog` |
| Buscar texto con patrón | `grep -ri "texto_a_buscar" carpeta/` |
| Contar líneas de un archivo | `wc -l archivo.txt` |
| Ordenar líneas | `sort archivo.txt` (o `sort -n` numérico) |
| Filtrar duplicados | `sort archivo.txt | uniq` |
| Cortar por delimitador | `cut -d ":" -f 1 /etc/passwd` |
| Reemplazar texto en archivo | `sed -i 's/antiguo/nuevo/g' config.env` |
| Imprimir columnas específicas | `awk '{print $1, $3}' archivo.txt` |

---

## 🔒 3. Permisos y Usuarios

| Acción | Comando |
| :--- | :--- |
| Dar permiso de ejecución | `chmod +x script.sh` |
| Permisos estándar (755 / 644) | `chmod 755 script.sh` / `chmod 644 doc.txt` |
| Cambiar dueño y grupo | `sudo chown usuario:grupo archivo` |
| Ver usuario actual | `whoami` |
| Ver UID, GID y grupos | `id` |
| Ejecutar como root | `sudo <comando>` |

---

## ⚙️ 4. Procesos y Monitoreo

| Acción | Comando |
| :--- | :--- |
| Listar todos los procesos | `ps aux` |
| Monitor en tiempo real | `htop` o `top` |
| Matar proceso por PID | `kill 1234` (o forzado: `kill -9 1234`) |
| Matar proceso por nombre | `killall node` o `pkill -f "python"` |
| Enviar a segundo plano | `comando &` |
| Ver trabajos en segundo plano | `jobs -l` |
| Traer a primer plano | `fg %1` |
| Ver estado de un servicio | `systemctl status nginx` |
| Iniciar / Reiniciar servicio | `sudo systemctl restart docker` |
| Ver espacio en disco | `df -h` |
| Ver tamaño de carpetas | `du -sh *` |
| Ver uso de memoria RAM | `free -h` |

---

## 🔀 5. Redirecciones y Pipes

| Acción | Operador / Comando |
| :--- | :--- |
| Sobrescribir salida a archivo | `comando > archivo.txt` |
| Añadir salida al final | `comando >> archivo.txt` |
| Redirigir errores | `comando 2> errores.log` |
| Redirigir todo (salida + error) | `comando &> todo.log` |
| Silenciar salida | `comando > /dev/null 2>&1` |
| Tubería (conectar comandos) | `comando1 | comando2` |
| Mostrar y guardar en archivo | `comando | tee archivo.txt` |
| Crear alias temporal | `alias ll="ls -lah"` |

---

## 🧭 Navegación

- Volver al índice general de BASH: **[[Index|Índice de BASH]]**
- Volver al panel principal: **[[Index|Índice de la Facultad]]**
