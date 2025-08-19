

***
### **Atajos**

Existen muchos atajos que podemos usar para que trabajar con Linux sea más fácil y rápido. Después de que nos hayamos familiarizado con los más importantes y los hayamos convertido en un hábito, nos ahorraremos mucho tecleo. Algunos de ellos incluso nos ayudarán a evitar el uso del ratón en la terminal.

#### **Autocompletar**
**[TAB]** - Inicia el autocompletado. Esto nos sugerirá diferentes opciones basadas en la entrada estándar (STDIN) que proporcionemos. Pueden ser sugerencias específicas como directorios en nuestro entorno de trabajo actual, comandos que comienzan con el mismo número de caracteres que ya hemos escrito, u opciones.

#### **Movimiento del Cursor**
**[CTRL] + A** - Mueve el cursor al principio de la línea actual.

**[CTRL] + E** - Mueve el cursor al final de la línea actual.

**[CTRL] + [←] / [→]** - Salta al principio de la palabra actual/anterior.

**[ALT] + B / F** - Salta una palabra hacia atrás/adelante.

#### **Borrar la Línea Actual**
**[CTRL] + U** - Borra todo desde la posición actual del cursor hasta el principio de la línea.

**[Ctrl] + K** - Borra todo desde la posición actual del cursor hasta el final de la línea.

**[Ctrl] + W** - Borra la palabra que precede a la posición del cursor.

#### **Pegar Contenido Borrado**
**[Ctrl] + Y** - Pega el texto o la palabra borrada.

#### **Finalizar Tarea**
**[CTRL] + C** - Finaliza la tarea/proceso actual enviando la señal `SIGINT`. Por ejemplo, esto puede ser un escaneo que está ejecutando una herramienta. Si estamos viendo el escaneo, podemos detenerlo / matar este proceso usando este atajo. Si no está configurado y desarrollado por la herramienta que estamos usando, el proceso se matará sin pedirnos confirmación.

#### **Fin de Archivo (EOF)**
**[CTRL] + D** - Cierra la tubería STDIN, lo que también se conoce como Fin de Archivo (EOF) o Fin de Transmisión.

#### **Limpiar la Terminal**
**[CTRL] + L** - Limpia la terminal. Una alternativa a este atajo es el comando `clear` que puedes escribir para limpiar nuestra terminal.

#### **Poner un Proceso en Segundo Plano**
**[CTRL] + Z** - Suspende el proceso actual enviando la señal `SIGTSTP`.

#### **Buscar en el Historial de Comandos**
**[CTRL] + R** - Busca a través del historial de comandos comandos que hayamos escrito previamente y que coincidan con nuestros patrones de búsqueda.

**[↑] / [↓]** - Ve al comando anterior/siguiente en el historial de comandos.

#### **Cambiar entre Aplicaciones**
**[ALT] + [TAB]** - Cambia entre las aplicaciones abiertas.

#### **Zoom**
**[CTRL] + [+]** - Acercar.

**[CTRL] + [-]** - Alejar.