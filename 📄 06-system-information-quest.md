# 🐧 Linux Fundamentals  
## 📄 Página 6: System Information - Preguntas

---

### 🧑‍💻 Conéctate a la máquina objetivo

**IP Objetivo:** `10.129.207.18 (ACADEMY-NIXFUND)`  
**Usuario:** `htb-student`  
**Contraseña:** `HTB_@cademy_stdnt!`

Conéctate vía SSH a la máquina con el siguiente comando:

```bash
ssh htb-student@10.129.207.18

📝 Respuestas a las preguntas:

    ¿Cuál es el nombre del hardware de la máquina?

    Ejecuta el siguiente comando para obtener el nombre del hardware:

uname -m

Respuesta:

x86_64

¿Cuál es la ruta al directorio home del usuario htb-student?

Puedes ver la ruta al directorio home del usuario htb-student ejecutando:

echo $HOME

Respuesta:

/home/htb-student

¿Cuál es la ruta al correo del usuario htb-student?

Para encontrar la ruta del correo, ejecuta:

echo $MAIL

Respuesta:

/var/mail/htb-student

¿Qué shell está especificado para el usuario htb-student?

Para encontrar la shell configurada para el usuario htb-student, usa:

grep htb-student /etc/passwd

Respuesta:

/bin/bash

¿Qué versión del kernel está instalada en el sistema?

Para obtener la versión del kernel, ejecuta:

uname -r

Respuesta:
4.15.0


¿Cuál es el nombre de la interfaz de red cuyo MTU está configurado a 1500?

Para encontrar la interfaz de red con MTU de 1500, usa el siguiente comando:

ip link show

Respuesta:

ens192

📌 Recuerda que estas respuestas están directamente relacionadas con los comandos ejecutados en la máquina de destino. Sigue paso a paso para completar el desafío.