# Visualización y Procesamiento de Texto

En los sistemas UNIX y GNU/Linux, **el texto plano es el formato universal** para archivos de configuración, código fuente y registros (*logs*). Bash ofrece un conjunto poderoso de utilidades para leer, filtrar y transformar texto eficientemente.

---

## 1. Visualización de Archivos

### `cat` (Concatenate)
Imprime todo el contenido de uno o más archivos directamente en la terminal.
```bash
# Mostrar contenido completo
cat archivo.txt

# Mostrar número de línea en cada línea no vacía (-b) o en todas (-n)
cat -n script.py

# Mostrar múltiples archivos concatenados
cat parte1.txt parte2.txt
```

### `tac` (Cat invertido)
Muestra el contenido de un archivo línea por línea en **orden inverso** (de la última a la primera).
```bash
tac historial.log
```

### `head` y `tail` (Inicio y Fin de un archivo)
Permiten inspeccionar solo las primeras o últimas líneas de un archivo.

```bash
# Mostrar las primeras 10 líneas por defecto
head archivo.log

# Mostrar las primeras 25 líneas
head -n 25 archivo.log

# Mostrar las últimas 10 líneas por defecto
tail archivo.log

# Mostrar las últimas 50 líneas
tail -n 50 archivo.log

# Seguir un archivo en tiempo real a medida que se escriben nuevas líneas (Ideal para LOGS)
tail -f /var/log/syslog
```

> [!TIP]
> **`tail -f`** es el comando estándar para monitorear servidores, APIs o bases de datos en vivo mientras se ejecutan.

### `less` y `more` (Paginadores interactivos)
Permiten navegar archivos muy largos pantalla por pantalla sin saturar la terminal.

```bash
less archivo_gigante.log
```
- **Navegación en `less`:**
  - `Espacio` o `PageDown`: Avanzar una pantalla.
  - `b` o `PageUp`: Retroceder una pantalla.
  - `/texto`: Buscar un término hacia adelante (presiona `n` para siguiente coincidencia).
  - `?texto`: Buscar un término hacia atrás.
  - `G`: Ir al final del archivo.
  - `g`: Ir al inicio del archivo.
  - `q`: Salir del paginador.

---

## 2. Búsqueda y Filtrado: `grep`

`grep` (*Global Regular Expression Print*) busca líneas que coincidan con un patrón o expresión regular.

```bash
# Buscar una palabra exacta dentro de un archivo
grep "ERROR" /var/log/nginx/error.log

# Búsqueda insensible a mayúsculas/minúsculas (-i = case-insensitive)
grep -i "warning" app.log

# Búsqueda recursiva en todos los archivos de un directorio (-r)
grep -r "DATABASE_URL" src/

# Mostrar el número de línea donde ocurrió la coincidencia (-n)
grep -n "function login" index.js

# Invertir la búsqueda: mostrar todas las líneas que NO coincidan (-v)
grep -v "^#" config.conf  # Oculta líneas comentadas

# Contar cuántas líneas coinciden con el patrón (-c)
grep -c "404" access.log

# Usar expresiones regulares extendidas (-E)
grep -E "([0-9]{1,3}\.){3}[0-9]{1,3}" access.log  # Encuentra direcciones IP
```

---

## 3. Utilidades de Transformación y Estadísticas

### `wc` (Word Count)
Calcula estadísticas de un archivo o flujo de texto:
```bash
# Mostrar: líneas, palabras y bytes
wc archivo.txt

# Contar únicamente el número de LÍNEAS (-l)
wc -l usuarios.csv

# Contar palabras (-w) o caracteres (-m)
wc -w ensayo.txt
```

### `sort` (Ordenar líneas)
```bash
# Ordenar alfabéticamente
sort nombres.txt

# Ordenar de forma inversa (-r = reverse)
sort -r nombres.txt

# Ordenar numéricamente (-n)
sort -n puntuaciones.txt

# Ordenar eliminando duplicados consecutivos (-u = unique)
sort -u ciudades.txt
```

### `uniq` (Eliminar o contar duplicados)
> [!NOTE]
> `uniq` solo detecta líneas repetidas que estén **adyacentes**. Por eso, casi siempre se utiliza después de un comando `sort`.

```bash
# Filtrar líneas duplicadas adyacentes
sort accesos.txt | uniq

# Contar cuántas veces se repite cada línea (-c = count)
sort accesos.txt | uniq -c

# Mostrar solo las líneas que estaban duplicadas (-d)
sort accesos.txt | uniq -d
```

### `cut` (Cortar columnas)
Extrae secciones de cada línea delimitadas por un separador.
```bash
# Extraer la columna 1 y 3 delimitadas por dos puntos (:) en /etc/passwd
cut -d ":" -f 1,3 /etc/passwd

# Extraer columnas delimitadas por comas (CSV)
cut -d "," -f 2 datos.csv
```

### `tr` (Translate / Reemplazar caracteres)
Traduce o elimina caracteres individuales de la entrada estándar:
```bash
# Convertir texto a mayúsculas
echo "hola mundo" | tr 'a-z' 'A-Z'

# Reemplazar espacios por guiones bajos
echo "mi archivo con espacios.txt" | tr ' ' '_'
```

---

## 4. Procesamiento Avanzado: `sed` y `awk`

### `sed` (Stream Editor)
Editor de flujo orientado a realizar transformaciones automáticas y reemplazos de texto mediante expresiones regulares.

```bash
# Sustituir la primera ocurrencia de 'viejo' por 'nuevo' en cada línea
sed 's/viejo/nuevo/' archivo.txt

# Sustituir TODAS las ocurrencias globales (g = global)
sed 's/http:\/\/localhost/https:\/\/miapi.com/g' config.env

# Modificar el archivo DIRECTAMENTE en disco (-i = in-place)
sed -i 's/DEBUG=True/DEBUG=False/g' settings.py

# Eliminar líneas vacías
sed '/^$/d' archivo.txt
```

### `awk` (Lenguaje de procesamiento de texto estructurado)
Herramienta ideal para trabajar con datos tabulares y generar reportes.

```bash
# Imprimir la primera y tercera columna de un comando (por defecto delimitado por espacios)
ls -l | awk '{print $9, $5}'  # Imprime nombre y tamaño

# Delimitar por dos puntos (:) e imprimir con formato
awk -F ":" '{print "Usuario: " $1 " | Directorio: " $6}' /etc/passwd

# Filtrar por condición numérica (ej. listar archivos mayores a 1 MB en ls)
ls -l | awk '$5 > 1048576 {print $9, $5 " bytes"}'
```

---

## 5. Editores de Texto de Terminal

| Editor | Curva de Aprendizaje | Uso recomendado |
| :--- | :--- | :--- |
| **`nano`** | Muy fácil (Intuitivo, muestra atajos abajo como `Ctrl+O` para guardar y `Ctrl+X` para salir) | Ediciones rápidas de configuración. |
| **`vim` / `vi`** | Media/Alta (Basado en modos: Normal, Insertar `i`, Comando `:wq` para guardar y salir) | Edición potente, presente en 100% de los servidores Linux. |

---

## 🧭 Navegación entre Notas

- Nota anterior: **[[Navegación y Archivos]]**
- Siguiente nota: **[[Permisos y Usuarios]]**
- Volver al índice general de BASH: **[[Index|Índice de BASH]]**
