# GNU Grep — Manual Completo y de Referencia Práctica

> **Referencia Oficial:** GNU Grep versión 3.12 (Enero de 2025)  
> **Autores originales:** Alain Magloire et al.  
> **Licencia:** GNU Free Documentation License (GFDL v1.3+) / GNU GPL  
> **Edición:** Documentación de referencia oficial traducida al español, complementada con casos de uso en terminal, ejemplos de comandos y guías de expresiones regulares para administración de sistemas y programación.

---

## 1. Introducción

Dado uno o más patrones, **`grep`** (*Global Regular Expression Print*) busca en los archivos de entrada líneas que coincidan con dichos patrones. Cuando encuentra una coincidencia en una línea, copia la línea a la salida estándar (`stdout`) por defecto o genera cualquier otro tipo de salida solicitado mediante las opciones de línea de comandos.

### Principios de Funcionamiento:
- **Búsqueda sobre texto sin límites de longitud:** Aunque `grep` asume que la búsqueda se realiza sobre texto plano, no tiene límites respecto a la longitud de las líneas de entrada (más allá de la memoria física disponible en el sistema) y puede coincidir con caracteres arbitrarios dentro de una línea.
- **Manejo del salto de línea final:** Si el último byte de un archivo de entrada no es un carácter de salto de línea (`\n`), `grep` añade uno de forma silenciosa e interna para procesar la última línea sin perderla.
- **Saltos de línea como separadores:** Puesto que el salto de línea actúa también como separador para la lista de patrones, no existe una forma directa de hacer coincidir caracteres de salto de línea dentro de un texto continuo en la modalidad habitual de coincidencia línea por línea (a menos que se utilice la opción `-z` / `--null-data`).

> [!TIP]
> **Ejemplo Básico de Introducción:**
> ```bash
> grep "root" /etc/passwd
> ```
> *Busca la palabra `"root"` en el archivo de usuarios del sistema y muestra las líneas completas coincidentes.*

---

## 2. Invocación de `grep`

### Sinopsis General:
```bash
grep [opción...] [patrones] [archivo...]
```

- Puede haber **cero o más argumentos de opciones**, y **cero o más argumentos de archivos**.
- Si no se especifica ningún archivo (o si se pasa `-`), `grep` lee directamente desde la **entrada estándar (`stdin`)**.
- El argumento `patrones` contiene uno o más patrones separados por saltos de línea. Se omite cuando los patrones se proporcionan mediante las opciones `-e patrones` o `-f archivo`.
- **Regla de oro de la Shell:** Por lo general, los patrones deben encerrarse entre comillas simples (`'...'`) o dobles (`"..."`) al invocar `grep` para evitar interpretaciones indebidas de comodines (*globbing*) o expansiones de variables de la shell.

---

## 2.1 Opciones de Línea de Comandos

`grep` incluye un nutrido conjunto de opciones: algunas provienen del estándar **POSIX** y otras son **extensiones de GNU**. Los nombres de opciones largas (ej. `--ignore-case`) siempre son extensiones de GNU, incluso para aquellas que proceden de las especificaciones POSIX.

### 2.1.1 Información Genérica del Programa

#### `--help`
Imprime un mensaje de uso que resume brevemente las opciones de línea de comandos y la dirección de reporte de errores, y luego finaliza.
```bash
grep --help | head -n 15
```
*Muestra las primeras 15 líneas de ayuda rápida de opciones.*

#### `-V`, `--version`
Imprime el número de versión de `grep` en el flujo de salida estándar. Este número de versión debe incluirse en todos los informes de errores.
```bash
grep --version
```
*Retorna: `grep (GNU grep) 3.12` junto con los datos de licencia y copyright.*

---

### 2.1.2 Control de Coincidencia

#### `-e patrones`, `--regexp=patrones`
Utiliza `patrones` como uno o más patrones de búsqueda; los caracteres de salto de línea dentro de `patrones` separan cada patrón del siguiente. Si esta opción se utiliza varias veces o se combina con `-f`, busca todos los patrones proporcionados. También es esencial para buscar patrones que comienzan con un guion `-` sin que se interpreten como flags.
```bash
# Buscar múltiples patrones alternativos:
grep -e "warning" -e "error" server.log

# Buscar de forma segura la cadena literal "-v" sin confundir a grep:
grep -e "-v" script.sh
```

#### `-f archivo`, `--file=archivo`
Obtiene los patrones desde `archivo`, uno por línea. Si esta opción se utiliza varias veces o se combina con `-e`, busca todos los patrones indicados. Si `archivo` es `-`, lee los patrones desde la entrada estándar.
```bash
echo -e "404\n500\n502" > codigos_error.txt
grep -f codigos_error.txt /var/log/nginx/access.log
```
*Filtra el log buscando cualquiera de los códigos HTTP listados en el archivo.*

#### `-i`, `-y`, `--ignore-case`
Ignora las diferencias entre mayúsculas y minúsculas tanto en el patrón como en los archivos de entrada (búsqueda *case-insensitive*).
```bash
grep -i "database" config.ini
```
*Coincide con `DATABASE`, `Database`, `database` y cualquier combinación mixta.*

#### `--no-ignore-case`
Restaura la distinción estricta de mayúsculas y minúsculas. Útil para anular un alias previo (como `alias grep='grep -i'`) en scripts o subshells.
```bash
grep --no-ignore-case "ERROR" log.txt
```
*Busca únicamente "ERROR" en mayúsculas sostenidas.*

#### `-v`, `--invert-match`
Invierte el sentido de la coincidencia para seleccionar las líneas que **no coinciden** con el patrón dado.
```bash
grep -v "^#" /etc/hosts
```
*Muestra las líneas del archivo `/etc/hosts` omitiendo los comentarios (líneas que empiezan con `#`).*

#### `-w`, `--word-regexp`
Selecciona únicamente aquellas líneas que contienen coincidencias que forman **palabras completas**, delimitadas por caracteres no alfanuméricos ni guion bajo (bordes de palabra).
```bash
grep -w "log" main.c
```
*Coincide con `log("iniciando")`, pero no coincide con `login_user()` ni `catalog`.*

#### `-x`, `--line-regexp`
Selecciona solo aquellas coincidencias que abarcan **exactamente la totalidad de la línea**. Equivale a rodear el patrón con anclajes estricto `^...$`.
```bash
grep -x "OK" status.txt
```
*Solo selecciona líneas que contengan estrictamente "OK" y ningún otro carácter.*

---

### 2.1.3 Control General de Salida

#### `-c`, `--count`
Suprime la salida normal de líneas; en su lugar imprime el **número total de líneas coincidentes** por cada archivo. Combinado con `-v`, cuenta las líneas que no coinciden.
```bash
grep -c "Failed password" /var/log/auth.log
```
*Imprime la cantidad total de intentos de acceso fallidos registrados.*

#### `--color[=CUÁNDO]`, `--colour[=CUÁNDO]`
Resalta en color la parte del texto coincidente en la terminal. Valores permitidos: `always`, `never` o `auto`.
```bash
grep --color=always "IP" connections.txt | less -R
```
*Conserva el resaltado de color al paginar con `less` mediante la opción `-R`.*

#### `-l`, `--files-with-matches`
Suprime las líneas coincidentes; solo imprime el **nombre de los archivos** que contienen al menos una coincidencia. Detiene la lectura de cada archivo tras el primer hallazgo (optimiza tiempo de I/O).
```bash
grep -l "import React" src/*.js
```
*Lista qué componentes de JavaScript importan React.*

#### `-L`, `--files-without-match`
Imprime el nombre de cada archivo que **no contiene ninguna coincidencia** con el patrón.
```bash
grep -L "Copyright" *.py
```
*Identifica los scripts en Python a los que les falta la cabecera de Copyright.*

#### `-m núm`, `--max-count=núm`
Se detiene tras haber encontrado y mostrado `núm` líneas coincidentes en un archivo.
```bash
grep -m 5 "GET /api/" access.log
```
*Extrae únicamente las primeras 5 peticiones a la API del log.*

#### `-o`, `--only-matching`
Imprime **única y exclusivamente la subcadena coincidente**, cada una en una línea nueva separada, en lugar de imprimir toda la línea de texto.
```bash
grep -oE "[0-9]+\.[0-9]+\.[0-9]+\.[0-9]+" access.log
```
*Extrae todas las direcciones IPv4 aisladas de un archivo de log.*

#### `-q`, `--quiet`, `--silent`
Modo silencioso. No imprime nada en consola. Retorna código de estado `0` si hubo coincidencia y `1` si no hubo. Ideal para condicionales `if` en scripts de Bash.
```bash
if grep -q "elias" /etc/passwd; then
    echo "Usuario existe"
fi
```

#### `-s`, `--no-messages`
Suprime los mensajes de advertencia o error producidos por archivos que no existen o sobre los que no se tienen permisos de lectura.
```bash
grep -s "root" /etc/shadow /etc/gshadow
```
*Evita que la pantalla se llene de avisos de `"Permission denied"`.*

---

### 2.1.4 Control del Prefijo de Línea de Salida

#### `-n`, `--line-number`
Prefija cada línea de salida con el **número de línea** que ocupa dentro de su archivo original (comenzando en 1).
```bash
grep -n "def main" *.py
```
*Salida de ejemplo: `app.py:14:def main():` (indica archivo y número de línea).*

#### `-b`, `--byte-offset`
Imprime el desplazamiento de bytes en base 0 dentro del archivo antes de cada línea de salida. Si se combina con `-o`, indica el byte exacto donde inicia la coincidencia.
```bash
grep -b -o "HTTP" payload.bin
```
*Muy útil en análisis forense digital y análisis de binarios para localizar offsets exactos.*

#### `-H`, `--with-filename` / `-h`, `--no-filename`
- `-H`: Fuerza a mostrar el nombre del archivo en cada línea coincidente (comportamiento por defecto al buscar en múltiples archivos).
- `-h`: Oculta el nombre del archivo en la salida (comportamiento por defecto al buscar en un único archivo).
```bash
grep -h "PORT=" .env*
```
*Muestra solo las variables de puerto de varios archivos de entorno sin incluir el nombre del archivo.*

#### `--label=ETIQUETA`
Asigna un nombre simulado al flujo recibido por entrada estándar (`stdin`).
```bash
curl -s https://api.ipify.org | grep --label="Mi_IP" -H ".*"
```
*Salida: `Mi_IP:181.xxx.xxx.xxx`*

#### `-Z`, `--null`
Emite un byte nulo ASCII NUL (`\0`) tras el nombre del archivo en lugar del salto de línea o los dos puntos habituales. Diseñado para procesar archivos de forma segura junto con `xargs -0`.
```bash
grep -lZ "TODO" *.txt | xargs -0 rm -f
```
*Elimina archivos con coincidencias de forma segura, incluso si sus nombres contienen espacios o saltos de línea.*

---

### 2.1.5 Control de Líneas de Contexto

Permite inspeccionar las líneas adyacentes a cada coincidencia, fundamental para depuración y análisis de trazas de error (*logs*).

```
          [ Líneas previas: -B (Before) ]
                      ↓
=============> LÍNEA COINCIDENTE <=============
                      ↓
          [ Líneas posteriores: -A (After) ]
```

#### `-A núm`, `--after-context=núm`
Muestra `núm` líneas de contexto posterior tras cada coincidencia.
```bash
grep -A 3 "Traceback" app.log
```
*Imprime la línea del Traceback y las siguientes 3 líneas del error.*

#### `-B núm`, `--before-context=núm`
Muestra `núm` líneas de contexto anterior antes de cada coincidencia.
```bash
grep -B 2 "Exception" app.log
```
*Muestra qué operación previa detonó la excepción.*

#### `-C núm`, `--context=núm`
Muestra `núm` líneas de contexto tanto antes como después (equivale a combinar `-A núm -B núm`).
```bash
grep -C 2 "CRITICAL" server.log
```
*Da una vista de 2 líneas antes y 2 líneas después del evento crítico.*

---

### 2.1.6 Selección de Archivos y Directorios

#### `-r`, `--recursive` / `-R`, `--dereference-recursive`
Busca recursivamente dentro de todos los subdirectorios.
- `-r`: No sigue enlaces simbólicos (*symlinks*) que apunten a directorios dentro del árbol.
- `-R`: Sigue todos los enlaces simbólicos recursivamente.
```bash
grep -r "API_KEY" /var/www/proyecto/
```
*Escanea todo el código fuente del proyecto en busca de la clave.*

#### `--include=patrón` / `--exclude=patrón` / `--exclude-dir=patrón`
Filtra archivos o carpetas específicas mediante patrones *glob* (comodines) durante búsquedas recursivas.
```bash
# Solo buscar dentro de archivos Python (.py):
grep -r --include="*.py" "import os" .

# Omitir carpetas pesadas o irrelevantes:
grep -r --exclude-dir=".git" --exclude-dir="node_modules" "test" .
```

#### `-a`, `--text`, `--binary-files=text`
Obliga a `grep` a procesar un archivo binario como si fuera texto plano.
```bash
grep -a "ELF" /bin/ls
```
*Lee el ejecutable binario y busca la cadena literal "ELF".*

#### `-I`, `--binary-files=without-match`
Omite de forma automática y silenciosa cualquier archivo detectado como binario.
```bash
grep -r -I "config" .
```
*Evita los mensajes de `"Binary file matches"` en imágenes, objetos o binarios compilados.*

---

### 2.1.7 Otras Opciones Especiales

#### `--` (Doble Guion)
Indica el **final explícito de las opciones**. Cualquier argumento que le siga se considerará estrictamente un patrón o nombre de archivo, evitando que cadenas que comiencen con `-` sean interpretadas como flags.
```bash
grep -- "-f" lista_argumentos.txt
```
*Busca la cadena `"-f"` sin que `grep` intente cargar un archivo de patrones.*

#### `--line-buffered`
Fuerza a vaciar el búfer de salida (*buffer flush*) inmediatamente tras procesar cada línea, evitando retenciones de I/O en tuberías continuas.
```bash
tail -f access.log | grep --line-buffered "404" | tee errores.log
```
*Permite monitoreo y escritura en disco en tiempo real sin retrasos por almacenamiento en búfer.*

#### `-z`, `--null-data`
Trata la entrada y la salida como líneas delimitadas por **bytes nulos (`\0`)** en lugar de saltos de línea (`\n`). Permite procesar salidas de comandos con `-print0` o realizar búsquedas que crucen múltiples líneas.
```bash
find . -name "*.txt" -print0 | grep -z "proyecto"
```
*Manejo completamente seguro de listas de archivos delimitadas por byte nulo.*

---

## 2.2 Variables de Entorno

El comportamiento operativo, lingüístico y visual de `grep` depende de las siguientes variables de entorno:

| Variable | Función | Ejemplo Práctico |
| :--- | :--- | :--- |
| **`GREP_COLORS`** | Configura los colores ANSI para resaltar texto (`ms`), números de línea (`ln`), rutas (`fn`) y separadores. | `export GREP_COLORS='ms=01;32:fn=01;34'`<br>*(Coincidencias en verde negrita, nombres de archivo en azul).* |
| **`LC_ALL` / `LANG`** | Define el idioma, la codificación (UTF-8, ASCII) y el orden de colación en rangos como `[a-z]`. | `LC_ALL=C grep "patron" archivo`<br>*(Máxima velocidad de procesamiento al forzar modo byte único).* |
| **`POSIXLY_CORRECT`** | Desactiva extensiones GNU y obliga a seguir la convención estricta del estándar POSIX. | `POSIXLY_CORRECT=1 grep ...` |

---

## 2.3 Códigos y Estado de Salida (*Exit Status*)

`grep` devuelve un código numérico al finalizar (`$?` en Bash) que describe el resultado de la operación. Es fundamental para la lógica de control en scripts:

| Código | Significado | Uso típico en automatización |
| :---: | :--- | :--- |
| **`0`** | Se encontró al menos una coincidencia. | Continuar ejecución normal o disparar una alerta de éxito. |
| **`1`** | El archivo fue leído pero **no hubo coincidencias**. | Flujo alternativo en scripts (no constituye un error crítico de sistema). |
| **`2`** | Ocurrió un **error del sistema** (sintaxis inválida, archivo inexistente, sin permisos). | Manejar excepción en scripts o abortar la ejecución inmediatamente. |

```bash
grep -q "error" /var/log/syslog
case $? in
  0) echo "Se encontraron errores en el log." ;;
  1) echo "No hay errores." ;;
  2) echo "Error al abrir o leer el archivo de log." ;;
esac
```

---

## 2.4 Variantes del Programa `grep`

`grep` puede operar bajo diferentes motores de análisis de patrones según la opción seleccionada:

### `-G`, `--basic-regexp`
**Expresiones Regulares Básicas (BRE).** Es el modo de operación predeterminado de GNU Grep. En BRE, los metacaracteres como `?`, `+`, `{`, `|`, `(`, `)` se consideran caracteres literales a menos que se escapen con barra invertida (`\?`, `\+`, `\{`, `\|`, `\(`).

### `-E`, `--extended-regexp`
**Expresiones Regulares Extendidas (ERE)** (antiguamente invocado como el ejecutable `egrep`). Permite el uso directo de operadores como `+`, `?`, `|` y paréntesis de agrupación `()` sin necesidad de barras invertidas.
```bash
grep -E "(nginx|apache|caddy)" /etc/services
```
*Busca líneas que contengan cualquiera de los tres servidores web.*

### `-F`, `--fixed-strings`
**Búsqueda de Cadenas Fijas y Literales Exactas** (antiguamente invocado como `fgrep`). Trata todos los caracteres como literales sin interpretar metacaracteres, ofreciendo la máxima velocidad de búsqueda posible mediante algoritmos de cadenas múltiples.
```bash
grep -F "user.name.*" doc.txt
```
*Busca literalmente el texto `"user.name.*"` sin interpretar el punto ni el asterisco.*

### `-P`, `--perl-regexp`
**Expresiones Regulares Compatibles con Perl (PCRE2).** Brinda la mayor potencia sintáctica, soportando aserciones *lookaround* (*lookahead* y *lookbehind*), clases de dígitos abreviadas `\d`, cuantificadores no codiciosos (*lazy/non-greedy* `.*?`), entre otros.
```bash
grep -P "(?<=token=)[a-zA-Z0-9]+" url.txt
```
*Extrae el valor del token usando lookbehind positivo (`(?<=...)`).*

---

## 3. Expresiones Regulares

### 3.1 Estructura Fundamental y Cuantificadores

| Operador | Descripción | Ejemplo de Expresión | Resultado / Coincidencia |
| :---: | :--- | :--- | :--- |
| **`.`** | Cualquier carácter individual excepto salto de línea. | `c.sa` | Coincide con `"casa"`, `"cosa"`, `"cusa"`. |
| **`?`** | Opcional: 0 o 1 ocurrencia (en ERE o `\?` en BRE). | `https?://` | Coincide con `"http://"` y `"https://"`. |
| **`*`** | Cero o más repeticiones consecutivas. | `go*l` | Coincide con `"gl"`, `"gol"`, `"goool"`. |
| **`+`** | Una o más repeticiones consecutivas (en ERE o `\+`). | `go+l` | Coincide con `"gol"`, `"goool"` (no con `"gl"`). |
| **`{n}`** | Exactamente `n` repeticiones consecutivas. | `[0-9]{4}` | Coincide con un año de 4 dígitos como `"2026"`. |
| **`{n,m}`** | Entre `n` y `m` repeticiones inclusive. | `[a-z]{3,5}` | Palabras de 3 a 5 letras minúsculas. |
| **`\|`** | Alternancia lógica (OR). | `grep -E "azul\|rojo\|verde" archivo.txt` | Líneas con alguna de las 3 opciones. |

---

### 3.2 Clases de Caracteres POSIX entre Corchetes

Las clases de caracteres POSIX proporcionan una forma estándar y portátil de definir conjuntos de caracteres independientes del idioma o locale:

| Clase POSIX | Equivalencia ASCII | Ejemplo de Comando | Propósito |
| :--- | :--- | :--- | :--- |
| **`[[:alnum:]]`** | `[0-9A-Za-z]` | `grep -E "^[[:alnum:]]+$" id.txt` | Caracteres alfanuméricos. |
| **`[[:alpha:]]`** | `[A-Za-z]` | `grep "[[:alpha:]]" datos.csv` | Letras únicamente. |
| **`[[:blank:]]`** | Espacio y tabulación (`[ \t]`) | `grep "^[[:blank:]]*$" vacios.txt` | Espacios en blanco y tabulaciones. |
| **`[[:digit:]]`** | `[0-9]` | `grep -E "[[:digit:]]{3}-[[:digit:]]{4}" tel.txt` | Dígitos numéricos. |
| **`[[:lower:]]`** | `[a-z]` | `grep "^[[:lower:]]" nombres.txt` | Letras minúsculas. |
| **`[[:upper:]]`** | `[A-Z]` | `grep "^[[:upper:]]{3}" codigos.txt` | Letras mayúsculas. |
| **`[[:punct:]]`** | Signos de puntuación | `grep "[[:punct:]]" passwords.txt` | Signos y caracteres especiales. |
| **`[[:space:]]`** | Espacios, tab, saltos de línea | `grep "[[:space:]]" archivo.txt` | Caracteres de espaciado en blanco. |

> [!NOTE]
> Para negar una clase o conjunto entre corchetes, se antepone el acento circunflejo `^` justo tras el corchete de apertura: `[^[:digit:]]` coincide con cualquier carácter que **no** sea un dígito.

---

### 3.3 Expresiones Especiales con Barra Invertida

- **`\b`** : Límite de palabra (*word boundary*).  
  *Ejemplo:* `grep -E "\btest\b" doc.txt` *(coincide con "test", pero no con "testing" ni "attest")*.
- **`\B`** : No límite de palabra (*non-word boundary*).  
  *Ejemplo:* `grep -E "\Bauto\B" doc.txt` *(coincide con la subcadena "auto" dentro de "desautomatización")*.
- **`\<` / `\>`** : Coinciden con el inicio y final de una palabra respectivamente.
- **`\w`** : Constituyente de palabra (letras, números y guion bajo `_`, equivalente a `[_[:alnum:]]`).  
  *Ejemplo:* `grep -E "^\w+$" usuarios.txt`
- **`\W`** : No constituyente de palabra (símbolos, espacios, puntuación, equivalente a `[^_[:alnum:]]`).  
  *Ejemplo:* `grep "\W" log.txt`
- **`\s`** : Espacio en blanco (espacios, tabs, saltos de línea, equivalente a `[[:space:]]`).
- **`\S`** : No espacio en blanco (cualquier carácter que no sea espacio, equivalente a `[^[:space:]]`).

---

### 3.4 Anclajes (*Anchors*)

Los anclajes restringen la posición donde el patrón debe coincidir dentro de la línea, en lugar de coincidir en cualquier posición intermedia:

- **`^`** : Ancla al inicio de la línea.
- **`$`** : Ancla al final de la línea.
- **`^$`** : Línea totalmente vacía.

```bash
# Solo líneas que comienzan con "root":
grep "^root" /etc/passwd

# Solo líneas que terminan con "bash":
grep "bash$" /etc/passwd

# Encuentra líneas totalmente vacías:
grep "^$" archivo.txt

# Muestra la configuración real omitiendo comentarios y líneas vacías:
grep -v "^#" /etc/config | grep -v "^$"
```

---

### 3.5 Referencias Hacia Atrás (*Back-references*)

Las referencias hacia atrás permiten hacer coincidir nuevamente la misma subcadena que fue capturada previamente por un grupo de captura `(...)`. Se hace referencia al grupo mediante `\n`, donde `n` es el número del grupo (de 1 a 9).

```bash
# Encontrar etiquetas HTML que abren y cierran con el mismo nombre:
grep -E "<([a-z]+)>.*</\1>" pagina.html
```
*Coincide con estructuras correctamente cerradas como `<b>texto</b>` o `<h1>título</h1>`, pero descarta discrepancias como `<b>texto</i>`.*

---

### 3.6 Expresiones Regulares Básicas (BRE) vs. Extendidas (ERE)

En GNU Grep existen dos sintaxis principales estandarizadas por POSIX:

| Característica | BRE (`grep` / `grep -G`) | ERE (`grep -E` / `egrep`) |
| :--- | :--- | :--- |
| **Operador OR (`\|`)** | Requiere barra: `\|` | Directo: `\|` |
| **Grupos de captura (`()`)** | Requiere barra: `\(...\)` | Directo: `(...)` |
| **Cuantificador una o más (`+`)** | Requiere barra: `\+` | Directo: `+` |
| **Cuantificador cero o una (`?`)** | Requiere barra: `\?` | Directo: `?` |
| **Cuantificadores de rango (`{n,m}`)** | Requiere barra: `\{n,m\}` | Directo: `{n,m}` |
| **Referencias hacia atrás** | Soportado (`\1` a `\9`) | Soportado en GNU Grep (`\1` a `\9`) |

---

### 3.7 Expresiones Regulares Problemáticas

Al diseñar patrones complejos, se deben tener en cuenta posibles penalizaciones de rendimiento:
- **Retroceso Catastrófico (*Catastrophic Backtracking*):** Ocurre en motores de expresiones regulares cuando se anidan cuantificadores ambiguos (ej. `(a+)+$`). Ante entradas que casi coinciden pero fallan al final, el motor evalúa un número exponencial de combinaciones antes de fallar.
- **Uso excesivo de Back-references:** El uso de `\1`, `\2` rompe la regularidad matemática de los autómatas finitos deterministas (DFA), obligando al motor a recurrir a algoritmos de retroceso de complejidad exponencial.

---

### 3.8 Codificación de Caracteres y Datos no ASCII

- **Impacto de Locales UTF-8:** En entornos modernos (`LANG=es_ES.UTF-8` o `en_US.UTF-8`), caracteres multibyte y colaciones complejas pueden alterar el significado de rangos como `[a-z]` (que en algunos locales incluye letras mayúsculas intermedias o con tildes).
- **Aceleración con `LC_ALL=C`:** Establecer `LC_ALL=C` fuerza la interpretación tradicional de bytes individuales ASCII (0–127/255), garantizando que `[a-z]` abarque estrictamente los caracteres ASCII estándar y acelerando radicalmente la ejecución.

---

## 4. Ejemplos Prácticos y Preguntas Frecuentes (FAQ)

### 1. ¿Cómo listar únicamente los nombres de los archivos que contienen coincidencias?
Utilice la opción `-l` (*files with matches*):
```bash
grep -l "main" *.c
```
*Lista qué archivos de código fuente en C contienen la función `main`.*

---

### 2. ¿Cómo buscar recursivamente en directorios ignorando archivos y carpetas de control de versiones?
Utilice `-r` combinado con `--exclude-dir`:
```bash
grep -r --exclude-dir={.git,node_modules,build} "DATABASE_URL" .
```

---

### 3. ¿Cómo buscar un patrón que empieza con guion «`-`» de forma segura?
Utilice la opción `-e` o el separador de fin de opciones `--`:
```bash
grep -e "-v" archivo.sh
# o también:
grep -- "-v" archivo.sh
```

---

### 4. ¿Cómo buscar una palabra exacta y no subcadenas?
Utilice la opción `-w` (*word regexp*):
```bash
grep -w "error" server.log
```
*Coincide con `"error"`, pero ignora palabras como `"erroneous"` o `"noerror"`.*

---

### 5. ¿Cómo ver las líneas que rodean a un error para entender su contexto?
Utilice la opción `-C` (*context*):
```bash
grep -C 3 "NullPointerException" catalina.out
```
*Muestra 3 líneas antes y 3 líneas después de la ocurrencia del error.*

---

### 6. ¿Cómo forzar la inclusión de nombres de archivos al buscar en un único archivo?
Por defecto, al consultar un único archivo `grep` no imprime el nombre del archivo. Fuerce su inclusión con `-H`:
```bash
grep -H "timeout" /etc/nginx/nginx.conf
```

---

### 7. ¿Por qué se utiliza el truco de corchetes `[c]ron` al filtrar `ps -ef`?
Al buscar procesos en ejecución, el propio comando `grep` aparece en la tabla de procesos. El truco de la clase de corchetes evita que `grep` se devuelva a sí mismo:
```bash
ps -ef | grep '[c]ron'
```
*La expresión regular busca la cadena `"cron"`. Sin embargo, en la lista de procesos el comando en ejecución aparece como `grep [c]ron`, por lo que no coincide con su propio patrón y no se lista.*

---

### 8. ¿Cómo solucionar el mensaje `"Binary file matches"`?
- Para forzar la lectura e impresión como texto plano:
  ```bash
  grep -a "cadena" archivo.bin
  ```
- Para ignorar silenciosamente los archivos binarios sin mostrar mensajes molestos:
  ```bash
  grep -I -r "cadena" .
  ```

---

### 9. ¿Cómo realizar una búsqueda lógica AND (líneas que contengan dos palabras distintas)?
Encadene múltiples invocaciones de `grep` mediante tuberías (*pipes*):
```bash
grep "usuario" app.log | grep "autenticado"
```

---

### 10. ¿Cómo buscar patrones que abarcan saltos de línea con `-z`?
Utilice la opción `-z` junto con expresiones compatibles con Perl (`-P`):
```bash
printf "primera_linea\nsegunda_linea\n" | grep -zP "primera_linea\nsegunda_linea"
```

---

## 5. Rendimiento y Optimización

GNU Grep es reconocido por ser uno de los programas de búsqueda de texto más rápidos que existen gracias a su diseño de ingeniería:
- **Algoritmo Boyer-Moore:** Empleado cuando se busca una única cadena fija. Permite inspeccionar el texto saltando bloques completos sin tener que analizar cada byte de entrada.
- **Algoritmo Aho-Corasick:** Empleado cuando se buscan conjuntos de múltiples patrones fijos (con `-F` y `-f`), realizando el escaneo en una sola pasada con complejidad lineal.

> [!TIP]
> **Consejo de Alto Rendimiento para Logs Masivos:**  
> Cuando busque en archivos de varios gigabytes (logs de servidores de alta concurrencia), anteponga `LC_ALL=C` para omitir el análisis de decodificación multibyte de UTF-8:
> ```bash
> LC_ALL=C grep -F "cadena_fija" access_log_10GB.log
> ```
> *Puede resultar entre **5 y 10 veces más rápido** que la ejecución estándar en locales UTF-8.*

---

## 6. Reporte de Errores y Bugs Conocidos

- **Seguimiento oficial de bugs:** Para verificar reportes de errores existentes en GNU Grep, visite el registro oficial de fallos del proyecto GNU en [debbugs.gnu.org](https://debbugs.gnu.org).
- **Reportar nuevos fallos:** Para notificar un nuevo bug a los mantenedores, envíe un correo a `bug-grep@gnu.org` incluyendo siempre la versión (`grep --version`) y el entorno (`LC_ALL` / `LANG`).
- **Rendimiento con Back-references:** El uso de referencias hacia atrás (`\1`, `\2`) en expresiones regulares complejas recurre a algoritmos de retroceso (*backtracking*) de orden exponencial en tiempo ($O(2^n)$), lo que puede ralentizar sustancialmente las búsquedas sobre volúmenes masivos de datos.

---

## 7. Términos de Licencia y Copia

- **Software:** GNU Grep es software libre publicado bajo la licencia **GNU GPL** (*General Public License*).
- **Documentación:** El manual de GNU Grep se distribuye bajo los términos de la **GNU Free Documentation License (GFDL)**, Versión 1.3 o cualquier versión posterior publicada por la Free Software Foundation; sin Secciones Invariantes, sin Textos de Portada y sin Textos de Contraportada.

---

## 8. Índice Alfabético Rápido de Opciones

| Opción / Comando | Sección de Referencia | Descripción Breve |
| :--- | :---: | :--- |
| **`-A núm`**, **`--after-context`** | 2.1.5 | Líneas de contexto posterior tras coincidencia. |
| **`-a`**, **`--text`** | 2.1.6 | Procesa archivos binarios como texto plano. |
| **`-B núm`**, **`--before-context`** | 2.1.5 | Líneas de contexto anterior antes de coincidencia. |
| **`-b`**, **`--byte-offset`** | 2.1.4 | Imprime desplazamiento en bytes (base 0) antes de cada línea. |
| **`-C núm`**, **`--context`** | 2.1.5 | Líneas de contexto anterior y posterior combinadas. |
| **`-c`**, **`--count`** | 2.1.3 | Muestra únicamente la cantidad de líneas coincidentes. |
| **`--color`** / **`--colour`** | 2.1.3 | Resalta las cadenas coincidentes con secuencias de color ANSI. |
| **`-E`**, **`--extended-regexp`** | 2.4 / 3.6 | Expresiones Regulares Extendidas (ERE). |
| **`-e`**, **`--regexp`** | 2.1.2 | Especifica patrones múltiples o patrones que inician con guion. |
| **`--exclude`** / **`--exclude-dir`** | 2.1.6 | Excluye archivos o carpetas por coincidencia de patrones glob. |
| **`-F`**, **`--fixed-strings`** | 2.4 | Búsqueda de cadenas fijas y literales sin metacaracteres. |
| **`-f`**, **`--file`** | 2.1.2 | Carga patrones de búsqueda desde un archivo de texto. |
| **`-G`**, **`--basic-regexp`** | 2.4 / 3.6 | Expresiones Regulares Básicas (BRE, modo predeterminado). |
| **`-H`** / **`-h`** | 2.1.4 | Fuerza (`-H`) o suprime (`-h`) el prefijo del nombre de archivo. |
| **`--help`** | 2.1.1 | Imprime mensaje de uso resumido y finaliza. |
| **`-I`** | 2.1.6 | Ignora silenciosamente archivos detectados como binarios. |
| **`-i`**, **`--ignore-case`** | 2.1.2 | Búsqueda sin distinción de mayúsculas/minúsculas. |
| **`--include`** | 2.1.6 | Restringe búsqueda recursiva a archivos que coincidan con el patrón. |
| **`-L`** | 2.1.3 | Imprime solo nombres de archivos **sin** coincidencias. |
| **`-l`**, **`--files-with-matches`** | 2.1.3 | Imprime solo nombres de archivos **con** coincidencias. |
| **`--label`** | 2.1.4 | Asigna un nombre simulado al flujo de entrada estándar (`stdin`). |
| **`--line-buffered`** | 2.1.7 | Vacía el búfer de salida tras cada línea procesada. |
| **`-m núm`**, **`--max-count`** | 2.1.3 | Detiene la lectura tras encontrar `núm` coincidencias en un archivo. |
| **`-n`**, **`--line-number`** | 2.1.4 | Prefija la salida con el número de línea original (base 1). |
| **`--no-ignore-case`** | 2.1.2 | Restaura distinción estricta de mayúsculas/minúsculas. |
| **`-o`**, **`--only-matching`** | 2.1.3 | Imprime única y exclusivamente la porción coincidente. |
| **`-P`**, **`--perl-regexp`** | 2.4 | Expresiones regulares compatibles con Perl (PCRE2). |
| **`-q`**, **`--quiet`**, **`--silent`** | 2.1.3 | Modo silencioso (solo devuelve código de salida `0` o `1`). |
| **`-R`** | 2.1.6 | Búsqueda recursiva siguiendo todos los enlaces simbólicos. |
| **`-r`**, **`--recursive`** | 2.1.6 | Búsqueda recursiva en subdirectorios sin seguir enlaces simbólicos. |
| **`-s`**, **`--no-messages`** | 2.1.3 | Suprime mensajes de error por archivos inexistentes o sin permisos. |
| **`-V`**, **`--version`** | 2.1.1 | Imprime la versión del programa y finaliza. |
| **`-v`**, **`--invert-match`** | 2.1.2 | Invierte la búsqueda (selecciona líneas que NO coinciden). |
| **`-w`**, **`--word-regexp`** | 2.1.2 | Coincidencia exacta de palabras completas delimitadas. |
| **`-x`**, **`--line-regexp`** | 2.1.2 | Coincidencia exacta abarcando la totalidad de la línea. |
| **`-Z`** | 2.1.4 | Imprime byte nulo (`\0`) tras el nombre de archivo (para `xargs -0`). |
| **`-z`**, **`--null-data`** | 2.1.7 | Trata entrada/salida delimitada por bytes nulos en vez de `\n`. |
| **`--`** | 2.1.7 | Final explícito de opciones en la línea de comandos. |
| **`GREP_COLORS`** | 2.2 | Variable de entorno para configurar estilos y colores ANSI. |
| **`LC_ALL` / `LANG`** | 2.2 / 5 | Variables de entorno para idioma, locale y aceleración `LC_ALL=C`. |
| **`POSIXLY_CORRECT`** | 2.2 | Fuerza el cumplimiento estricto del estándar POSIX. |

---

## 🧭 Navegación entre Notas

- Nota complementaria: **[[Procesamiento de Texto]]**
- Atajos y comandos frecuentes: **[[Comandos Frecuentes]]**
- Volver al índice general de BASH: **[[Index|Índice de BASH]]**
- Volver al panel principal: **[[Materias/SOII/WSL|Sistemas Operativos II]]**
