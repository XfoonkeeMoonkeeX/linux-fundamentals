
```markdown
# Gestión de Servicios y Procesos

Los servicios, también conocidos como demonios (daemons), son componentes fundamentales de un sistema Linux que se ejecutan silenciosamente en segundo plano "sin interacción directa del usuario". Realizan tareas cruciales que mantienen el sistema operativo y proporcionan funcionalidades adicionales. Generalmente, los servicios se pueden clasificar en dos tipos:

### Servicios del Sistema

Estos son servicios internos necesarios durante el arranque del sistema. Realizan tareas esenciales relacionadas con el hardware e inicializan componentes del sistema necesarios para que el sistema operativo funcione correctamente. Son como el motor y los sistemas de transmisión de un coche. Se inician cuando giras la llave de contacto y son esenciales para que el coche funcione. Sin ellos, el coche no se movería.

### Servicios Instalados por el Usuario

Estos servicios son añadidos por los usuarios y típicamente incluyen aplicaciones de servidor y otros procesos en segundo plano que proporcionan características o capacidades específicas. Este tipo de servicios son como el aire acondicionado o el sistema de navegación GPS de un coche. Aunque no son críticos para que el coche funcione, mejoran la funcionalidad y proporcionan características adicionales según las preferencias del conductor.

Los demonios a menudo se identifican por la letra `d` al final de sus nombres de programa, como `sshd` (demonio de SSH) o `systemd`. Así como un coche depende tanto de sus componentes principales como de sus características opcionales para proporcionar una experiencia completa, un sistema Linux utiliza tanto los servicios del sistema como los instalados por el usuario para funcionar de manera eficiente y satisfacer las necesidades del usuario.

En general, hay solo algunos objetivos que tenemos cuando tratamos con un servicio o un proceso:

- Iniciar/Reiniciar un servicio/proceso
- Detener un servicio/proceso
- Ver qué está/estaba sucediendo con un servicio/proceso
- Habilitar/Deshabilitar un servicio/proceso en el arranque
- Encontrar un servicio/proceso

La mayoría de las distribuciones modernas de Linux han adoptado `systemd` como su sistema de inicialización (sistema init). Es el primer proceso que se inicia durante el proceso de arranque y se le asigna el ID de Proceso (PID). A todos los procesos en un sistema Linux se les asigna un PID y se pueden ver en el directorio `/proc/`, que contiene información sobre cada proceso. Los procesos también pueden tener un ID de Proceso Padre (PPID), lo que indica que fueron iniciados por otro proceso (el padre), convirtiéndolos en procesos hijos.

## Systemctl

Después de instalar OpenSSH en nuestra VM, podemos iniciar el servicio con el siguiente comando.

```bash
foonkeemoonkee@htb[/htb]$ systemctl start ssh
```

Después de haber iniciado el servicio, ahora podemos verificar si se ejecuta sin errores.

```bash
foonkeemoonkee@htb[/htb]$ systemctl status ssh

● ssh.service - OpenBSD Secure Shell server
   Loaded: loaded (/lib/systemd/system/ssh.service; enabled; vendor preset: enabled)
   Active: active (running) since Thu 2020-05-14 15:08:23 CEST; 24h ago
   Main PID: 846 (sshd)
   Tasks: 1 (limit: 4681)
   CGroup: /system.slice/ssh.service
           └─846 /usr/sbin/sshd -D

Mai 14 15:08:22 inlane systemd[1]: Starting OpenBSD Secure Shell server...
Mai 14 15:08:23 inlane sshd[846]: Server listening on 0.0.0.0 port 22.
Mai 14 15:08:23 inlane sshd[846]: Server listening on :: port 22.
Mai 14 15:08:23 inlane systemd[1]: Started OpenBSD Secure Shell server.
Mai 14 15:08:30 inlane systemd[1]: Reloading OpenBSD Secure Shell server.
Mai 14 15:08:31 inlane sshd[846]: Received SIGHUP; restarting.
Mai 14 15:08:31 inlane sshd[846]: Server listening on 0.0.0.0 port 22.
Mai 14 15:08:31 inlane sshd[846]: Server listening on :: port 22.
```

Para agregar OpenSSH al script SysV para indicarle al sistema que ejecute este servicio después del arranque, podemos vincularlo con el siguiente comando:

```bash
foonkeemoonkee@htb[/htb]$ systemctl enable ssh

Synchronizing state of ssh.service with SysV service script with /lib/systemd/systemd-sysv-install.
Executing: /lib/systemd/systemd-sysv-install enable ssh
```

Una vez que reiniciemos el sistema, el servidor OpenSSH se ejecutará automáticamente. Podemos verificar esto con una herramienta llamada `ps`.

```bash
foonkeemoonkee@htb[/htb]$ ps -aux | grep ssh

root       846  0.0  0.1  72300  5660 ?        Ss   Mai14   0:00 /usr/sbin/sshd -D
```

También podemos usar `systemctl` para listar todos los servicios.

```bash
foonkeemoonkee@htb[/htb]$ systemctl list-units --type=service

UNIT                                                       LOAD   ACTIVE SUB     DESCRIPTION              
accounts-daemon.service                                    loaded active running Accounts Service         
acpid.service                                              loaded active running ACPI event daemon        
apache2.service                                            loaded active running The Apache HTTP Server   
apparmor.service                                           loaded active exited  AppArmor initialization  
apport.service                                             loaded active exited  LSB: automatic crash repor
avahi-daemon.service                                       loaded active running Avahi mDNS/DNS-SD Stack  
bolt.service                                               loaded active running Thunderbolt system service
```

Es muy posible que los servicios no se inicien debido a un error. Para ver el problema, podemos usar la herramienta `journalctl` para ver los registros.

```bash
foonkeemoonkee@htb[/htb]$ journalctl -u ssh.service --no-pager

-- Logs begin at Wed 2020-05-13 17:30:52 CEST, end at Fri 2020-05-15 16:00:14 CEST. --
Mai 13 20:38:44 inlane systemd[1]: Starting OpenBSD Secure Shell server...
Mai 13 20:38:44 inlane sshd[2722]: Server listening on 0.0.0.0 port 22.
Mai 13 20:38:44 inlane sshd[2722]: Server listening on :: port 22.
Mai 13 20:38:44 inlane systemd[1]: Started OpenBSD Secure Shell server.
Mai 13 20:39:06 inlane sshd[3939]: Connection closed by 10.22.2.1 port 36444 [preauth]
Mai 13 20:39:27 inlane sshd[3942]: Accepted password for master from 10.22.2.1 port 36452 ssh2
Mai 13 20:39:27 inlane sshd[3942]: pam_unix(sshd:session): session opened for user master by (uid=0)
Mai 13 20:39:28 inlane sshd[3942]: pam_unix(sshd:session): session closed for user master
Mai 14 02:04:49 inlane sshd[2722]: Received signal 15; terminating.
Mai 14 02:04:49 inlane systemd[1]: Stopping OpenBSD Secure Shell server...
Mai 14 02:04:49 inlane systemd[1]: Stopped OpenBSD Secure Shell server.
-- Reboot --
```

## Matar un Proceso

Un proceso puede estar en los siguientes estados:

- **En ejecución** (Running)
- **En espera** (Waiting) (esperando un evento o un recurso del sistema)
- **Detenido** (Stopped)
- **Zombie** (detenido pero aún tiene una entrada en la tabla de procesos).

Los procesos pueden controlarse usando `kill`, `pkill`, `pgrep`, y `killall`. Para interactuar con un proceso, debemos enviarle una señal. Podemos ver todas las señales con el siguiente comando:

```bash
foonkeemoonkee@htb[/htb]$ kill -l

 1) SIGHUP       2) SIGINT       3) SIGQUIT      4) SIGILL       5) SIGTRAP
 2) SIGABRT      7) SIGBUS       8) SIGFPE       9) SIGKILL     10) SIGUSR1
3) SIGSEGV     12) SIGUSR2     13) SIGPIPE     14) SIGALRM     15) SIGTERM
4) SIGSTKFLT   17) SIGCHLD     18) SIGCONT     19) SIGSTOP     20) SIGTSTP
5) SIGTTIN     22) SIGTTOU     23) SIGURG      24) SIGXCPU     25) SIGXFSZ
6) SIGVTALRM   27) SIGPROF     28) SIGWINCH    29) SIGIO       30) SIGPWR
7) SIGSYS      34) SIGRTMIN    35) SIGRTMIN+1  36) SIGRTMIN+2  37) SIGRTMIN+3
8) SIGRTMIN+4  39) SIGRTMIN+5  40) SIGRTMIN+6  41) SIGRTMIN+7  42) SIGRTMIN+8
9) SIGRTMIN+9  44) SIGRTMIN+10 45) SIGRTMIN+11 46) SIGRTMIN+12 47) SIGRTMIN+13
10) SIGRTMIN+14 49) SIGRTMIN+15 50) SIGRTMAX-14 51) SIGRTMAX-13 52) SIGRTMAX-12
11) SIGRTMAX-11 54) SIGRTMAX-10 55) SIGRTMAX-9  56) SIGRTMAX-8  57) SIGRTMAX-7
12) SIGRTMAX-6  59) SIGRTMAX-5  60) SIGRTMAX-4  61) SIGRTMAX-3  62) SIGRTMAX-2
13) SIGRTMAX-1  64) SIGRTMAX
```

Las señales más utilizadas son:

| Señal | Descripción |
|---|---|
| 1 | **SIGHUP** - Se envía a un proceso cuando la terminal que lo controla se cierra. |
| 2 | **SIGINT** - Se envía cuando un usuario presiona `[Ctrl] + C` en la terminal de control para interrumpir un proceso. |
| 3 | **SIGQUIT** - Se envía cuando un usuario presiona `[Ctrl] + D` para salir. |
| 9 | **SIGKILL** - Mata inmediatamente un proceso sin operaciones de limpieza. |
| 15| **SIGTERM** - Terminación del programa. |
| 19| **SIGSTOP** - Detiene el programa. No se puede manejar más. |
| 20| **SIGTSTP** - Se envía cuando un usuario presiona `[Ctrl] + Z` para solicitar la suspensión de un servicio. El usuario puede manejarlo después. |

Por ejemplo, si un programa se congelara, podríamos forzar su finalización con el siguiente comando:

```bash
foonkeemoonkee@htb[/htb]$ kill 9 <PID> 
```

## Poner un Proceso en Segundo Plano

A veces será necesario poner el escaneo o proceso que acabamos de iniciar en segundo plano para continuar usando la sesión actual para interactuar con el sistema o iniciar otros procesos. Como ya hemos visto, podemos hacer esto con el atajo `[Ctrl + Z]`. Como se mencionó anteriormente, enviamos la señal **SIGTSTP** al kernel, que suspende el proceso.

```bash
foonkeemoonkee@htb[/htb]$ ping -c 10 www.hackthebox.eu

foonkeemoonkee@htb[/htb]$ vim tmpfile
[Ctrl + Z]
[2]+  Stopped                 vim tmpfile
```

Ahora todos los procesos en segundo plano se pueden mostrar con el siguiente comando.

```bash
foonkeemoonkee@htb[/htb]$ jobs

[1]+  Stopped                 ping -c 10 www.hackthebox.eu
[2]+  Stopped                 vim tmpfile
```

El atajo `[Ctrl] + Z` suspende los procesos, y no se ejecutarán más. Para mantenerlo funcionando en segundo plano, tenemos que introducir el comando `bg` para poner el proceso en segundo plano.

```bash
foonkeemoonkee@htb[/htb]$ bg

foonkeemoonkee@htb[/htb]$ 
--- www.hackthebox.eu ping statistics ---
10 packets transmitted, 0 received, 100% packet loss, time 113482ms

[ENTER]
[1]+  Exit 1                  ping -c 10 www.hackthebox.eu
```

Otra opción es poner automáticamente el proceso en segundo plano con un signo AND (`&`) al final del comando.

```bash
foonkeemoonkee@htb[/htb]$ ping -c 10 www.hackthebox.eu &

[1] 10825
PING www.hackthebox.eu (172.67.1.1) 56(84) bytes of data.
```

Una vez que el proceso termine, veremos los resultados.

```bash
foonkeemoonkee@htb[/htb]$ 

--- www.hackthebox.eu ping statistics ---
10 packets transmitted, 0 received, 100% packet loss, time 9210ms

[ENTER]
[1]+  Exit 1                  ping -c 10 www.hackthebox.eu
```

## Poner un Proceso en Primer Plano

Después de eso, podemos usar el comando `jobs` para listar todos los procesos en segundo plano. Los procesos en segundo plano no requieren interacción del usuario, y podemos usar la misma sesión de shell sin esperar a que el proceso termine primero. Una vez que el escaneo o proceso termine su trabajo, la terminal nos notificará que el proceso ha finalizado.

```bash
foonkeemoonkee@htb[/htb]$ jobs

[1]+  Running                 ping -c 10 www.hackthebox.eu &
```

Si queremos traer el proceso en segundo plano al primer plano e interactuar con él nuevamente, podemos usar el comando `fg <ID>`.

```bash
foonkeemoonkee@htb[/htb]$ fg 1
ping -c 10 www.hackthebox.eu

--- www.hackthebox.eu ping statistics ---
10 packets transmitted, 0 received, 100% packet loss, time 9206ms
```

## Ejecutar Múltiples Comandos

Hay tres posibilidades para ejecutar varios comandos, uno tras otro. Estos se separan por:

- Punto y coma (`;`)
- Doble ampersand (`&&`)
- Tuberías o pipes (`|`)

La diferencia entre ellos radica en el tratamiento de los procesos anteriores y depende de si el proceso anterior se completó con éxito o con errores. El punto y coma (`;`) es un separador de comandos y ejecuta los comandos ignorando los resultados y errores de los comandos anteriores.

```bash
foonkeemoonkee@htb[/htb]$ echo '1'; echo '2'; echo '3'

1
2
3
```

Por ejemplo, si ejecutamos el mismo comando pero reemplazamos en segundo lugar el comando `ls` con un archivo que no existe, obtenemos un error, y el tercer comando se ejecutará de todos modos.

```bash
foonkeemoonkee@htb[/htb]$ echo '1'; ls MISSING_FILE; echo '3'

1
ls: cannot access 'MISSING_FILE': No such file or directory
3
```

Sin embargo, la cosa cambia si usamos los dobles caracteres AND (`&&`) para ejecutar los comandos uno tras otro. Si hay un error en uno de los comandos, los siguientes ya no se ejecutarán, y todo el proceso se detendrá.

```bash
foonkeemoonkee@htb[/htb]$ echo '1' && ls MISSING_FILE && echo '3'

1
ls: cannot access 'MISSING_FILE': No such file or directory
```

Las tuberías (`|`) no solo dependen del funcionamiento correcto y sin errores de los procesos anteriores, sino también de los resultados de los procesos anteriores.

---

## Servidores VPN

**Advertencia:** Cada vez que "Cambias" (Switch), tus claves de conexión se regeneran y debes volver a descargar tu archivo de conexión VPN.

Todas las instancias de VM asociadas con el antiguo servidor VPN serán terminadas al cambiar a un nuevo servidor VPN.
Las instancias existentes de PwnBox cambiarán automáticamente al nuevo servidor VPN.

**PROTOCOLO**
- UDP 1337
- TCP 443

```
# Preguntas

¡Responde la(s) pregunta(s) a continuación para completar esta Sección y ganar cubos!

**Objetivo(s):** Los objetivos se están generando...

Conéctate por SSH con el usuario `htb-student` y la contraseña `HTB_@cademy_stdnt!`

---

### Pregunta 1

Usa el comando "systemctl" para listar todas las unidades de servicios y envía como respuesta el nombre de la unidad con la descripción "Load AppArmor profiles managed internally by snapd".

*(Original: Use the "systemctl" command to list all units of services and submit the unit name with the description "Load AppArmor profiles managed internally by snapd" as the answer.)*

**Solución:**

Para encontrar la respuesta, debes ejecutar el comando `systemctl list-units --type=service` y buscar la descripción solicitada. Para hacerlo más rápido, puedes usar `grep` para filtrar la salida:

```bash
systemctl list-units --type=service | grep "Load AppArmor profiles managed internally by snapd"
```

La salida te mostrará la línea del servicio correspondiente. El nombre de la unidad (`UNIT`) es la respuesta.

```
UNIT                          LOAD   ACTIVE SUB    DESCRIPTION
snapd.apparmor.service        loaded active exited Load AppArmor profiles managed internally by snapd
```

**Respuesta:**
`snapd.apparmor.service`

