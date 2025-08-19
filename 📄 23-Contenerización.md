
***
**Contenerización**

La contenerización es el proceso de empaquetar y ejecutar aplicaciones en entornos aislados, comúnmente conocidos como contenedores. Estos contenedores proporcionan entornos ligeros y consistentes para que las aplicaciones se ejecuten, asegurando que se comporten de la misma manera, independientemente de dónde se desplieguen. Tecnologías como Docker, Docker Compose y Linux Containers (LXC) hacen posible la contenerización, principalmente en sistemas basados en Linux. Los contenedores se diferencian de las máquinas virtuales en que comparten el kernel del sistema anfitrión, lo que los hace mucho más ligeros y eficientes. Con estas tecnologías, los usuarios pueden crear, desplegar y gestionar aplicaciones rápidamente con una seguridad, portabilidad y escalabilidad mejoradas.

Los contenedores son altamente configurables, permitiendo a los usuarios adaptarlos a sus necesidades específicas, y su naturaleza ligera facilita la ejecución de múltiples contenedores simultáneamente en el mismo sistema anfitrión. Esta característica es particularmente ventajosa para escalar aplicaciones y gestionar arquitecturas de microservicios complejas.

Imagina que estás organizando un gran concierto y cada banda necesita su propia configuración de escenario personalizada. En lugar de construir un escenario completamente nuevo para cada banda (lo que sería como usar una máquina virtual), creas "cápsulas de escenario" portátiles y autónomas que incluyen todo lo que la banda necesita: luces, instrumentos, altavoces, etc. Estas cápsulas son ligeras, reutilizables y se pueden mover fácilmente de un lugar a otro. La clave es que todas las cápsulas funcionan sin problemas en el mismo escenario principal (el sistema anfitrión), pero cada una está lo suficientemente aislada como para que la configuración de una banda no interfiera con las demás.

De la misma manera, los contenedores empaquetan una aplicación con todas sus herramientas y configuraciones necesarias, permitiéndole ejecutarse de manera consistente en diferentes sistemas sin conflictos, todo mientras comparten el mismo "escenario principal" (los recursos del sistema subyacente).

La seguridad es un aspecto crítico de la contenerización. Los contenedores aíslan las aplicaciones del sistema anfitrión y entre sí, proporcionando una barrera que reduce el riesgo de que actividades maliciosas afecten al anfitrión u otros contenedores. Este aislamiento, junto con una configuración y técnicas de fortalecimiento (*hardening*) adecuadas, añade una capa adicional de seguridad. Sin embargo, es importante señalar que los contenedores no ofrecen el mismo nivel de aislamiento que las máquinas virtuales tradicionales. Si no se gestionan adecuadamente, se pueden explotar vulnerabilidades como la escalada de privilegios o los escapes de contenedores para obtener acceso no autorizado al sistema anfitrión u otros contenedores.

Además de la seguridad mejorada y la eficiencia de recursos, los contenedores facilitan el despliegue, la gestión y la escalabilidad de las aplicaciones. Dado que los contenedores encapsulan todo lo que la aplicación necesita (por ejemplo, bibliotecas, dependencias), permiten la consistencia en los entornos de desarrollo, pruebas y producción. Esta portabilidad asegura que las aplicaciones se ejecuten de manera fiable en diferentes entornos.

Sin embargo, es importante reconocer que, a pesar de sus ventajas, los contenedores no son inmunes a los riesgos de seguridad. Existen métodos que podemos usar para escalar privilegios o escapar del aislamiento que proporcionan los contenedores.

### **Docker**

Docker es una plataforma de código abierto para automatizar el despliegue de aplicaciones como unidades autónomas llamadas contenedores. Utiliza un sistema de archivos por capas y características de aislamiento de recursos para proporcionar flexibilidad y portabilidad. Además, proporciona un robusto conjunto de herramientas para crear, desplegar y gestionar aplicaciones, lo que ayuda a agilizar el proceso de contenerización.

Imagina los contenedores de Docker como una fiambrera sellada. Puedes comerte la comida (ejecutar aplicaciones) que hay dentro, pero una vez que cierras la fiambrera (detienes el contenedor), todo se reinicia. Para hacer una nueva fiambrera (nuevo contenedor) con contenido actualizado (configuraciones modificadas), creas una nueva receta (Dockerfile) basada en la original. Al servir múltiples fiambreras en un restaurante (producción), usarías un sistema de cocina (Kubernetes/Docker Compose) para gestionar todos los pedidos sin problemas.

#### **Instalar Docker-Engine**

Instalar Docker es relativamente sencillo. Podemos usar el siguiente script para instalarlo en un anfitrión Ubuntu:
```bash
#!/bin/bash

# Preparación
sudo apt update -y
sudo apt install ca-certificates curl gnupg lsb-release -y
sudo mkdir -m 0755 -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# Instalar Docker Engine
sudo apt update -y
sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin -y

# Añadir el usuario htb-student al grupo de Docker
sudo usermod -aG docker htb-student
echo '[!] Necesitas cerrar sesión y volver a iniciarla para que los cambios en el grupo surtan efecto.'

# Probar la instalación de Docker
docker run hello-world
```

Se necesitan el motor de Docker e imágenes de Docker específicas para ejecutar un contenedor. Estas se pueden obtener de Docker Hub, un repositorio de imágenes pre-hechas, o ser creadas por el usuario. Docker Hub es un registro basado en la nube para repositorios de software o una biblioteca de imágenes de Docker. Se divide en un área pública y una privada. El área pública permite a los usuarios subir y compartir imágenes con la comunidad. También contiene imágenes oficiales del equipo de desarrollo de Docker y de proyectos de código abierto establecidos. Las imágenes subidas a un área privada del registro no son accesibles públicamente. Se pueden compartir dentro de una empresa o con equipos y conocidos.

La creación de una imagen de Docker se realiza creando un `Dockerfile`, que contiene todas las instrucciones que el motor de Docker necesita para crear el contenedor. Podemos usar contenedores de Docker como nuestro servidor de "alojamiento de archivos" al transferir archivos específicos a nuestros sistemas objetivo. Por lo tanto, debemos crear un Dockerfile basado en Ubuntu 22.04 con un servidor Apache y SSH en ejecución. Con esto, podemos usar `scp` para transferir archivos a la imagen de Docker, y Apache nos permite alojar archivos y usar herramientas como `curl`, `wget` y otras en el sistema objetivo para descargar los archivos necesarios. Dicho Dockerfile podría tener el siguiente aspecto:

**Dockerfile**
```bash
# Usar la última versión de Ubuntu 22.04 LTS como imagen base
FROM ubuntu:22.04

# Actualizar el repositorio de paquetes e instalar los paquetes necesarios
RUN apt-get update && \
    apt-get install -y \
        apache2 \
        openssh-server \
        && \
    rm -rf /var/lib/apt/lists/*

# Crear un nuevo usuario llamado "docker-user"
RUN useradd -m docker-user && \
    echo "docker-user:password" | chpasswd

# Dar al usuario docker-user acceso completo a los servicios de Apache y SSH
RUN chown -R docker-user:docker-user /var/www/html && \
    chown -R docker-user:docker-user /var/run/apache2 && \
    chown -R docker-user:docker-user /var/log/apache2 && \
    chown -R docker-user:docker-user /var/lock/apache2 && \
    usermod -aG sudo docker-user && \
    echo "docker-user ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers

# Exponer los puertos necesarios
EXPOSE 22 80

# Iniciar los servicios de SSH y Apache
CMD service ssh start && /usr/sbin/apache2ctl -D FOREGROUND
```

Una vez que hemos definido nuestro Dockerfile, necesitamos convertirlo en una imagen. Con el comando `build`, tomamos el directorio con el Dockerfile, ejecutamos los pasos del Dockerfile y almacenamos la imagen en nuestro motor de Docker local. Si uno de los pasos falla debido a un error, la creación del contenedor se abortará. Con la opción `-t`, le damos a nuestro contenedor una etiqueta, para que sea más fácil de identificar y trabajar con él más tarde.

**Docker Build**
foonkeemoonkee@htb[/htb]$ docker build -t FS_docker .

Una vez que se ha creado la imagen de Docker, se puede ejecutar a través del motor de Docker, lo que la convierte en una forma muy eficiente y fácil de ejecutar un contenedor. Es similar al concepto de máquina virtual, basado en imágenes. Aún así, estas imágenes son plantillas de solo lectura y proporcionan el sistema de archivos necesario para el tiempo de ejecución y todos los parámetros. Un contenedor puede considerarse un proceso en ejecución de una imagen. Cuando se va a iniciar un contenedor en un sistema, primero se carga un paquete con la imagen respectiva si no está disponible localmente. Podemos iniciar el contenedor con el siguiente comando `docker run`:

**Docker Run - Sintaxis**
foonkeemoonkee@htb[/htb]$ docker run -p <puerto_del_anfitrión>:<puerto_de_docker> -d <nombre_del_contenedor_docker>

**Docker Run**
foonkeemoonkee@htb[/htb]$ docker run -p 8022:22 -p 8080:80 -d FS_docker

En este caso, iniciamos un nuevo contenedor a partir de la imagen `FS_docker` y mapeamos los puertos del anfitrión 8022 y 8080 a los puertos del contenedor 22 y 80, respectivamente. El contenedor se ejecuta en segundo plano, lo que nos permite acceder a los servicios SSH y HTTP dentro del contenedor utilizando los puertos del anfitrión especificados.

#### **Gestión de Docker**

Al gestionar contenedores de Docker, Docker proporciona un conjunto completo de herramientas que nos permiten crear, desplegar y gestionar contenedores fácilmente. Con estas potentes herramientas, podemos listar, iniciar y detener contenedores y gestionarlos eficazmente, asegurando una ejecución sin problemas de las aplicaciones. Algunos de los comandos de gestión de Docker más utilizados son:

| Comando | Descripción |
| :--- | :--- |
| `docker ps` | Lista todos los contenedores en ejecución |
| `docker stop` | Detiene un contenedor en ejecución. |
| `docker start` | Inicia un contenedor detenido. |
| `docker restart` | Reinicia un contenedor en ejecución. |
| `docker rm` | Elimina un contenedor. |
| `docker rmi` | Elimina una imagen de Docker. |
| `docker logs` | Muestra los registros de un contenedor. |

Es importante tener en cuenta que los comandos de Docker se pueden combinar con varias opciones para agregar funcionalidad adicional. Por ejemplo, puedes especificar qué puertos exponer, montar volúmenes para retener datos o establecer variables de entorno para configurar tus contenedores. Esta flexibilidad te permite personalizar tus contenedores de Docker para satisfacer necesidades y requisitos específicos.

Al trabajar con imágenes de Docker, es crucial entender que cualquier cambio realizado en un contenedor en ejecución basado en una imagen no se guarda automáticamente en la imagen. Para preservar estos cambios, necesitas crear una nueva imagen que los incluya. Esto se hace escribiendo un nuevo `Dockerfile`, que comienza con la instrucción `FROM` (especificando la imagen base) y luego incluye los comandos necesarios para aplicar los cambios. Una vez que el Dockerfile está listo, puedes usar el comando `docker build` para construir la nueva imagen y asignarle una etiqueta única para identificarla. Este proceso asegura que la imagen original permanezca sin cambios, mientras que la nueva imagen refleja las actualizaciones.

También es importante recordar que los contenedores de Docker son, por diseño, sin estado (*stateless*), lo que significa que cualquier cambio realizado dentro de un contenedor en ejecución (por ejemplo, modificar archivos) se pierde una vez que el contenedor se detiene o se elimina. Por esta razón, es una buena práctica usar volúmenes para persistir datos fuera del contenedor o para almacenar el estado de la aplicación.

En entornos de producción, la gestión de contenedores a escala se vuelve más compleja. Herramientas como Docker Compose o Kubernetes ayudan a orquestar contenedores, permitiéndote gestionar, escalar y vincular múltiples contenedores de manera eficiente.

### **Linux Containers (LXC)**

Linux Containers (LXC) es una tecnología de virtualización ligera que permite que múltiples sistemas Linux aislados (llamados contenedores) se ejecuten en un único anfitrión. LXC utiliza características clave de aislamiento de recursos, como los grupos de control (*cgroups*) y los espacios de nombres (*namespaces*), para asegurar que cada contenedor funcione de manera independiente. A diferencia de las máquinas virtuales tradicionales, que requieren un sistema operativo completo para cada instancia, los contenedores comparten el kernel del anfitrión, lo que hace a LXC más eficiente en términos de uso de recursos.

LXC proporciona un conjunto completo de herramientas y APIs para gestionar y configurar contenedores, lo que lo convierte en una opción popular para la contenerización en sistemas Linux. Sin embargo, aunque LXC y Docker son ambas tecnologías de contenerización, sirven para diferentes propósitos y tienen características únicas.

Docker se basa en la idea de la contenerización añadiendo facilidad de uso y portabilidad, lo que lo ha hecho muy popular en el mundo de DevOps. Docker se enfoca en empaquetar aplicaciones con todas sus dependencias en una "imagen" portátil, permitiendo que se desplieguen fácilmente en diferentes entornos. Sin embargo, existen algunas diferencias entre los dos que se pueden distinguir según las siguientes categorías:

| Categoría | Descripción |
| :--- | :--- |
| **Enfoque** | A menudo se considera a LXC como una herramienta de contenerización más tradicional a nivel de sistema, centrada en crear entornos Linux aislados que se comportan como máquinas virtuales ligeras. Docker, por otro lado, está centrado en la aplicación, lo que significa que está optimizado para empaquetar y desplegar aplicaciones únicas o microservicios. |
| **Construcción de imágenes** | Docker utiliza un formato de imagen estandarizado (imágenes de Docker) que incluye todo lo necesario para ejecutar una aplicación (código, bibliotecas, configuraciones). LXC, aunque es capaz de una funcionalidad similar, generalmente requiere una configuración más manual para construir y gestionar entornos. |
| **Portabilidad** | Docker sobresale en portabilidad. Sus imágenes de contenedor se pueden compartir fácilmente entre diferentes sistemas a través de Docker Hub u otros registros. Los entornos LXC son menos portátiles en este sentido, ya que están más estrechamente integrados con la configuración del sistema anfitrión. |
| **Facilidad de uso** | Docker está diseñado pensando en la simplicidad, ofreciendo una CLI fácil de usar y un amplio soporte de la comunidad. LXC, aunque potente, puede requerir un conocimiento más profundo de la administración de sistemas Linux, lo que lo hace menos sencillo para los principiantes. |
| **Seguridad** | Los contenedores de Docker son generalmente más seguros de fábrica, gracias a capas de aislamiento adicionales como AppArmor y SELinux, junto con su función de sistema de archivos de solo lectura. Los contenedores LXC, aunque seguros, pueden necesitar configuraciones adicionales para igualar el nivel de aislamiento que Docker ofrece por defecto. Curiosamente, si se configuran incorrectamente, tanto Docker como LXC pueden presentar un vector para la escalada de privilegios local (estas técnicas se cubren en profundidad en nuestro módulo de Escalada de Privilegios Local en Linux. |

En LXC, las imágenes se construyen manualmente creando un sistema de archivos raíz e instalando los paquetes y configuraciones necesarios. Esos contenedores están ligados al sistema anfitrión, pueden no ser fácilmente portátiles y pueden requerir más experiencia técnica para configurar y gestionar.

Por otro lado, Docker es una plataforma centrada en la aplicación que se basa en LXC y proporciona una interfaz más amigable para la contenerización. Sus imágenes se construyen usando un `Dockerfile`, que especifica la imagen base y los pasos necesarios para construir la imagen. Esas imágenes están diseñadas para ser portátiles, de modo que se puedan mover fácilmente de un entorno a otro.

Para instalar LXC en una distribución de Linux, podemos usar el gestor de paquetes de la distribución. Por ejemplo, en Ubuntu, podemos usar el gestor de paquetes `apt` para instalar LXC con el siguiente comando:

**Instalar LXC**
foonkeemoonkee@htb[/htb]$ sudo apt-get install lxc lxc-utils -y

Una vez que LXC está instalado, podemos empezar a crear y gestionar contenedores en el anfitrión Linux. Vale la pena señalar que LXC requiere que el kernel de Linux soporte las características necesarias para la contenerización. La mayoría de los kernels de Linux modernos tienen soporte integrado para la contenerización, pero algunos kernels más antiguos pueden requerir configuración o parches adicionales para habilitar el soporte para LXC.

#### **Creando un Contenedor LXC**

Para crear un nuevo contenedor LXC, podemos usar el comando `lxc-create` seguido del nombre del contenedor y la plantilla a utilizar. Por ejemplo, para crear un nuevo contenedor Ubuntu llamado `linuxcontainer`, podemos usar el siguiente comando:

foonkeemoonkee@htb[/htb]$ sudo lxc-create -n linuxcontainer -t ubuntu

#### **Gestionando Contenedores LXC**

Al trabajar con contenedores LXC, hay varias tareas involucradas en su gestión. Estas tareas incluyen la creación de nuevos contenedores, la configuración de sus ajustes, iniciarlos y detenerlos según sea necesario, y monitorear su rendimiento. Afortunadamente, hay muchas herramientas de línea de comandos y archivos de configuración disponibles que pueden ayudar con estas tareas. Estas herramientas nos permiten gestionar rápida y fácilmente nuestros contenedores, asegurando que estén optimizados para nuestras necesidades y requisitos específicos. Al aprovechar estas herramientas de manera efectiva, podemos asegurar que nuestros contenedores LXC funcionen de manera eficiente y efectiva, permitiéndonos maximizar el rendimiento y las capacidades de nuestro sistema.

| Comando | Descripción |
| :--- | :--- |
| `lxc-ls` | Lista todos los contenedores existentes. |
| `lxc-stop -n <contenedor>` | Detiene un contenedor en ejecución. |
| `lxc-start -n <contenedor>` | Inicia un contenedor detenido. |
| `lxc-restart -n <contenedor>` | Reinicia un contenedor en ejecución. |
| `lxc-config -n <nombre_contenedor> -s storage` | Gestiona el almacenamiento del contenedor. |
| `lxc-config -n <nombre_contenedor> -s network` | Gestiona la configuración de red del contenedor. |
| `lxc-config -n <nombre_contenedor> -s security` | Gestiona la configuración de seguridad del contenedor. |
| `lxc-attach -n <contenedor>` | Se conecta a un contenedor. |
| `lxc-attach -n <contenedor> -f /ruta/a/compartir`| Se conecta a un contenedor y comparte un directorio o archivo específico. |

Como pentesters, a menudo nos enfrentamos a situaciones en las que necesitamos probar software o sistemas que tienen dependencias complejas o configuraciones difíciles de replicar en nuestras máquinas locales. Aquí es donde los contenedores de Linux se vuelven extremadamente útiles. Un contenedor de Linux es un paquete ligero e independiente que incluye todo lo necesario para ejecutar una pieza de software específica, como el código, las bibliotecas y los archivos de configuración. Los contenedores ofrecen un entorno aislado, permitiendo que el software se ejecute de manera consistente en cualquier máquina Linux, independientemente de la configuración del sistema anfitrión.

Los contenedores son particularmente útiles porque nos permiten crear y ejecutar rápidamente entornos aislados adaptados a nuestras necesidades de prueba específicas. Por ejemplo, si necesitamos probar una aplicación web que depende de una versión particular de una base de datos o un servidor web, podemos crear un contenedor con las versiones y configuraciones exactas que necesitamos. Esto elimina la molestia de configurar manualmente estos componentes en nuestra máquina, lo que puede consumir mucho tiempo y ser propenso a errores.

También podemos usarlos para probar exploits o malware en un entorno controlado donde creamos un contenedor que simula un sistema o red vulnerable y luego usamos ese contenedor para probar exploits de forma segura sin arriesgar dañar nuestras máquinas o redes. Sin embargo, es importante configurar la seguridad del contenedor LXC para evitar el acceso no autorizado o actividades maliciosas dentro del contenedor. Esto se puede lograr implementando varias medidas de seguridad, tales como:

*   Restringir el acceso al contenedor
*   Limitar los recursos
*   Aislar el contenedor del anfitrión
*   Hacer cumplir el control de acceso obligatorio
*   Mantener el contenedor actualizado

Se puede acceder a los contenedores LXC mediante varios métodos, como SSH o consola. Se recomienda restringir el acceso al contenedor deshabilitando servicios innecesarios, usando protocolos seguros y aplicando mecanismos de autenticación fuertes. Por ejemplo, podemos deshabilitar el acceso SSH al contenedor eliminando el paquete `openssh-server` o configurando SSH para que solo permita el acceso desde direcciones IP de confianza. Esos contenedores también comparten el mismo kernel que el sistema anfitrión, lo que significa que pueden acceder a todos los recursos disponibles en el sistema. Podemos usar límites de recursos o cuotas para evitar que los contenedores consuman recursos excesivos. Por ejemplo, podemos usar cgroups para limitar la cantidad de CPU, memoria o espacio en disco que un contenedor puede usar.

#### **Asegurando LXC**

Limitemos los recursos al contenedor. Para configurar cgroups para LXC y limitar la CPU y la memoria, un contenedor puede crear un nuevo archivo de configuración en el directorio `/usr/share/lxc/config/<nombre_contenedor>.conf` con el nombre de nuestro contenedor. Por ejemplo, para crear un archivo de configuración para un contenedor llamado `linuxcontainer`, podemos usar el siguiente comando:

foonkeemoonkee@htb[/htb]$ sudo vim /usr/share/lxc/config/linuxcontainer.conf

En este archivo de configuración, podemos agregar las siguientes líneas para limitar la CPU y la memoria que el contenedor puede usar.```txt
lxc.cgroup.cpu.shares = 512
lxc.cgroup.memory.limit_in_bytes = 512M
```

Al trabajar con contenedores, es importante entender el parámetro `lxc.cgroup.cpu.shares`. Este parámetro determina el tiempo de CPU que un contenedor puede usar en relación con los otros contenedores en el sistema. Por defecto, este valor está establecido en 1024, lo que significa que el contenedor puede usar hasta su parte justa del tiempo de CPU. Sin embargo, si establecemos este valor en 512, por ejemplo, el contenedor solo podrá usar la mitad del tiempo de CPU disponible en el sistema. Esta puede ser una forma útil de gestionar los recursos y asegurar que todos los contenedores tengan el acceso necesario al tiempo de CPU.

Uno de los parámetros clave para controlar la asignación de recursos de un contenedor es el parámetro `lxc.cgroup.memory.limit_in_bytes`. Este parámetro te permite establecer la cantidad máxima de memoria que un contenedor puede usar. Es importante tener en cuenta que este valor se puede especificar en una variedad de unidades, incluyendo bytes, kilobytes (K), megabytes (M), gigabytes (G) o terabytes (T), lo que permite un alto grado de granularidad al definir los límites de recursos del contenedor. Después de agregar estas dos líneas, podemos guardar y cerrar el archivo escribiendo:

*   `[Esc]`
*   `:`
*   `wq`

Para aplicar estos cambios, debemos reiniciar el servicio LXC.

foonkeemoonkee@htb[/htb]$ sudo systemctl restart lxc.service

LXC utiliza espacios de nombres (*namespaces*) para proporcionar un entorno aislado para procesos, redes y sistemas de archivos del sistema anfitrión. Los espacios de nombres son una característica del kernel de Linux que permite la creación de entornos aislados al proporcionar una abstracción de los recursos del sistema.

Los espacios de nombres son un aspecto crucial de la contenerización, ya que proporcionan un alto grado de aislamiento para los procesos, interfaces de red, tablas de enrutamiento y reglas de cortafuegos del contenedor. A cada contenedor se le asigna un espacio de números de ID de proceso (pid) único, aislado de los ID de proceso del sistema anfitrión. Esto asegura que los procesos del contenedor no puedan interferir con los procesos del sistema anfitrión, mejorando la estabilidad y fiabilidad del sistema. Además, cada contenedor tiene sus propias interfaces de red (net), tablas de enrutamiento y reglas de cortafuegos, que están completamente separadas de las interfaces de red del sistema anfitrión. Cualquier actividad relacionada con la red dentro del contenedor está acordonada de la red del sistema anfitrión, proporcionando una capa extra de seguridad de red.

Además, los contenedores vienen con su propio sistema de archivos raíz (mnt), que es completamente diferente del sistema de archivos raíz del sistema anfitrión. Esta separación entre los dos asegura que cualquier cambio o modificación realizada dentro del sistema de archivos del contenedor no afecte al sistema de archivos del anfitrión. Sin embargo, es importante recordar que, si bien los espacios de nombres proporcionan un alto nivel de aislamiento, no ofrecen una seguridad completa. Por lo tanto, siempre es aconsejable implementar medidas de seguridad adicionales para proteger aún más el contenedor y el sistema anfitrión de posibles brechas de seguridad.

Aquí hay 9 ejercicios opcionales para practicar LXC:

1.  Instala LXC en tu máquina y crea tu primer contenedor.
2.  Configura los ajustes de red para tu contenedor LXC.
3.  Crea una imagen LXC personalizada y úsala para lanzar un nuevo contenedor.
4.  Configura límites de recursos para tus contenedores LXC (CPU, memoria, espacio en disco).
5.  Explora los comandos `lxc-*` para gestionar contenedores.
6.  Usa LXC para crear un contenedor que ejecute una versión específica de un servidor web (por ejemplo, Apache, Nginx).
7.  Configura el acceso SSH a tus contenedores LXC y conéctate a ellos de forma remota.
8.  Crea un contenedor con persistencia, para que los cambios realizados en el contenedor se guarden y puedan reutilizarse.
9.  Usa LXC para probar software en un entorno controlado, como una aplicación web vulnerable o malware.