# 💬 Prompt en Bash

El **prompt de Bash** es fácil de entender. Por defecto, muestra información como:

- 👤 Tu **nombre de usuario** (`username`)
- 🖥️ El **nombre del equipo** (`hostname`)
- 📁 El **directorio en el que estás trabajando**

Este aparece como una línea de texto en la terminal, seguida del cursor (la línea o bloque que parpadea), indicando que el sistema está listo para que escribas un comando.

---

## 🧱 Estructura básica del prompt

```bash
<usuario>@<equipo><directorio>$

Cuando estás en tu carpeta personal (home), esta se representa con una virgulilla ~. Ejemplo:

usuario@equipo[~]$

🔒 Si cambias al usuario root, el símbolo cambia de $ a #, así:

root@htb[/htb]#

Si al obtener una shell remota no ves ni el usuario ni el hostname, probablemente la variable de entorno PS1 no está bien configurada.

    Usuario sin privilegios:

$

Usuario root (con privilegios):

    #

🔧 ¿Qué es la variable PS1?

La variable PS1 define cómo se ve tu prompt. Funciona como una plantilla para mostrar información útil. Puedes personalizarla para mostrar:

    Nombre de usuario

    Hostname

    Directorio actual

    Fecha y hora

    Resultado del último comando

    Dirección IP, etc.

Esta personalización es muy útil durante pruebas de penetración, ya que te ayuda a tener un contexto claro en todo momento.

📁 También puedes registrar tus comandos en .bash_history, y usar script para guardar sesiones completas.
🧪 Caracteres especiales para PS1

Aquí tienes algunos caracteres especiales que puedes usar al personalizar tu prompt:
Carácter	Significado
\u	Nombre de usuario actual
\h	Nombre corto del equipo
\H	Nombre completo del equipo
\w	Ruta completa del directorio actual
\d	Fecha (ej. Mon Feb 6)
\D{%Y-%m-%d}	Fecha en formato personalizado
\t	Hora (24h - HH:MM:SS)
\T	Hora (12h - HH:MM:SS)
\@	Hora (AM/PM)
\j	Número de trabajos activos en la shell
\n	Nueva línea
\r	Retorno de carro
\s	Nombre de la shell
🖌️ Personalización visual del terminal

Además del contenido del prompt, puedes personalizar:

    Colores

    Tipografías

    Estilos del terminal

Esto hace que tu entorno sea más claro, eficiente y cómodo para trabajar.

💡 Aunque no entra en profundidad en este módulo, puedes usar herramientas como:

    bash-prompt-generator

    powerline

Estas herramientas permiten crear prompts profesionales y visuales para adaptarlos a tus necesidades.
📌 Idea clave

El prompt en Bash no es solo estético, sino una poderosa herramienta para mejorar la experiencia, la productividad y la organización, especialmente en contextos de pentesting o administración de sistemas.


---


