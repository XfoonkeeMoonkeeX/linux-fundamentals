

***

**Copia de Seguridad y Restauración**

Los sistemas Linux proporcionan una gama de potentes herramientas para realizar copias de seguridad y restaurar datos, diseñadas para ser tanto eficientes como seguras. Estas herramientas ayudan a asegurar que nuestros datos no solo estén protegidos contra pérdidas o corrupción, sino que también sean fácilmente accesibles cuando los necesitemos.

Al hacer una copia de seguridad de datos en un sistema Ubuntu, tenemos varias opciones, entre ellas:

*   Rsync
*   Deja Dup
*   Duplicity

**Rsync** es una herramienta de código abierto que permite realizar copias de seguridad rápidas y seguras, ya sea localmente o en una ubicación remota. Una de sus ventajas clave es que solo transfiere las porciones de los archivos que han cambiado, lo que la hace altamente eficiente al manejar grandes cantidades de datos. Rsync es particularmente útil para transferencias en red, como sincronizar archivos entre servidores o crear copias de seguridad incrementales a través de internet.

**Duplicity** es otra potente herramienta que se basa en Rsync, pero añade funciones de cifrado para proteger las copias de seguridad. Te permite cifrar tus copias de respaldo, asegurando que los datos sensibles permanezcan seguros incluso si se almacenan en servidores remotos, sitios FTP o servicios en la nube como Amazon S3. Duplicity proporciona una capa extra de seguridad mientras mantiene las eficientes capacidades de transferencia de datos de Rsync.

Piensa en tus datos como tesoros valiosos guardados en una casa. Las herramientas de copia de seguridad en Linux, como Rsync, Duplicity y Deja Dup, actúan como diferentes tipos de cajas fuertes. **Rsync** es como un transporte rápido que solo lleva lo nuevo o lo que ha cambiado, convirtiéndolo en la forma ideal de enviar actualizaciones a una bóveda remota. **Duplicity** es una caja fuerte de alta seguridad que no solo almacena el tesoro, sino que también lo cierra con un código complejo, asegurando que nadie más pueda acceder a él. **Deja Dup** es una caja fuerte simple y accesible que cualquiera puede operar, sin dejar de ofrecer el mismo nivel de protección. Cifrar tus copias de seguridad añade una cerradura adicional a tu caja fuerte, garantizando que incluso si alguien la encuentra, no pueda abrirla.

Para los usuarios que prefieren una opción más simple y amigable, **Deja Dup** ofrece una interfaz gráfica que hace que el proceso de copia de seguridad sea sencillo. Detrás de escena, también utiliza Rsync y, al igual que Duplicity, admite copias de seguridad cifradas. Deja Dup es ideal para usuarios que desean un acceso rápido y fácil a las opciones de copia de seguridad y restauración sin necesidad de sumergirse en la línea de comandos.

Asegurar la seguridad de tus copias de seguridad es tan importante como crearlas. Cifrar tus datos de respaldo ayuda a protegerlos del acceso no autorizado, brindando la tranquilidad de que la información sensible permanece protegida. En sistemas Ubuntu, puedes usar herramientas de cifrado adicionales como GnuPG, eCryptfs o LUKS para añadir otra capa de protección a tus copias de seguridad.

Realizar copias de seguridad y restaurar datos es una práctica esencial para cualquiera que trabaje con Ubuntu. Al usar herramientas como Rsync, Duplicity y Deja Dup, puedes asegurarte de que tus datos estén almacenados de forma segura y sean fácilmente recuperables, dándote la confianza de que, en caso de una pérdida de datos inesperada, tu información puede ser restaurada de manera rápida y fiable.

Para instalar Rsync en Ubuntu, podemos usar el gestor de paquetes apt:

**Instalar Rsync**
foonkeemoonkee@htb[/htb]$ sudo apt install rsync -y

Esto instalará la última versión de Rsync en el sistema. Una vez que la instalación esté completa, podemos comenzar a usar la herramienta para respaldar y restaurar datos. Para respaldar un directorio completo usando rsync, podemos usar el siguiente comando:

**Rsync - Respaldar un Directorio local a nuestro Servidor de Respaldo**
foonkeemoonkee@htb[/htb]$ rsync -av /ruta/a/midirectorio usuario@servidor_respaldo:/ruta/a/respaldo/directorio

Este comando copiará el directorio completo (`/ruta/a/midirectorio`) a un host remoto (`servidor_respaldo`), en el directorio `/ruta/a/respaldo/directorio`. La opción `archive` (`-a`) se usa para preservar los atributos originales del archivo, como permisos, marcas de tiempo, etc., y usar la opción `verbose` (`-v`) proporciona una salida detallada del progreso de la operación de rsync.

También podemos añadir opciones adicionales para personalizar el proceso de respaldo, como el uso de compresión y copias de seguridad incrementales. Podemos hacerlo de la siguiente manera:

foonkeemoonkee@htb[/htb]$ rsync -avz --backup --backup-dir=/ruta/a/respaldo/carpeta --delete /ruta/a/midirectorio usuario@servidor_respaldo:/ruta/a/respaldo/directorio

Con esto, respaldamos el `midirectorio` en el `servidor_respaldo` remoto, preservando los atributos originales del archivo, marcas de tiempo y permisos, y habilitamos la compresión (`-z`) para transferencias más rápidas. La opción `--backup` crea copias de seguridad incrementales en el directorio `/ruta/a/respaldo/carpeta`, y la opción `--delete` elimina archivos del host remoto que ya no están presentes en el directorio de origen.

Si queremos restaurar nuestro directorio desde nuestro servidor de respaldo a nuestro directorio local, podemos usar el siguiente comando:

**Rsync - Restaurar nuestro Respaldo**
foonkeemoonkee@htb[/htb]$ rsync -av usuario@host_remoto:/ruta/a/respaldo/directorio /ruta/a/midirectorio

### **Rsync Cifrado**

Para asegurar la seguridad de nuestra transferencia de archivos con rsync entre nuestro host local y nuestro servidor de respaldo, podemos combinar el uso de SSH y otras medidas de seguridad. Al usar SSH, podemos cifrar nuestros datos mientras se transfieren, haciendo que sea mucho más difícil para cualquier individuo no autorizado acceder a ellos. Adicionalmente, también podemos usar cortafuegos y otros protocolos de seguridad para asegurar que nuestros datos se mantengan seguros durante la transferencia. Tomando estas medidas, podemos tener la confianza de que nuestros datos están protegidos y nuestra transferencia de archivos es segura. Por lo tanto, le decimos a rsync que use SSH de la siguiente manera:

**Transferencia Segura de nuestro Respaldo**
foonkeemoonkee@htb[/htb]$ rsync -avz -e ssh /ruta/a/midirectorio usuario@servidor_respaldo:/ruta/a/respaldo/directorio

La transferencia de datos entre nuestro host local y el servidor de respaldo ocurre sobre la conexión SSH cifrada, que proporciona confidencialidad y protección de la integridad de los datos que se están transfiriendo. Este proceso de cifrado asegura que los datos estén protegidos de cualquier posible actor malicioso que de otro modo podría acceder y modificar los datos sin autorización. La clave de cifrado en sí misma también está protegida por un conjunto completo de protocolos de seguridad, lo que hace aún más difícil que cualquier persona no autorizada obtenga acceso a los datos. Además, la conexión cifrada está diseñada para ser altamente resistente a cualquier intento de violar la seguridad, lo que nos permite tener confianza en la protección de los datos que se transfieren.

### **Sincronización Automática**

Para habilitar la sincronización automática usando rsync, puedes usar una combinación de cron y rsync para automatizar el proceso de sincronización. Programar la tarea de cron para que se ejecute a intervalos regulares asegura que los contenidos de los dos sistemas se mantengan sincronizados. Esto puede ser especialmente beneficioso para organizaciones que necesitan mantener sus datos sincronizados en múltiples máquinas. Además, configurar la sincronización automática con rsync puede ser una excelente manera de ahorrar tiempo y esfuerzo, ya que elimina la necesidad de sincronización manual. También ayuda a asegurar que los archivos y datos almacenados en los sistemas se mantengan actualizados y consistentes, lo que ayuda a reducir errores y mejorar la eficiencia.

Por lo tanto, creamos un nuevo script llamado `RSYNC_Backup.sh`, que activará el comando rsync para sincronizar nuestro directorio local con el remoto. Sin embargo, como estamos usando un script para realizar la conexión SSH para rsync, necesitamos configurar la autenticación basada en claves. Esto es para evitar la necesidad de introducir nuestra contraseña al conectarnos con SSH.

Primero, generamos un par de claves para nuestro usuario.

foonkeemoonkee@htb[/htb]$ ssh-keygen -t rsa -b 2048

Sigue las indicaciones para especificar la ubicación (la predeterminada es `~/.ssh/id_rsa`) y opcionalmente proporciona una frase de contraseña (déjala vacía para no usar frase de contraseña). Luego, necesitamos copiar nuestra clave pública al servidor remoto.

foonkeemoonkee@htb[/htb]$ ssh-copy-id usuario@servidor_respaldo

Ahora, podemos crear nuestro script para automatizar el respaldo con rsync.

**RSYNC_Backup.sh**
```bash
#!/bin/bash

rsync -avz -e ssh /ruta/a/midirectorio usuario@servidor_respaldo:/ruta/a/respaldo/directorio```

Luego, para asegurar que el script pueda ejecutarse correctamente, debemos proporcionar los permisos necesarios. Adicionalmente, también es importante asegurarse de que el script sea propiedad del usuario correcto, ya que esto asegurará que solo el usuario correcto tenga acceso al script y que el script no sea manipulado por ningún otro usuario.

foonkeemoonkee@htb[/htb]$ chmod +x RSYNC_Backup.sh

Después de eso, podemos crear un crontab que le diga a cron que ejecute el script cada hora en el minuto 0.

foonkeemoonkee@htb[/htb]$ crontab -e

Podemos ajustar el tiempo para que se adapte a nuestras necesidades. Para hacerlo, el crontab necesita el siguiente contenido:

**Sincronización Automática - Crontab**
foonkeemoonkee@htb[/htb]$ 0 * * * * /ruta/a/RSYNC_Backup.sh

Con esta configuración, cron será responsable de ejecutar el script en el intervalo deseado, asegurando que el comando rsync se ejecute y los contenidos del directorio local se sincronicen con el host remoto.

Te animamos a que pruebes rsync usando Pwnbox. En lugar de sincronizar archivos con un servidor remoto, puedes usar Pwnbox como tu origen y destino, lo que simplifica las pruebas. Para hacer esto, crea dos directorios en Pwnbox:

*   `to_backup` (donde se almacenan tus datos originales) y otro llamado
*   `synced_backup` (donde se copiarán los datos sincronizados)

Luego, transferirás los datos del directorio `to_backup` al directorio `synced_backup` usando rsync. Para automatizar este proceso, configura una tarea de cron que se ejecute cada minuto para asegurar una sincronización continua. Recuerda, como estamos probando esto localmente, podemos usar la dirección IP de loopback `127.0.0.1` como la dirección para el host "remoto".