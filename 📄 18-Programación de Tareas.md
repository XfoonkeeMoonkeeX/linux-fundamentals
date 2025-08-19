
```markdown
# Programación de Tareas

La programación de tareas es una característica crítica en los sistemas Linux que permite a los usuarios y administradores automatizar tareas ejecutándolas en momentos específicos o a intervalos regulares, eliminando la necesidad de una iniciación manual. Disponible en distribuciones como Ubuntu, Red Hat Linux y Solaris, esta funcionalidad gestiona una amplia gama de tareas como actualizaciones automáticas de software, ejecución de scripts, mantenimiento de bases de datos y automatización de copias de seguridad. Al programar tareas regulares y repetitivas, se asegura de que se realicen de manera consistente y fiable. Además, se pueden configurar alertas para notificar a los administradores o usuarios cuando ocurren ciertos eventos. Aunque existen numerosas aplicaciones para este tipo de automatización, estos ejemplos representan los casos de uso más comunes.

La programación de tareas en general es como configurar una cafetera o tetera para que prepare la infusión automáticamente cada mañana. Una vez programada, prepara el café o el té a la hora deseada sin más intervención, asegurando que una taza fresca esté lista cuando la necesites.

Comprender la programación de tareas en los sistemas Linux es esencial para nosotros como especialistas en ciberseguridad y pentesters porque puede servir tanto como una herramienta administrativa legítima como un vector para actividades maliciosas. El conocimiento de cómo se automatizan las tareas te permite identificar posibles riesgos de seguridad, como trabajos cron no autorizados que ejecutan scripts dañinos o mantienen puertas traseras persistentes a intervalos programados. Al comprender las complejidades de la programación de tareas, puedes detectar y analizar estas amenazas ocultas, mejorar las auditorías del sistema e incluso utilizar tareas programadas para simular escenarios de ataque durante las pruebas de penetración.

## Systemd

Systemd es un servicio utilizado en sistemas Linux como Ubuntu, Red Hat Linux y Solaris para iniciar procesos y scripts en un momento específico. Con él, podemos configurar procesos y scripts para que se ejecuten en un momento o intervalo de tiempo específico, y también podemos especificar eventos y disparadores concretos que activarán una tarea específica. Para hacer esto, debemos seguir algunos pasos y precauciones antes de que nuestros scripts o procesos sean ejecutados automáticamente por el sistema.

1.  Crear un temporizador (programa cuándo debe ejecutarse tu `mytimer.service`)
2.  Crear un servicio (ejecuta los comandos o el script)
3.  Activar el temporizador

### Crear un Temporizador

Para crear un temporizador para `systemd`, necesitamos crear un directorio donde se almacenará el script del temporizador.

```bash
foonkeemoonkee@htb[/htb]$ sudo mkdir /etc/systemd/system/mytimer.timer.d
foonkeemoonkee@htb[/htb]$ sudo vim /etc/systemd/system/mytimer.timer
```

A continuación, necesitamos crear un script que configure el temporizador. El script debe contener las siguientes opciones: "Unit", "Timer" e "Install". La opción "Unit" especifica una descripción para el temporizador. La opción "Timer" especifica cuándo iniciar el temporizador y cuándo activarlo. Finalmente, la opción "Install" especifica dónde instalar el temporizador.

**Mytimer.timer**
```ini
[Unit]
Description=My Timer

[Timer]
OnBootSec=3min
OnUnitActiveSec=1hour

[Install]
WantedBy=timers.target
```

Aquí depende de cómo queramos usar nuestro script. Por ejemplo, si queremos ejecutar nuestro script solo una vez después del arranque del sistema, debemos usar la configuración `OnBootSec` en `Timer`. Sin embargo, si queremos que nuestro script se ejecute regularmente, entonces debemos usar `OnUnitActiveSec` para que el sistema ejecute el script a intervalos regulares. A continuación, debemos crear nuestro servicio.

### Crear un Servicio

```bash
foonkeemoonkee@htb[/htb]$ sudo vim /etc/systemd/system/mytimer.service
```

Aquí establecemos una descripción y especificamos la ruta completa al script que queremos ejecutar. El "multi-user.target" es el sistema de unidades que se activa al iniciar un modo multiusuario normal. Define los servicios que deben iniciarse en un arranque normal del sistema.

```ini
[Unit]
Description=My Service

[Service]
ExecStart=/full/path/to/my/script.sh

[Install]
WantedBy=multi-user.target
```

Después de eso, tenemos que hacer que `systemd` vuelva a leer las carpetas para incluir los cambios.

### Recargar Systemd

```bash
foonkeemoonkee@htb[/htb]$ sudo systemctl daemon-reload
```

Después de eso, podemos usar `systemctl` para iniciar el servicio manualmente y habilitar el inicio automático.

### Iniciar el Temporizador y el Servicio

```bash
foonkeemoonkee@htb[/htb]$ sudo systemctl start mytimer.timer
foonkeemoonkee@htb[/htb]$ sudo systemctl enable mytimer.timer
```

De esta manera, `mytimer.service` se lanzará automáticamente según los intervalos (o retrasos) que establezcas en `mytimer.timer`.

## Cron

Cron es otra herramienta que se puede usar en los sistemas Linux para programar y automatizar procesos. Permite a los usuarios y administradores ejecutar tareas en un momento específico o dentro de intervalos específicos. Para los ejemplos anteriores, también podemos usar Cron para automatizar las mismas tareas. Solo necesitamos crear un script y luego decirle al demonio de cron que lo llame en un momento específico.

Con Cron, podemos automatizar las mismas tareas, pero el proceso para configurar el demonio de Cron es un poco diferente al de Systemd. Para configurar el demonio de cron, necesitamos almacenar las tareas en un archivo llamado `crontab` y luego decirle al demonio cuándo ejecutar las tareas. Luego podemos programar y automatizar las tareas configurando el demonio de cron en consecuencia. La estructura de Cron consta de los siguientes componentes:

| Marco de Tiempo      | Descripción                                                |
| -------------------- | ---------------------------------------------------------- |
| Minutos (0-59)       | Especifica en qué minuto se debe ejecutar la tarea.        |
| Horas (0-23)         | Especifica en qué hora se debe ejecutar la tarea.          |
| Días del mes (1-31)  | Especifica en qué día del mes se debe ejecutar la tarea.   |
| Meses (1-12)         | Especifica en qué mes se debe ejecutar la tarea.           |
| Días de la semana (0-7) | Especifica en qué día de la semana se debe ejecutar la tarea (0 y 7 son domingo). |

Por ejemplo, un `crontab` podría verse así:

```cron
# System Update
0 */6 * * * /path/to/update_software.sh

# Execute Scripts
0 0 1 * * /path/to/scripts/run_scripts.sh

# Cleanup DB
0 0 * * 0 /path/to/scripts/clean_database.sh

# Backups
0 0 * * 7 /path/to/scripts/backup.sh
```

La primera tarea, `System Update`, debe ejecutarse una vez cada seis horas. Esto se indica con la entrada `0 */6` en la columna de la hora. La tarea es ejecutada por el script `update_software.sh`, cuya ruta se proporciona en la última columna.

La segunda tarea, `Execute Scripts`, debe ejecutarse cada primer día del mes a medianoche. Esto se indica con las entradas `0` y `0` en las columnas de minuto y hora, y `1` en la columna de días del mes. La tarea es ejecutada por el script `run_scripts.sh`, cuya ruta se proporciona en la última columna.

La tercera tarea, `Cleanup DB`, debe ejecutarse todos los domingos a medianoche. Esto se especifica con las entradas `0` y `0` en las columnas de minuto y hora, y `0` en la columna de días de la semana. La tarea es ejecutada por el script `clean_database.sh`, cuya ruta se proporciona en la última columna.

La cuarta tarea, `Backups`, debe ejecutarse todos los domingos a medianoche. Esto se indica con las entradas `0` y `0` en las columnas de minuto y hora, y `7` en la columna de días de la semana. La tarea es ejecutada por el script `backup.sh`, cuya ruta se proporciona en la última columna.

También es posible recibir notificaciones cuando una tarea se ejecuta con éxito o sin él. Además, podemos crear registros para monitorear la ejecución de las tareas.

## Systemd vs. Cron

Systemd y Cron son ambas herramientas que se pueden usar en los sistemas Linux para programar y automatizar procesos. La diferencia clave entre estas dos herramientas es cómo se configuran. Con Systemd, necesitas crear un temporizador y un script de servicio que le digan al sistema operativo cuándo ejecutar las tareas. Por otro lado, con Cron, necesitas crear un archivo `crontab` que le diga al demonio de cron cuándo ejecutar las tareas.
```¡Por supuesto! Aquí está la sección de preguntas traducida y formateada para Obsidian, con la solución y respuesta detallada.

```markdown
# Preguntas

¡Responde la(s) pregunta(s) a continuación para completar esta Sección y ganar cubos!

---

### Pregunta 1

¿Cuál es el Tipo del servicio "dconf.service"?

*(Original: What is the Type of the service of the "dconf.service"?)*

**Solución:**

Para encontrar el tipo de un servicio específico de `systemd`, puedes usar el comando `systemctl show` seguido del nombre del servicio. Este comando muestra todas las propiedades del servicio. Para encontrar solo la propiedad "Type", puedes filtrar la salida usando `grep`.

Ejecuta el siguiente comando en la terminal:

```bash
systemctl show dconf.service | grep Type
```

La salida del comando te mostrará directamente el valor de la propiedad `Type`.

```
Type=dbus
```

**Respuesta:**
`dbus`
```