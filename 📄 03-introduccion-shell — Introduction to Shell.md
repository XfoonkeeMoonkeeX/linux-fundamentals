# 🖥️ Introduction to Shell

Es crucial aprender a usar el **shell** de Linux, ya que muchos servidores se basan en Linux. Esto se debe a que Linux suele ser más **seguro** y **menos propenso a errores** que los servidores basados en Windows. Por ejemplo, los servidores web suelen estar basados en Linux. Conocer cómo usar el sistema operativo para controlarlo de manera efectiva requiere entender y dominar la parte esencial de Linux: **el Shell**.

Cuando hicimos el cambio de Windows a Linux, esto es lo que vimos:

![Imagen de terminal de Parrot mostrando el prompt con el usuario 'user6@htb-wpjudq32ze' y el comando 'okay google'.](imagenes-shell/first_linux2.webp)

## 🧑‍💻 ¿Qué es un Shell?

Un terminal de Linux, también llamado **shell** o **línea de comandos**, es una interfaz de texto entre el **usuario** y el **núcleo (kernel)** del sistema operativo. A través de esta interfaz, podemos ejecutar comandos para controlar el sistema. A veces se le llama **consola**, pero este término generalmente se refiere a una pantalla en modo texto.

Podemos pensar en el shell como una **interfaz gráfica de usuario** (GUI) basada en texto, donde introducimos comandos para realizar acciones como:

- Navegar entre directorios
- Trabajar con archivos
- Obtener información del sistema

¡Pero con mucho más poder y flexibilidad!

---

## 💻 Emuladores de Terminal

Los **emuladores de terminal** son programas que emulan el funcionamiento de un terminal, permitiendo usar programas basados en texto dentro de un **entorno gráfico** (GUI). Existen también interfaces de línea de comandos (**CLI**) que se ejecutan como terminales adicionales dentro de un terminal. En resumen, un terminal actúa como una interfaz para interactuar con el **intérprete de comandos (shell)**.

### Ejemplo Visual:

Imagina que estás en un **gran edificio de oficinas**. El **shell** sería la **sala principal del servidor**, donde se procesan todos los datos y comandos. El **terminal** sería como el **mostrador de recepción**, que sirve de punto de comunicación entre tú (usuario) y el servidor.

Si estuvieras trabajando de manera remota, un **emulador de terminal** sería como una **mesa de recepción virtual** en tu pantalla (GUI), permitiéndote interactuar con la sala de servidores sin estar físicamente presente en la oficina.

---

## 🖱️ Multiplexores de Terminal

Los **multiplexores de terminal** son extensiones útiles que permiten trabajar con el terminal de formas más avanzadas. Nos proporcionan métodos para trabajar con múltiples terminales o directorios a la vez, dividir la ventana del terminal en varias secciones, crear diferentes espacios de trabajo, y mucho más.

### Ejemplo con Tmux:

Aquí hay un ejemplo visual de cómo se ve un terminal usando **Tmux**, un multiplexor de terminal:

![Imagen de terminal con tres paneles mostrando listados de directorios: BloodHound, Impacket y SecLists.](imagenes-shell/tmux.webp)

---

## ⌨️ Shell más Común: BASH

El **Bash** (Bourne Again Shell) es el **shell** más utilizado en Linux, y forma parte del **proyecto GNU**. A través del shell, podemos hacer **todo lo que hacemos con la interfaz gráfica** (GUI), pero con más potencia y control. 

El shell nos brinda muchas más posibilidades para interactuar con programas y procesos, obteniendo información de manera más rápida y eficiente. Además, muchos procesos se pueden automatizar fácilmente con scripts pequeños o grandes, lo que hace mucho más fácil el trabajo manual.

### Otros Shells Comunes

Además de Bash, existen otros shells populares, como:
- **Tcsh/Csh**
- **Ksh**
- **Zsh**
- **Fish shell**

---
