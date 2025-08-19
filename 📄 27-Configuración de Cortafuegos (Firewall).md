
### **Configuración de Cortafuegos (Firewall)**

El objetivo principal de los cortafuegos es proporcionar un mecanismo de seguridad para controlar y monitorear el tráfico de red entre diferentes segmentos de red, como redes internas y externas o diferentes zonas de red. Los cortafuegos desempeñan un papel crucial en la protección de las redes informáticas contra el acceso no autorizado, el tráfico malicioso y otras amenazas de seguridad. Linux, al ser un sistema operativo popular utilizado en servidores y otros dispositivos de red, proporciona capacidades de cortafuegos integradas que se pueden utilizar para controlar el tráfico de red. En otras palabras, pueden filtrar el tráfico entrante y saliente basándose en reglas predefinidas, protocolos, puertos y otros criterios para prevenir el acceso no autorizado y mitigar las amenazas de seguridad. El objetivo específico de una implementación de cortafuegos puede variar según las necesidades específicas de la organización, como garantizar la confidencialidad, integridad y disponibilidad de los recursos de la red.

Un ejemplo de la historia de los cortafuegos de Linux es el desarrollo de la herramienta `iptables`, que reemplazó a las herramientas anteriores `ipchains` e `ipfwadm`. La utilidad `iptables` se introdujo por primera vez en el kernel de Linux 2.4 en el año 2000 y proporcionó un mecanismo flexible y eficiente para filtrar el tráfico de red. `iptables` se convirtió en la solución de cortafuegos estándar de facto para los sistemas Linux, y ha sido ampliamente adoptada por muchas organizaciones y usuarios.

La utilidad `iptables` proporcionaba una interfaz de línea de comandos simple pero potente para configurar reglas de cortafuegos, que podían usarse para filtrar el tráfico basándose en diversos criterios como direcciones IP, puertos, protocolos y más. `iptables` fue diseñado para ser altamente personalizable y podía usarse para crear conjuntos de reglas de cortafuegos complejas que podían proteger contra diversas amenazas de seguridad como ataques de denegación de servicio (DoS), escaneos de puertos e intentos de intrusión en la red.

En Linux, la funcionalidad de cortafuegos se implementa típicamente utilizando el framework Netfilter, que es una parte integral del kernel. Netfilter proporciona un conjunto de "ganchos" (*hooks*) que se pueden usar para interceptar y modificar el tráfico de red a medida que pasa por el sistema. La utilidad `iptables` se usa comúnmente para configurar las reglas del cortafuegos en los sistemas Linux.

### **Iptables**

La utilidad `iptables` proporciona un conjunto flexible de reglas para filtrar el tráfico de red basándose en diversos criterios como direcciones IP de origen y destino, números de puerto, protocolos y más. También existen otras soluciones como `nftables`, `ufw` y `firewalld`. `Nftables` proporciona una sintaxis más moderna y un rendimiento mejorado sobre `iptables`. Sin embargo, la sintaxis de las reglas de `nftables` no es compatible con `iptables`, por lo que la migración a `nftables` requiere cierto esfuerzo. `UFW` significa "Uncomplicated Firewall" (Cortafuegos sin complicaciones) y proporciona una interfaz simple y fácil de usar para configurar reglas de cortafuegos. `UFW` se basa en el framework de `iptables` al igual que `nftables` y proporciona una forma más fácil de gestionar las reglas del cortafuegos. Finalmente, `FirewallD` proporciona una solución de cortafuegos dinámica y flexible que se puede utilizar para gestionar configuraciones de cortafuegos complejas, y admite un rico conjunto de reglas para filtrar el tráfico de red y se puede utilizar para crear zonas y servicios de cortafuegos personalizados. Consiste en varios componentes que trabajan juntos para proporcionar una solución de cortafuegos flexible y potente. Los componentes principales de `iptables` son:

| Componente | Descripción |
| :--- | :--- |
| **Tablas** | Las tablas se utilizan para organizar y categorizar las reglas del cortafuegos. |
| **Cadenas**| Las cadenas se utilizan para agrupar un conjunto de reglas de cortafuegos que se aplican a un tipo específico de tráfico de red. |
| **Reglas** | Las reglas definen los criterios para filtrar el tráfico de red y las acciones a tomar para los paquetes que coinciden con los criterios. |
| **Coincidencias (Matches)** | Las coincidencias se utilizan para encontrar criterios específicos para filtrar el tráfico de red, como direcciones IP de origen o destino, puertos, protocolos y más. |
| **Objetivos (Targets)** | Los objetivos especifican la acción para los paquetes que coinciden con una regla específica. Por ejemplo, los objetivos pueden usarse para aceptar, descartar o rechazar paquetes, o para modificarlos de otra manera. |

#### **Tablas**

Cuando se trabaja con cortafuegos en sistemas Linux, es importante entender cómo funcionan las tablas en `iptables`. Las tablas en `iptables` se utilizan para categorizar y organizar las reglas del cortafuegos según el tipo de tráfico que están diseñadas para manejar. Estas tablas se utilizan para organizar y categorizar las reglas del cortafuegos. Cada tabla es responsable de realizar un conjunto específico de tareas.

| Nombre de la Tabla | Descripción | Cadenas Incorporadas |
| :--- | :--- | :--- |
| **filter** | Se utiliza para filtrar el tráfico de red basado en direcciones IP, puertos y protocolos. | INPUT, OUTPUT, FORWARD |
| **nat** | Se utiliza para modificar las direcciones IP de origen o destino de los paquetes de red. | PREROUTING, POSTROUTING |
| **mangle** | Se utiliza para modificar los campos de la cabecera de los paquetes de red. | PREROUTING, OUTPUT, INPUT, FORWARD, POSTROUTING |

Además de las tablas incorporadas, `iptables` proporciona una cuarta tabla llamada tabla `raw`, que se utiliza para configurar opciones especiales de procesamiento de paquetes. La tabla `raw` contiene dos cadenas incorporadas: `PREROUTING` y `OUTPUT`.

#### **Cadenas**

En `iptables`, las cadenas organizan reglas que definen cómo debe filtrarse o modificarse el tráfico de red. Hay dos tipos de cadenas en `iptables`:

*   Cadenas incorporadas
*   Cadenas definidas por el usuario

Las **cadenas incorporadas** están predefinidas y se crean automáticamente cuando se crea una tabla. Cada tabla tiene un conjunto diferente de cadenas incorporadas. Por ejemplo, la tabla `filter` tiene tres cadenas incorporadas:

*   `INPUT`
*   `OUTPUT`
*   `FORWARD`

Estas cadenas se utilizan para filtrar el tráfico de red entrante y saliente, así como el tráfico que se reenvía entre diferentes interfaces de red. La tabla `nat` tiene dos cadenas incorporadas:

*   `PREROUTING`
*   `POSTROUTING`

La cadena `PREROUTING` se utiliza para modificar la dirección IP de destino de los paquetes entrantes antes de que la tabla de enrutamiento los procese. La cadena `POSTROUTING` se utiliza para modificar la dirección IP de origen de los paquetes salientes después de que la tabla de enrutamiento los haya procesado. La tabla `mangle` tiene cinco cadenas incorporadas:

*   `PREROUTING`
*   `OUTPUT`
*   `INPUT`
*   `FORWARD`
*   `POSTROUTING`

Estas cadenas se utilizan para modificar los campos de la cabecera de los paquetes entrantes y salientes, y los paquetes que están siendo procesados por las cadenas correspondientes.

Las **cadenas definidas por el usuario** pueden simplificar la gestión de reglas al agrupar reglas de cortafuegos basadas en criterios específicos, como la dirección IP de origen, el puerto de destino o el protocolo. Se pueden agregar a cualquiera de las tres tablas principales. Por ejemplo, si una organización tiene múltiples servidores web que requieren reglas de cortafuegos similares, las reglas para cada servidor podrían agruparse en una cadena definida por el usuario. Otro ejemplo es cuando una cadena definida por el usuario podría filtrar el tráfico destinado a un puerto específico, como el puerto 80 (HTTP). El usuario podría entonces agregar reglas a esta cadena que filtren específicamente el tráfico destinado al puerto 80.

#### **Reglas y Objetivos**

Las reglas de `iptables` se utilizan para definir los criterios para filtrar el tráfico de red y las acciones a tomar para los paquetes que coinciden con los criterios. Las reglas se añaden a las cadenas utilizando la opción `-A` seguida del nombre de la cadena, y pueden ser modificadas o eliminadas utilizando varias otras opciones.

Cada regla consiste en un conjunto de criterios o **coincidencias (matches)** y un **objetivo (target)** que especifica la acción para los paquetes que coinciden con los criterios. Los criterios o coincidencias se usan para encontrar campos específicos en la cabecera IP, como la dirección IP de origen o destino, el protocolo, el puerto de origen, el número de puerto de destino, y más. El objetivo especifica la acción para los paquetes que coinciden con los criterios. Especifican la acción a tomar para los paquetes que coinciden con una regla específica. Por ejemplo, los objetivos pueden aceptar, descartar, rechazar o modificar los paquetes. Algunos de los objetivos comunes utilizados en las reglas de `iptables` incluyen los siguientes:

| Nombre del Objetivo | Descripción |
| :--- | :--- |
| **ACCEPT** | Permite que el paquete pase a través del cortafuegos y continúe hacia su destino. |
| **DROP** | Descarta el paquete, bloqueando efectivamente que pase a través del cortafuegos. |
| **REJECT** | Descarta el paquete y envía un mensaje de error a la dirección de origen, notificándoles que el paquete fue bloqueado. |
| **LOG** | Registra la información del paquete en el registro del sistema. |
| **SNAT** | Modifica la dirección IP de origen del paquete, típicamente usado para Traducción de Direcciones de Red (NAT) para traducir direcciones IP privadas a públicas. |
| **DNAT** | Modifica la dirección IP de destino del paquete, típicamente usado para NAT para reenviar tráfico de una dirección IP a otra. |
| **MASQUERADE** | Similar a SNAT pero se usa cuando la dirección IP de origen no es fija, como en un escenario de dirección IP dinámica. |
| **REDIRECT** | Redirige los paquetes a otro puerto o dirección IP. |
| **MARK** | Añade o modifica el valor de la marca de Netfilter del paquete, que puede usarse para enrutamiento avanzado u otros propósitos. |

Ilustremos una regla y consideremos que queremos añadir una nueva entrada a la cadena `INPUT` que permita que el tráfico TCP entrante en el puerto 22 (SSH) sea aceptado. El comando para eso se vería así:

foonkeemoonkee@htb[/htb]$ sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT

#### **Coincidencias (Matches)**

Las coincidencias se utilizan para especificar los criterios que determinan si una regla de cortafuegos debe aplicarse a un paquete o conexión en particular. Las coincidencias se utilizan para encontrar características específicas del tráfico de red, como la dirección IP de origen o destino, el protocolo, el número de puerto y más.

| Nombre de la Coincidencia | Descripción |
| :--- | :--- |
| **-p** o **--protocol** | Especifica el protocolo a encontrar (ej. tcp, udp, icmp). |
| **--dport** | Especifica el puerto de destino a encontrar. |
| **--sport** | Especifica el puerto de origen a encontrar. |
| **-s** o **--source** | Especifica la dirección IP de origen a encontrar. |
| **-d** o **--destination** | Especifica la dirección IP de destino a encontrar. |
| **-m state** | Encuentra el estado de una conexión (ej. NEW, ESTABLISHED, RELATED). |
| **-m multiport** | Encuentra múltiples puertos o rangos de puertos. |
| **-m tcp** | Encuentra paquetes TCP e incluye opciones adicionales específicas de TCP. |
| **-m udp** | Encuentra paquetes UDP e incluye opciones adicionales específicas de UDP. |
| **-m string** | Encuentra paquetes que contienen una cadena específica. |
| **-m limit** | Encuentra paquetes a una tasa límite especificada. |
| **-m conntrack** | Encuentra paquetes basados en su información de seguimiento de conexión. |
| **-m mark** | Encuentra paquetes basados en su valor de marca de Netfilter. |
| **-m mac** | Encuentra paquetes basados en su dirección MAC. |
| **-m iprange** | Encuentra paquetes basados en un rango de direcciones IP. |

En general, las coincidencias se especifican usando la opción `-m` en `iptables`. Por ejemplo, el siguiente comando añade una regla a la cadena `INPUT` en la tabla `filter` que encuentra tráfico TCP entrante en el puerto 80:

foonkeemoonkee@htb[/htb]$ sudo iptables -A INPUT -p tcp -m tcp --dport 80 -j ACCEPT

Esta regla de ejemplo encuentra tráfico TCP entrante (`-p tcp`) en el puerto 80 (`--dport 80`) y salta al objetivo `accept` (`-j ACCEPT`) si la coincidencia es exitosa.

---
**Ejercicios Opcionales**
1.  Lanza un servidor web en el puerto TCP/8080 en tu objetivo y usa `iptables` para bloquear el tráfico entrante en ese puerto.
2.  Cambia las reglas de `iptables` para permitir el tráfico entrante en el puerto TCP/8080.
3.  Bloquea el tráfico de una dirección IP específica.
4.  Permite el tráfico de una dirección IP específica.
5.  Bloquea el tráfico basado en un protocolo.
6.  Permite el tráfico basado en un protocolo.
7.  Crea una nueva cadena.
8.  Reenvía el tráfico a una cadena específica.
9.  Elimina una regla específica.
10. Lista todas las reglas existentes.