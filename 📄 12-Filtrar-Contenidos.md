### Filtrar Contenidos

En la sección anterior, exploramos cómo usar la redirección para enviar la salida de un programa a otro para su procesamiento posterior. Ahora, vamos a hablar sobre cómo leer archivos directamente desde la línea de comandos, sin necesidad de abrir un editor de texto.

Hay dos herramientas poderosas para esto: **more** y **less**. Estas se conocen como _pagers_ y te permiten ver el contenido de un archivo de manera interactiva, una pantalla a la vez. Aunque ambas herramientas cumplen una función similar, tienen algunas diferencias en su funcionalidad, de las cuales hablaremos más adelante.

Usando **more** y **less**, puedes desplazarte fácilmente a través de archivos grandes, buscar texto y navegar hacia adelante o hacia atrás sin modificar el archivo en sí. Esto es especialmente útil cuando trabajas con archivos de registro o archivos de texto que no caben bien en una sola pantalla.

El objetivo de esta sección es aprender cómo filtrar el contenido y manejar la salida redirigida de los comandos anteriores. Pero antes de sumergirnos en el filtrado, necesitamos familiarizarnos con algunas herramientas y comandos esenciales que están diseñados específicamente para hacer que el filtrado sea más eficiente y poderoso.

Antes de comenzar a filtrar la salida de los comandos, exploremos algunas herramientas fundamentales que te ayudarán a filtrar y manipular el texto de manera eficiente. Estas herramientas son cruciales cuando trabajas con grandes cantidades de datos o cuando necesitas automatizar tareas que impliquen la búsqueda, ordenación o procesamiento de información.

Veamos algunos ejemplos para entender cómo funcionan estas herramientas en la práctica.

## More

```bash
foonkeemoonkee@htb[/htb]$ cat /etc/passwd | more
```

El archivo **/etc/passwd** en Linux es como una guía telefónica para los usuarios en el sistema. Incluye detalles como el nombre de usuario, el ID de usuario, el ID de grupo, el directorio home y el shell predeterminado que utilizan.

Después de leer el contenido con `cat` y redirigirlo a `more`, se abre el *pager* mencionado, y automáticamente comenzamos desde el principio del archivo.

**Filtrar Contenidos**

```
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
<SNIP>
--More--
```

Con la tecla **[Q]**, podemos salir de este *pager*. Notaremos que la salida permanece en la terminal.

## Less

Si ahora echamos un vistazo a la herramienta `less`, notaremos en la página del manual que tiene muchas más características que `more`.

### Filtrar Contenidos

```bash
foonkeemoonkee@htb[/htb]$ less /etc/passwd

La presentación es casi la misma que con more.
Filtrar Contenidos

root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
<SNIP>
:

Cuando cerramos less con la tecla [Q], notaremos que la salida que hemos visto, a diferencia de more, no permanece en la terminal.

HEAD

A veces solo estaremos interesados en ciertos aspectos, ya sea al **comienzo** o al **final** de un archivo.  
Si solo queremos obtener las primeras líneas de un archivo, podemos usar la herramienta `head`.  
Por defecto, `head` imprime las **primeras diez líneas** del archivo o entrada proporcionada, a menos que se indique lo contrario.

### Filtrar Contenidos

```bash
foonkeemoonkee@htb[/htb]$ head /etc/passwd

root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
games:x:5:60:games:/usr/games:/usr/sbin/nologin
man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin
mail:x:8:8:mail:/var/mail:/usr/sbin/nologin
news:x:9:9:news:/var/spool/news:/usr/sbin/nologin

## Tail

Si solo queremos ver las últimas partes de un archivo o resultados, podemos usar el complemento de `head` llamado `tail`, el cual devuelve las **últimas diez líneas**.

### Filtrar Contenidos

```bash
foonkeemoonkee@htb[/htb]$ tail /etc/passwd

miredo:x:115:65534::/var/run/miredo:/usr/sbin/nologin
usbmux:x:116:46:usbmux daemon,,,:/var/lib/usbmux:/usr/sbin/nologin
rtkit:x:117:119:RealtimeKit,,,:/proc:/usr/sbin/nologin
nm-openvpn:x:118:120:NetworkManager OpenVPN,,,:/var/lib/openvpn/chroot:/usr/sbin/nologin
nm-openconnect:x:119:121:NetworkManager OpenConnect plugin,,,:/var/lib/NetworkManager:/usr/sbin/nologin
pulse:x:120:122:PulseAudio daemon,,,:/var/run/pulse:/usr/sbin/nologin
beef-xss:x:121:124::/var/lib/beef-xss:/usr/sbin/nologin
lightdm:x:122:125:Light Display Manager:/var/lib/lightdm:/bin/false
do-agent:x:998:998::/home/do-agent:/bin/false
user6:x:1000:1000:,,,:/home/user6:/bin/bash

Sería muy útil explorar las opciones disponibles que ofrecen estas herramientas y experimentar con ellas.

Sort

Dependiendo de los resultados y archivos con los que estemos trabajando, rara vez están ordenados.  
A menudo es necesario ordenar los resultados deseados **alfabéticamente** o **numéricamente** para obtener una mejor visión general.  
Para ello, podemos usar una herramienta llamada `sort`.

### Filtrar Contenidos

```bash
foonkeemoonkee@htb[/htb]$ cat /etc/passwd | sort

_apt:x:104:65534::/nonexistent:/usr/sbin/nologin
backup:x:34:34:backup:/var/backups:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
cry0l1t3:x:1001:1001::/home/cry0l1t3:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
dnsmasq:x:107:65534:dnsmasq,,,:/var/lib/misc:/usr/sbin/nologin
dovecot:x:114:117:Dovecot mail server,,,:/usr/lib/dovecot:/usr/sbin/nologin
dovenull:x:115:118:Dovecot login user,,,:/nonexistent:/usr/sbin/nologin
ftp:x:113:65534::/srv/ftp:/usr/sbin/nologin
games:x:5:60:games:/usr/games:/usr/sbin/nologin
gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/usr/sbin/nologin
htb-student:x:1002:1002::/home/htb-student:/bin/bash
<SNIP>

Como podemos ver ahora, la salida ya no comienza con root, sino que está ordenada alfabéticamente.

## Grep

En muchos casos necesitaremos buscar resultados específicos que coincidan con patrones que definimos.  
Una de las herramientas más utilizadas para este propósito es `grep`, que ofrece una amplia gama de funcionalidades potentes para buscar patrones.  

Por ejemplo, podemos usar `grep` para buscar usuarios que tienen su shell predeterminado configurado como `/bin/bash`.

### Filtrar Contenidos

```bash
foonkeemoonkee@htb[/htb]$ cat /etc/passwd | grep "/bin/bash"

root:x:0:0:root:/root:/bin/bash
mrb3n:x:1000:1000:mrb3n:/home/mrb3n:/bin/bash
cry0l1t3:x:1001:1001::/home/cry0l1t3:/bin/bash
htb-student:x:1002:1002::/home/htb-student:/bin/bash

Este es solo un ejemplo de cómo grep puede aplicarse para filtrar datos de manera eficiente según patrones predefinidos.

Otra posibilidad es excluir resultados específicos. Para esto se usa la opción -v con grep.
En el siguiente ejemplo, se excluyen todos los usuarios que tienen deshabilitada la terminal estándar con los nombres /bin/false o /usr/sbin/nologin.
Filtrar Contenidos

foonkeemoonkee@htb[/htb]$ cat /etc/passwd | grep -v "false\\|nologin"

root:x:0:0:root:/root:/bin/bash
sync:x:4:65534:sync:/bin:/bin/sync
postgres:x:111:117:PostgreSQL administrator,,,:/var/lib/postgresql:/bin/bash
user6:x:1000:1000:,,,:/home/user6:/bin/bash
 
Cut

Resultados específicos pueden estar separados por distintos caracteres utilizados como **delimitadores**.  
En estos casos, es útil saber cómo eliminar ciertos delimitadores y mostrar las palabras de una línea según una **posición específica**.  

Una de las herramientas que podemos usar para esto es `cut`.  
Para ello usamos la opción `-d` para establecer el delimitador (en este caso, el carácter dos puntos `:`),  
y luego usamos la opción `-f` para definir qué **campo** (posición) de la línea queremos mostrar.

### Filtrar Contenidos

```bash
foonkeemoonkee@htb[/htb]$ cat /etc/passwd | grep -v "false\\|nologin" | cut -d":" -f1

root
sync
postgres
mrb3n
cry0l1t3
htb-student

Tr

Otra posibilidad para reemplazar ciertos caracteres de una línea por caracteres definidos por nosotros es la herramienta `tr`.  
Como primera opción, definimos **qué carácter queremos reemplazar**, y como segunda opción, definimos **con qué carácter** lo queremos reemplazar.  

En el siguiente ejemplo, reemplazamos el carácter **dos puntos (:)** por un **espacio**.

### Filtrar Contenidos

```bash
foonkeemoonkee@htb[/htb]$ cat /etc/passwd | grep -v "false\\|nologin" | tr ":" " "

root x 0 0 root /root /bin/bash
sync x 4 65534 sync /bin /bin/sync
postgres x 111 117 PostgreSQL administrator,,, /var/lib/postgresql /bin/bash
mrb3n x 1000 1000 mrb3n /home/mrb3n /bin/bash
cry0l1t3 x 1001 1001  /home/cry0l1t3 /bin/bash
htb-student x 1002 1002  /home/htb-student /bin/bash

Column

Dado que los resultados de búsqueda a menudo pueden tener una representación poco clara, la herramienta `column` es muy adecuada para mostrar tales resultados en **forma tabular** utilizando la opción `-t`.

### Filtrar Contenidos

```bash
foonkeemoonkee@htb[/htb]$ cat /etc/passwd | grep -v "false\\|nologin" | tr ":" " " | column -t

root         x  0     0      root               /root        		 /bin/bash
sync         x  4     65534  sync               /bin         		 /bin/sync
postgres     x  111   117    PostgreSQL         administrator,,,    /var/lib/postgresql		/bin/bash
mrb3n        x  1000  1000   mrb3n              /home/mrb3n  	     /bin/bash
cry0l1t3     x  1001  1001   /home/cry0l1t3     /bin/bash
htb-student  x  1002  1002   /home/htb-student  /bin/bash

Awk

Como hemos notado, la línea para el usuario "postgres" tiene una columna de más.  
Para hacer más sencillo ordenar tales resultados, la programación (g)awk es útil, ya que nos permite mostrar el **primer** (`$1`) y **último** (`$NF`) resultado de la línea.

### Filtrar Contenidos

```bash
foonkeemoonkee@htb[/htb]$ cat /etc/passwd | grep -v "false\\|nologin" | tr ":" " " | awk '{print $1, $NF}'

root /bin/bash
sync /bin/sync
postgres /bin/bash
mrb3n /bin/bash
cry0l1t3 /bin/bash
htb-student /bin/bash


Sed

Habrá momentos en los que querremos cambiar nombres específicos en todo el archivo o entrada estándar.  
Una de las herramientas que podemos usar para esto es el editor de flujo llamado `sed`.  
Uno de los usos más comunes de esta herramienta es **sustituir texto**.  
En este caso, `sed` busca los patrones que hemos definido en forma de **expresiones regulares** (regex) y los reemplaza por otro patrón que también hemos definido.

Tomemos los últimos resultados y digamos que queremos reemplazar la palabra "bin" por "HTB".

El flag **"s"** al principio representa el comando de sustitución.  
Luego especificamos el patrón que queremos reemplazar.  
Después de la barra (/), introducimos el patrón que queremos usar como reemplazo en la tercera posición.  
Finalmente, usamos el flag **"g"**, que significa reemplazar todas las coincidencias.

### Filtrar Contenidos

```bash
foonkeemoonkee@htb[/htb]$ cat /etc/passwd | grep -v "false\\|nologin" | tr ":" " " | awk '{print $1, $NF}' | sed 's/bin/HTB/g'

root /HTB/bash
sync /HTB/sync
postgres /HTB/bash
mrb3n /HTB/bash
cry0l1t3 /HTB/bash
htb-student /HTB/bash


## Wc

Por último, pero no menos importante, a menudo será útil saber cuántas coincidencias exitosas hemos tenido.  
Para evitar contar las líneas o caracteres manualmente, podemos usar la herramienta `wc`.  
Con la opción **"-l"**, especificamos que solo se cuenten las líneas.

### Filtrar Contenidos

```bash
foonkeemoonkee@htb[/htb]$ cat /etc/passwd | grep -v "false\\|nologin" | tr ":" " " | awk '{print $1, $NF}' | wc -l

# Práctica

Ten en cuenta que existen muchas otras herramientas que puedes utilizar e incorporar a lo largo de tu recorrido. Se recomienda encarecidamente explorar herramientas alternativas para tareas específicas para ampliar tu conjunto de habilidades, ya que podrías descubrir opciones que se adapten mejor a tus preferencias personales y flujos de trabajo. No hay limitaciones estrictas, así que siéntete libre de explorar diferentes posibilidades y aprovechar los recursos que la comunidad comparte.

Al principio, puede ser un poco abrumador lidiar con tantas herramientas diferentes y sus funciones si no estamos familiarizados con ellas. Tómate tu tiempo y experimenta con las herramientas. Echa un vistazo a las páginas de manual (man <herramienta>) o llama a la ayuda para ella (<herramienta> -h / <herramienta> --help). La mejor manera de familiarizarse con todas las herramientas es practicando. Trata de usarlas tan a menudo como sea posible, y después de un corto período de tiempo, podremos filtrar muchas cosas de manera intuitiva.

## Ejercicios opcionales

A continuación, se presentan algunos ejercicios opcionales que podemos utilizar para mejorar nuestras habilidades de filtrado y familiarizarnos más con la terminal y los comandos. El archivo con el que necesitaremos trabajar es el archivo `/etc/passwd` en nuestro objetivo y podemos usar cualquiera de los comandos mostrados anteriormente. Nuestro objetivo es filtrar y mostrar solo contenidos específicos. Lee el archivo y filtra su contenido de tal manera que veamos solo:

1. Una línea con el nombre de usuario `cry0l1t3`.
2. Los nombres de usuario.
3. El nombre de usuario `cry0l1t3` y su UID.
4. El nombre de usuario `cry0l1t3` y su UID separados por una coma (,).
5. El nombre de usuario `cry0l1t3`, su UID y el shell configurado separados por una coma (,).
6. Todos los nombres de usuario con su UID y shells configurados, separados por una coma (,).
7. Todos los nombres de usuario con su UID y shells configurados, separados por una coma (,), excluyendo los que contienen `nologin` o `false`.
8. Todos los nombres de usuario con su UID y shells configurados, separados por una coma (,), excluyendo los que contienen `nologin` y contando todas las líneas de la salida filtrada.

# Preguntas

Responde a las siguientes preguntas para completar esta sección y ganar cubos.

## Objetivo(s): 

Haz clic aquí para generar el sistema objetivo.

1. **¿Cuántos servicios están escuchando en el sistema objetivo en todas las interfaces? (No en localhost y solo IPv4)**

   + 7

2. **Determina bajo qué usuario se está ejecutando el servidor ProFTPd. Envía el nombre de usuario como respuesta.**

   + proftpd

3. **Usa cURL desde tu Pwnbox (no desde la máquina objetivo) para obtener el código fuente del sitio web "https://www.inlanefreight.com" y filtra todas las rutas únicas (por ejemplo, "https://www.inlanefreight.com/directory" o "/another/directory") de ese dominio. Envía el número de estas rutas como respuesta.**

   + 34