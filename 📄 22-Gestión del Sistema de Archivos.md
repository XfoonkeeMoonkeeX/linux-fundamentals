### **Fundamentos de Linux**

**Página 22**
**Gestión del Sistema de Archivos**

La gestión de sistemas de archivos en Linux es una tarea crucial que implica organizar, almacenar y mantener datos en un disco u otro dispositivo de almacenamiento. Linux es un sistema operativo versátil que admite muchos sistemas de archivos diferentes, incluidos ext2, ext3, ext4, XFS, Btrfs y NTFS, entre otros. Cada uno de estos sistemas de archivos tiene características únicas y es adecuado para casos de uso específicos. La mejor elección de sistema de archivos depende de los requisitos específicos de la aplicación o del usuario, tales como:

*   **ext2** es un sistema de archivos más antiguo sin capacidades de *journaling* (registro por diario), lo que lo hace menos adecuado para sistemas modernos, pero aún útil en ciertos escenarios de baja sobrecarga (como unidades USB).
*   **ext3** y **ext4** son más avanzados, con *journaling* (que ayuda a recuperarse de fallos), y **ext4** es la elección predeterminada para la mayoría de los sistemas Linux modernos porque ofrece un equilibrio entre rendimiento, fiabilidad y soporte para archivos grandes.
*   **Btrfs** es conocido por sus características avanzadas como la creación de instantáneas (*snapshotting*) y las comprobaciones de integridad de datos incorporadas, lo que lo hace ideal para configuraciones de almacenamiento complejas.
*   **XFS** sobresale en el manejo de archivos grandes y tiene un alto rendimiento. Es más adecuado para entornos con altas demandas de E/S.
*   **NTFS**, desarrollado originalmente para Windows, es útil para la compatibilidad al tratar con sistemas de arranque dual o unidades externas que necesitan funcionar tanto en sistemas Linux como Windows.

Al seleccionar un sistema de archivos, es esencial analizar las necesidades de la aplicación o los factores del usuario, como el rendimiento, la integridad de los datos, la compatibilidad y los requisitos de almacenamiento, que influirán en la decisión.

La arquitectura del sistema de archivos de Linux se basa en el modelo de Unix, organizada en una estructura jerárquica. Esta estructura consta de varios componentes, siendo los más críticos los **inodos**. Los inodos son estructuras de datos que almacenan metadatos sobre cada archivo y directorio, incluidos permisos, propiedad, tamaño y marcas de tiempo. Los inodos no almacenan los datos reales del archivo ni su nombre, pero contienen punteros a los bloques donde se almacenan los datos del archivo en el disco.

La tabla de inodos es una colección de estos inodos, actuando esencialmente como una base de datos que el kernel de Linux utiliza para rastrear cada archivo y directorio en el sistema. Esta estructura permite al sistema operativo acceder y gestionar archivos de manera eficiente. Comprender y gestionar los inodos es un aspecto crucial de la gestión del sistema de archivos en Linux, especialmente en escenarios donde un disco se queda sin espacio de inodos antes de quedarse sin capacidad de almacenamiento real.

Usemos una analogía, piensa en el sistema de archivos de Linux como una biblioteca. Los **inodos** son como las tarjetas de índice en el sistema de catálogo de la biblioteca (la tabla de inodos). Cada tarjeta contiene información detallada sobre un libro (archivo): su título, autor, ubicación y otros detalles, pero no el libro en sí. La **tabla de inodos** es el catálogo completo que ayuda a la biblioteca (el sistema operativo) a encontrar y gestionar rápidamente los libros (archivos).

En Linux, los archivos se pueden almacenar en uno de varios tipos clave:

*   Archivos regulares
*   Directorios
*   Enlaces simbólicos

#### **Archivos Regulares**

Los archivos regulares son el tipo más común y típicamente consisten en datos de texto (como ASCII) y/o datos binarios (como imágenes, audio o ejecutables). Residen en varios directorios a lo largo del sistema de archivos, no solo en el directorio raíz. El directorio raíz (`/`) es simplemente la cima del árbol de directorios jerárquico, y los archivos pueden existir en cualquier directorio dentro de esa estructura.

#### **Directorios**

Los directorios son tipos especiales de archivos que actúan como contenedores para otros archivos (tanto archivos regulares como otros directorios). Cuando un archivo se almacena en un directorio, ese directorio se denomina el directorio padre del archivo. Los directorios ayudan a organizar los archivos dentro del sistema de archivos de Linux, permitiendo una forma eficiente de gestionar colecciones de archivos.

#### **Enlaces Simbólicos**

Además de los archivos regulares y los directorios, Linux también admite enlaces simbólicos (*symlinks*), que actúan como accesos directos o referencias a otros archivos o directorios. Los enlaces simbólicos permiten un acceso rápido a archivos ubicados en diferentes partes del sistema de archivos sin duplicar el archivo en sí. Los symlinks se pueden usar para agilizar el acceso u organizar estructuras de directorios complejas apuntando a archivos importantes en diversas ubicaciones.

Cada categoría de usuario puede tener diferentes niveles de permiso. Por ejemplo, el propietario de un archivo puede tener permiso para leerlo, escribirlo y ejecutarlo, mientras que otros solo pueden tener acceso de lectura. Estos permisos son independientes para cada categoría, lo que significa que los cambios en los permisos de un usuario no afectan necesariamente a los demás.

```bash
foonkeemoonkee@htb[/htb]$ ls -il
```

```
total 0
10678872 -rw-r--r--  1 cry0l1t3  htb  234123 Feb 14 19:30 myscript.py
10678869 -rw-r--r--  1 cry0l1t3  htb   43230 Feb 14 11:52 notes.txt
```

### **Discos y Unidades**

La gestión de discos en Linux implica administrar dispositivos de almacenamiento físico, incluidos discos duros, unidades de estado sólido y dispositivos de almacenamiento extraíbles. La principal herramienta para la gestión de discos en Linux es `fdisk`, que nos permite crear, eliminar y gestionar particiones en una unidad. También puede mostrar información sobre la tabla de particiones, incluido el tamaño y el tipo de cada partición. Particionar una unidad en Linux implica dividir el espacio de almacenamiento físico en secciones lógicas separadas. Cada partición puede luego ser formateada con un sistema de archivos específico, como ext4, NTFS o FAT32, y puede ser montada como un sistema de archivos separado. Las herramientas de particionamiento más comunes en Linux también son `fdisk`, `gpart` y `GParted`.

**Fdisk**
```bash
foonkeemoonkee@htb[/htb]$ sudo fdisk -l
```

```
Disk /dev/vda: 160 GiB, 171798691840 bytes, 335544320 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0x5223435f

Device     Boot     Start       End   Sectors  Size Id Type
/dev/vda1  *         2048 158974027 158971980 75.8G 83 Linux
/dev/vda2       158974028 167766794   8792767  4.2G 82 Linux swap / Solaris

Disk /dev/vdb: 452 KiB, 462848 bytes, 904 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
```

### **Montaje**

Cada partición lógica o unidad de almacenamiento debe ser asignada a un directorio específico en el sistema de archivos. Este proceso se conoce como **montaje**. El montaje implica vincular una unidad o partición a un directorio, haciendo que su contenido sea accesible dentro de la jerarquía general del sistema de archivos. Una vez que una unidad se monta en un directorio (también llamado punto de montaje), se puede acceder y usar como cualquier otro directorio en el sistema.

El comando `mount` se usa comúnmente para montar manualmente sistemas de archivos en Linux. Sin embargo, si deseas que ciertos sistemas de archivos o particiones se monten automáticamente cuando el sistema arranca, puedes definirlos en el archivo `/etc/fstab`. Este archivo lista los sistemas de archivos y sus puntos de montaje asociados, junto con opciones como permisos de lectura/escritura y tipos de sistema de archivos, asegurando que unidades o particiones específicas estén disponibles al inicio sin necesidad de intervención manual.

**Sistemas de archivos montados en el arranque**
```bash
foonkeemoonkee@htb[/htb]$ cat /etc/fstab
```

```
# /etc/fstab: static file system information.
#
# Use 'blkid' to print the universally unique identifier for a device; this may
# be used with UUID= as a more robust way to name devices that works even if
# disks are added and removed. See fstab(5).
#
# <file system>                      <mount point>  <type>  <options>  <dump>  <pass>
UUID=3d6a020d-...SNIP...-9e085e9c927a /              btrfs   subvol=@,defaults,noatime,nodiratime,nodatacow,space_cache,autodefrag 0 1
UUID=3d6a020d-...SNIP...-9e085e9c927a /home          btrfs   subvol=@home,defaults,noatime,nodiratime,nodatacow,space_cache,autodefrag 0 2
UUID=21f7eb94-...SNIP...-d4f58f94e141 swap           swap    defaults,noatime 0 0
```

Para ver los sistemas de archivos actualmente montados, podemos usar el comando `mount` sin ningún argumento. La salida mostrará una lista de todos los sistemas de archivos actualmente montados, incluido el nombre del dispositivo, el tipo de sistema de archivos, el punto de montaje y las opciones.

**Listar Unidades Montadas**
```bash
foonkeemoonkee@htb[/htb]$ mount
```

```
sysfs on /sys type sysfs (rw,nosuid,nodev,noexec,relatime)
proc on /proc type proc (rw,nosuid,nodev,noexec,relatime)
udev on /dev type devtmpfs (rw,nosuid,relatime,size=4035812k,nr_inodes=1008953,mode=755,inode64)
devpts on /dev/pts type devpts (rw,nosuid,noexec,relatime,gid=5,mode=620,ptmxmode=000)
tmpfs on /run type tmpfs (rw,nosuid,nodev,noexec,relatime,size=814580k,mode=755,inode64)
/dev/vda1 on / type btrfs (rw,noatime,nodiratime,nodatasum,nodatacow,space_cache,autodefrag,subvolid=257,subvol=/@)
```

Para montar un sistema de archivos, podemos usar el comando `mount` seguido del nombre del dispositivo y el punto de montaje. Por ejemplo, para montar una unidad USB con el nombre de dispositivo `/dev/sdb1` en el directorio `/mnt/usb`, usaríamos el siguiente comando:

**Montar una unidad USB**
```bash
foonkeemoonkee@htb[/htb]$ sudo mount /dev/sdb1 /mnt/usb
foonkeemoonkee@htb[/htb]$ cd /mnt/usb && ls -l
```

```
total 32
drwxr-xr-x 1 root root   18 Oct 14  2021 'Account Takeover'
drwxr-xr-x 1 root root   18 Oct 14  2021 'API Key Leaks'
drwxr-xr-x 1 root root   18 Oct 14  2021 'AWS Amazon Bucket S3'
drwxr-xr-x 1 root root   34 Oct 14  2021 'Command Injection'
drwxr-xr-x 1 root root   18 Oct 14  2021 'CORS Misconfiguration'
drwxr-xr-x 1 root root   52 Oct 14  2021 'CRLF Injection'
drwxr-xr-x 1 root root   30 Oct 14  2021 'CSRF Injection'
drwxr-xr-x 1 root root   18 Oct 14  2021 'CSV Injection'
drwxr-xr-x 1 root root 1166 Oct 14  2021 'CVE Exploits'
...SNIP...
```

Para desmontar un sistema de archivos en Linux, podemos usar el comando `umount` seguido del punto de montaje del sistema de archivos que queremos desmontar. El punto de montaje es la ubicación en el sistema de archivos donde el sistema de archivos está montado y es accesible para nosotros. Por ejemplo, para desmontar la unidad USB que se montó previamente en el directorio `/mnt/usb`, usaríamos el siguiente comando:

**Desmontar**
```bash
foonkeemoonkee@htb[/htb]$ sudo umount /mnt/usb
```

Es importante tener en cuenta que debemos tener permisos suficientes para desmontar un sistema de archivos. Tampoco podemos desmontar un sistema de archivos que esté en uso por un proceso en ejecución. Para asegurarnos de que no haya procesos en ejecución que estén usando el sistema de archivos, podemos usar el comando `lsof` para listar los archivos abiertos en el sistema de archivos.

```bash
cry0l1t3@htb:~$ lsof | grep cry0l1t3
```

```
vncserver 6006        cry0l1t3  mem       REG      0,24       402274 /usr/bin/perl (path dev=0,26)
vncserver 6006        cry0l1t3  mem       REG      0,24      1554101 /usr/lib/locale/aa_DJ.utf8/LC_COLLATE (path dev=0,26)
vncserver 6006        cry0l1t3  mem       REG      0,24       402326 /usr/lib/x86_64-linux-gnu/perl-base/auto/POSIX/POSIX.so (path dev=0,26)
vncserver 6006        cry0l1t3  mem       REG      0,24       402059 /usr/lib/x86_64-linux-gnu/perl/5.32.1/auto/Time/HiRes/HiRes.so (path dev=0,26)
vncserver 6006        cry0l1t3  mem       REG      0,24      1444250 /usr/lib/x86_64-linux-gnu/libnss_files-2.31.so (path dev=0,26)
vncserver 6006        cry0l1t3  mem       REG      0,24       402327 /usr/lib/x86_64-linux-gnu/perl-base/auto/Socket/Socket.so (path dev=0,26)
vncserver 6006        cry0l1t3  mem       REG      0,24       402324 /usr/lib/x86_64-linux-gnu/perl-base/auto/IO/IO.so (path dev=0,26)
...SNIP...```

Si encontramos algún proceso que esté utilizando el sistema de archivos, debemos detenerlo antes de poder desmontar el sistema de archivos. Además, también podemos desmontar un sistema de archivos automáticamente cuando el sistema se apaga agregando una entrada al archivo `/etc/fstab`. El archivo `/etc/fstab` contiene información sobre todos los sistemas de archivos que están montados en el sistema, incluidas las opciones para el montaje automático en el momento del arranque y otras opciones de montaje. Para desmontar un sistema de archivos automáticamente al apagar, necesitamos agregar la opción `noauto` a la entrada en el archivo `/etc/fstab` para ese sistema de archivos. Esto se vería, por ejemplo, de la siguiente manera:

**Archivo Fstab**
```txt
/dev/sda1 / ext4 defaults 0 0
/dev/sda2 /home ext4 defaults 0 0
/dev/sdb1 /mnt/usb ext4 rw,noauto,user 0 0
192.168.1.100:/nfs /mnt/nfs nfs defaults 0 0
```

### **SWAP**

El espacio de intercambio (*swap*) es una parte esencial de la gestión de memoria en Linux y juega un papel fundamental para garantizar un rendimiento fluido del sistema, especialmente cuando la memoria física disponible (RAM) está completamente utilizada. Cuando el sistema se queda sin memoria física, el kernel mueve las páginas de memoria inactivas (datos que no están en uso inmediato) al espacio de intercambio, liberando RAM para los procesos activos. Este proceso se conoce como *swapping*.

#### **Creación de Espacio de Intercambio**

El espacio de intercambio se puede configurar durante la instalación del sistema operativo o agregarse más tarde usando los comandos `mkswap` y `swapon`.

*   **`mkswap`** se utiliza para preparar un dispositivo o archivo para ser usado como espacio de intercambio creando un área de intercambio de Linux.
*   **`swapon`** activa el espacio de intercambio, permitiendo que el sistema lo utilice.

#### **Dimensionamiento y Gestión del Espacio de Intercambio**

El tamaño del espacio de intercambio no es fijo y depende de la memoria física de tu sistema y del uso previsto. Por ejemplo, un sistema con menos RAM o que ejecuta aplicaciones que consumen mucha memoria podría necesitar más espacio de intercambio. Sin embargo, los sistemas modernos con grandes cantidades de RAM pueden requerir menos o incluso ningún espacio de intercambio, dependiendo de los casos de uso específicos.

Al configurar el espacio de intercambio, es importante asignarlo en una partición o archivo dedicado, separado del resto del sistema de archivos. Esto evita la fragmentación y asegura un uso eficiente del área de intercambio cuando sea necesario. Además, debido a que los datos sensibles pueden almacenarse temporalmente en el espacio de intercambio, se recomienda cifrar el espacio de intercambio para protegerse contra una posible exposición de datos.

#### **Espacio de Intercamb-bio para Hibernación**

Además de extender la memoria física, el espacio de intercambio también se utiliza para la hibernación. La hibernación es una función de ahorro de energía que guarda el estado del sistema (incluidas las aplicaciones y procesos abiertos) en el espacio de intercambio y apaga el sistema. Cuando el sistema se vuelve a encender, restaura su estado anterior desde el espacio de intercambio, reanudando exactamente donde lo dejó.

### **Preguntas**

¡Responde la(s) pregunta(s) a continuación para completar esta Sección y ganar cubos!

**+ 0** ¿Cuántas particiones existen en nuestra Pwnbox? (Formato: 0)

3
