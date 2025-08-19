# 🆘 Obtener Ayuda en Linux

Ahora que ya manejamos la estructura de Linux, sus distribuciones y la terminal, es momento de **usar comandos reales**... y también de aprender a **pedir ayuda** cuando no sepamos cómo usarlos correctamente.

En Linux es **muy común** encontrarnos con herramientas cuyos parámetros no conocemos o comandos completamente nuevos. Por eso es **esencial saber cómo ayudarnos a nosotros mismos** usando:

- `man` (manuales)
- `--help` (ayuda corta)
- `-h` (ayuda rápida)
- `apropos` (búsqueda en los títulos de los manuales)

---

## 📁 Primer Comando: `ls`

```bash
foonkeemoonkee@htb[/htb]$ ls

Esto muestra el contenido del directorio actual. Ejemplo:

cacert.der  Documents  Music     Public     Videos
Desktop     Downloads  Pictures  Templates

📖 Usar man

Los manuales (man) son una fuente completa de documentación para cada herramienta o comando:

foonkeemoonkee@htb[/htb]$ man ls

Esto abre una página como esta:

LS(1)               User Commands               LS(1)

NAME
       ls - list directory contents

SYNOPSIS
       ls [OPTION]... [FILE]...

DESCRIPTION
       List information about the FILEs (the current directory by default).
       ...

Presiona q para salir del manual.
🆘 Usar --help

También puedes obtener una ayuda rápida directamente desde el comando:

foonkeemoonkee@htb[/htb]$ ls --help

Salida (resumida):

Usage: ls [OPTION]... [FILE]...
  -a, --all            do not ignore entries starting with .
  -A, --almost-all     do not list implied . and ..
  ...

🧾 Usar -h (ayuda corta)

Algunos comandos como curl usan -h en lugar de --help:

foonkeemoonkee@htb[/htb]$ curl -h

Salida (resumida):

Usage: curl [options...] <url>
 -a, --append         Append to target file
 --basic             Use HTTP Basic Authentication
 -E, --cert <cert>   Client certificate file
 ...

🔍 Usar apropos

apropos busca una palabra clave en los títulos y descripciones de los manuales:

foonkeemoonkee@htb[/htb]$ apropos sudo

Resultado:

```shell-session
foonkeemoonkee@htb[/htb]$ apropos sudo

sudo (8)             - execute a command as another user
sudo.conf (5)        - configuration for sudo front end
sudo_plugin (8)      - Sudo Plugin API
sudo_root (8)        - How to run administrative commands
sudoedit (8)         - execute a command as another user
sudoers (5)          - default sudo security policy plugin
sudoreplay (8)       - replay sudo session logs
visudo (8)           - edit the sudoers file
```

🌐 Ayuda online

Si un comando te resulta muy confuso, puedes usar esta herramienta:

👉 https://explainshell.com/

Solo copia un comando largo, y te explicará cada parte.
🧠 Consejo final

A lo largo del curso usaremos muchos comandos nuevos, ¡pero ahora ya sabes cómo investigar por tu cuenta!

Tómate tu tiempo para explorar, probar y experimentar, incluso equivocarte es parte del aprendizaje.
Siempre valdrá la pena.
✅ Comandos clave de esta lección

man <comando>
<comando> --help
<comando> -h
apropos <palabra>

