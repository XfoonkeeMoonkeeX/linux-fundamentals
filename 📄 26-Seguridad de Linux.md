

***
### **Seguridad de Linux**

Todos los sistemas informáticos tienen un riesgo inherente de intrusión. Algunos presentan un riesgo mayor que otros, como un servidor web expuesto a Internet que aloja múltiples aplicaciones web complejas. Los sistemas Linux también son menos propensos a los virus que afectan a los sistemas operativos Windows y no presentan una superficie de ataque tan grande como los anfitriones unidos a un dominio de Active Directory. A pesar de todo, es esencial tener ciertos fundamentos establecidos para asegurar cualquier sistema Linux.

Una de las medidas de seguridad más importantes de los sistemas operativos Linux es mantener el sistema operativo y los paquetes instalados actualizados. Esto se puede lograr con un comando como:

foonkeemoonkee@htb[/htb]$ apt update && apt dist-upgrade

Si las reglas del cortafuegos no están configuradas apropiadamente a nivel de red, podemos usar el cortafuegos de Linux y/o `iptables` para restringir el tráfico de entrada y salida del anfitrión.

Si SSH está abierto en el servidor, la configuración debe establecerse para no permitir el inicio de sesión con contraseña y no permitir que el usuario `root` inicie sesión a través de SSH. También es importante evitar iniciar sesión y administrar el sistema como el usuario `root` siempre que sea posible y gestionar adecuadamente el control de acceso. El acceso de los usuarios debe determinarse basándose en el principio de mínimo privilegio. Por ejemplo, si un usuario necesita ejecutar un comando como `root`, entonces ese comando debe especificarse en la configuración de `sudoers` en lugar de darle derechos completos de `sudo`. Otro mecanismo de protección común que se puede utilizar es `fail2ban`. Esta herramienta cuenta el número de intentos de inicio de sesión fallidos, y si un usuario ha alcanzado el número máximo, el anfitrión que intentó conectarse será tratado según lo configurado.

También es importante auditar periódicamente el sistema para asegurarse de que no existan problemas que puedan facilitar la escalada de privilegios, como un kernel desactualizado, problemas de permisos de usuario, archivos con permisos de escritura para todos (*world-writable*) y tareas cron mal configuradas, o servicios mal configurados. Muchos administradores olvidan la posibilidad de que algunas versiones del kernel deban actualizarse manualmente.

Una opción para asegurar aún más los sistemas Linux es Security-Enhanced Linux (SELinux) o AppArmor. Este es un módulo de seguridad del kernel que se puede utilizar para políticas de control de acceso de seguridad. En SELinux, a cada proceso, archivo, directorio y objeto del sistema se le asigna una etiqueta. Se crean reglas de política para controlar el acceso entre estos procesos y objetos etiquetados y son aplicadas por el kernel. Esto significa que el acceso se puede configurar para controlar qué usuarios y aplicaciones pueden acceder a qué recursos. SELinux proporciona controles de acceso muy granulares, como especificar quién puede añadir contenido a un archivo o moverlo.

Además, existen diferentes aplicaciones y servicios como Snort, chkrootkit, rkhunter, Lynis y otros que pueden contribuir a la seguridad de Linux. Adicionalmente, se deben realizar algunas configuraciones de seguridad, tales como:

*   Eliminar o deshabilitar todos los servicios y software innecesarios.
*   Eliminar todos los servicios que dependen de mecanismos de autenticación no cifrados.
*   Asegurar que NTP esté habilitado y que Syslog esté en ejecución.
*   Asegurar que cada usuario tenga su propia cuenta.
*   Forzar el uso de contraseñas seguras.
*   Configurar el envejecimiento de contraseñas y restringir el uso de contraseñas anteriores.
*   Bloquear las cuentas de usuario después de fallos de inicio de sesión.
*   Deshabilitar todos los binarios SUID/SGID no deseados.

Esta lista está incompleta, ya que la seguridad no es un producto sino un proceso. Esto significa que siempre se deben tomar medidas específicas para proteger mejor los sistemas, y depende de los administradores qué tan bien conozcan sus sistemas operativos. Cuanto mejor estén familiarizados los administradores con el sistema y cuanto más capacitados estén, mejores y más seguras serán sus precauciones y medidas de seguridad.

### **TCP Wrappers**

TCP wrapper es un mecanismo de seguridad utilizado en los sistemas Linux que permite al administrador del sistema controlar a qué servicios se les permite el acceso al sistema. Funciona restringiendo el acceso a ciertos servicios basándose en el nombre de host o la dirección IP del usuario que solicita el acceso. Cuando un cliente intenta conectarse a un servicio, el sistema primero consultará las reglas definidas en los archivos de configuración de TCP wrappers para determinar la dirección IP del cliente. Si la dirección IP coincide con los criterios especificados en los archivos de configuración, el sistema concederá al cliente acceso al servicio. Sin embargo, si no se cumplen los criterios, la conexión será denegada, proporcionando una capa adicional de seguridad para el servicio. Los TCP wrappers utilizan los siguientes archivos de configuración:

*   `/etc/hosts.allow`
*   `/etc/hosts.deny`

En resumen, el archivo `/etc/hosts.allow` especifica qué servicios y anfitriones tienen permitido el acceso al sistema, mientras que el archivo `/etc/hosts.deny` especifica qué servicios y anfitriones no tienen permitido el acceso. Estos archivos se pueden configurar añadiendo reglas específicas a los mismos.

**/etc/hosts.allow**
foonkeemoonkee@htb[/htb]$ cat /etc/hosts.allow
```
# Permitir acceso a SSH desde la red local
sshd : 10.129.14.0/24

# Permitir acceso a FTP desde un host específico
ftpd : 10.129.14.10

# Permitir acceso a Telnet desde cualquier host en el dominio inlanefreight.local
telnetd : .inlanefreight.local
```
**/etc/hosts.deny**
foonkeemoonkee@htb[/htb]$ cat /etc/hosts.deny
```
# Denegar acceso a todos los servicios desde cualquier host en el dominio inlanefreight.com
ALL : .inlanefreight.com

# Denegar acceso a SSH desde un host específico
sshd : 10.129.22.22

# Denegar acceso a FTP desde hosts con direcciones IP en el rango de 10.129.22.0 a 10.129.22.255
ftpd : 10.129.22.0/24
```
Es importante recordar que el orden de las reglas en los archivos es importante. La primera regla que coincida con el servicio y el anfitrión solicitados es la que se aplicará. También es importante señalar que los TCP wrappers no son un reemplazo de un cortafuegos, ya que están limitados por el hecho de que solo pueden controlar el acceso a los servicios y no a los puertos.