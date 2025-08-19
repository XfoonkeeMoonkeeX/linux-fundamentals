Trabajando con Archivos y Directorios

La principal diferencia entre trabajar con archivos en Linux, en comparación con Windows, radica en cómo accedemos y gestionamos esos archivos. En Windows, generalmente usamos herramientas gráficas como el Explorador para encontrar, abrir y editar archivos. Sin embargo, en Linux, la terminal ofrece una alternativa poderosa donde los archivos pueden ser accedidos y editados directamente mediante comandos. Este método no solo es más rápido, sino también más eficiente, ya que permite editar archivos de manera interactiva sin necesidad de editores como `vim` o `nano`.

La eficiencia de la terminal proviene de su capacidad para acceder a archivos con solo unos pocos comandos y permitir modificar archivos de manera selectiva utilizando expresiones regulares (regex). Además, puedes ejecutar varios comandos a la vez, redirigiendo la salida a archivos y automatizando tareas de edición por lotes, lo que ahorra mucho tiempo al trabajar con numerosos archivos simultáneamente. Este enfoque basado en la línea de comandos agiliza el flujo de trabajo, convirtiéndolo en una herramienta invaluable para tareas que serían más lentas a través de una interfaz gráfica.

A continuación, exploraremos cómo trabajar con archivos y directorios para gestionar eficazmente el contenido en nuestro sistema operativo.

## Crear, Mover y Copiar

Comencemos aprendiendo a realizar operaciones clave como crear, renombrar, mover, copiar y eliminar archivos. Antes de ejecutar los siguientes comandos, primero debemos conectarnos por SSH al objetivo (usando las instrucciones de conexión al final de la sección). Ahora, digamos que queremos crear un nuevo archivo o directorio. La sintaxis para esto es la siguiente:

### Sintaxis - touch

```bash
foonkeemoonkee@htb[/htb]$ touch <nombre>

Sintaxis - mkdir

foonkeemoonkee@htb[/htb]$ mkdir <nombre>

En el siguiente ejemplo, vamos a crear un archivo llamado info.txt y un directorio llamado Storage. Para crear estos, seguimos los comandos y su sintaxis como se muestra arriba.
Crear un Archivo Vacío

foonkeemoonkee@htb[/htb]$ touch info.txt

Crear un Directorio

foonkeemoonkee@htb[/htb]$ mkdir Storage

Cuando estés organizando tu sistema, puede que necesites crear múltiples directorios dentro de otros directorios. Ejecutar manualmente el comando mkdir para cada uno sería una pérdida de tiempo. Afortunadamente, el comando mkdir tiene la opción -p (padres), que permite crear directorios padres automáticamente.
Sintaxis con Opción -p

foonkeemoonkee@htb[/htb]$ mkdir -p Storage/local/user/documents

Podemos ver toda la estructura después de crear los directorios padres con la herramienta tree.
Ver la Estructura de Directorios

foonkeemoonkee@htb[/htb]$ tree .
.
├── info.txt
└── Storage
    └── local
        └── user
            └── documents

4 directorios, 1 archivo

Puedes crear archivos directamente dentro de directorios específicos indicando la ruta donde debe almacenarse el archivo, y puedes usar el punto (.) para indicar que deseas comenzar desde el directorio actual. Esta es una forma conveniente de trabajar dentro de tu ubicación actual sin necesidad de escribir la ruta completa. Así que, el comando para crear otro archivo vacío se vería así:
Crear userinfo.txt

foonkeemoonkee@htb[/htb]$ touch ./Storage/local/user/userinfo.txt

Estructura Actualizada

foonkeemoonkee@htb[/htb]$ tree .
.
├── info.txt
└── Storage
    └── local
        └── user
            ├── documents
            └── userinfo.txt

4 directorios, 2 archivos

Con el comando mv, podemos mover y también renombrar archivos y directorios. La sintaxis para esto es la siguiente:
Sintaxis - mv

foonkeemoonkee@htb[/htb]$ mv <archivo/directorio> <archivo/directorio renombrado>

Primero, vamos a renombrar el archivo info.txt a information.txt y luego moverlo al directorio Storage.
Renombrar Archivo

foonkeemoonkee@htb[/htb]$ mv info.txt information.txt

Ahora vamos a crear un archivo llamado readme.txt en el directorio actual y luego copiar los archivos information.txt y readme.txt al directorio Storage/.
Crear readme.txt

foonkeemoonkee@htb[/htb]$ touch readme.txt

Mover Archivos a un Directorio Específico

foonkeemoonkee@htb[/htb]$ mv information.txt readme.txt Storage/

Ver la Estructura de Directorios

foonkeemoonkee@htb[/htb]$ tree .
.
└── Storage
    ├── information.txt
    ├── local
    │   └── user
    │       ├── documents
    │       └── userinfo.txt
    └── readme.txt

4 directorios, 3 archivos

Supongamos que queremos tener readme.txt en el directorio local/. Entonces, podemos copiarlo allí con las rutas especificadas.
Copiar readme.txt

foonkeemoonkee@htb[/htb]$ cp Storage/readme.txt Storage/local/

Ahora, podemos verificar si el archivo está allí usando nuevamente la herramienta tree.
Verificar la Estructura Actualizada

foonkeemoonkee@htb[/htb]$ tree .
.
└── Storage
    ├── information.txt
    ├── local
    │   ├── readme.txt
    │   └── user
    │       ├── documents
    │       └── userinfo.txt
    └── readme.txt

4 directorios, 4 archivos

Además de los comandos básicos para la gestión de archivos, existen muchas otras formas poderosas de trabajar con archivos en Linux, como el uso de redirección y editores de texto. La redirección te permite manipular el flujo de entrada y salida entre comandos y archivos, haciendo tareas como crear o modificar archivos más rápidas y eficientes. También puedes usar editores de texto populares como vim y nano para ediciones más interactivas.

Exploraremos y discutiremos estos métodos en detalle en secciones posteriores. A medida que te familiarices con estas técnicas, tendrás más flexibilidad en cómo crear, editar y gestionar archivos en tu sistema.

## Preguntas

Responde las siguientes preguntas para completar esta sección y ganar cubos:

**Objetivo(s):** ¡Haz clic aquí para generar el sistema de destino!

Conéctate por SSH usando el usuario `"htb-student"` y la contraseña `"HTB_@cademy_stdnt!"`

1. ¿Cuál es el nombre del archivo **modificado más recientemente** en el directorio `/var/backups`?

apt.extended_states.0

1. ¿Cuál es el **número de inodo** del archivo `shadow.bak` en el directorio `/var/backups`?

265293
