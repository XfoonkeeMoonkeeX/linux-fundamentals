
***
### **Solaris**

Solaris es un sistema operativo basado en Unix desarrollado por Sun Microsystems (posteriormente adquirida por Oracle Corporation) en la década de 1990. Es conocido por su robustez, escalabilidad y soporte para sistemas de hardware y software de gama alta. Solaris es ampliamente utilizado en entornos empresariales para aplicaciones de misión crítica, como la gestión de bases de datos, la computación en la nube y la virtualización. Por ejemplo, incluye un hipervisor integrado llamado Oracle VM Server for SPARC, que permite que múltiples máquinas virtuales se ejecuten en un único servidor físico. En general, está diseñado para manejar grandes cantidades de datos y proporcionar servicios fiables y seguros a los usuarios, y a menudo se utiliza en entornos empresariales donde la seguridad, el rendimiento y la estabilidad son requisitos clave.

El objetivo de Solaris es proporcionar una plataforma altamente estable, segura y escalable para la computación empresarial. Tiene características integradas de alta disponibilidad, tolerancia a fallos y gestión del sistema, lo que lo hace ideal para aplicaciones de misión crítica. Es ampliamente utilizado en los sectores bancario, financiero y gubernamental, donde la seguridad, la fiabilidad y el rendimiento son primordiales. También se utiliza en centros de datos a gran escala, entornos de computación en la nube y plataformas de virtualización. Empresas como Amazon, IBM y Dell utilizan Solaris en sus productos y servicios, lo que destaca su importancia en la industria.

#### **Distribuciones de Linux vs Solaris**

Solaris y las distribuciones de Linux son dos tipos de sistemas operativos que difieren significativamente. En primer lugar, Solaris es un sistema operativo propietario, propiedad y desarrollado por Oracle Corporation, y su código fuente no está disponible para el público en general. En contraste, la mayoría de las distribuciones de Linux son de código abierto, lo que significa que su código fuente está disponible para que cualquiera lo modifique y lo use. Además, las distribuciones de Linux utilizan comúnmente el Zettabyte File System (ZFS), que es un sistema de archivos muy avanzado que ofrece características como la compresión de datos, instantáneas y alta escalabilidad. Por otro lado, Solaris utiliza una Service Management Facility (SMF), que es un marco de gestión de servicios muy avanzado que proporciona una mejor fiabilidad y disponibilidad para los servicios del sistema.

| Directorio | Descripción |
| :--- | :--- |
| **/** | El directorio raíz contiene todos los demás directorios y archivos del sistema de archivos. |
| **/bin** | Contiene binarios esenciales del sistema que son necesarios para el arranque y las operaciones básicas del sistema. |
| **/boot** | El directorio de arranque contiene archivos relacionados con el arranque, como el cargador de arranque y las imágenes del kernel. |
| **/dev** | El directorio dev contiene archivos de dispositivo que representan dispositivos físicos y lógicos conectados al sistema. |
| **/etc** | El directorio etc contiene archivos de configuración del sistema, como scripts de inicio del sistema y datos de autenticación de usuarios. |
| **/home** | Directorios de inicio de los usuarios. |
| **/kernel** | Este directorio contiene módulos del kernel y otros archivos relacionados con el kernel. |
| **/lib** | Directorio para las bibliotecas requeridas por los binarios en los directorios /bin y /sbin. |
| **/lost+found** | Este directorio es utilizado por la herramienta de comprobación y reparación de la consistencia del sistema de archivos para almacenar archivos recuperados. |
| **/mnt** | Directorio para montar sistemas de archivos temporalmente. |
| **/opt** | Este directorio contiene paquetes de software opcionales que se instalan en el sistema. |
| **/proc** | El directorio proc proporciona una vista del estado de los procesos y del kernel del sistema como archivos. |
| **/sbin** | Este directorio contiene binarios del sistema necesarios para tareas de administración del sistema. |
| **/tmp** | Los archivos temporales creados por el sistema y las aplicaciones se almacenan en este directorio. |
| **/usr** | El directorio usr contiene datos y programas de solo lectura para todo el sistema, como documentación, bibliotecas y ejecutables. |
| **/var** | Este directorio contiene archivos de datos variables, como registros del sistema, colas de correo y colas de impresión. |

Solaris tiene una serie de características únicas que lo distinguen de otros sistemas operativos. Una de sus fortalezas clave es su soporte para sistemas de hardware y software de gama alta. Está diseñado para funcionar con centros de datos a gran escala e infraestructuras de red complejas, y puede manejar grandes cantidades de datos sin problemas de rendimiento.

En términos de gestión de paquetes, Solaris utiliza el gestor de paquetes Image Packaging System (IPS), que proporciona una forma potente y flexible de gestionar paquetes y actualizaciones. Solaris también proporciona características de seguridad avanzadas, como el Control de Acceso Basado en Roles (RBAC) y los controles de acceso obligatorios, que no están disponibles en todas las distribuciones de Linux.

#### **Diferencias**

Profundicemos en las diferencias entre Solaris y las distribuciones de Linux. Una de las diferencias más importantes es que el código fuente no es de código abierto y solo se conoce en círculos cerrados. Esto significa que, a diferencia de Ubuntu o muchas otras distribuciones, el código fuente no puede ser visto y analizado por el público. En resumen, las principales diferencias se pueden agrupar en las siguientes categorías:

*   Sistema de archivos
*   Gestión de procesos
*   Gestión de paquetes
*   Kernel y soporte de hardware
*   Monitoreo del sistema
*   Seguridad

Para comprender mejor las diferencias, echemos un vistazo a algunos ejemplos y comandos.

**Información del Sistema**

En Ubuntu, usamos el comando `uname` para mostrar información sobre el sistema, como el nombre del kernel, el nombre de host y el sistema operativo. Esto podría verse así:
foonkeemoonkee@htb[/htb]$ uname -a
```
Linux ubuntu 5.4.0-1045 #48-Ubuntu SMP Fri Jan 15 10:47:29 UTC 2021 x86_64 x86_64 x86_64 GNU/Linux
```
Por otro lado, en Solaris, el comando `showrev` se puede usar para mostrar información del sistema, incluida la versión de Solaris, el tipo de hardware y el nivel de parches. Aquí hay un ejemplo de salida:
$ showrev -a
```
Hostname: solaris
Kernel architecture: sun4u
OS version: Solaris 10 8/07 s10s_u4wos_12b SPARC
Application architecture: sparc
Hardware provider: Sun_Microsystems
Domain: sun.com
Kernel version: SunOS 5.10 Generic_139555-08
```
La principal diferencia entre los dos comandos es que `showrev` proporciona información más detallada sobre el sistema Solaris, como el nivel de parches y el proveedor de hardware, mientras que `uname` solo proporciona información básica sobre el sistema Linux.

**Instalación de Paquetes**

En Ubuntu, se usa el comando `apt-get` para instalar paquetes. Esto podría verse de la siguiente manera:
foonkeemoonkee@htb[/htb]$ sudo apt-get install apache2

Sin embargo, en Solaris, necesitamos usar `pkgadd` para instalar paquetes como `SUNWapchr`.
$ pkgadd -d SUNWapchr

La principal diferencia entre los dos comandos es la sintaxis y el gestor de paquetes utilizado. Ubuntu utiliza la Herramienta Avanzada de Paquetes (APT) para gestionar paquetes, mientras que Solaris utiliza el Gestor de Paquetes de Solaris (SPM). También, ten en cuenta que no usamos `sudo` en este caso. Esto se debe a que Solaris usaba la herramienta de gestión de privilegios RBAC, que permitía la asignación de permisos granulares a los usuarios. Sin embargo, `sudo` es compatible desde Solaris 11.

**Gestión de Permisos**

En sistemas Linux como Ubuntu, pero también en Solaris, se usa el comando `chmod` para cambiar los permisos de archivos y directorios. Aquí hay un ejemplo de comando para dar permisos de lectura, escritura y ejecución al propietario del archivo:
foonkeemoonkee@htb[/htb]$ chmod 700 filename

Para encontrar archivos con permisos específicos en Ubuntu, usamos el comando `find`. Echemos un vistazo a un ejemplo de un archivo con el bit SUID establecido:
foonkeemoonkee@htb[/htb]$ find / -perm 4000

Para encontrar archivos con permisos específicos, como con el bit SUID establecido en Solaris, también podemos usar el comando `find`, pero con un pequeño ajuste.
$ find / -perm -4000

La principal diferencia entre estos dos comandos es el uso del `-` antes del valor del permiso en el comando de Solaris. Esto se debe a que Solaris utiliza un sistema de permisos diferente al de Linux.

**NFS en Solaris**

Solaris tiene su propia implementación de NFS, que es ligeramente diferente a la de distribuciones de Linux como Ubuntu. En Solaris, el servidor NFS se puede configurar usando el comando `share`, que se utiliza para compartir un directorio a través de la red, y también nos permite especificar varias opciones como permisos de lectura/escritura, restricciones de acceso y más. Para compartir un directorio a través de NFS en Solaris, podemos usar el siguiente comando:
$ share -F nfs -o rw /export/home

Este comando comparte el directorio `/export/home` con permisos de lectura y escritura a través de NFS. Un cliente NFS puede montar el sistema de archivos NFS usando el comando `mount`, de la misma manera que con Ubuntu. Para montar un sistema de archivos NFS en Solaris, necesitamos especificar el nombre del servidor y la ruta al directorio compartido. Por ejemplo, para montar un recurso compartido NFS desde un servidor con la dirección IP 10.129.15.122 y el directorio compartido `/nfs_share`, usamos el siguiente comando:
foonkeemoonkee@htb[/htb]$ mount -F nfs 10.129.15.122:/nfs_share /mnt/local

En Solaris, la configuración para NFS se almacena en el archivo `/etc/dfs/dfstab`. Este archivo contiene entradas para cada directorio compartido, junto con las diversas opciones para compartir NFS.
$ cat /etc/dfs/dfstab```
share -F nfs -o rw /export/home
```

**Mapeo de Procesos**

El mapeo de procesos es un aspecto esencial de la administración y solución de problemas del sistema. El comando `lsof` es una utilidad potente que lista todos los archivos abiertos por un proceso, incluidos los sockets de red y otros descriptores de archivo que podemos usar en distribuciones de Debian como Ubuntu. Podemos usar `lsof` para listar todos los archivos abiertos por un proceso. Por ejemplo, para listar todos los archivos abiertos por el proceso del servidor web Apache, podemos usar el siguiente comando:
foonkeemoonkee@htb[/htb]$ sudo lsof -c apache2

En Solaris, el comando `pfiles` se puede usar para listar todos los archivos abiertos por un proceso. Por ejemplo, para listar todos los archivos abiertos por el proceso del servidor web Apache, podemos usar el siguiente comando:
$ pfiles `pgrep httpd`

Este comando lista todos los archivos abiertos por el proceso del servidor web Apache. La salida del comando `pfiles` es similar a la salida del comando `lsof` y proporciona información sobre el tipo de descriptor de archivo, el número del descriptor de archivo y el nombre del archivo.

**Acceso a Ejecutables**

En Solaris, se usa `truss`, que es una utilidad muy útil para desarrolladores y administradores de sistemas que necesitan depurar problemas complejos de software en el sistema operativo Solaris. Al rastrear las llamadas al sistema realizadas por un proceso, `truss` puede ayudar a identificar la fuente de errores, problemas de rendimiento y otros problemas, pero también puede revelar información sensible que puede surgir durante el desarrollo de aplicaciones o el mantenimiento del sistema. La utilidad también puede proporcionar información detallada sobre las llamadas al sistema, incluidos los argumentos que se les pasan y sus valores de retorno, lo que permite a los usuarios comprender mejor el comportamiento de sus aplicaciones y del sistema operativo subyacente.

`Strace` es una alternativa a `truss` pero para Ubuntu, y es una herramienta esencial tanto para administradores de sistemas como para desarrolladores, ayudándoles a diagnosticar y solucionar problemas en tiempo real. Permite a los usuarios analizar las interacciones entre el sistema operativo y las aplicaciones que se ejecutan en él, lo que es especialmente útil en entornos muy complejos y de misión crítica. Con `truss` (el texto original parece tener un error aquí, debería ser `strace` para Ubuntu), los usuarios pueden identificar y aislar rápidamente problemas relacionados con el rendimiento de la aplicación, la conectividad de red y la utilización de recursos del sistema, entre otros.

Por ejemplo, para rastrear las llamadas al sistema realizadas por el proceso del servidor web Apache, podemos usar el siguiente comando:
foonkeemoonkee@htb[/htb]$ sudo strace -p `pgrep apache2`

Aquí hay un ejemplo de cómo usar `truss` para rastrear las llamadas al sistema realizadas por el comando `ls` en Solaris:
$ truss ls
```
execve("/usr/bin/ls", 0xFFBFFDC4, 0xFFBFFDC8)  argc = 1
...SNIP...
```
La salida es similar a `strace`, pero el formato es ligeramente diferente. Una diferencia entre `strace` y `truss` es que `truss` también puede rastrear las señales enviadas a un proceso, mientras que `strace` no puede. Otra diferencia es que `truss` tiene la capacidad de rastrear las llamadas al sistema realizadas por procesos hijos, mientras que `strace` solo puede rastrear las llamadas al sistema realizadas por el proceso especificado en la línea de comandos.