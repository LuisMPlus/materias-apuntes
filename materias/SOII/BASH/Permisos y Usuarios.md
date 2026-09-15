# Permisos y Gestión de Usuarios

En los sistemas operativos GNU/Linux y UNIX, la seguridad y el control de acceso se basan en un modelo estricto de **usuarios, grupos y permisos sobre archivos y directorios**.

---

## 1. Estructura de Permisos en Linux

Al ejecutar `ls -l`, la primera columna muestra una cadena de 10 caracteres que define el tipo de archivo y sus permisos:

### Significado de los Permisos:
Primer carácter

| Símbolo | Tipo de Elemento                             | Descripción                                                                               |
| ------- | -------------------------------------------- | ----------------------------------------------------------------------------------------- |
| `d`     | Directorio (Directory)                       | Una carpeta que contiene otros archivos o subdirectorios.                                 |
| `-`     | Archivo regular                              | Cualquier archivo estándar (un .txt, .py, imagen, ejecutable, etc).                       |
| `l`     | Enlace simbólico (link)                      | Un acceso directo que apunta a otro archivo o carpeta                                     |
| `c`     | Dispositivo de caracteres (Character device) | Dispositivos que envían datos carácter por carácter (ej. /dev/null, terminales /dev/tty). |
| `b`     | Dispositivo de bloques (Block decie)         | Dispositivos de almacenamiento físico (ej. discos duros como /dev/sda).                   |
| `s`     | Socket                                       | Canal de comunicación de red/procesos local                                               |
| `p`     | Tubería con nombre (Named Pipe / FIFO)       | Flujo de datos para comunicar dos procesos                                                |
Los demás caracteres divididos en: usuario, grupo y otros

|   Permiso   | Símbolo | Valor Numérico (Octal) | Efecto en Archivos                               | Efecto en Directorios                             |
|:-----------:|:-------:|:----------------------:|:------------------------------------------------ |:------------------------------------------------- |
|  **Read**   |   `r`   |         **4**          | Permite leer/abrir el contenido del archivo      | Permite listar el contenido (`ls`)                |
|  **Write**  |   `w`   |         **2**          | Permite modificar o sobreescribir el archivo     | Permite crear, borrar o renombrar archivos dentro |
| **Execute** |   `x`   |         **1**          | Permite ejecutar el archivo como programa/script | Permite acceder/entrar a la carpeta (`cd`)        |
| **Ninguno** |   `-`   |         **0**          | Sin ningún permiso                               | Sin ningún permiso                                |
![](../../../Adjuntos/Pasted%20image%2020260827100924.png)
![](../../../Adjuntos/Pasted%20image%2020260827094622.png)

En este ejemplo el tp0 de SOII
![](../../../Adjuntos/Pasted%20image%2020260827094206.png)
Para el archivo `test.sh` separando los caracteres
tipo de archivo[ - ] usuario[ rw-] grupo[ r-- ] otro[ r-- ]

---

## 2. Modificar Permisos: `chmod` (Change Mode)

Existen dos formas de modificar permisos: mediante **notación octal (numérica)** o **notación simbólica**.

### A. Notación Octal (Suma de valores por grupo)
Cada dígito representa la suma de los permisos (`r=4`, `w=2`, `x=1`):

```bash
# 755: Propietario (4+2+1=7), Grupo (4+0+1=5), Otros (4+0+1=5)
# Muy común para ejecutables y scripts públicos
chmod 755 script.sh

# 644: Propietario (4+2=6), Grupo (4), Otros (4)
# Permiso estándar por defecto para archivos de texto y documentos
chmod 644 documento.txt

# 700: Propietario (7), Grupo (0), Otros (0)
# Privado: solo el dueño puede leer, escribir y ejecutar
chmod 700 clave_privada.pem

# Modificar permisos recursivamente a toda una carpeta (-R)
chmod -R 755 /var/www/html
```

### B. Notación Simbólica
Utiliza letras para referirse a usuarios (`u` = user, `g` = group, `o` = others, `a` = all) y operadores (`+` agregar, `-` quitar, `=` asignar):

```bash
# Dar permisos de ejecución al propietario (u = user)
chmod u+x script.sh

# Dar permisos de ejecución a TODOS (usuario, grupo y otros)
chmod +x script.sh
# (o explícito)
chmod a+x script.sh

# Quitar permisos de escritura al grupo y a otros
chmod go-w archivo.txt

# Asignar exactamente lectura y escritura al propietario, y solo lectura al grupo
chmod u=rw,g=r archivo.txt
```

Para el ejemplo:
ahora `test.sh` tiene permiso de ejecución para usuario
![](../../../Adjuntos/Pasted%20image%2020260827101821.png)

---

## 3. Gestión de Propietario y Grupo: `chown` y `chgrp`

### `chown` (Change Owner)
Cambia el usuario propietario y/o el grupo de un archivo o directorio (requiere `sudo`).

```bash
# Cambiar solo el usuario propietario
sudo chown juan reporte.txt

# Cambiar usuario y grupo simultáneamente (formato: usuario:grupo)
sudo chown juan:desarrolladores app.py

# Cambiar de forma recursiva a todos los archivos dentro de un directorio (-R)
sudo chown -R www-data:www-data /var/www/html
```

### `chgrp` (Change Group)
Cambia únicamente el grupo asociado a un archivo:
```bash
sudo chgrp estudiantes notas.txt
```

---

## 4. Máscara de Permisos por Defecto: `umask`

El comando `umask` determina los permisos que tendrán los archivos y carpetas nuevos al ser creados. Funciona como una **máscara de sustracción**:

- Permisos base máximos para carpetas: `777` (`rwxrwxrwx`)
- Permisos base máximos para archivos: `666` (`rw-rw-rw-`)

```bash
# Consultar umask actual
umask
# Salida típica: 0022

# Con umask 022:
# Carpetas nuevas: 777 - 022 = 755 (rwxr-xr-x)
# Archivos nuevos: 666 - 022 = 644 (rw-r--r--)
```

---

## 5. Usuarios, Grupos e Identidad

### Comandos de Identidad:
```bash
# Ver el usuario actual activo
whoami

# Ver ID de usuario (UID), ID de grupo principal (GID) y grupos a los que pertenece
id

# Ver a qué grupos pertenece un usuario específico
groups usuario
```

### Elevación de Privilegios (`sudo` y `su`):
```bash
# Ejecutar un único comando con privilegios de superusuario (root)
sudo apt update

# Iniciar una sesión interactiva como superusuario root
sudo -i
# o cambiar de usuario completamente:
su - root

# Cambiar la contraseña del usuario actual
passwd

# Cambiar la contraseña de otro usuario (como root)
sudo passwd juan
```

---

## 🧭 Navegación entre Notas

- Nota anterior: **[[Procesamiento de Texto]]**
- Siguiente nota: **[[Procesos y Monitoreo]]**
- Volver al índice general de BASH: **[[Index|Índice de BASH]]**
