# Gestión de Usuarios en Linux

La gestión eficaz de usuarios es un aspecto fundamental de la administración de sistemas Linux. Los administradores necesitan con frecuencia crear nuevas cuentas de usuario o asignar usuarios existentes a grupos específicos para aplicar controles de acceso adecuados. Además, ejecutar comandos como un usuario diferente es a menudo necesario para tareas que requieren distintos privilegios. Por ejemplo, ciertos grupos pueden tener permisos exclusivos para ver o modificar archivos o directorios específicos, lo cual es esencial para mantener la seguridad e integridad del sistema. Esta capacidad nos permite recopilar información más detallada localmente en la máquina, lo que puede ser de importancia crítica para la resolución de problemas o con fines de auditoría.

> [!EXAMPLE] Caso de Uso: Nuevo Empleado
> Imagina que un nuevo empleado llamado Alex se une a tu empresa y se le proporciona una estación de trabajo basada en Linux para realizar sus tareas. Como administrador del sistema, necesitas crear una cuenta de usuario para Alex y añadirlo a los grupos apropiados que otorgan acceso a los recursos necesarios, como archivos de proyecto o herramientas de desarrollo. Adicionalmente, puede haber situaciones en las que Alex necesite ejecutar comandos con privilegios elevados o como un usuario diferente para completar ciertas tareas.

### Ejecución como un usuario
Al intentar acceder a archivos restringidos, un usuario estándar recibirá un error de permisos.

```bash
foonkeemoonkee@htb[/htb]$ cat /etc/shadow

cat: /etc/shadow: Permission denied
```

> [!WARNING] Archivo Crítico: `/etc/shadow`
> El archivo `/etc/shadow` es un archivo crítico del sistema que almacena información de contraseñas cifradas para todas las cuentas de usuario. Por razones de seguridad, solo es legible y escribible por el usuario `root` para prevenir el acceso no autorizado a datos de autenticación sensibles.

> [!TIP] Elevación de Privilegios con `sudo`
> Para realizar tareas que requieren privilegios elevados, los usuarios pueden utilizar el comando `sudo`. El comando `sudo`, abreviatura de "superuser do" (superusuario hace), permite a los usuarios autorizados ejecutar comandos con los privilegios de seguridad de otro usuario, típicamente el superusuario o `root`. Esto permite a los usuarios realizar tareas administrativas sin iniciar sesión como `root`, lo cual es una buena práctica para mantener la seguridad del sistema. Exploraremos los permisos de `sudo` con mayor detalle en la sección de Seguridad de Linux.

### Ejecución como `root`
Usando `sudo`, el mismo comando ahora se ejecuta con éxito, mostrando el contenido del archivo.

```bash
foonkeemoonkee@htb[/htb]$ sudo cat /etc/shadow

root:<SNIP>:18395:0:99999:7:::
daemon:*:17737:0:99999:7:::
bin:*:17737:0:99999:7:::
<SNIP>
```

---

### Tabla de Comandos de Gestión de Usuarios

Aquí tienes una lista de comandos que nos ayudarán a entender y manejar mejor la gestión de usuarios.

| Comando  | Descripción                                                                                                                              |
| :------- | :--------------------------------------------------------------------------------------------------------------------------------------- |
| `sudo`   | Ejecuta un comando como otro usuario.                                                                                                    |
| `su`     | La utilidad `su` solicita credenciales de usuario a través de PAM y cambia a esa ID de usuario (el usuario por defecto es el superusuario). A continuación, se ejecuta un shell. |
| `useradd`| Crea un nuevo usuario o actualiza la información por defecto para nuevos usuarios.                                                       |
| `userdel`| Elimina una cuenta de usuario y sus archivos relacionados.                                                                               |
| `usermod`| Modifica una cuenta de usuario.                                                                                                          |
| `addgroup`| Añade un grupo al sistema.                                                                                                               |
| `delgroup`| Elimina un grupo del sistema.                                                                                                            |
| `passwd` | Cambia la contraseña de un usuario.                                                                                                      |

---

Entender cómo operan las cuentas de usuario, los permisos y los mecanismos de autenticación nos permite identificar vulnerabilidades, explotar malas configuraciones y evaluar la postura de seguridad de un sistema de manera efectiva. La forma más eficaz de adquirir competencia en la gestión de usuarios es practicar el uso de los comandos individuales junto con sus diversas opciones en un entorno controlado.

> [!NOTE] ¡A Practicar!
> Siéntete libre de experimentar con los diversos comandos y explorar sus funcionalidades. Es importante que dejes que tu creatividad te guíe para decidir qué quieres lograr. Al combinar estas herramientas de gestión de usuarios con el conocimiento que has adquirido en las secciones anteriores, te darás cuenta de lo mucho que ya has aprendido.
> 
> Aplica tu comprensión del sistema Linux: crea nuevas cuentas de usuario, configura archivos y directorios para estos usuarios, selecciona archivos, lee y filtra elementos específicos, y redirígelos a los archivos y directorios de los nuevos usuarios que has creado. Siéntete libre de explorar ampliamente. En tu sistema objetivo, no hay nada que no se pueda arreglar, e incluso si algo sale mal, tienes la capacidad de reiniciar el objetivo y empezar de nuevo hasta que te sientas seguro.


---

```markdown
## Preguntas

1.  ¿Qué opción se necesita para crear un directorio personal (home) para un nuevo usuario usando el comando `useradd`?

-m

2.  ¿Qué opción se necesita para bloquear una cuenta de usuario usando el comando `usermod`? (versión larga de la opción)

--lock

3.  ¿Qué opción se necesita para ejecutar un comando como un usuario diferente usando el comando `su`? (versión larga de la opción)

--command
```
