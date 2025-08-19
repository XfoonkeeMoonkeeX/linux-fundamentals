      ````
En Linux, los permisos son como llaves que controlan el acceso a archivos y directorios. Estos permisos se asignan tanto a **usuarios** como a **grupos**, de forma muy parecida a cómo se distribuyen llaves a individuos y equipos específicos dentro de una organización. Cada usuario puede pertenecer a múltiples grupos, y formar parte de un grupo otorga derechos de acceso adicionales, permitiendo a los usuarios realizar acciones específicas en archivos y directorios.

Cada archivo y directorio tiene un **propietario** (un usuario) y está asociado a un **grupo**. Los permisos para estos archivos se definen tanto para el propietario como para el grupo, determinando qué acciones —como leer, escribir o ejecutar— están permitidas. Cuando creas un nuevo archivo o directorio, automáticamente se convierte en "tuyo" y se asocia al grupo al que perteneces.

En esencia, los permisos de Linux actúan como un conjunto de reglas o llaves que dictan quién puede acceder o modificar ciertos recursos, garantizando la seguridad y la colaboración adecuada en todo el sistema.

> [!NOTE] Nota Clave
> Este sistema de Propietario, Grupo y Otros es la base de la seguridad de archivos en Linux y es fundamental para la [[Escalada de Privilegios]].

---
## Permiso de Ejecución en Directorios

Cuando un usuario quiere acceder al contenido de un directorio en Linux, es similar a desbloquear una puerta antes de entrar. Para "atravesar" o navegar hacia un directorio, el usuario primero debe tener la llave correcta: esta llave es el **permiso de ejecución (`x`)** sobre el directorio.

En otras palabras, tener permisos de ejecución en un directorio es como tener permiso para caminar por un pasillo para acceder a las habitaciones que hay dentro. No te permite ver o modificar lo que hay en ellas, pero sí te concede la capacidad de entrar y explorar la estructura del directorio. Sin este permiso, el usuario no puede acceder al contenido del directorio y se encontrará con un mensaje de error **“Permission Denied” (Permiso denegado)**.

### Ejemplo Práctico

Observa los permisos del directorio `scripts`. No tiene el permiso de ejecución (`x`) para nadie (`drw-rw-r--`).

```bash
cry0l1t3@htb[/htb]$ ls -l

drw-rw-r-- 3 cry0l1t3 cry0l1t3   4096 Jan 12 12:30 scripts
````
Cuando intentamos listar el contenido de un directorio con una configuración similar (mydirectory), el sistema nos deniega el acceso.

```
cry0l1t3@htb[/htb]$ ls -al mydirectory/

ls: no se puede acceder a 'mydirectory/script.sh': Permiso denegado
ls: no se puede acceder a 'mydirectory/..': Permiso denegado
ls: no se puede acceder a 'mydirectory/subdirectory': Permiso denegado
ls: no se puede acceder a 'mydirectory/.': Permiso denegado
total 0
d????????? ? ? ? ?            ? .
d????????? ? ? ? ?            ? ..
-????????? ? ? ? ?            ? script.sh
d????????? ? ? ? ?            ? subdirectory
```

> [!TIP] Consejo de Pentesting  
> Si encuentras un directorio donde tienes permiso de lectura (r) pero no de ejecución (x), no puedes hacer cd en él, pero podrías ser capaz de listar sus archivos si otros permisos lo permiten. ¡Esto puede revelar nombres de archivos interesantes!

Es importante destacar que los permisos de ejecución son necesarios para **atravesar un directorio**. Además, los permisos de ejecución en un directorio **no** permiten a un usuario ejecutar o modificar ningún archivo o contenido dentro de él.

- Para **ejecutar archivos** dentro del directorio, un usuario necesita permisos de **ejecución (x)** sobre el archivo correspondiente.
    
- Para **modificar el contenido** de un directorio (crear, eliminar o renombrar archivos), el usuario necesita permisos de **escritura (w)** sobre el directorio.
    

---

### Tipos de Permisos Básicos

Todo el sistema de permisos en Linux se basa en el [[Sistema de Permisos Octal]] y, básicamente, hay tres tipos diferentes de permisos:

- **(r) - Read (Lectura):** Permite ver el contenido de un archivo o listar los archivos de un directorio.
    
- **(w) - Write (Escritura):** Permite modificar un archivo o crear/borrar archivos dentro de un directorio.
    
- **(x) - Execute (Ejecución):** Permite ejecutar un archivo (si es un script o binario) o entrar (cd) en un directorio.

````
````
Estos permisos se asignan a tres tipos de identidades diferentes: el propietario del archivo, el grupo asociado al archivo y "todos los demás".

---

## Desglose de `ls -l`: Propietario, Grupo y Otros

Los permisos se pueden establecer para el **propietario (owner)**, el **grupo (group)** y los **demás (others)**, como se presenta en el siguiente ejemplo. El comando `ls -l` es fundamental para visualizar esta estructura.

Veamos la salida para el archivo `/etc/passwd`:

```bash
cry0l1t3@htb[/htb]$ ls -l /etc/passwd

-rwxrw-r-- 1 root root 1641 May  4 23:42 /etc/passwd
````

```
- rwx rw- r--   1 root root 1641 May  4 23:42 /etc/passwd
- --- --- ---   |  |    |    |   |__________|
|  |   |   |    |  |    |    |        |_ Fecha de modificación
|  |   |   |    |  |    |    |__________ Tamaño del archivo (en bytes)
|  |   |   |    |  |    |_______________ Grupo
|  |   |   |    |  |____________________ Usuario (Propietario)
|  |   |   |    |_______________________ Número de enlaces duros
|  |   |   |_ Permisos para otros (solo lectura)
|  |   |_____ Permisos para el grupo (lectura, escritura)
|  |_________ Permisos para el propietario (lectura, escritura, ejecución)
|____________ Tipo de archivo (- = Archivo, d = Directorio, l = Enlace simbólico, ...)
```
> [!INFO] Entendiendo la Salida de ls -l  
> Dominar la lectura de esta salida es una habilidad no negociable en Linux. Te permite identificar rápidamente posibles vectores de ataque, como archivos con permisos de escritura para "otros" (w en el último bloque) o scripts con el [[SUID Bit]] activado, un concepto clave en la escalada de privilegios.

````
# Modificar Permisos con `chmod`

Podemos modificar los permisos de archivos y directorios usando el comando `chmod`. Este comando se puede utilizar de dos maneras principales: con **referencias simbólicas** o con **valores octales**.

---
## 1. Modo Simbólico (Letras)

En el modo simbólico, usamos referencias para especificar a quién y cómo queremos cambiar los permisos:

-   **A quién afecta:**
    -   `u` (user): El propietario.
    -   `g` (group): El grupo.
    -   `o` (others): Los demás.
    -   `a` (all): Todos (usuario, grupo y otros).
-   **Qué acción realizar:**
    -   `+`: Añadir un permiso.
    -   `-`: Quitar un permiso.
    -   `=`: Establecer los permisos exactos (sobrescribe los anteriores).

### Ejemplo práctico

Supongamos que tenemos un script llamado `shell` y queremos modificar sus permisos.

```bash
cry0l1t3@htb[/htb]$ ls -l shell

-rwxr-x--x 1 cry0l1t3 htbteam 0 May 4 22:12 shell
````
Ahora, vamos a **añadir (+)** el permiso de **lectura (r)** para **todos (a)**. Podemos encadenar comandos con && para ejecutar ls -l inmediatamente después y ver el resultado.

```
cry0l1t3@htb[/htb]$ chmod a+r shell && ls -l shell

-rwxr-xr-x 1 cry0l1t3 htbteam 0 May 4 22:12 shell
```
## 2. Modo Numérico (Octal)

El modo octal es una forma más rápida y común de establecer todos los permisos de una vez usando números. Cada permiso tiene un valor numérico:

- r (lectura) = **4**
    
- w (escritura) = **2**
    
- x (ejecución) = **1**
    

Para cada grupo (propietario, grupo, otros), sumamos los valores de los permisos que queremos asignar. Por ejemplo, rwx sería 4+2+1=7.

### Ejemplo práctico

Vamos a establecer los permisos del archivo shell a -rwxr-xr-- usando el modo octal.

- **Propietario (rwx):** 4+2+1 = 7
    
- **Grupo (r-x):** 4+0+1 = 5
    
- **Otros (r--):** 4+0+0 = 4
    

El comando sería chmod 754 shell.
```
cry0l1t3@htb[/htb]$ chmod 754 shell && ls -l shell

-rwxr-xr-- 1 cry0l1t3 htbteam 0 May 4 22:12 shell
```
### Entendiendo la Lógica Octal

Para comprender mejor cómo se calcula la asignación de permisos, veamos la representación completa asociada al valor 754.

> [!NOTE] Relación entre Notación Binaria y Octal  
> Cada número octal (0-7) representa una combinación única de 3 bits (encendido/apagado para r, w y x). Esto hace que sea un sistema muy compacto para representar permisos.

```
Notación Binaria:           4 2 1  |  4 2 1  |  4 2 1
----------------------------------------------------------
Representación Binaria:     1 1 1  |  1 0 1  |  1 0 0
----------------------------------------------------------
Valor Octal:                  7    |    5    |    4
----------------------------------------------------------
Representación de Permisos: r w x  |  r - x  |  r - -
```
Si sumamos los bits "encendidos" (1) de la **Representación Binaria**, usando los valores de la **Notación Binaria**, obtenemos el **Valor Octal**. La **Representación de Permisos** es simplemente una forma más legible de ver qué bits están activados (r, w, x) o desactivados (-).

 ````
 Cambiar el Propietario con `chown`

Para cambiar el **propietario (owner)** y/o el **grupo** de un archivo o directorio, usamos el comando `chown` (change owner).

La sintaxis es la siguiente:

```bash
# Sintaxis de chown
chown <usuario>:<grupo> <archivo_o_directorio>
````
### Ejemplo práctico

En este ejemplo, vamos a cambiar el propietario y el grupo del archivo shell para que pertenezcan a root.

```
# Antes del cambio
cry0l1t3@htb[/htb]$ ls -l shell
-rwxr-xr--   1 cry0l1t3 htbteam 0 May  4 22:12 shell

# Ejecutamos chown y vemos el resultado
cry0l1t3@htb[/htb]$ chown root:root shell && ls -l shell

-rwxr-xr--   1 root     root     0 May  4 22:12 shell
```

> [!TIP] Consejo  
> Si solo quieres cambiar el propietario, puedes usar chown usuario archivo. Si solo quieres cambiar el grupo, el comando más específico es chgrp grupo archivo, aunque chown :grupo archivo también funciona.

---

## Permisos Especiales: SUID y SGID

Además de los permisos estándar de usuario y grupo, Linux nos permite configurar permisos especiales en archivos a través de los bits **SUID (Set User ID)** y **SGID (Set Group ID)**.

Estos bits funcionan como pases de acceso temporal, permitiendo a los usuarios ejecutar ciertos programas con los privilegios de otro usuario o grupo. Por ejemplo, los administradores pueden usar SUID o SGID para conceder derechos elevados a los usuarios para aplicaciones específicas, permitiendo que se realicen tareas con los permisos necesarios, incluso si el propio usuario no los tiene normalmente.

> [!INFO] ¿Cómo se ven los bits SUID/SGID?  
> La presencia de estos permisos se indica con una s (o S) en lugar de la x habitual en el conjunto de permisos del archivo.
> 
> - **SUID:** Aparece en la sección de permisos del **propietario** (ej: -rwsr-xr-x).
>     
> - **SGID:** Aparece en la sección de permisos del **grupo** (ej: -rwxr-sr-x).
>     

Cuando se ejecuta un programa con el bit SUID o SGID activado, se ejecuta con los permisos del **propietario del archivo** (SUID) o del **grupo del archivo** (SGID), en lugar de los del usuario que lo lanzó. Esto puede ser útil para ciertas tareas del sistema, pero también introduce riesgos de seguridad potenciales si no se usa con cuidado.

### Riesgos de Seguridad de SUID/SGID

> [!WARNING] ¡Punto Clave de Escalada de Privilegios!  
> Los binarios con el bit SUID activado y propiedad de root son uno de los principales objetivos durante la fase de escalada de privilegios. Si un binario vulnerable tiene el bit SUID, un atacante puede explotarlo para obtener una shell como root.

Un riesgo común ocurre cuando los administradores, sin estar familiarizados con la funcionalidad completa de una aplicación, asignan bits SUID o SGID de manera indiscriminada.

Por ejemplo, si se aplica el bit SUID a un programa como journalctl, que incluye una función para lanzar una shell desde su interfaz, cualquier usuario que ejecute este programa podría ejecutar una shell como root. Esto les otorgaría control completo sobre el sistema, presentando una vulnerabilidad de seguridad significativa.

Puedes encontrar más información sobre esta y otras aplicaciones que pueden ser abusadas de esta manera en **[GTFOBins](https://www.google.com/url?sa=E&q=https%3A%2F%2Fgtfobins.github.io%2F)**.

````
Permisos Especiales: El Sticky Bit

El **Sticky Bit** en Linux es como una cerradura especial para los archivos dentro de espacios compartidos. Cuando se establece en un **directorio**, el Sticky Bit añade una capa extra de seguridad, asegurando que solo ciertos individuos puedan modificar o eliminar archivos, incluso si otros tienen acceso a ese directorio.

> [!NOTE] Analogía del Espacio Compartido
> Imagina un espacio de trabajo comunitario (`/tmp`) donde muchas personas pueden entrar y usar las mismas herramientas, pero cada persona tiene su propio cajón que solo ella (o el administrador) puede abrir. El Sticky Bit actúa como la cerradura de esos cajones, impidiendo que cualquier otra persona manipule su contenido.

En un directorio compartido con el Sticky Bit activado, esto significa que **solo el propietario del archivo, el propietario del directorio o el usuario `root`** pueden eliminar o renombrar los archivos que contiene. Los otros usuarios pueden seguir accediendo al directorio, pero no pueden modificar los archivos que no les pertenecen.

Esta característica es especialmente útil en entornos compartidos, como los directorios públicos (`/tmp`, `/var/tmp`), donde múltiples usuarios y procesos trabajan juntos. Al establecer el Sticky Bit, te aseguras de que los archivos importantes no sean alterados accidental o maliciosamente por alguien que no debería tener esa autoridad.

### ¿Cómo se ve el Sticky Bit?

El Sticky Bit se muestra en la sección de permisos de "otros", reemplazando el permiso de ejecución (`x`). Se representa con una `t` (minúscula) o una `T` (mayúscula).

```bash
cry0l1t3@htb[/htb]$ ls -l

drw-rw-r-t 3 cry0l1t3 cry0l1t3   4096 Jan 12 12:30 scripts
drw-rw-r-T 3 cry0l1t3 cry0l1t3   4096 Jan 12 12:32 reports
````
En este ejemplo, vemos que ambos directorios tienen el Sticky Bit activado. Sin embargo, el directorio reports tiene una **T mayúscula**, mientras que el directorio scripts tiene una **t minúscula**.

> [!IMPORTANT] La diferencia entre t y T
> 
> - **t (minúscula):** El Sticky Bit está activado **Y** el permiso de ejecución (x) para "otros" también está activado. Los usuarios pueden entrar en el directorio.
>     
> - **T (mayúscula):** El Sticky Bit está activado, **PERO** el permiso de ejecución (x) para "otros" **NO** está activado. Los usuarios (que no son el propietario ni del grupo) no pueden entrar en el directorio ni ver su contenido.
>     

---

### Resumen de Permisos Especiales (SUID, SGID, Sticky Bit)

Para establecer estos permisos en modo octal, se utiliza un cuarto dígito al principio del número:

- **SUID:** 4 (ej: chmod 4755 archivo)
    
- **SGID:** 2 (ej: chmod 2755 archivo)
    
- **Sticky Bit:** 1 (ej: chmod 1777 directorio)