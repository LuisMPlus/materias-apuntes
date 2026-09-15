
1. Configurar el script
```bash
echo "Hola Mundo"
# listar permisos
ls -l
# Dar permiso de ejecución a usuario
chmod u+x test.sh
chmod u+x ejercicios.sh

# Ejecutar el script test.sh
# sh: es un archivo de texto plano para ser ejecutado en una shell
# test.sh: al ejecutar este archivo nos evaluará si cumplimos con las consignas y nos asignara un puntaje entre 0 y 100
# ejericios.sh: archivo en donde tendremos que poner todos los comandos necesarios para completar los ejercicios
./test.sh
./ejercicios.sh

```

```bash
#!/usr/bin/env bash

# ==============================================================================
# SISTEMAS OPERATIVOS II - 2026
# TRABAJO PRÁCTICO N° 0: Introducción a Linux, Shell y Git
# ==============================================================================
# Nombre del Alumno: Mamaní Luis Carlos Manuel
# Legajo: INF009819
# Usuario de GitHub: LuisMPlus
# ==============================================================================

# INSTRUCCIONES:
# 1. Complete cada una de las funciones con los comandos de Bash necesarios.
# 2. No modifique los nombres de las funciones ni las rutas de los archivos solicitados.
# 3. Para probar sus soluciones localmente, ejecute: ./test.sh
# ==============================================================================
# ------------------------------------------------------------------------------
# EJERCICIO 1: Navegación y Estructura de Directorios (20 Pts)
# ------------------------------------------------------------------------------

# Consigna:
# 1. Cree la siguiente estructura de carpetas dentro del directorio 'soluciones/':
#    - soluciones/entorno/config/
#    - soluciones/entorno/logs/
#    - soluciones/entorno/backup/
# 2. Cree un archivo vacío llamado 'app.conf' en 'soluciones/entorno/config/'.
# 3. Cree un archivo llamado 'version.txt' en 'soluciones/entorno/config/' que
#    contenga exactamente la siguiente línea de texto:
#    Sistemas Operativos II - 2026
# ------------------------------------------------------------------------------

ejercicio1_estructura() {
    echo "Ejecutando Ejercicio 1..."
    # TODO: Escriba sus comandos aquí debajo
    cd soluciones
    #Crear las carpetas
    mkdir -p entorno/{config,logs,backup}
    #Crear los archivos
    touch entorno/config/app.conf
    touch entorno/config/version.txt
    #Editar el archivo
    echo "Sistemas Operativos II - 2026" > entorno/config/version.txt
    cd ..
}
# ------------------------------------------------------------------------------
# EJERCICIO 2: Filosofía UNIX - Redirecciones y Conteo (20 Pts)
# ------------------------------------------------------------------------------

# Consigna:
# 1. Extraiga las primeras 15 líneas del archivo 'datos/servidores.log' usando el
#    comando 'head' y guarde el resultado en 'soluciones/primeros_15.log'.
# 2. Cuente la cantidad total de líneas que tiene 'datos/servidores.log' usando
#    el comando 'wc -l' y guarde ÚNICAMENTE el número resultante en el archivo
#    'soluciones/total_lineas.txt' (sin el nombre del archivo adjunto).
# ------------------------------------------------------------------------------

ejercicio2_redirecciones() {
    echo "Ejecutando Ejercicio 2..."
    # TODO: Escriba sus comandos aquí debajo
    head -n 15 datos/servidores.log > soluciones/primeros_15.log
    touch soluciones/total_lineas.txt
    wc -l < datos/servidores.log > soluciones/total_lineas.txt
}

  

# ------------------------------------------------------------------------------
# EJERCICIO 3: Tuberías y Filtros POSIX - Grep, Cut y Sort (20 Pts)
# ------------------------------------------------------------------------------
# Consigna:
# A partir del archivo de logs 'datos/servidores.log':
# 1. Filtre únicamente los registros que contengan el nivel 'ERROR'.
# 2. Extraiga la segunda columna (dirección IP del servidor).
# 3. Ordene las direcciones IP alfabéticamente y elimine los duplicados.
# 4. Guarde el listado final en 'soluciones/ips_con_error.txt'.
# (Pista: Utilice una tubería que encadene 'grep', 'cut' o 'awk', y 'sort -u')
# ------------------------------------------------------------------------------

ejercicio3_tuberias() {
    echo "Ejecutando Ejercicio 3..."
    # TODO: Escriba sus comandos aquí debajo
    grep "ERROR"  datos/servidores.log | cut -d " " -f 2 | sort -u > soluciones/ips_con_error.txt
}
# ------------------------------------------------------------------------------
# EJERCICIO 4: Procesamiento de Archivos CSV (20 Pts)
# ------------------------------------------------------------------------------
# Consigna:
# El archivo 'datos/usuarios.csv' contiene registros con el formato:
# ID,Nombre,Rol,Departamento,Activo
# Debe procesar este archivo para:
# 1. Filtrar únicamente a los usuarios del departamento 'Sistemas' que estén activos ('true').
# 2. Extraer solo el campo 'Nombre' (segunda columna).
# 3. Ordenar los nombres alfabéticamente de la A a la Z.
# 4. Guardar la lista de nombres en 'soluciones/usuarios_sistemas_activos.txt'.
# ------------------------------------------------------------------------------

ejercicio4_usuarios() {
    echo "Ejecutando Ejercicio 4..."
    # TODO: Escriba sus comandos aquí debajo
    grep "Sistemas"  datos/usuarios.csv | grep "true" | cut -d "," -f 2 | sort > soluciones/usuarios_sistemas_activos.txt
}
# ------------------------------------------------------------------------------
# EJERCICIO 5: Automatización y Permisos de Script (20 Pts)
# ------------------------------------------------------------------------------

# Consigna:
# 1. Cree un script de bash en 'soluciones/generar_reporte.sh'.
# 2. El script debe comenzar con el shebang '#!/usr/bin/env bash'.
# 3. Al ejecutarse, el script debe imprimir por consola:
#    - Línea 1: "=== REPORTE DEL SISTEMA ==="
#    - Línea 2: La fecha actual generada con el comando 'date'
#    - Línea 3: "Usuario: " seguido del nombre del usuario actual ('whoami')
# 4. Otorgue permisos de ejecución al script 'soluciones/generar_reporte.sh' (chmod +x).
# ------------------------------------------------------------------------------

ejercicio5_script_reporte() {
    echo "Ejecutando Ejercicio 5..."
    # TODO: Escriba sus comandos aquí debajo
    echo '#!/usr/bin/env bash ' > soluciones/generar_reporte.sh
    echo -e "echo \"=== REPORTE DEL SISTEMA ===\"" >> soluciones/generar_reporte.sh
    echo -e "date \necho -n \"Usuario: \"; whoami" >> soluciones/generar_reporte.sh
    chmod u+x soluciones/generar_reporte.sh
}
```