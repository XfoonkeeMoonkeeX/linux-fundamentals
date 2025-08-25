## El Prompt de Bash: Tu Centro de Comandos Personalizado

El **prompt de Bash** es la línea de texto que aparece en la terminal para indicarte que el sistema está listo para recibir comandos. Lejos de ser un simple elemento estético, es una herramienta funcional que te proporciona información crucial de un solo vistazo.

Por defecto, el prompt suele mostrar:

*   👤 Tu **nombre de usuario** (`username`).
*   🖥️ El **nombre del equipo** o *hostname* (`hostname`).
*   📁 El **directorio actual** en el que te encuentras.

Esta información está seguida por un cursor (una línea o bloque parpadeante) que espera tus instrucciones.

---

### 🧱 Estructura y Símbolos Clave

La estructura más común sigue este formato:

```bash
usuario@equipo:directorio$
```

Cuando te encuentras en tu directorio personal (home), la ruta se abrevia con el símbolo de la virgulilla (`~`):

```bash
usuario@equipo:~$
```

🔒 Un detalle fundamental es el símbolo final, que indica el nivel de privilegios del usuario:
*   `$` **Usuario estándar:** Indica que estás operando como un usuario sin privilegios elevados.
*   `#` **Usuario root:** Advierte que tienes privilegios de superusuario, lo que significa que tus comandos pueden afectar profundamente al sistema.

```bash
root@equipo:/htb#
```

Si en alguna ocasión, al obtener una shell remota, solo ves un `$` o `#` sin más información, es una señal de que la variable de entorno que define el prompt no está configurada correctamente.

### 🔧 La Variable `PS1`: El Corazón del Prompt

El aspecto y la información de tu prompt se definen en la variable de entorno `PS1` (Prompt String 1). Esta variable funciona como una plantilla que la shell interpreta cada vez que va a mostrar el prompt.

Para ver la configuración actual de tu `PS1`, puedes usar el comando:

```bash
echo $PS1```

Aunque `PS1` es la variable principal, existen otras que se usan en contextos específicos, como `PS2`, que se muestra cuando un comando necesita más información (por ejemplo, en un script multilínea).

#### 🧪 Caracteres Especiales para Personalizar `PS1`

Puedes personalizar el contenido del prompt utilizando secuencias de escape (caracteres especiales precedidos por una barra invertida `\`). Estas secuencias se reemplazarán dinámicamente con la información correspondiente:

| Carácter | Significado |
| :--- | :--- |
| `\u` | Nombre del usuario actual. |
| `\h` | Nombre del equipo (hostname) hasta el primer punto. |
| `\H` | Nombre completo del equipo (hostname). |
| `\w` | Ruta completa del directorio actual (ej: `/home/usuario/docs`). |
| `\W` | Nombre base del directorio actual, sin la ruta completa (ej: `docs`). |
| `\d` | Fecha en formato "Día Mes DíaNum" (ej: "Lun Ago 25"). |
| `\t` | Hora actual en formato 24h (HH:MM:SS). |
| `\T` | Hora actual en formato 12h (HH:MM:SS). |
| `\@` | Hora actual en formato 12h con AM/PM. |
| `\n` | Inserta un salto de línea, útil para prompts multilínea. |
| `\s` | El nombre de la shell (ej: `bash`). |
| `\$` | Muestra `$` si eres usuario normal, o `#` si eres `root`. |
| `\!` | El número del comando en el historial. |

#### 🖌️ Personalización Visual: Añadiendo Colores

Además de la información, puedes añadir colores usando códigos de escape ANSI para mejorar la legibilidad. La estructura básica es `\[\e[CódigoColor_m\]` para iniciar un color y `\[\e[0m\]` para resetearlo.

*   **Importante:** Los códigos de color deben encerrarse entre `\[` y `\]` para indicarle a Bash que no ocupan espacio en la línea, evitando problemas de posicionamiento del cursor.

**Ejemplo práctico:** Un prompt verde con `[usuario@equipo]` seguido de la ruta en azul.

```bash
export PS1="\[\e[0;32m\][\u@\h]\[\e[0m\]:\[\e[0;34m\]\w\[\e[0m\]\$ "
```

Para que estos cambios sean permanentes, debes añadir la línea `export PS1="..."` al final de tu archivo de configuración `~/.bashrc` (o `~/.bash_profile` en macOS).

### 🚀 Herramientas para una Personalización Avanzada

Aunque la personalización manual es potente, existen herramientas que facilitan la creación de prompts modernos y funcionales con información adicional (como el estado de repositorios Git, versiones de lenguajes de programación, etc.).

*   **Powerline:** Un plugin popular que crea prompts con un estilo de "línea de estado", usando símbolos especiales y colores para delimitar segmentos.
*   **Starship:** Un personalizador de prompts minimalista, rápido y altamente configurable. Es compatible con múltiples shells, no solo con Bash.
*   **Oh My Posh:** Otra herramienta multiplataforma y altamente personalizable que permite crear temas visuales muy elaborados.
*   **Bash Prompt Generator:** Sitios web que ofrecen una interfaz gráfica para arrastrar y soltar elementos, generando el código `PS1` automáticamente.

### 📌 Conclusión: Una Herramienta Estratégica

El prompt de Bash es mucho más que una simple decoración. Personalizarlo te permite tener un **contexto claro y constante** de quién eres, dónde estás y con qué privilegios operas. En entornos complejos como la administración de sistemas o durante pruebas de penetración, un prompt bien configurado mejora la eficiencia, reduce errores y ayuda a mantener una organización impecable de tu trabajo.
