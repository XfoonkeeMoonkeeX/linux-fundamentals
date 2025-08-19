# Servicios de Red

Cuando se trabaja con Linux, es esencial gestionar diversos servicios de red. La competencia en el manejo de estos servicios es crucial por varias razones. Los servicios de red están diseñados para realizar tareas específicas, muchas de las cuales permiten operaciones remotas. Es importante tener el conocimiento y las habilidades para comunicarse con otras computadoras a través de la red, establecer conexiones, transferir archivos, analizar el tráfico de red y configurar estos servicios de manera efectiva. Esta pericia nos permite identificar vulnerabilidades potenciales durante las pruebas de penetración. Además, comprender las opciones de configuración de cada servicio mejora nuestra comprensión general de la seguridad de la red.

Considera un escenario en el que estamos realizando una prueba de penetración y encontramos un host Linux que está siendo evaluado por vulnerabilidades. Al monitorear el tráfico de red, observamos que un usuario en este host Linux se está conectando a otro servidor a través de un servidor FTP no cifrado. En consecuencia, somos capaces de capturar las credenciales del usuario en texto plano. Esta situación sería mucho menos probable si el usuario fuera consciente de que FTP no cifra las conexiones ni los datos transmitidos. Para un administrador de Linux, esto representa un descuido crítico, ya que no solo expone debilidades de seguridad dentro de la red, sino que también refleja mal a los administradores responsables de mantener la seguridad de la red.

Aunque no es factible cubrir todos los servicios de red, nos centraremos en los más importantes. Este enfoque es beneficioso no solo para administradores y usuarios, sino también para los pentesters que necesitan comprender las interacciones entre diferentes hosts y sus propios sistemas.

## SSH

Secure Shell (SSH) es un protocolo de red que permite la transmisión segura de datos y comandos a través de una red. Se utiliza ampliamente para gestionar sistemas remotos de forma segura y acceder a ellos para ejecutar comandos o transferir archivos. Para conectarse a nuestro host Linux o a uno remoto a través de SSH, debe haber un servidor SSH correspondiente disponible y en ejecución.

El servidor SSH más comúnmente utilizado es el servidor OpenSSH. OpenSSH es una implementación libre y de código abierto del protocolo Secure Shell (SSH) que permite la transmisión segura de datos y comandos a través de una red.

Los administradores utilizan OpenSSH para gestionar sistemas remotos de forma segura estableciendo una conexión cifrada a un host remoto. Con OpenSSH, los administradores pueden ejecutar comandos en sistemas remotos, transferir archivos de forma segura y establecer una conexión remota segura sin que la transmisión de datos y comandos sea interceptada por terceros.

### Instalar OpenSSH

```bash
foonkeemoonkee@htb[/htb]$ sudo apt install openssh-server -y
```

Para verificar si el servidor está en ejecución, podemos usar el siguiente comando:

### Estado del Servidor

```bash
foonkeemoonkee@htb[/htb]$ systemctl status ssh

● ssh.service - OpenBSD Secure Shell server
     Loaded: loaded (/lib/system/system/ssh.service; enabled; vendor preset: enabled)
     Active: active (running) since Sun 2023-02-12 21:15:27 GMT; 1min 22s ago
       Docs: man:sshd(8)
             man:sshd_config(5)
   Main PID: 7740 (sshd)
      Tasks: 1 (limit: 9458)
     Memory: 2.5M
        CPU: 236ms
     CGroup: /system.slice/ssh.service
             └─7740 sshd: /usr/sbin/sshd -D [listener] 0 of 10-100 startups
```

Como pentesters, usamos OpenSSH para acceder de forma segura a sistemas remotos al realizar una auditoría de red. Para hacer esto, podemos usar el siguiente comando:

### SSH - Iniciar Sesión

```bash
foonkeemoonkee@htb[/htb]$ ssh cry0l1t3@10.129.17.122

The authenticity of host '10.129.17.122 (10.129.17.122)' can't be established.
ECDSA key fingerprint is SHA256:bKzhv+n2pYqr2r...Egf8LfqaHNxk.

Are you sure you want to continue connecting (yes/no/[fingerprint])? yes

Warning: Permanently added '10.129.17.122' (ECDSA) to the list of known hosts.

cry0l1t3@10.129.17.122's password: ***********
```

OpenSSH se puede configurar y personalizar editando el archivo `/etc/ssh/sshd_config` con un editor de texto. Aquí podemos ajustar configuraciones como el número máximo de conexiones concurrentes, el uso de contraseñas o claves para los inicios de sesión, la verificación de la clave del host y más. Sin embargo, es importante que tengamos en cuenta que los cambios en el archivo de configuración de OpenSSH deben realizarse con cuidado.

Por ejemplo, podemos usar SSH para iniciar sesión de forma segura en un sistema remoto y ejecutar comandos o usar túneles y reenvío de puertos para tunelizar datos a través de una conexión cifrada para verificar la configuración de red y otros ajustes del sistema sin la posibilidad de que terceros intercepten la transmisión de datos y comandos.

## NFS

Network File System (NFS) es un protocolo de red que nos permite almacenar y gestionar archivos en sistemas remotos como si estuvieran almacenados en el sistema local. Permite una gestión fácil y eficiente de archivos a través de las redes. Por ejemplo, los administradores utilizan NFS para almacenar y gestionar archivos de forma centralizada (para sistemas Linux y Windows) para permitir una colaboración y gestión de datos sencillas. Para Linux, existen varios servidores NFS, incluyendo NFS-UTILS (Ubuntu), NFS-Ganesha (Solaris) y OpenNFS (Red Hat Linux).

También se puede utilizar para compartir y gestionar recursos de manera eficiente, por ejemplo, para replicar sistemas de archivos entre servidores. También ofrece características como controles de acceso, transferencia de archivos en tiempo real y soporte para que múltiples usuarios accedan a los datos simultáneamente. Podemos usar este servicio al igual que FTP en caso de que no haya un cliente FTP instalado en el sistema objetivo, o si se está ejecutando NFS en lugar de FTP.

Podemos instalar NFS en Linux con el siguiente comando:

### Instalar NFS

```bash
foonkeemoonkee@htb[/htb]$ sudo apt install nfs-kernel-server -y
```

Para verificar si el servidor está en ejecución, podemos usar el siguiente comando:

### Estado del Servidor

```bash
foonkeemoonkee@htb[/htb]$ systemctl status nfs-kernel-server

● nfs-server.service - NFS server and services
     Loaded: loaded (/lib/system/system/nfs-server.service; enabled; vendor preset: enabled)
     Active: active (exited) since Sun 2023-02-12 21:35:17 GMT; 13s ago
    Process: 9234 ExecStartPre=/usr/sbin/exportfs -r (code=exited, status=0/SUCCESS)
    Process: 9235 ExecStart=/usr/sbin/rpc.nfsd $RPCNFSDARGS (code=exited, status=0/SUCCESS)
   Main PID: 9235 (code=exited, status=0/SUCCESS)
        CPU: 10ms
```

Podemos configurar NFS a través del archivo de configuración `/etc/exports`. Este archivo especifica qué directorios deben compartirse y los derechos de acceso para usuarios y sistemas. También es posible configurar ajustes como la velocidad de transferencia y el uso de cifrado. Los derechos de acceso de NFS determinan qué usuarios y sistemas pueden acceder a los directorios compartidos y qué acciones pueden realizar. Aquí hay algunos derechos de acceso importantes que se pueden configurar en NFS:

| Permisos       | Descripción                                                                                                     |
| -------------- | --------------------------------------------------------------------------------------------------------------- |
| `rw`           | Otorga a los usuarios y sistemas permisos de lectura y escritura en el directorio compartido.                   |
| `ro`           | Otorga a los usuarios y sistemas acceso de solo lectura al directorio compartido.                               |
| `no_root_squash` | Evita que el usuario `root` en el cliente sea restringido a los derechos de un usuario normal.                  |
| `root_squash`  | Restringe los derechos del usuario `root` en el cliente a los derechos de un usuario normal.                      |
| `sync`         | Sincroniza la transferencia de datos para asegurar que los cambios solo se transfieran después de haber sido guardados en el sistema de archivos. |
| `async`        | Transfiere datos de forma asíncrona, lo que hace la transferencia más rápida, pero puede causar inconsistencias en el sistema de archivos si los cambios no se han confirmado por completo. |

Por ejemplo, podemos crear una nueva carpeta y compartirla temporalmente en NFS. Lo haríamos de la siguiente manera:

### Crear un Recurso Compartido NFS

```bash
cry0l1t3@htb:~$ mkdir nfs_sharing
cry0l1t3@htb:~$ echo '/home/cry0l1t3/nfs_sharing hostname(rw,sync,no_root_squash)' >> /etc/exports
cry0l1t3@htb:~$ cat /etc/exports | grep -v "#"

/home/cry0l1t3/nfs_sharing hostname(rw,sync,no_root_squash)
```

Si hemos creado un recurso compartido NFS y queremos trabajar con él en el sistema objetivo, primero tenemos que montarlo. Podemos hacerlo con el siguiente comando:

### Montar un Recurso Compartido NFS

```bash
cry0l1t3@htb:~$ mkdir ~/target_nfs
cry0l1t3@htb:~$ mount 10.129.12.17:/home/john/dev_scripts ~/target_nfs
cry0l1t3@htb:~$ tree ~/target_nfs

target_nfs/
├── css.css
├── html.html
├── javascript.js
├── php.php
└── xml.xml

0 directories, 5 files
```

Así hemos montado el recurso compartido NFS (`dev_scripts`) de nuestro objetivo (`10.129.12.17`) localmente en nuestro sistema en el punto de montaje `target_nfs` a través de la red y podemos ver el contenido como si estuviéramos en el sistema objetivo. Incluso existen algunos métodos que se pueden utilizar en casos específicos para escalar nuestros privilegios en el sistema remoto utilizando NFS.

## Servidor Web

Comprender el funcionamiento de los servidores web es esencial para los pentesters, ya que estos servidores son parte integral de las aplicaciones web y con frecuencia sirven como objetivos principales durante las evaluaciones de seguridad. Un servidor web es un software que entrega datos, documentos, aplicaciones y diversas funciones a través de Internet. Utiliza el Protocolo de Transferencia de Hipertexto (HTTP) para transmitir datos a clientes como navegadores web y para recibir solicitudes de estos clientes. Los datos recibidos se representan como Lenguaje de Marcado de Hipertexto (HTML) dentro del navegador del cliente, facilitando la creación de páginas web dinámicas que responden interactivamente a las solicitudes del usuario. En consecuencia, una comprensión profunda de las funcionalidades del servidor web es vital para desarrollar aplicaciones web seguras y eficientes y para mantener la seguridad general del sistema. Entre los servidores web más utilizados en plataformas Linux se encuentran Apache, Nginx, Lighttpd y Caddy, siendo Apache particularmente popular debido a su amplia compatibilidad con sistemas operativos como Ubuntu, Solaris y Red Hat Linux.

Para los pentesters, los servidores web ofrecen diversas utilidades. Pueden emplearse para facilitar la transferencia de archivos, permitiendo a los testers iniciar sesión e interactuar con los sistemas objetivo a través de los puertos HTTP o HTTPS. Además, los servidores web pueden aprovecharse para realizar ataques de phishing alojando réplicas de páginas objetivo, intentando así capturar las credenciales de los usuarios. Más allá de estas aplicaciones, los servidores web proporcionan numerosas otras oportunidades para probar y explotar vulnerabilidades dentro de una red.

El servidor web Apache tiene una variedad de características que nos permiten alojar una aplicación web segura y eficiente. Además, también podemos configurar el registro para obtener información sobre el tráfico en nuestro servidor, lo que nos ayuda a analizar ataques. Podemos instalar Apache usando el siguiente comando:

### Instalar Servidor Web Apache

```bash
foonkeemoonkee@htb[/htb]$ sudo apt install apache2 -y
```

Para Apache2, para especificar a qué carpetas se puede acceder, podemos editar el archivo `/etc/apache2/apache2.conf` con un editor de texto. Este archivo contiene la configuración global. Podemos cambiar la configuración para especificar a qué directorios se puede acceder y qué acciones se pueden realizar en esos directorios.

### Configuración de Apache

```apacheconf
<Directory /var/www/html>
        Options Indexes FollowSymLinks
        AllowOverride All
        Require all granted
</Directory>
```

Esta sección especifica que la carpeta predeterminada `/var/www/html` es accesible, que los usuarios pueden usar las opciones `Indexes` y `FollowSymLinks`, que los cambios en los archivos de este directorio se pueden anular con `AllowOverride All`, y que `Require all granted` otorga acceso a todos los usuarios a este directorio. Por ejemplo, si queremos transferir archivos a uno de nuestros sistemas objetivo utilizando un servidor web, podemos poner los archivos apropiados en la carpeta `/var/www/html` y usar `wget` o `curl` u otras aplicaciones para descargar estos archivos en el sistema objetivo.

También es posible personalizar configuraciones individuales a nivel de directorio utilizando el archivo `.htaccess`, que podemos crear en el directorio en cuestión. Este archivo nos permite configurar ciertos ajustes a nivel de directorio, como controles de acceso, sin tener que personalizar el archivo de configuración de Apache. También podemos agregar módulos para obtener características como `mod_rewrite`, `mod_security` y `mod_ssl` que nos ayudan a mejorar la seguridad de nuestra aplicación web.

El servidor web de Python es una alternativa simple y rápida a Apache y se puede usar para alojar una sola carpeta con un solo comando para transferir archivos a otro sistema. Para instalar el servidor web de Python, necesitamos instalar Python3 en nuestro sistema y luego ejecutar el siguiente comando:

### Instalar Python y Servidor Web

```bash
foonkeemoonkee@htb[/htb]$ sudo apt install python3 -y
foonkeemoonkee@htb[/htb]$ python3 -m http.server
```

Cuando ejecutamos este comando, nuestro servidor web de Python se iniciará en el puerto `TCP/8000`, y podremos acceder a la carpeta en la que nos encontramos actualmente. También podemos alojar otra carpeta con el siguiente comando:

```bash
foonkeemoonkee@htb[/htb]$ python3 -m http.server --directory /home/cry0l1t3/target_files
```

Esto iniciará un servidor web de Python en el puerto `TCP/8000`, y podremos acceder a la carpeta `/home/cry0l1t3/target_files` desde el navegador, por ejemplo. Cuando accedemos a nuestro servidor web de Python, podemos transferir archivos al otro sistema escribiendo el enlace en nuestro navegador y descargando los archivos. También podemos alojar nuestro servidor web de Python en un puerto diferente al predeterminado:

```bash
foonkeemoonkee@htb[/htb]$ python3 -m http.server 443
```

Esto alojará nuestro servidor web de Python en el puerto `443` en lugar del puerto predeterminado `TCP/8000`. Podemos acceder a este servidor web escribiendo el enlace en nuestro navegador.

## VPN

Una Red Privada Virtual (VPN) funciona como un túnel seguro e invisible que nos conecta a otra red, permitiendo un acceso fluido y protegido como si estuviéramos físicamente presentes en ella. Esto se logra estableciendo un túnel cifrado entre el cliente y el servidor, asegurando que todos los datos transmitidos a través de esta conexión permanezcan confidenciales y protegidos contra accesos no autorizados.

Las organizaciones utilizan principalmente las VPN para otorgar a sus empleados acceso seguro a la red interna sin requerir que estén en el sitio. Esta flexibilidad permite a los empleados acceder a recursos y aplicaciones internas desde cualquier ubicación, mejorando la productividad y la movilidad. Además, las VPN sirven para anonimizar el tráfico de Internet y bloquear intrusiones externas, reforzando aún más la seguridad.

Entre las soluciones de VPN más utilizadas para servidores Linux se encuentran OpenVPN, L2TP/IPsec, PPTP, SSTP y SoftEther. OpenVPN se destaca como una opción popular de código abierto compatible con varios sistemas operativos, incluidos Ubuntu, Solaris y Red Hat Linux. Los administradores aprovechan OpenVPN para facilitar el acceso remoto seguro a las redes corporativas, cifrar el tráfico de red y mantener el anonimato de los usuarios en línea.

Para los pentesters, OpenVPN ofrece capacidades invaluables. Permite a los testers conectarse de forma segura a redes internas, especialmente cuando el acceso directo no es factible debido a restricciones geográficas. Al utilizar OpenVPN, los pentesters pueden realizar evaluaciones de seguridad exhaustivas de los sistemas internos, identificando y abordando posibles vulnerabilidades. La versatilidad de OpenVPN, con características como cifrado, túneles, modelado de tráfico, enrutamiento de red y adaptabilidad a entornos de red dinámicos, lo convierte en una herramienta esencial en el arsenal tanto de los administradores de red como de los profesionales de la seguridad. Podemos instalar el servidor y el cliente con el siguiente comando:

### Instalar OpenVPN

```bash
foonkeemoonkee@htb[/htb]$ sudo apt install openvpn -y
```

OpenVPN puede personalizarse y configurarse editando el archivo de configuración `/etc/openvpn/server.conf`. Este archivo contiene la configuración para el servidor OpenVPN. Podemos cambiar la configuración para configurar ciertas características como el cifrado, los túneles, el modelado de tráfico, etc.

Si queremos conectarnos a un servidor OpenVPN, podemos usar el archivo `.ovpn` que recibimos del servidor y guardarlo en nuestro sistema. Podemos hacer esto con el siguiente comando en la línea de comandos:

### Conectarse a la VPN

```bash
foonkeemoonkee@htb[/htb]$ sudo openvpn --config internal.ovpn
```
```
