
# 🧱 Linux Structure

Linux es un sistema operativo utilizado en computadores personales, servidores e incluso dispositivos móviles. En ciberseguridad, destaca por su **robustez, flexibilidad** y por ser **código abierto**.

En este módulo aprenderás sobre la estructura general de Linux: su historia, filosofía, arquitectura y jerarquía del sistema de archivos. Este conocimiento es esencial para cualquier profesional de la ciberseguridad.

---

## 🧠 ¿Qué es Linux?

Linux es un sistema operativo (SO), como Windows, macOS, iOS o Android. Su función es **administrar los recursos del hardware** y permitir la comunicación entre el sistema y las aplicaciones.

A diferencia de otros sistemas, Linux tiene muchas variantes conocidas como **distribuciones** o *distros*. Ejemplos:
- Ubuntu
- Debian
- Fedora
- OpenSUSE
- Manjaro
- Linux Mint
- Red Hat
- Gentoo
- elementary OS

---

## 🕰️ Historia de Linux

- **1970**: Unix es creado por Ken Thompson y Dennis Ritchie en AT&T.
- **1977**: Se publica BSD, pero una demanda limita su desarrollo.
- **1983**: Richard Stallman inicia el proyecto GNU y lanza la licencia **GPL** (General Public License).
- **1991**: Linus Torvalds crea el **núcleo Linux (kernel)** como proyecto personal.

Desde entonces, el núcleo ha crecido a más de **23 millones de líneas de código** (sin contar comentarios). Linux está licenciado bajo **GPL v2**.

---

## 🔐 Seguridad y uso

- Linux es más seguro que otros SO y menos vulnerable a malware.
- Es **estable**, **rápido** y **frecuentemente actualizado**.
- Tiene menos soporte de hardware que Windows y puede ser más difícil para principiantes.

Linux es libre y de código abierto: cualquiera puede modificarlo y distribuirlo.  
Se usa en servidores, desktops, routers, consolas, televisores y más.  
Android se basa en el núcleo Linux, por lo tanto, **Linux es el sistema más instalado del mundo**.

---

## 🧪 Nuestro entorno: Parrot OS

Usaremos **Pwnbox**, una versión de **Parrot OS**, que es una distribución basada en Debian enfocada en:
- Seguridad
- Privacidad
- Desarrollo

---

## 🧘 Filosofía de Linux

Linux promueve la **simplicidad**, **modularidad** y **transparencia**. Sus principios clave:

| Principio | Descripción |
|----------|-------------|
| Todo es un archivo | Configuraciones y servicios se almacenan en archivos de texto. |
| Programas pequeños | Herramientas que cumplen una única función bien definida. |
| Composición de programas | Se pueden encadenar comandos para tareas complejas. |
| Sin interfaces cautivas | El terminal da mayor control al usuario. |
| Configuración en texto plano | Ej.: `/etc/passwd` contiene los usuarios del sistema. |

---

## 🧩 Componentes de Linux

| Componente | Descripción |
|------------|-------------|
| Bootloader | Inicia el sistema operativo (ej. GRUB). |
| Kernel | Núcleo que gestiona los recursos del hardware. |
| Daemons | Servicios que corren en segundo plano. |
| Shell | Interfaz CLI entre usuario y sistema (ej. Bash, Zsh). |
| Servidor gráfico | X-Server, permite ejecutar entornos gráficos. |
| Administrador de ventanas (GUI) | Entorno gráfico (GNOME, KDE, MATE, etc). |
| Utilidades | Programas que ejecutan funciones específicas. |

---

## 🏗️ Arquitectura de Linux

Linux se compone de capas:

| Capa | Función |
|------|---------|
| Hardware | CPU, RAM, discos, periféricos. |
| Kernel | Virtualiza y controla los recursos físicos. |
| Shell | CLI para que el usuario ejecute comandos. |
| Utilidades del sistema | Acceso a funciones y herramientas del SO. |

---

## 🌲 Jerarquía del sistema de archivos (FHS)

Estructura en forma de árbol, definida por el **Filesystem Hierarchy Standard**:

| Ruta | Descripción |
|------|-------------|
| `/` | Directorio raíz. Contiene todo el sistema. |
| `/bin` | Comandos esenciales. |
| `/boot` | Archivos de arranque y kernel. |
| `/dev` | Dispositivos del sistema. |
| `/etc` | Archivos de configuración. |
| `/home` | Directorios de usuario. |
| `/lib` | Bibliotecas esenciales. |
| `/media` | Dispositivos externos (USB, etc). |
| `/mnt` | Punto de montaje temporal. |
| `/opt` | Software opcional o de terceros. |
| `/root` | Carpeta personal del usuario root. |
| `/sbin` | Herramientas de administración del sistema. |
| `/tmp` | Archivos temporales. Se limpia al reiniciar. |
| `/usr` | Programas, manuales, bibliotecas. |
| `/var` | Datos variables como logs, correos, etc. |

---

