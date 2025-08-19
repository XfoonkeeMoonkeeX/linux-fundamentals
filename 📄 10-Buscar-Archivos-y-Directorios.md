# Linux Fundamentals - Página 10  
## Buscar Archivos y Directorios

Poder encontrar los archivos y carpetas que necesitamos es fundamental. Una vez que tenemos acceso a un sistema basado en Linux, será esencial encontrar archivos de configuración, scripts creados por usuarios o administradores, y otros archivos y directorios importantes.  
No necesitamos explorar cada carpeta manualmente ni revisar cuándo fue la última vez que se modificó. Existen herramientas que nos facilitan mucho esta tarea.

---

### 🔍 Comando `which`

Una de las herramientas más comunes es `which`. Esta herramienta devuelve la ruta del archivo o enlace que se ejecutaría si llamamos ese comando.  
Esto nos permite saber si ciertos programas (como `curl`, `netcat`, `wget`, `python`, `gcc`, etc.) están disponibles en el sistema.

```bash
foonkeemoonkee@htb[/htb]$ which python
/usr/bin/python

Si el programa que buscamos no existe, no se mostrará ningún resultado.
🔎 Comando find

Otra herramienta útil es find.
Además de buscar archivos y carpetas, esta herramienta permite aplicar filtros como:

    Tamaño

    Fecha de modificación

    Tipo de archivo (archivo o directorio)

    Dueño del archivo

Sintaxis:

find <ubicación> <opciones>

Ejemplo con múltiples filtros:

foonkeemoonkee@htb[/htb]$ find / -type f -name *.conf -user root -size +20k -newermt 2020-03-03 -exec ls -al {} \; 2>/dev/null

Resultado (recortado):

-rw-r--r-- 1 root root 136392 /usr/src/.../auto.conf
-rw-r--r-- 1 root root 82290  /usr/src/.../tristate.conf
-rw-r--r-- 1 root root 25086  /etc/dnsmasq.conf
-rw-r--r-- 1 root root 21254  /etc/sqlmap/sqlmap.conf
...

Explicación de las opciones:
Opción	Descripción
-type f	Busca archivos (no carpetas).
-name *.conf	Busca todos los archivos que terminan en .conf.
-user root	Muestra solo archivos cuyo dueño es root.
-size +20k	Solo archivos mayores a 20 KiB.
-newermt 2020-03-03	Solo archivos modificados después del 3 de marzo de 2020.
-exec ls -al {} \;	Ejecuta ls -al sobre cada archivo encontrado.
2>/dev/null	Oculta los mensajes de error del terminal (redirección de STDERR a null).
⚡ Comando locate

Buscar con find puede demorar mucho porque recorre todo el sistema.
Para búsquedas más rápidas, se puede usar locate, que trabaja con una base de datos local con información de todos los archivos y carpetas.

Primero, actualizamos la base de datos:

sudo updatedb

Luego, buscamos por archivos .conf:

locate *.conf

Ejemplo de salida:

/etc/GeoIP.conf
/etc/NetworkManager/NetworkManager.conf
/etc/UPower/UPower.conf
/etc/adduser.conf
...

⚠️ A diferencia de find, locate no permite aplicar filtros avanzados como tamaño, usuario o fecha.
Entonces, debes decidir cuál comando usar según lo que necesites buscar.
🧪 Ejercicio Opcional:

Prueba los comandos which, find y locate para buscar todo lo relacionado con la herramienta netcat o nc.

# Preguntas

**Responde las siguientes preguntas para completar esta sección y ganar cubos**

**Objetivo(s):** ¡Haz clic aquí para generar el sistema de destino!

Conéctate por SSH a con el usuario "htb-student" y la contraseña "HTB_@cademy_stdnt!"

1. **¿Cuál es el nombre del archivo de configuración que se ha creado después del 2020-03-03 y que es más pequeño de 28k pero más grande de 25k?**

 00-mesa-defaults.conf

2. **¿Cuántos archivos existen en el sistema con la extensión ".bak"?**
 
 4

3. **Envía la ruta completa del binario "xxd".**

 /usr/bin/xxd
