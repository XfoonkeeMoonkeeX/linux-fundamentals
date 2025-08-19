
---

# Gestión de Paquetes

Ya sea trabajando como administrador de sistemas, manteniendo nuestras propias máquinas Linux en casa, o construyendo/actualizando/manteniendo nuestra distribución de pruebas de penetración preferida, es crucial tener un sólido conocimiento de los gestores de paquetes de Linux disponibles y las diversas formas de utilizarlos para instalar, actualizar o eliminar paquetes. Los paquetes son archivos que contienen binarios de software, archivos de configuración, información sobre dependencias y realizan un seguimiento de las actualizaciones y mejoras. Las características que la mayoría de los sistemas de gestión de paquetes proporcionan son:

*   Descarga de paquetes
*   Resolución de dependencias
*   Un formato de paquete binario estándar
*   Ubicaciones comunes de instalación y configuración
*   Configuración y funcionalidad adicional relacionada con el sistema
*   Control de calidad

Podemos usar muchos sistemas de gestión de paquetes diferentes que cubren distintos tipos de archivos como `.deb`, `.rpm`, y otros. El requisito para la gestión de paquetes es que el software a instalar esté disponible en su formato de paquete correspondiente. Típicamente, este es creado, ofrecido y mantenido de forma centralizada por las distribuciones de Linux. De esta manera, el software se integra directamente en el sistema y sus diversos directorios se distribuyen por todo el sistema. El software de gestión de paquetes obtiene los cambios necesarios para la instalación en el sistema desde el propio paquete y luego implementa estos cambios para instalar el paquete con éxito. Si el software de gestión de paquetes reconoce que se requieren paquetes adicionales para el correcto funcionamiento del paquete (y que aún no han sido instalados), se considera una dependencia y o bien advierte al administrador o intenta descargar el software faltante desde un repositorio, por ejemplo, e instalarlo por adelantado.

Si se desinstala un software, el sistema de gestión de paquetes recupera la información del paquete, la modifica según su configuración y elimina los archivos. Existen diferentes programas de gestión de paquetes que podemos usar para esto. Aquí hay una lista de ejemplos de dichos programas:

| Comando | Descripción |
| :--- | :--- |
| **`dpkg`** | `dpkg` es una herramienta para instalar, construir, eliminar y gestionar paquetes de Debian. La interfaz principal y más amigable para `dpkg` es `aptitude`. |
| **`apt`** | `Apt` proporciona una interfaz de línea de comandos de alto nivel para el sistema de gestión de paquetes. |
| **`aptitude`** | `Aptitude` es una alternativa a `apt` y es una interfaz de alto nivel para el gestor de paquetes. |
| **`snap`** | Instala, configura, actualiza y elimina paquetes snap. Los snaps permiten la distribución segura de las últimas aplicaciones y utilidades para la nube, servidores, escritorios e internet de las cosas. |
| **`gem`** | `Gem` es la interfaz para RubyGems, el gestor de paquetes estándar de Ruby. |
| **`pip`** | `Pip` es un instalador de paquetes de Python recomendado para instalar paquetes de Python que no están disponibles en el archivo de Debian. Puede trabajar con repositorios de control de versiones (actualmente solo repositorios Git, Mercurial y Bazaar), registra la salida de forma exhaustiva y previene instalaciones parciales al descargar todos los requisitos antes de iniciar la instalación. |
| **`git`** | `Git` es un sistema de control de revisiones distribuido, rápido y escalable con un conjunto de comandos inusualmente rico que proporciona tanto operaciones de alto nivel como acceso completo a sus componentes internos. |

Es muy recomendable configurar nuestra máquina virtual (VM) localmente para experimentar con ella. Experimentemos un poco en nuestra VM local y extendámosla con algunos paquetes adicionales. Primero, instalemos `git` usando `apt`.

¡Perfecto! Aquí tienes la traducción de la segunda parte, manteniendo el formato y la coherencia con la sección anterior.

---

## Gestor de Paquetes Avanzado (APT)

Las distribuciones de Linux basadas en Debian utilizan el gestor de paquetes APT. Un paquete es un archivo comprimido que contiene múltiples archivos `.deb`. La utilidad `dpkg` se utiliza para instalar programas desde el archivo `.deb` asociado. APT facilita la actualización e instalación de programas porque muchos programas tienen dependencias. Al instalar un programa desde un archivo `.deb` independiente, podemos encontrarnos con problemas de dependencias y necesitar descargar e instalar uno o varios paquetes adicionales. APT hace este proceso más fácil y eficiente al agrupar todas las dependencias necesarias para instalar un programa.

Cada distribución de Linux utiliza repositorios de software que se actualizan con frecuencia. Cuando actualizamos un programa o instalamos uno nuevo, el sistema consulta estos repositorios en busca del paquete deseado. Los repositorios pueden estar etiquetados como `stable` (estable), `testing` (de prueba) o `unstable` (inestable). La mayoría de las distribuciones de Linux utilizan el repositorio más estable o principal (`main`). Esto se puede comprobar revisando el contenido del archivo `/etc/apt/sources.list`. La lista de repositorios para Parrot OS se encuentra en `/etc/apt/sources.list.d/parrot.list`.

```bash
foonkeemoonkee@htb[/htb]$ cat /etc/apt/sources.list.d/parrot.list

# parrot repository
# this file was automatically generated by parrot-mirror-selector
deb http://htb.deb.parrot.sh/parrot/ rolling main contrib non-free
#deb-src https://deb.parrot.sh/parrot/ rolling main contrib non-free
deb http://htb.deb.parrot.sh/parrot/ rolling-security main contrib non-free
#deb-src https://deb.parrot.sh/parrot/ rolling-security main contrib non-free
```


---

APT utiliza una base de datos llamada caché de APT. Esta se utiliza para proporcionar información sobre los paquetes instalados en nuestro sistema de forma offline (sin conexión). Podemos buscar en la caché de APT, por ejemplo, para encontrar todos los paquetes relacionados con `Impacket`.

```bash
foonkeemoonkee@htb[/htb]$ apt-cache search impacket

impacket-scripts - Enlaces a ejemplos de scripts útiles de impacket
polenum - Extrae la política de contraseñas de un sistema Windows
python-pcapy - Interfaz de Python para la librería de captura de paquetes libpcap (Python 2)
python3-impacket - Módulo de Python3 para construir y analizar fácilmente protocolos de red
python3-pcapy - Interfaz de Python para la librería de captura de paquetes libpcap (Python 3)
```

También podemos ver información adicional sobre un paquete.

```bash
foonkeemoonkee@htb[/htb]$ apt-cache show impacket-scripts

Package: impacket-scripts
Version: 1.4
Architecture: all
Maintainer: Kali Developers <devel@kali.org>
Installed-Size: 13
Depends: python3-impacket (>= 0.9.20), python3-ldap3 (>= 2.5.0), python3-ldapdomaindump
Breaks: python-impacket (<< 0.9.18)
Replaces: python-impacket (<< 0.9.18)
Priority: optional
Section: misc
Filename: pool/main/i/impacket-scripts/impacket-scripts_1.4_all.deb
Size: 2080
<SNIP>
```

Además, podemos listar todos los paquetes instalados.

```bash
foonkeemoonkee@htb[/htb]$ apt list --installed

Listando... Hecho
accountsservice/rolling,now 0.6.55-2 amd64 [instalado, automático]
adapta-gtk-theme/rolling,now 3.95.0.11-1 all [instalado]
adduser/rolling,now 3.118 all [instalado]
adwaita-icon-theme/rolling,now 3.36.1-2 all [instalado, automático]
aircrack-ng/rolling,now 1:1.6-4 amd64 [instalado, automático]
<SNIP>
```

Si nos falta algún paquete, podemos buscarlo e instalarlo usando el siguiente comando.

```bash
foonkeemoonkee@htb[/htb]$ sudo apt install impacket-scripts -y

Leyendo las listas de paquetes... Hecho
Creando árbol de dependencias       
Leyendo la información de estado... Hecho
Se instalarán los siguientes paquetes NUEVOS:
  impacket-scripts
0 actualizados, 1 nuevos instalados, 0 para eliminar y 0 no actualizados.
Se necesita descargar 2.080 B de archivos.
Después de esta operación, se utilizarán 13,3 kB de espacio en disco adicional.
Des:1 https://euro2-emea-mirror.parrot.sh/mirrors/parrot rolling/main amd64 impacket-scripts all 1.4 [2.080 B]
Descargados 2.080 B en 0s (15,2 kB/s)
Seleccionando el paquete impacket-scripts no seleccionado previamente.
(Leyendo la base de datos... 378459 ficheros y directorios instalados actualmente.)
Preparando para desempaquetar .../impacket-scripts_1.4_all.deb ...
Desempaquetando impacket-scripts (1.4) ...
Configurando impacket-scripts (1.4) ...
Escaneando lanzadores de aplicaciones
Eliminando lanzadores duplicados de Debian
Los lanzadores están actualizados
```
De acuerdo, aquí tienes la siguiente sección traducida. He incluido un marcador de posición para la imagen como solicitaste.

---

## Git

Ahora que tenemos `git` instalado, podemos usarlo para descargar herramientas útiles desde Github. Uno de estos proyectos se llama 'Nishang'. Trabajaremos con el proyecto más adelante. Primero, debemos navegar al repositorio del proyecto y copiar el enlace de Github antes de usar `git` para descargarlo.

![[git-nishang.webp]]

Sin embargo, antes de descargar el proyecto con sus scripts y listas, deberíamos crear una carpeta específica.

```bash
foonkeemoonkee@htb[/htb]$ mkdir ~/nishang/ && git clone https://github.com/samratashok/nishang.git ~/nishang

Clonando en '/opt/nishang/'...
remoto: Enumerando objetos: 15, listo.
remoto: Contando objetos: 100% (15/15), listo.
remoto: Comprimiendo objetos: 100% (13/13), listo.
remoto: Total 1691 (delta 4), reutilizados 6 (delta 2), paquetes reutilizados 1676
Recibiendo objetos: 100% (1691/1691), 7.84 MiB | 4.86 MiB/s, listo.
Resolviendo deltas: 100% (1055/1055), listo.
```


---
## DPKG

También podemos descargar los programas y herramientas de los repositorios por separado. En este ejemplo, descargamos `strace` para Ubuntu 18.04 LTS.

```bash
foonkeemoonkee@htb[/htb]$ wget http://archive.ubuntu.com/ubuntu/pool/main/s/strace/strace_4.21-1ubuntu1_amd64.deb

--2020-05-15 03:27:17--  http://archive.ubuntu.com/ubuntu/pool/main/s/strace/strace_4.21-1ubuntu1_amd64.deb
Resolviendo archive.ubuntu.com (archive.ubuntu.com)... 91.189.88.142, 91.189.88.152, 2001:67c:1562::18, ...
Conectando con archive.ubuntu.com (archive.ubuntu.com)|91.189.88.142|:80... conectado.
Petición HTTP enviada, esperando respuesta... 200 OK
Longitud: 333388 (326K) [application/x-debian-package]
Guardando en: ‘strace_4.21-1ubuntu1_amd64.deb’

strace_4.21-1ubuntu1_amd64.deb       100%[===================================================================>] 325,57K  --.-KB/s    en 0,1s    

2020-05-15 03:27:18 (2,69 MB/s) - ‘strace_4.21-1ubuntu1_amd64.deb’ guardado [333388/333388]
```

Ahora, podemos usar tanto `apt` como `dpkg` para instalar el paquete. Como ya hemos trabajado con `apt`, en el siguiente ejemplo utilizaremos `dpkg`.

```bash
foonkeemoonkee@htb[/htb]$ sudo dpkg -i strace_4.21-1ubuntu1_amd64.deb 

(Leyendo la base de datos... 154680 ficheros y directorios instalados actualmente.)
Preparando para desempaquetar strace_4.21-1ubuntu1_amd64.deb ...
Desempaquetando strace (4.21-1ubuntu1) sobre (4.21-1ubuntu1) ...
Configurando strace (4.21-1ubuntu1) ...
Procesando disparadores para man-db (2.8.3-2ubuntu0.1) ...
```

Con esto, ya hemos instalado la herramienta y podemos probar si funciona correctamente.

```bash
foonkeemoonkee@htb[/htb]$ strace -h

uso: strace [-CdffhiqrtttTvVwxxy] [-I n] [-e expr]...
              [-a column] [-o file] [-s strsize] [-P path]...
              -p pid... / [-D] [-E var=val]... [-u username] PROG [ARGS]
   o: strace -c[dfw] [-I n] [-e expr]... [-O overhead] [-S sortby]
              -p pid... / [-D] [-E var=val]... [-u username] PROG [ARGS]

Formato de salida:
  -a column      alineación de COLUMNA para imprimir los resultados de la llamada al sistema (por defecto 40)
  -i             imprime el puntero de instrucción en el momento de la llamada al sistema

```

> **Ejercicio Opcional:**
>
> Busca la herramienta "evil-winrm" en Github e instálala en nuestras instancias interactivas. Prueba todos los métodos de instalación diferentes.