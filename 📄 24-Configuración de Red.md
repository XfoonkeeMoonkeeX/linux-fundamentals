
***
### **Configuración de Red**

Como pentester, una de las habilidades esenciales es configurar y gestionar los ajustes de red en sistemas Linux. Dominar esto nos permite configurar eficientemente entornos de prueba, manipular el tráfico de red e identificar o explotar vulnerabilidades. Un sólido entendimiento de la configuración de red en Linux nos da la capacidad de adaptar nuestro enfoque de prueba para satisfacer necesidades específicas, ayudando a optimizar tanto nuestros procedimientos de prueba como los resultados.

Una de las tareas principales en la configuración de red es la gestión de las interfaces de red. Esto implica asignar direcciones IP, configurar dispositivos de red como routers y switches, y establecer diversos protocolos de red. Es fundamental tener un conocimiento profundo de los protocolos de red, incluyendo TCP/IP (el conjunto de protocolos principal para las comunicaciones de Internet), DNS (resolución de nombres de dominio), DHCP (para la asignación dinámica de direcciones IP) y FTP (transferencia de archivos). También debemos estar familiarizados con los diferentes tipos de interfaces de red —ya sean cableadas o inalámbricas— y ser capaces de solucionar problemas de conectividad.

#### **Control de Acceso a la Red (NAC)**

Otro componente vital de la configuración de red es el control de acceso a la red (NAC, por sus siglas en inglés). Como pentesters, necesitamos estar bien versados en cómo el NAC puede mejorar la seguridad de la red y en las diversas tecnologías disponibles. Los modelos clave de NAC incluyen:

| Tipo | Descripción |
| :--- | :--- |
| **Control de Acceso Discrecional (DAC)** | Este modelo permite al propietario del recurso establecer permisos sobre quién puede acceder a él. |
| **Control de Acceso Obligatorio (MAC)** | Los permisos son aplicados por el sistema operativo, no por el propietario del recurso, lo que lo hace más seguro pero menos flexible. |
| **Control de Acceso Basado en Roles (RBAC)**| Los permisos se asignan en función de los roles dentro de una organización, lo que facilita la gestión de los privilegios de los usuarios. |

La configuración de dispositivos de red Linux para NAC implica establecer políticas de seguridad como SELinux (Security-Enhanced Linux), perfiles de AppArmor para la seguridad de las aplicaciones y el uso de TCP wrappers para controlar el acceso a los servicios basándose en direcciones IP. Veremos más sobre esto en futuras secciones.

Herramientas como `syslog`, `rsyslog`, `ss` (para estadísticas de sockets), `lsof` (para listar archivos abiertos) y el stack ELK (Elasticsearch, Logstash y Kibana) pueden ser usadas para monitorear y analizar el tráfico de red. Estas herramientas ayudan a identificar anomalías, posibles divulgaciones de información, brechas de seguridad y otros problemas críticos de la red.

Piensa en la configuración de red en Linux como la construcción y seguridad de un gran edificio de oficinas. Configurar las interfaces de red es como instalar el cableado y la infraestructura, asegurando que cada habitación (dispositivo de red) tenga una conexión funcional. El **NAC** es como gestionar la seguridad del edificio, donde algunas habitaciones están abiertas para todos (DAC), mientras que otras solo son accesibles para ciertas personas según reglas estrictas (MAC o RBAC). Monitorear el tráfico de red es similar a instalar cámaras de vigilancia y alarmas, vigilando quién se mueve por el edificio, y la **solución de problemas** es como tener una caja de herramientas a mano para solucionar cualquier problema, ya sea una conexión rota (`ping`), una cerradura defectuosa (`nslookup`) o una entrada vulnerable (`nmap`). Exploraremos el NAC y las herramientas con mayor detalle un poco más adelante en esta sección.

### **Configurando Interfaces de Red**

Cuando trabajas con Ubuntu, puedes configurar las interfaces de red locales usando los comandos `ifconfig` o `ip`. Estos potentes comandos nos permiten ver y configurar las interfaces de red de nuestro sistema. Ya sea que busquemos hacer cambios en nuestra configuración de red existente o necesitemos verificar el estado de nuestras interfaces, estos comandos pueden simplificar enormemente el proceso. Además, desarrollar un firme dominio de las complejidades de las interfaces de red es una habilidad esencial en el mundo moderno e interconectado. Con el rápido avance de la tecnología y la creciente dependencia de la comunicación digital, tener un conocimiento exhaustivo de cómo trabajar con interfaces de red puede permitirte navegar eficazmente por la diversa gama de redes que existen hoy en día.

Una forma de obtener información sobre las interfaces de red, como direcciones IP, máscaras de red y estado, es usando el comando `ifconfig`. Al ejecutar este comando, podemos ver las interfaces de red disponibles y sus respectivos atributos de manera clara y organizada. Esta información puede ser particularmente útil al solucionar problemas de conectividad de red o al configurar una nueva red. Cabe señalar que el comando `ifconfig` ha quedado obsoleto en las versiones más nuevas de Linux y ha sido reemplazado por el comando `ip`, que ofrece características más avanzadas. Sin embargo, el comando `ifconfig` todavía se usa ampliamente en muchas distribuciones de Linux y sigue siendo una herramienta fiable para la gestión de redes.

**Ajustes de Red**
cry0l1t3@htb:~$ ifconfig
```
eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 178.62.32.126  netmask 255.255.192.0  broadcast 178.62.63.255
        inet6 fe80::88d9:faff:fecf:797a  prefixlen 64  scopeid 0x20<link>
        ether 8a:d9:fa:cf:79:7a  txqueuelen 1000  (Ethernet)
        RX packets 7910  bytes 717102 (700.2 KiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 7072  bytes 24215666 (23.0 MiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

eth1: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 10.106.0.66  netmask 255.255.240.0  broadcast 10.106.15.255
        inet6 fe80::b8ab:52ff:fe32:1f33  prefixlen 64  scopeid 0x20<link>
        ether ba:ab:52:32:1f:33  txqueuelen 1000  (Ethernet)
        RX packets 14  bytes 1574 (1.5 KiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 15  bytes 1700 (1.6 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 15948  bytes 24561302 (23.4 MiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 15948  bytes 24561302 (23.4 MiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```
cry0l1t3@htb:~$ ip addr
```
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 8a:d9:fa:cf:79:7a brd ff:ff:ff:ff:ff:ff
    altname enp0s3
    altname ens3
    inet 178.62.32.126/18 brd 178.62.63.255 scope global dynamic eth0
       valid_lft 85274sec preferred_lft 85274sec
    inet6 fe80::88d9:faff:fecf:797a/64 scope link 
       valid_lft forever preferred_lft forever
3: eth1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether ba:ab:52:32:1f:33 brd ff:ff:ff:ff:ff:ff
    altname enp0s4
    altname ens4
    inet 10.106.0.66/20 brd 10.106.15.255 scope global dynamic eth1
       valid_lft 85274sec preferred_lft 85274sec
    inet6 fe80::b8ab:52ff:fe32:1f33/64 scope link 
       valid_lft forever preferred_lft forever
```
Cuando se trata de activar interfaces de red, `ifconfig` e `ip` son dos herramientas comúnmente utilizadas. Estos comandos permiten a los usuarios modificar y activar configuraciones para una interfaz específica, como `eth0`. Podemos ajustar la configuración de la red para satisfacer nuestras necesidades utilizando la sintaxis apropiada y especificando el nombre de la interfaz.

**Activar Interfaz de Red**
foonkeemoonkee@htb[/htb]$ sudo ifconfig eth0 up     # O
foonkeemoonkee@htb[/htb]$ sudo ip link set eth0 up

Una forma de asignar una dirección IP a una interfaz de red es utilizando el comando `ifconfig`. Debemos especificar el nombre de la interfaz y la dirección IP como argumentos para hacer esto. Este es un paso crucial en la configuración de una conexión de red. La dirección IP sirve como un identificador único para la interfaz y permite la comunicación entre dispositivos en la red.

**Asignar Dirección IP a una Interfaz**
foonkeemoonkee@htb[/htb]$ sudo ifconfig eth0 192.168.1.2

Para establecer la máscara de red para una interfaz de red, podemos ejecutar el siguiente comando con el nombre de la interfaz y la máscara de red:

**Asignar una Máscara de Red a una Interfaz**
foonkeemoonkee@htb[/htb]$ sudo ifconfig eth0 netmask 255.255.255.0

Cuando queremos establecer la puerta de enlace predeterminada (*default gateway*) para una interfaz de red, podemos usar el comando `route` con la opción `add`. Esto nos permite especificar la dirección IP de la puerta de enlace y la interfaz de red a la que debe aplicarse. Al establecer la puerta de enlace predeterminada, estamos designando la dirección IP del router que se utilizará para enviar tráfico a destinos fuera de la red local. Es importante asegurarse de que la puerta de enlace predeterminada esté configurada correctamente, ya que una configuración incorrecta puede provocar problemas de conectividad.

**Asignar la Ruta a una Interfaz**
foonkeemoonkee@htb[/htb]$ sudo route add default gw 192.168.1.1 eth0

Al configurar una interfaz de red en Linux, a menudo es necesario establecer servidores de Sistema de Nombres de Dominio (DNS) para garantizar una funcionalidad de red adecuada. Los servidores DNS son responsables de traducir nombres de dominio (como example.com) en direcciones IP, lo que permite a los dispositivos localizarse y conectarse entre sí en internet. Una configuración de DNS adecuada es crucial para permitir que los dispositivos accedan a sitios web, servicios en línea y otros recursos en red. Sin servidores DNS configurados correctamente, los dispositivos pueden experimentar problemas como la incapacidad de resolver nombres de dominio, lo que lleva a problemas de conectividad de red.

En los sistemas Linux, esto se puede lograr actualizando el archivo `/etc/resolv.conf`, que es un archivo de texto simple que contiene la información de DNS del sistema. Al agregar las direcciones de servidor DNS apropiadas (el DNS público de Google - 8.8.8.8 o 8.8.4.4), el sistema puede resolver correctamente los nombres de dominio a direcciones IP, asegurando una comunicación fluida a través de la red.

**Editando la Configuración de DNS**
foonkeemoonkee@htb[/htb]$ sudo vim /etc/resolv.conf

**/etc/resolv.conf**
```txt
nameserver 8.8.8.8
nameserver 8.8.4.4
```
Después de completar las modificaciones necesarias en la configuración de la red, es esencial asegurarse de que estos cambios se guarden para que persistan después de reiniciar. Esto se puede lograr editando el archivo `/etc/network/interfaces`, que define las interfaces de red para los sistemas operativos basados en Linux. Por lo tanto, es vital guardar cualquier cambio realizado en este archivo para evitar posibles problemas con la conectividad de la red.

Es importante tener en cuenta que los cambios realizados directamente en el archivo `/etc/resolv.conf` no son persistentes a través de reinicios o cambios en la configuración de la red. Esto se debe a que el archivo puede ser sobrescrito automáticamente por servicios de gestión de red como NetworkManager o systemd-resolved. Para que los cambios de DNS sean permanentes, debe configurar los ajustes de DNS a través de la herramienta de gestión de red apropiada, como editar archivos de configuración de red o usar utilidades de gestión de red que almacenen configuraciones persistentes.

**Editando Interfaces**
foonkeemoonkee@htb[/htb]$ sudo vim /etc/network/interfaces

Esto abrirá el archivo de interfaces en el editor vim. Podemos agregar los ajustes de configuración de red al archivo de esta manera:

**/etc/network/interfaces**
```txt
auto eth0
iface eth0 inet static
  address 192.168.1.2
  netmask 255.255.255.0
  gateway 192.168.1.1
  dns-nameservers 8.8.8.8 8.8.4.4
```
Al configurar la interfaz de red `eth0` para usar una dirección IP estática de 192.168.1.2, con una máscara de red de 255.255.255.0 y una puerta de enlace predeterminada de 192.168.1.1, podemos asegurar que nuestra conexión de red permanezca estable y fiable. Además, al especificar los servidores DNS 8.8.8.8 y 8.8.4.4, podemos asegurar que nuestro ordenador pueda acceder fácilmente a Internet y resolver nombres de dominio. Una vez que hemos realizado estos cambios en el archivo de configuración, es importante guardar el archivo y salir del editor. Después de eso, debemos reiniciar el servicio de red para aplicar los cambios.

**Reiniciar el Servicio de Red**
foonkeemoonkee@htb[/htb]$ sudo systemctl restart networking

### **Control de Acceso a la Red**

El control de acceso a la red (NAC) es un componente crucial de la seguridad de la red, especialmente en la era actual de crecientes amenazas cibernéticas. Como pentester, es vital comprender la importancia del NAC para proteger la red y las diversas tecnologías NAC que se pueden utilizar para mejorar las medidas de seguridad. El NAC es un sistema de seguridad que garantiza que solo los dispositivos autorizados y conformes tengan acceso a la red, evitando el acceso no autorizado, las brechas de datos y otras amenazas de seguridad. Al implementar el NAC, las organizaciones pueden confiar en su capacidad para proteger sus activos y datos de los ciberdelincuentes que siempre buscan explotar las vulnerabilidades del sistema. Las siguientes son las diferentes tecnologías NAC que se pueden utilizar para mejorar las medidas de seguridad:

*   Control de acceso discrecional (DAC)
*   Control de acceso obligatorio (MAC)
*   Control de acceso basado en roles (RBAC)

Estas tecnologías están diseñadas para proporcionar diferentes niveles de control de acceso y seguridad. Cada tecnología tiene sus características únicas y es adecuada para diferentes casos de uso. Como pentester, es esencial comprender estas tecnologías y sus casos de uso específicos para probar y evaluar eficazmente la seguridad de la red.

#### **Control de Acceso Discrecional (DAC)**
El DAC es un componente crucial de los sistemas de seguridad modernos, ya que ayuda a las organizaciones a proporcionar acceso a sus recursos mientras gestionan los riesgos asociados de acceso no autorizado. Es un sistema de control de acceso ampliamente utilizado que permite a los usuarios gestionar el acceso a sus recursos otorgando a los propietarios de los recursos la responsabilidad de controlar los permisos de acceso a sus recursos. Esto significa que los usuarios y grupos que poseen un recurso específico pueden decidir quién tiene acceso a sus recursos y qué acciones están autorizados a realizar. Estos permisos se pueden establecer para leer, escribir, ejecutar o eliminar el recurso.

#### **Control de Acceso Obligatorio (MAC)**
El MAC se utiliza en infraestructuras que proporcionan un control más detallado sobre el acceso a los recursos que los sistemas DAC. Esos sistemas definen reglas que determinan el acceso a los recursos en función del nivel de seguridad del recurso y del nivel de seguridad del usuario o proceso que solicita el acceso. A cada recurso se le asigna una etiqueta de seguridad que identifica su nivel de seguridad, y a cada usuario o proceso se le asigna una autorización de seguridad que identifica su nivel de seguridad. El acceso a un recurso solo se concede si el nivel de seguridad del usuario o proceso es igual o superior al nivel de seguridad del recurso. El MAC se utiliza a menudo en sistemas operativos y aplicaciones que requieren un alto nivel de seguridad, como sistemas militares o gubernamentales, sistemas financieros y sistemas de atención médica. Los sistemas MAC están diseñados para evitar el acceso no autorizado a los recursos y minimizar el impacto de las brechas de seguridad.

#### **Control de Acceso Basado en Roles (RBAC)**
El RBAC asigna permisos a los usuarios en función de sus roles dentro de una organización. A los usuarios se les asignan roles basados en sus responsabilidades laborales u otros criterios, y a cada rol se le otorga un conjunto de permisos que determinan las acciones que pueden realizar. El RBAC simplifica la gestión de los permisos de acceso, reduce el riesgo de errores y garantiza que los usuarios solo puedan acceder a los recursos necesarios para realizar sus funciones laborales. Puede restringir el acceso a recursos y datos sensibles, limitar el impacto de las brechas de seguridad y garantizar el cumplimiento de los requisitos reglamentarios. En comparación con los sistemas de Control de Acceso Discrecional (DAC), el RBAC proporciona un enfoque más flexible y escalable para gestionar el acceso a los recursos. En un sistema RBAC, a cada usuario se le asigna uno o más roles, y a cada rol se le asigna un conjunto de permisos que definen las acciones del usuario. El acceso a los recursos se concede en función del rol asignado al usuario en lugar de su identidad o propiedad del recurso. Los sistemas RBAC se utilizan normalmente en entornos con muchos usuarios y recursos, como grandes organizaciones, agencias gubernamentales e instituciones financieras.

### **Monitoreo**

El monitoreo de red implica capturar, analizar e interpretar el tráfico de red para identificar amenazas de seguridad, problemas de rendimiento y comportamientos sospechosos. El objetivo principal de analizar y monitorear el tráfico de red es identificar amenazas y vulnerabilidades de seguridad. Por ejemplo, como pentesters, podemos capturar credenciales cuando alguien usa una conexión no cifrada e intenta iniciar sesión en un servidor FTP. Como resultado, obtendremos las credenciales de este usuario que podrían ayudarnos a infiltrarnos aún más en la red o a escalar nuestros privilegios a un nivel superior. En resumen, al analizar el tráfico de red, podemos obtener información sobre el comportamiento de la red e identificar patrones que pueden indicar amenazas de seguridad. Dicho análisis incluye la detección de actividad de red sospechosa, la identificación de tráfico malicioso y la identificación de posibles riesgos de seguridad. Sin embargo, cubrimos este vasto tema en el módulo de *Introducción al Análisis de Tráfico de Red*, donde usamos varias herramientas para el monitoreo de red en sistemas Linux como Ubuntu y sistemas Windows, como Wireshark, tshark y Tcpdump.

### **Solución de Problemas (Troubleshooting)**

La solución de problemas de red es un proceso esencial que implica diagnosticar y resolver problemas de red que pueden afectar negativamente el rendimiento y la fiabilidad de la red. Este proceso es fundamental para garantizar que la red funcione de manera óptima y evitar interrupciones que puedan afectar las operaciones comerciales durante nuestras pruebas de penetración. También implica identificar, analizar e implementar soluciones para resolver problemas. Dichos problemas incluyen problemas de conectividad, velocidades de red lentas y errores de red. Varias herramientas pueden ayudarnos a identificar y resolver problemas relacionados con la solución de problemas de red en sistemas Linux. Algunas de las herramientas más utilizadas incluyen:

*   `Ping`
*   `Traceroute`
*   `Netstat`
*   `Tcpdump`
*   `Wireshark`
*   `Nmap`

Al usar estas herramientas y otras similares, podemos comprender mejor cómo funciona la red y diagnosticar rápidamente cualquier problema que pueda surgir. Por ejemplo, `ping` es una herramienta de línea de comandos que se utiliza para probar la conectividad entre dos dispositivos. Envía paquetes a un host remoto y mide el tiempo que tardan en regresar. Para usar ping, podemos introducir el siguiente comando:

**Ping**
foonkeemoonkee@htb[/htb]$ ping <host_remoto>

Por ejemplo, hacer ping al servidor DNS de Google enviará paquetes ICMP al servidor DNS de Google y mostrará los tiempos de respuesta.
foonkeemoonkee@htb[/htb]$ ping 8.8.8.8
```
PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data.
64 bytes from 8.8.8.8: icmp_seq=1 ttl=119 time=1.61 ms
64 bytes from 8.8.8.8: icmp_seq=2 ttl=119 time=1.06 ms
64 bytes from 8.8.8.8: icmp_seq=3 ttl=119 time=0.636 ms
64 bytes from 8.8.8.8: icmp_seq=4 ttl=119 time=0.685 ms
^C
--- 8.8.8.8 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3017ms
rtt min/avg/max/mdev = 0.636/0.996/1.607/0.388 ms
```Otra herramienta es `traceroute`, que traza la ruta que toman los paquetes para llegar a un host remoto. Envía paquetes con valores crecientes de Tiempo de Vida (TTL) a un host remoto y muestra las direcciones IP de los dispositivos por los que pasan los paquetes. Por ejemplo, para trazar la ruta al servidor DNS de Google, introduciríamos el siguiente comando:

**Traceroute**
foonkeemoonkee@htb[/htb]$ traceroute www.inlanefreight.com
```
traceroute to www.inlanefreight.com (134.209.24.248), 30 hops max, 60 byte packets
 1  * * *
 2  10.80.71.5 (10.80.71.5)  2.716 ms  2.700 ms  2.730 ms
 3  * * *
 4  10.80.68.175 (10.80.68.175)  7.147 ms  7.132 ms 10.80.68.161 (10.80.68.161)  7.393 ms
```
Esto mostrará las direcciones IP de los dispositivos por los que pasan los paquetes para llegar al servidor DNS de Google. La salida de un comando traceroute muestra cómo se utiliza para trazar la ruta de los paquetes al sitio web www.inlanefreight.com, que tiene una dirección IP de 134.209.24.248. Cada línea de la salida contiene información valiosa.

Al configurar una conexión de red, es importante especificar el host de destino y la dirección IP. En este ejemplo, el host de destino es 134.209.24.248, y el número máximo de saltos permitidos es 30. Esto garantiza que la conexión se establezca de manera eficiente y fiable. Al proporcionar esta información, el sistema puede enrutar el tráfico al destino correcto y limitar el número de paradas intermedias que los datos deben hacer.

La segunda línea muestra el primer salto en el traceroute, que es la puerta de enlace de la red local con la dirección IP 10.80.71.5, seguido de las siguientes tres columnas que muestran el tiempo que tardó cada uno de los tres paquetes enviados en llegar a la puerta de enlace en milisegundos (2.716 ms, 2.700 ms y 2.730 ms).

A continuación, vemos el segundo salto en el traceroute. Sin embargo, no hubo respuesta del dispositivo en ese salto, indicado por los tres asteriscos en lugar de la dirección IP. Esto podría significar que el dispositivo está caído, bloqueando el tráfico ICMP, o un problema de red provocó que los paquetes se perdieran.

En la cuarta línea, podemos ver el tercer salto en el traceroute, que consta de dos dispositivos con direcciones IP 10.80.68.175 y 10.80.68.161, y de nuevo las siguientes tres columnas muestran el tiempo que tardó cada uno de los tres paquetes en llegar al primer dispositivo (7.147 ms, 7.132 ms y 7.393 ms).

#### **Netstat**
`Netstat` se utiliza para mostrar las conexiones de red activas y sus puertos asociados. Se puede utilizar para identificar el tráfico de red y solucionar problemas de conectividad. Para usar netstat, podemos introducir el siguiente comando:

foonkeemoonkee@htb[/htb]$ netstat -a
```
Active Internet connections (servers and established)
Proto Recv-Q Send-Q Local Address           Foreign Address         State      
tcp        0      0 localhost:5901          0.0.0.0:*               LISTEN     
tcp        0      0 0.0.0.0:sunrpc          0.0.0.0:*               LISTEN     
tcp        0      0 0.0.0.0:http            0.0.0.0:*               LISTEN     
tcp        0      0 0.0.0.0:ssh             0.0.0.0:*               LISTEN
...SNIP...
```
Podemos esperar recibir información detallada sobre cada conexión al usar esta herramienta. Esto incluye el protocolo utilizado, el número de bytes recibidos y enviados, las direcciones IP, los números de puerto de los dispositivos locales y remotos, y el estado actual de la conexión. La salida proporciona información valiosa sobre la actividad de la red en el sistema, destacando cuatro conexiones específicas que están actualmente activas y escuchando en puertos específicos. Estas conexiones incluyen el software de escritorio remoto VNC, el servicio Sun Remote Procedure Call, el protocolo HTTP para el tráfico web y el protocolo SSH para el acceso seguro a la shell remota. Al saber qué puertos son utilizados por qué servicios, los usuarios pueden identificar rápidamente cualquier problema de red y solucionarlo en consecuencia. Los problemas de red más comunes que encontraremos durante nuestras pruebas de penetración son los siguientes:

*   Problemas de conectividad de red
*   Problemas de resolución de DNS (siempre es culpa del DNS)
*   Pérdida de paquetes de datos
*   Problemas de rendimiento de la red

Las causas más comunes para ellos son:

*   Cortafuegos o routers mal configurados,
*   cables o conexiones de red dañados,
*   configuración de red incorrecta,
*   fallos de hardware,
*   configuración incorrecta del servidor DNS o fallos del servidor DNS
*   entradas de DNS mal configuradas,
*   congestión de la red,
*   hardware de red obsoleto o configuración de red incorrecta,
*   software o firmware sin parches y falta de controles de seguridad.

Comprender estos problemas de red comunes y sus causas es importante para identificar y explotar eficazmente las vulnerabilidades en los sistemas de red durante nuestras pruebas.

### **Fortalecimiento (Hardening)**
Varios mecanismos son altamente efectivos para asegurar los sistemas Linux y mantener seguros nuestros datos y los de otras empresas. Tres de estos mecanismos son SELinux, AppArmor y TCP wrappers. Estas herramientas están diseñadas para proteger los sistemas Linux contra diversas amenazas de seguridad, desde el acceso no autorizado hasta los ataques maliciosos. Esto es fundamental no solo durante las pruebas de penetración, donde los sistemas se someten a estrés intencionadamente para descubrir vulnerabilidades, sino también en escenarios del mundo real donde un compromiso real podría tener consecuencias graves (pocas situaciones son tan graves como una brecha en la vida real). Al implementar estas medidas de seguridad y asegurarnos de establecer la protección correspondiente contra posibles atacantes, podemos reducir significativamente el riesgo de fugas de datos y garantizar que nuestros sistemas permanezcan seguros. Si bien estas herramientas comparten algunas similitudes, también tienen diferencias importantes.

#### **Security-Enhanced Linux (SELinux)**
Security-Enhanced Linux (SELinux) es un sistema de control de acceso obligatorio (MAC) integrado en el kernel de Linux. Proporciona un control detallado sobre el acceso a los recursos del sistema y las aplicaciones mediante la aplicación de políticas de seguridad. Estas políticas definen los permisos para cada proceso y archivo en el sistema, limitando significativamente el daño que un proceso o servicio comprometido puede causar. SELinux opera a bajo nivel y, aunque ofrece una seguridad sólida, puede ser complejo de configurar y gestionar debido a sus controles granulares.

#### **AppArmor**
Al igual que SELinux, AppArmor es un sistema MAC que controla el acceso a los recursos del sistema y las aplicaciones, pero opera de una manera más simple y fácil de usar. AppArmor se implementa como un Módulo de Seguridad de Linux (LSM) y utiliza perfiles de aplicación para definir a qué recursos puede acceder una aplicación. Si bien puede no proporcionar el mismo nivel de control detallado que SELinux, AppArmor suele ser más fácil de configurar y generalmente se considera más sencillo para el uso diario.

#### **TCP Wrappers**
TCP wrappers es una herramienta de control de acceso a la red basada en el host que restringe el acceso a los servicios de red según la dirección IP de las conexiones entrantes. Cuando se realiza una solicitud de red, los TCP wrappers la interceptan, verificando la solicitud contra una lista de direcciones IP permitidas o denegadas. Esta es una forma simple pero efectiva de controlar el acceso a los servicios, especialmente para bloquear el acceso de sistemas no autorizados a los recursos en red. Si bien no ofrece el control detallado de SELinux o AppArmor, los TCP wrappers son una excelente herramienta para la protección básica a nivel de red.

En cuanto a las similitudes, los tres mecanismos de seguridad comparten el objetivo común de garantizar la seguridad de los sistemas Linux. Además de proporcionar protección adicional, pueden restringir el acceso a recursos y servicios, reduciendo así el riesgo de acceso no autorizado y brechas de datos. También vale la pena señalar que estos mecanismos están fácilmente disponibles como parte de la mayoría de las distribuciones de Linux, lo que los hace accesibles para mejorar la seguridad de nuestros sistemas. Además, estos mecanismos se pueden personalizar y configurar fácilmente utilizando herramientas y utilidades estándar, lo que los convierte en una opción conveniente para los usuarios de Linux.

Aunque tanto SELinux como AppArmor son sistemas MAC que proporcionan un control detallado, funcionan de diferentes maneras. SELinux está profundamente integrado en el kernel y ofrece controles de seguridad más detallados, pero puede ser más complejo de configurar y mantener. En contraste, AppArmor opera como un módulo del kernel y utiliza seguridad basada en perfiles, lo que lo hace más fácil de gestionar, aunque puede no ofrecer el mismo nivel de granularidad que SELinux.

Por otro lado, los TCP wrappers se centran en controlar el acceso a los servicios de red en función de las direcciones IP del cliente, lo que lo hace más simple pero limitado al control de acceso a nivel de red. No ofrece las protecciones más amplias de recursos del sistema que proporcionan SELinux y AppArmor, pero es útil para restringir el acceso a los servicios desde sistemas no autorizados.

### **Puesta en Práctica**
A medida que navegamos por el mundo de Linux, inevitablemente nos encontramos con una amplia gama de tecnologías, aplicaciones y servicios con los que debemos familiarizarnos. Esta es una habilidad crucial, especialmente si trabajamos en ciberseguridad y nos esforzamos por mejorar continuamente nuestra experiencia. Por esta razón, recomendamos encarecidamente dedicar tiempo a aprender sobre la configuración de medidas de seguridad importantes como SELinux, AppArmor y TCP wrappers por tu cuenta. Al asumir este desafío (opcional pero muy eficiente), profundizarás tu comprensión de estas tecnologías, desarrollarás tus habilidades para resolver problemas y obtendrás una valiosa experiencia que te servirá en el futuro. Recomendamos encarecidamente utilizar una VM personal y hacer instantáneas (*snapshots*) antes de realizar cambios.

Cuando se trata de implementar medidas de ciberseguridad, no existe un enfoque único para todos. Es importante considerar la información específica que deseas proteger y las herramientas que utilizarás para hacerlo. Sin embargo, puedes practicar e implementar varias tareas opcionales con otros en el canal de Discord para aumentar tus conocimientos y habilidades en esta área. Al aprovechar la amabilidad de los demás y compartir tu propia experiencia, puedes profundizar tu comprensión de la ciberseguridad y ayudar a otros a hacer lo mismo. Recuerda, explicar conceptos a otros es esencial para enseñar y aprender.

#### **SELinux**
1.  Instala SELinux en tu VM.
2.  Configura SELinux para evitar que un usuario acceda a un archivo específico.
3.  Configura SELinux para permitir que un solo usuario acceda a un servicio de red específico pero denegar el acceso a todos los demás.
4.  Configura SELinux para denegar el acceso a un usuario o grupo específico para un servicio de red determinado.

#### **AppArmor**
5.  Configura AppArmor para evitar que un usuario acceda a un archivo específico.
6.  Configura AppArmor para permitir que un solo usuario acceda a un servicio de red específico pero denegar el acceso a todos los demás.
7.  Configura AppArmor para denegar el acceso a un usuario o grupo específico para un servicio de red determinado.

#### **TCP Wrappers**
8.  Configura TCP wrappers para permitir el acceso a un servicio de red específico desde una dirección IP específica.
9.  Configura TCP wrappers para denegar el acceso a un servicio de red específico desde una dirección IP específica.
10. Configura TCP wrappers para permitir el acceso a un servicio de red específico desde un rango de direcciones IP.