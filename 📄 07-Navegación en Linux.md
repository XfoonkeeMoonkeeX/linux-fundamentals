
## Introducción

La **navegación** en Linux es tan esencial como usar el ratón en un sistema operativo gráfico como Windows. A través de la línea de comandos, nos movemos por el sistema de archivos, trabajando con directorios y archivos según sea necesario. Para ello, utilizamos diversas herramientas y comandos que nos permiten listar información sobre los directorios y archivos, y emplear opciones avanzadas para personalizar la salida según nuestras necesidades.

Una de las mejores maneras de aprender algo nuevo es **experimentar** con ello. En esta sección, cubriremos cómo navegar por Linux, crear, mover, editar y eliminar archivos y directorios, cómo encontrarlos en el sistema operativo, los diferentes tipos de redirecciones y qué son los descriptores de archivo. También aprenderemos algunos **atajos** que hacen el trabajo con la terminal más fácil y eficiente.

Recomendamos practicar estos conceptos en una **máquina virtual local** que tengamos configurada. Además, es importante crear un **snapshot** de la máquina virtual para restaurarla en caso de algún problema inesperado.

---

## Comenzando con la navegación

### Ver el directorio actual

Para saber en qué directorio nos encontramos, utilizamos el comando `pwd` (Print Working Directory):

```bash
cry0l1t3@htb[~]$ pwd
/home/cry0l1t3

El comando devuelve el directorio completo donde estamos ubicados. En este caso, estamos en el directorio /home/cry0l1t3.
Listar el contenido de un directorio

Para ver los contenidos de un directorio, usamos el comando ls. Este muestra los archivos y carpetas del directorio actual:

cry0l1t3@htb[~]$ ls
Desktop  Documents  Downloads  Music  Pictures  Public  Templates  Videos

Si deseamos más detalles, como el tamaño, permisos y fecha de modificación, podemos añadir la opción -l:

cry0l1t3@htb[~]$ ls -l
total 32
drwxr-xr-x 2 cry0l1t3 htbacademy 4096 Nov 13 17:37 Desktop
drwxr-xr-x 2 cry0l1t3 htbacademy 4096 Nov 13 17:34 Documents
...

Esto nos da una vista más detallada de los archivos y directorios. Aquí podemos ver la siguiente información:
Columna	Descripción
drwxr-xr-x	Tipo de archivo y permisos
2	Número de enlaces duros
cry0l1t3	Propietario del archivo o directorio
htbacademy	Grupo propietario
4096	Tamaño del archivo o número de bloques usados
Nov 13 17:37	Fecha y hora de la última modificación
Desktop	Nombre del archivo o directorio
Archivos ocultos

Los archivos y directorios que comienzan con un punto (.) son ocultos por defecto. Para verlos, usamos el comando ls -la:

cry0l1t3@htb[~]$ ls -la
total 403188
drwxr-xr-x 2 cry0l1t3 htbacademy 4096 Nov 13 17:37 .bash_history
drwxr-xr-x 2 cry0l1t3 htbacademy 4096 Nov 13 17:37 .bashrc
...

Especificar rutas para listar contenido

No es necesario estar en un directorio específico para ver su contenido. Podemos listar los archivos de otro directorio especificando su ruta:

cry0l1t3@htb[~]$ ls -l /var/
total 52
drwxr-xr-x  2 root root     4096 Mai 15 18:54 backups
drwxr-xr-x 18 root root     4096 Nov 15 16:55 cache
...

Navegar entre directorios

Para movernos entre directorios, usamos el comando cd. Por ejemplo, para cambiar al directorio /dev/shm, simplemente escribimos:

cry0l1t3@htb[~]$ cd /dev/shm
cry0l1t3@htb[/dev/shm]$

Para volver al directorio anterior, podemos usar cd -:

cry0l1t3@htb[/dev/shm]$ cd -
cry0l1t3@htb[~]$

Autocompletar

La función de autocompletado facilita la navegación. Si escribimos cd /dev/s y presionamos [TAB] dos veces, el shell nos mostrará las opciones disponibles que comienzan con "s":

cry0l1t3@htb[~]$ cd /dev/s [TAB 2x]
shm/ snd/

Esto nos ayuda a completar rutas rápidamente sin tener que escribir todo el nombre.
Acceso a directorios superiores

El directorio .. representa el directorio padre. Para movernos al directorio superior, usamos cd ..:

cry0l1t3@htb[/dev/shm]$ cd ..
cry0l1t3@htb[/dev]$

Limpiar la terminal

Para limpiar la terminal, podemos usar el comando clear:

cry0l1t3@htb[/dev]$ clear

Otra forma de hacerlo es utilizando el atajo [Ctrl] + [L].
Historial de comandos

Podemos desplazarnos por el historial de comandos con las teclas ↑ (arriba) y ↓ (abajo). Además, podemos buscar en el historial con [Ctrl] + [R] y escribir una parte del comando que queremos encontrar.

Sugerencia adicional: Practicar con estos comandos en una máquina virtual es una excelente forma de aprender y familiarizarse con la terminal. A medida que avances, te será útil conocer otras combinaciones y atajos que agilicen tu trabajo.

### Preguntas

Responde las siguientes preguntas para completar esta sección y ganar cubos.

**Objetivo(s):** 10.129.193.255 (ACADEMY-NIXFUND)

**Tiempo restante:** 14 minutos

1. **¿Cuál es el nombre del archivo oculto de "history" en el directorio home del usuario htb-user?**

.bash_history

2. **¿Cuál es el número de índice del archivo "sudoers" en el directorio "/etc"?**

147627