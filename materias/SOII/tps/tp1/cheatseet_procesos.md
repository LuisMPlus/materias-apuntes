# 📖 Cheatsheet: Administración de Procesos, Señales y /proc

> **Sistemas Operativos II — Ciclo Lectivo 2026**  
> **Universidad Nacional de Jujuy (UNJu) — Facultad de Ingeniería**

---

## 1. 🌲 Jerarquía y Visualización de Procesos

| Comando                                                                                                                                                 | Descripción                                                                   | Ejemplo de Uso       |
| ------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------- | -------------------- |
| `ps`                                                                                                                                                    | Muestra los procesos de la sesión actual de termi                             | `ps`                 |
| `ps aux`                                                                                                                                                | Muestra **todos los procesos** del sistema (formato BSD: user, pid, cpu, mem, | `ps aux`             |
| `ps -ef`        Muestra todos los procesos con UID, PID, PPID (formato estándar POSIX).<br><br>Los que estan en TTY con '?'   son los demonios  en  tan | `ps -ef`                                                                      |                      |
| `ps aux --sort=-%mem`                                                                                                                                   | Ordena los procesos de mayor a menor consumo de me                            | `ps aux --sort=-%mem |
| `ps aux --sort=-%cpu`                                                                                                                                   | Ordena los procesos de mayor a menor con                                      | `ps aux --sort=-%cpu |
| `pstree`                                                                                                                                                | Muestra el **árbol jerárquico** de procesos (quién es pa                      | `pstree`             |
| `pstree -p`                                                                                                                                             | Muestra el árbol de procesos incluyendo los                                   | `pstree -p`          |
| `top`                                                                                                                                                   | Monitor de procesos interacti                                                 | `top`                |
| `htop`                                                                                                                                                  | Monitor interactivo visual mejorado co                                        | `htop`               |

---

## 2. ⚡ Señales y Control de Procesos

|Señal|Número|Significado|Atajo en Teclado / Comando|
|---|---|---|---|
|`SIGINT`|`2`|Interrupción de teclado (Termina el proceso amablemente).|`Ctrl + C` o `kill -2 <PID>`|
|`SIGKILL`|`9`|Terminación forzada e inapelable (el proceso no la puede ignorar ni capturar).|`kill -9 <PID>`|
|`SIGTERM`|`15`|Petición de terminación limpia (por defecto en `kill`).|`kill <PID>` o `kill -15 <PID>`|
|`SIGTSTP`|`20`|Pausa el proceso en ejecución y lo envía a segundo plano en estado _Stopped_.|`Ctrl + Z`|
|`SIGCONT`|`18`|Reanuda la ejecución de un proceso detenido.|`kill -CONT <PID>` o `bg` / `fg`|

---

## 3. 🔄 Gestión de Trabajos en Segundo Plano (_Jobs_)

| Comando     | Descripción                                                                         | Ejemplo              |
| ----------- | ----------------------------------------------------------------------------------- | -------------------- |
| `comando &` | Ejecuta el comando en **segundo plano** (_background_) liberando la consola.        | `sleep 60 &`         |
| `$!`        | Variable especial que contiene el **PID del último proceso lanzado en background**. | `sleep 60 & echo $!` |
| `$$`        | Variable especial que contiene el **PID del proceso o script actual**.              | `echo $$`            |
| `$PPID`     | PID del proceso padre del shell actual.                                             | `echo $PPID`         |
| `jobs`      | Lista los trabajos en segundo plano iniciados desde este shell.                     | `jobs -l`            |
| `fg %1`     | Trae el trabajo número 1 al **primer plano** (_foreground_).                        | `fg %1`              |
| `bg %1`     | Reanuda la ejecución del trabajo detenido número 1 en **segundo plano**.            | `bg %1`              |
|             |                                                                                     |                      |
el proceso con pid 0 es init (booteo)
#### Ejemplo con sleep 400 &
![](../../../../Adjuntos/Pasted%20image%2020260903185156.png)

```bash
grep -E "^(Pid|PPid|Name)" /proc/$$/status
# La montañita "^" nos sirve para indicar que nos devuelve todos los que comienzan con alguno de esas opciones
```


---

## 4. 🗂️ El Pseudo-Sistema de Archivos `/proc`

`/proc` es un sistema de archivos virtual generado en memoria RAM por el Kernel:

- `/proc/cpuinfo`: Información del procesador (modelo, núcleos, arquitectura).
- `/proc/meminfo`: Métricas detalladas de memoria RAM física y Swap.
- `/proc/version`: Versión exacta del Kernel Linux compilado.
- `/proc/[PID]/status`: Estado detallado de un proceso específico (Nombre, Estado `State:`, `Pid:`, `PPid:`, `VmSize:`, `Threads:`).
- `/proc/[PID]/cmdline`: Comando exacto con sus argumentos que inició el proceso.
- `/proc/[PID]/fd/`: Descriptores de archivos abiertos (`stdin`, `stdout`, `stderr`, sockets).