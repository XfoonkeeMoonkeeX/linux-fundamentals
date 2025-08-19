# 🐧 Linux Fundamentals  
## 📄 Página 6: System Information

---

### 📚 Introducción

Ya conociendo la estructura de Linux, sus distribuciones y el uso del shell, ahora vamos a aplicar estos conocimientos en la práctica, usando comandos directamente desde la terminal y aprendiendo cómo obtener ayuda cuando la necesitemos.

---

## 🔍 Comandos Básicos para Obtener Información del Sistema

> *Todos estos comandos pueden usarse con `-h`, `--help` o `man` para ver su documentación.*

| Comando     | Descripción                                                                 |
|-------------|-----------------------------------------------------------------------------|
| `whoami`    | Muestra el nombre del usuario actual.                                       |
| `id`        | Devuelve la identidad del usuario y sus grupos.                             |
| `hostname`  | Muestra o configura el nombre del host actual.                              |
| `uname`     | Muestra información básica del sistema operativo.                           |
| `pwd`       | Muestra el directorio de trabajo actual.                                    |
| `ifconfig`  | Asigna o muestra la dirección IP y configuración de interfaces de red.      |
| `ip`        | Muestra o manipula rutas, dispositivos e interfaces de red.                 |
| `netstat`   | Muestra el estado de la red.                                                 |
| `ss`        | Muestra información de sockets (más moderno que netstat).                   |
| `ps`        | Muestra el estado de procesos en ejecución.                                 |
| `who`       | Muestra qué usuarios están conectados.                                      |
| `env`       | Muestra las variables de entorno.                                            |
| `lsblk`     | Lista los dispositivos de bloque (discos, particiones).                     |
| `lsusb`     | Lista los dispositivos USB conectados.                                      |
| `lsof`      | Lista archivos abiertos por procesos.                                       |
| `lspci`     | Lista dispositivos PCI (hardware interno).                                  |

---

## 🔐 Conexión Remota con SSH

SSH (Secure Shell) permite conectarse de forma segura a otros sistemas de manera remota, sin interfaz gráfica, ideal para administración remota de servidores.

```bash
ssh htb-student@[IP address]

🧪 Ejemplos Prácticos
🔹 hostname

hostname
# Salida:
nixfund

🔹 whoami

whoami
# Salida:
cry0l1t3

🔹 id

id
# Salida:
uid=1000(cry0l1t3) gid=1000(cry0l1t3) groups=1000(cry0l1t3),1337(hackthebox),4(adm),27(sudo),...

    👀 Importante: Ver grupos como sudo, adm, hackthebox puede indicar posibles escaladas de privilegio.

🔹 uname

Muestra información del sistema.

uname -a
# Salida:
Linux box 4.15.0-99-generic #100-Ubuntu SMP Wed Apr 22 20:32:56 UTC 2020 x86_64 x86_64 x86_64 GNU/Linux

También puedes obtener detalles específicos:

uname -r     # Versión del kernel
uname -n     # Hostname
uname -s     # Nombre del kernel
uname -v     # Versión del kernel
uname -m     # Arquitectura de la máquina

    📌 Este comando es útil para buscar posibles exploits del kernel, como:
    Buscar en Google → 4.15.0-99-generic exploit

🧠 Consejo del Módulo

    Estudiar los manpages puede parecer tedioso, pero te ayudará a descubrir funciones que ni sabías que existían.
    Mucha de esta info será clave para detectar vulnerabilidades, errores de configuración y posibles escaladas de privilegios en sistemas Linux.

💪 Filosofía de Aprendizaje

Aprender Linux al principio puede ser difícil, como cuando manejaste un auto por primera vez. Pero con práctica constante, cada nuevo comando te hará más ágil, más seguro, y más capaz para enfrentar cualquier desafío de ciberseguridad.
