

***
### **Registros del Sistema (System Logs)**

Los registros del sistema en Linux son un conjunto de archivos que contienen información sobre el sistema y las actividades que tienen lugar en él. Estos registros son importantes para monitorear y solucionar problemas del sistema, ya que pueden proporcionar información sobre el comportamiento del sistema, la actividad de las aplicaciones y los eventos de seguridad. Estos registros del sistema también pueden ser una valiosa fuente de información para identificar posibles debilidades de seguridad y vulnerabilidades dentro de un sistema Linux. Al analizar los registros en nuestros sistemas objetivo, podemos obtener información sobre el comportamiento del sistema, la actividad de la red y la actividad del usuario, y podemos usar esta información para identificar cualquier actividad anormal, como inicios de sesión no autorizados, intentos de ataque, credenciales en texto claro o acceso inusual a archivos, lo que podría indicar una posible brecha de seguridad.

Nosotros, como pentesters, también podemos usar los registros del sistema para monitorear la efectividad de nuestras actividades de prueba de seguridad. Al revisar los registros después de realizar pruebas de seguridad, podemos determinar si nuestras actividades desencadenaron algún evento de seguridad, como alertas de detección de intrusos o advertencias del sistema. Esta información puede ayudarnos a refinar nuestras estrategias de prueba y mejorar la seguridad general del sistema.

Para garantizar la seguridad de un sistema Linux, es importante configurar los registros del sistema correctamente. Esto incluye establecer los niveles de registro apropiados, configurar la rotación de registros para evitar que los archivos de registro se vuelvan demasiado grandes y asegurarse de que los registros se almacenen de forma segura y estén protegidos contra el acceso no autorizado. Además, es importante revisar y analizar regularmente los registros para identificar posibles riesgos de seguridad y responder a cualquier evento de seguridad de manera oportuna. Existen varios tipos diferentes de registros del sistema en Linux, que incluyen:

*   Registros del Kernel
*   Registros del Sistema
*   Registros de Autenticación
*   Registros de Aplicaciones
*   Registros de Seguridad

#### **Registros del Kernel**

Estos registros contienen información sobre el kernel del sistema, incluidos los controladores de hardware, las llamadas al sistema y los eventos del kernel. Se almacenan en el archivo `/var/log/kern.log`. Por ejemplo, los registros del kernel pueden revelar la presencia de controladores vulnerables o desactualizados que podrían ser el objetivo de los atacantes para obtener acceso al sistema. También pueden proporcionar información sobre fallos del sistema, limitaciones de recursos y otros eventos que podrían conducir a una denegación de servicio u otros problemas de seguridad. Además, los registros del kernel pueden ayudarnos a identificar llamadas al sistema sospechosas u otras actividades que podrían indicar la presencia de malware u otro software malicioso en el sistema. Al monitorear el archivo `/var/log/kern.log`, podemos detectar cualquier comportamiento inusual y tomar las medidas apropiadas para prevenir daños mayores en el sistema.

#### **Registros del Sistema**

Estos registros contienen información sobre eventos a nivel de sistema, como el inicio y la detención de servicios, intentos de inicio de sesión y reinicios del sistema. Se almacenan en el archivo `/var/log/syslog`. Al analizar los intentos de inicio de sesión, el inicio y la detención de servicios y otros eventos a nivel de sistema, podemos detectar cualquier posible acceso o actividad en el sistema. Esto puede ayudarnos a identificar cualquier vulnerabilidad que pueda ser explotada y ayudarnos a recomendar medidas de seguridad para mitigar estos riesgos. Además, podemos usar el syslog para identificar problemas potenciales que podrían afectar la disponibilidad o el rendimiento del sistema, como inicios de servicio fallidos o reinicios del sistema. Aquí hay un ejemplo de cómo podría verse un archivo syslog:

**Syslog**
```
Feb 28 2023 15:00:01 server CRON[2715]: (root) CMD (/usr/local/bin/backup.sh)
Feb 28 2023 15:04:22 server sshd[3010]: Failed password for htb-student from 10.14.15.2 port 50223 ssh2
Feb 28 2023 15:05:02 server kernel: [  138.303596] ata3.00: exception Emask 0x0 SAct 0x0 SErr 0x0 action 0x6 frozen
Feb 28 2023 15:06:43 server apache2[2904]: 127.0.0.1 - - [28/Feb/2023:15:06:43 +0000] "GET /index.html HTTP/1.1" 200 13484 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/80.0.3987.149 Safari/537.36"
Feb 28 2023 15:07:19 server sshd[3010]: Accepted password for htb-student from 10.14.15.2 port 50223 ssh2
Feb 28 2023 15:09:54 server kernel: [  367.543975] EXT4-fs (sda1): re-mounted. Opts: errors=remount-ro
Feb 28 2023 15:12:07 server systemd[1]: Started Clean PHP session files.
```

#### **Registros de Autenticación**

Estos registros contienen información sobre los intentos de autenticación de los usuarios, incluidos los intentos exitosos y fallidos. Se almacenan en el archivo `/var/log/auth.log`. Es importante tener en cuenta que, si bien el archivo `/var/log/syslog` puede contener información de inicio de sesión similar, el archivo `/var/log/auth.log` se centra específicamente en los intentos de autenticación de los usuarios, lo que lo convierte en un recurso más valioso para identificar posibles amenazas de seguridad. Por lo tanto, es esencial que los pentesters revisen los registros almacenados en el archivo `/var/log/auth.log` para asegurarse de que el sistema esté seguro y no haya sido comprometido.

**Auth.log**
```
Feb 28 2023 18:15:01 sshd[5678]: Accepted publickey for admin from 10.14.15.2 port 43210 ssh2: RSA SHA256:+KjEzN2cVhIW/5uJpVX9n5OB5zVJ92FtCZxVzzcKjw
Feb 28 2023 18:15:03 sudo:   admin : TTY=pts/1 ; PWD=/home/admin ; USER=root ; COMMAND=/bin/bash
Feb 28 2023 18:15:05 sudo:   admin : TTY=pts/1 ; PWD=/home/admin ; USER=root ; COMMAND=/usr/bin/apt-get install netcat-traditional
Feb 28 2023 18:15:08 sshd[5678]: Disconnected from 10.14.15.2 port 43210 [preauth]
Feb 28 2023 18:15:12 kernel: [  778.941871] firewall: unexpected traffic allowed on port 22
Feb 28 2023 18:15:15 auditd[9876]: Audit daemon started successfully
Feb 28 2023 18:15:18 systemd-logind[1234]: New session 4321 of user admin.
Feb 28 2023 18:15:21 CRON[2345]: pam_unix(cron:session): session opened for user root by (uid=0)
Feb 28 2023 18:15:24 CRON[2345]: pam_unix(cron:session): session closed for user root
```
En este ejemplo, podemos ver en la primera línea que se ha utilizado una clave pública exitosa para la autenticación del usuario `admin`. Además, podemos ver que este usuario está en el grupo `sudoers` porque puede ejecutar comandos usando `sudo`. El mensaje del kernel indica que se permitió tráfico inesperado en el puerto 22, lo que podría indicar una posible brecha de seguridad. Después de eso, vemos que se creó una nueva sesión para el usuario "admin" por `systemd-logind` y que una sesión de cron se abrió y cerró para el usuario `root`.

#### **Registros de Aplicaciones**

Estos registros contienen información sobre las actividades de aplicaciones específicas que se ejecutan en el sistema. A menudo se almacenan en sus propios archivos, como `/var/log/apache2/error.log` para el servidor web Apache o `/var/log/mysql/error.log` para el servidor de base de datos MySQL. Estos registros son particularmente importantes cuando estamos atacando aplicaciones específicas, como servidores web o bases de datos, ya que pueden proporcionar información sobre cómo estas aplicaciones procesan y manejan los datos. Al examinar estos registros, podemos identificar posibles vulnerabilidades o configuraciones incorrectas. Por ejemplo, los registros de acceso se pueden usar para rastrear las solicitudes realizadas a un servidor web, mientras que los registros de auditoría se pueden usar para rastrear los cambios realizados en el sistema o en archivos específicos. Estos registros se pueden usar para identificar intentos de acceso no autorizados, exfiltración de datos u otra actividad sospechosa.

Además, los registros de acceso y auditoría son registros críticos que registran información sobre las acciones de los usuarios y procesos en el sistema. Son cruciales para fines de seguridad y cumplimiento, y podemos usarlos para identificar posibles problemas de seguridad y vectores de ataque.

Por ejemplo, los registros de acceso mantienen un registro de la actividad de los usuarios y procesos en el sistema, incluidos los intentos de inicio de sesión, los accesos a archivos y las conexiones de red. Los registros de auditoría registran información sobre eventos relevantes para la seguridad en el sistema, como modificaciones en los archivos de configuración del sistema o intentos de modificar archivos o configuraciones del sistema. Estos registros ayudan a rastrear posibles ataques y actividades o a identificar brechas de seguridad u otros problemas. Una entrada de ejemplo en un archivo de registro de acceso puede verse así:

**Entrada de Registro de Acceso**
```
2023-03-07T10:15:23+00:00 servername privileged.sh: htb-student accessed /root/hidden/api-keys.txt
```
En esta entrada de registro, podemos ver que el usuario `htb-student` usó el script `privileged.sh` para acceder al archivo `api-keys.txt` en el directorio `/root/hidden/`. En los sistemas Linux, los servicios más comunes tienen ubicaciones predeterminadas para los registros de acceso:

| Servicio | Descripción |
| :--- | :--- |
| **Apache** | Los registros de acceso se almacenan en el archivo `/var/log/apache2/access.log` (o similar, dependiendo de la distribución). |
| **Nginx** | Los registros de acceso se almacenan en el archivo `/var/log/nginx/access.log` (o similar). |
| **OpenSSH** | Los registros de acceso se almacenan en el archivo `/var/log/auth.log` en Ubuntu y en `/var/log/secure` en CentOS/RHEL. |
| **MySQL** | Los registros de acceso se almacenan en el archivo `/var/log/mysql/mysql.log`. |
| **PostgreSQL**| Los registros de acceso se almacenan en el archivo `/var/log/postgresql/postgresql-version-main.log`. |
| **Systemd** | Los registros de acceso se almacenan en el directorio `/var/log/journal/`. |

#### **Registros de Seguridad**

Estos registros de seguridad y sus eventos a menudo se registran en una variedad de archivos de registro, dependiendo de la aplicación o herramienta de seguridad específica en uso. Por ejemplo, la aplicación Fail2ban registra los intentos de inicio de sesión fallidos en el archivo `/var/log/fail2ban.log`, mientras que el cortafuegos UFW registra la actividad en el archivo `/var/log/ufw.log`. Otros eventos relacionados con la seguridad, como cambios en los archivos o configuraciones del sistema, pueden registrarse en registros del sistema más generales como `/var/log/syslog` o `/var/log/auth.log`. Como pentesters, podemos usar herramientas y técnicas de análisis de registros para buscar eventos o patrones de actividad específicos que puedan indicar un problema de seguridad y usar esa información para probar más a fondo el sistema en busca de vulnerabilidades o posibles vectores de ataque.

Es importante estar familiarizado con las ubicaciones predeterminadas de los registros de acceso y otros archivos de registro en los sistemas Linux, ya que esta información puede ser útil al realizar una evaluación de seguridad o una prueba de penetración. Al comprender cómo se registran y almacenan los eventos relacionados con la seguridad, podemos analizar los datos de los registros de manera más efectiva e identificar posibles problemas de seguridad.

Todos estos registros se pueden acceder y analizar utilizando una variedad de herramientas, incluidos los visores de archivos de registro integrados en la mayoría de los entornos de escritorio de Linux, así como herramientas de línea de comandos como los comandos `tail`, `grep` y `sed`. Un análisis adecuado de los registros del sistema puede ayudar a identificar y solucionar problemas del sistema, así como a detectar brechas de seguridad y otros eventos de interés.