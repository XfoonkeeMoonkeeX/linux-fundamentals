## Editando Archivos

Después de aprender a crear archivos y directorios, pasamos a trabajar con ellos. Hay varias formas de editar archivos en Linux, siendo algunos de los editores de texto más comunes **Vi** y **Vim**. Sin embargo, comenzaremos con el editor **Nano**, que es menos usado pero más fácil de entender.

Para crear y editar un archivo usando Nano, puedes especificar directamente el nombre del archivo al lanzar el editor. Por ejemplo, para crear y abrir un nuevo archivo llamado `notes.txt`, usamos el siguiente comando:

```bash
foonkeemoonkee@htb[/htb]$ nano notes.txt

Este comando abrirá el editor Nano, permitiéndonos comenzar a editar inmediatamente el archivo notes.txt. La interfaz sencilla de Nano (también conocida como "pager") lo hace ideal para editar rápidamente archivos de texto, especialmente si estás comenzando.
Editor Nano

  GNU nano 2.9.3                                    notes.txt                                              

Aquí podemos escribir todo lo que queramos y tomar nuestras notas.▓

^G Ayuda     ^O Guardar     ^W Buscar       ^K Cortar texto   ^J Justificar    ^C Pos. Cursor   M-U Deshacer
^X Salir     ^R Leer archivo ^\ Reemplazar   ^U Pegar texto    ^T Ortografía    ^_ Ir a línea    M-E Rehacer

Abajo vemos dos líneas con atajos y sus descripciones. El símbolo ^ representa la tecla [CTRL]. Por ejemplo, si presionamos [CTRL + W], aparece una línea de búsqueda donde podemos escribir la palabra que buscamos. Si buscamos la palabra "we" y presionamos [ENTER], el cursor se moverá a la primera coincidencia.

Para ir a la siguiente coincidencia, presionamos [CTRL + W] nuevamente y luego [ENTER] sin ingresar texto nuevo.

Una vez editado el archivo, lo guardamos con [CTRL + O] y confirmamos el nombre con [ENTER]. Luego salimos del editor con [CTRL + X].
Ver el contenido del archivo

Podemos ver el contenido del archivo desde la terminal usando:

foonkeemoonkee@htb[/htb]$ cat notes.txt

Archivos importantes para pentesters

En sistemas Linux, hay archivos que pueden ser muy valiosos para pentesters, especialmente si tienen permisos mal configurados o carecen de seguridad adecuada. Uno de estos archivos es /etc/passwd.

Este archivo contiene información esencial sobre los usuarios del sistema: nombres de usuario, UID, GID, directorios home, etc.

Antiguamente también incluía hashes de contraseñas, pero ahora esos hashes están en /etc/shadow, el cual tiene permisos mucho más estrictos. Si estos archivos críticos tienen permisos mal configurados, pueden exponer datos sensibles o permitir escalada de privilegios.

    Como pentesters, identificar archivos con permisos inadecuados puede revelar vulnerabilidades como cuentas débiles o acceso indebido a archivos restringidos.

VIM

Vim es un editor de texto de código abierto, muy poderoso, y sucesor mejorado de Vi. Está diseñado para edición eficiente de texto y ofrece integración con herramientas externas como grep, awk, sed, etc.

Vim sigue el principio de Unix: "muchos programas pequeños y especializados que juntos crean sistemas poderosos".

foonkeemoonkee@htb[/htb]$ vim

Cuando abrimos Vim, vemos algo como esto:

  VIM - Vi IMproved                                
  versión 8.0.1453                                
  by Bram Moolenaar et al.                         
  Modificado por pkg-vim-maintainers@...           
  Vim es de código abierto y libremente distribuible

Modos en Vim

Vim es un editor modal, lo que significa que distingue entre los modos de entrada de texto y de comandos. Estos son los principales modos:
Modo	Descripción
Normal	Al iniciar Vim, estás en este modo. Todos los caracteres ingresados son interpretados como comandos.
Insert	Puedes ingresar texto en el buffer (presionando i desde el modo Normal).
Visual	Permite seleccionar texto para cortar, copiar o reemplazar.
Command	Desde aquí puedes ingresar comandos (:) para guardar, cerrar, buscar, etc.
Replace	Reemplaza texto existente mientras escribes.
Ex	Emula al editor Ex, permitiendo ejecutar múltiples comandos seguidos.
Comandos comunes de Vim

    :q → salir

    :w → guardar

    :wq → guardar y salir

    i → entrar a modo inserción

    ESC → volver a modo normal

VimTutor

Vim tiene una herramienta interactiva para aprender a usarlo llamada vimtutor. Es una excelente forma de practicar y familiarizarte con el editor.

foonkeemoonkee@htb[/htb]$ vimtutor

Verás lo siguiente:

===============================================================================
=    B I E N V E N I D O   A L   V I M   T U T O R    -    Versión 1.7        =
===============================================================================

Vim es un editor muy potente con muchos comandos.  
Este tutor está diseñado para enseñarte lo suficiente para usar Vim fácilmente.

El tiempo estimado para completar el tutor es de 25–30 minutos.

¡Recuerda! Este tutor está diseñado para aprender **usando los comandos**.  
¡No solo leas! ¡Ejecuta los comandos para aprender de verdad!

🧪 Ejercicio Opcional

    Juega con vimtutor. Familiarízate con el editor y experimenta con sus funciones.
    ¡Puede parecer difícil al principio, pero la eficiencia que ganarás lo vale!

