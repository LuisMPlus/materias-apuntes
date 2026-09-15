```bash
#!/usr/bin/env bash

# ==============================================================================

# SISTEMAS OPERATIVOS II - 2026

# DEMOSTRACIÓN EN CLASE: Introducción a Linux, Shell y Git (TP 0 Demo)

# ==============================================================================

# Este script sirve como demostración práctica para explicar cómo resolver,

# estructurar comandos y autoevaluar las tareas de la materia.

# ==============================================================================

  

# ------------------------------------------------------------------------------

# DEMO 1: Creación de Jerarquía de Proyecto y Archivos

# ------------------------------------------------------------------------------

# Consigna de la Demo:

# 1. Crear las siguientes carpetas en 'soluciones_demo/':

#    - soluciones_demo/proyecto/src/

#    - soluciones_demo/proyecto/data/

#    - soluciones_demo/proyecto/docs/

# 2. Crear un archivo vacío 'soluciones_demo/proyecto/src/main.sh'.

# 3. Crear el archivo 'soluciones_demo/proyecto/docs/autor.txt' con el texto:

#    Catedra Sistemas Operativos II

# ------------------------------------------------------------------------------

demo1_estructura() {

    echo "Ejecutando Ejercicio 1..."

    cd soluciones_demo

    #Crear las carpetas

    mkdir -p proyecto/{src,data,docs}

    #Crear los archivos

    touch proyecto/src/main.sh

    touch proyecto/docs/autor.txt

    #Editar el archivo

    echo "Catedra Sistemas Operativos II - 2026" > proyecto/docs/autor.txt

    cd ..

}

  

# ------------------------------------------------------------------------------

# DEMO 2: Redirecciones, Filtros y Conteo

# ------------------------------------------------------------------------------

# Consigna de la Demo:

# 1. Extraer los últimos 10 accesos del log 'datos/accesos.log' usando 'tail -n 10'

#    y guardarlos en 'soluciones_demo/ultimos_accesos.log'.

# 2. Contar cuántas peticiones tuvieron código de error HTTP 500 en 'datos/accesos.log'

#    y guardar únicamente el número en 'soluciones_demo/total_errores_500.txt'.

# ------------------------------------------------------------------------------

demo2_redirecciones() {

    echo "Ejecutando Ejercicio 2..."

    tail -n 10 datos/accesos.log > soluciones_demo/ultimos_accesos.log

    touch soluciones_demo/total_lineas.txt

    grep "500" datos/accesos.log | wc -l > soluciones_demo/total_errores_500.txt

}

  

# ------------------------------------------------------------------------------

# DEMO 3: Tuberías (Pipes) y Extracción de Rutas Únicas

# ------------------------------------------------------------------------------

# Consigna de la Demo:

# A partir de 'datos/accesos.log':

# 1. Extraer la 4ta columna (la ruta/endpoint solicitada, ej: /index.html).

# 2. Ordenar las rutas alfabéticamente y eliminar duplicados ('sort -u').

# 3. Guardar el resultado en 'soluciones_demo/endpoints_unicos.txt'.

# ------------------------------------------------------------------------------

demo3_tuberias() {

    echo "Ejecutando Ejercicio 3..."

    cut -d " " -f 4 datos/accesos.log | sort -u > soluciones_demo/endpoints_unicos.txt

}

  

# ------------------------------------------------------------------------------

# DEMO 4: Procesamiento de Archivo CSV de Servidores

# ------------------------------------------------------------------------------

# Consigna de la Demo:

# A partir del archivo 'datos/inventario.csv' (ID,Hostname,IP,RAM_GB,CPU_Cores,Estado):

# 1. Filtrar únicamente los servidores en estado 'ONLINE'.

# 2. Extraer las columnas Hostname e IP separadas por coma (columnas 2 y 3).

# 3. Ordenar alfabéticamente por Hostname.

# 4. Guardar la salida en 'soluciones_demo/nodos_online.txt'.

# ------------------------------------------------------------------------------

demo4_inventario_csv() {

    echo "Ejecutando Ejercicio 4..."

  

    grep "ONLINE"  datos/inventario.csv | cut -d "," -f 2,3 | sort > soluciones_demo/nodos_online.txt

  

}

  

# ------------------------------------------------------------------------------

# DEMO 5: Generación de Script Ejecutable y Permisos

# ------------------------------------------------------------------------------

# Consigna de la Demo:

# 1. Crear el script 'soluciones_demo/check_health.sh'.

# 2. El script debe imprimir:

#    - Línea 1: "=== ESTADO DE SALUD DEL NODO ==="

#    - Línea 2: La fecha actual generada con 'date'

#    - Línea 3: "Host: " seguido del nombre del equipo ('hostname' o '$HOSTNAME')

# 3. Asignar permisos de ejecución (chmod +x).

# ------------------------------------------------------------------------------

demo5_script_monitoreo() {

    echo "Ejecutando Ejercicio 5..."

  

    echo '#!/usr/bin/env bash ' > soluciones_demo/check_health.sh

    echo -e "echo \"=== ESTADO DE SALUD DEL NODO ===\"" >> soluciones_demo/check_health.sh

    echo -e "date \necho -n \"Host: \"; hostname" >> soluciones_demo/check_health.sh

    chmod u+x soluciones_demo/check_health.sh

}
```