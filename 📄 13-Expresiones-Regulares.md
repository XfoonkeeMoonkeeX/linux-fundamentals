Las expresiones regulares (RegEx) son como el arte de crear planos precisos para buscar patrones en texto o archivos. Te permiten encontrar, reemplazar y manipular datos con una precisión increíble. Piensa en las expresiones regulares como un filtro altamente personalizable que te permite examinar cadenas de texto, buscando exactamente lo que necesitas, ya sea para analizar datos, validar entradas o realizar operaciones de búsqueda avanzadas.

En su núcleo, una expresión regular es una secuencia de caracteres y símbolos que juntas forman un patrón de búsqueda. Estos patrones a menudo involucran símbolos especiales llamados metacaracteres, que definen la estructura de la búsqueda en lugar de representar texto literal. Por ejemplo, los metacaracteres te permiten especificar si estás buscando dígitos, letras o cualquier carácter que se ajuste a un patrón determinado.

Las expresiones regulares están disponibles en muchos lenguajes de programación y herramientas, como `grep` o `sed`, lo que las convierte en una herramienta versátil y poderosa en nuestro conjunto de herramientas.

# Agrupación

Entre otras cosas, las expresiones regulares nos ofrecen la posibilidad de agrupar los patrones de búsqueda deseados. Básicamente, las expresiones regulares siguen tres conceptos diferentes, que se distinguen por los tres tipos de corchetes:

## Operadores de Agrupación

| Operador | Descripción |
|----------|-------------|
| (a)      | Los paréntesis redondos se utilizan para agrupar partes de una expresión regular. Dentro de los paréntesis, puedes definir patrones adicionales que deben procesarse juntos. |
| [a-z]    | Los corchetes cuadrados se utilizan para definir clases de caracteres. Dentro de los corchetes, puedes especificar una lista de caracteres para buscar. |
| {1,10}   | Los corchetes curvos se utilizan para definir cuantificadores. Dentro de los corchetes, puedes especificar un número o un rango que indique cuántas veces debe repetirse un patrón anterior. |
| \|       | También llamado el operador OR, muestra resultados cuando una de las dos expresiones coincide. |
| .*       | Opera de manera similar a un operador AND, mostrando resultados solo cuando ambas expresiones están presentes y coinciden en el orden especificado. |

Supongamos que usamos el operador OR. La expresión regular busca uno de los parámetros de búsqueda dados. En el siguiente ejemplo, buscamos líneas que contengan la palabra "my" o "false". Para usar estos operadores, necesitas aplicar la expresión regular extendida usando la opción `-E` en `grep`.

# Operador OR  
## Expresiones Regulares

```bash
cry0l1t3@htb:~$ grep -E "(my|false)" /etc/passwd

Salida:

lxd:x:105:65534::/var/lib/lxd/:/bin/false
pollinate:x:109:1::/var/cache/pollinate:/bin/false
mysql:x:116:120:MySQL Server,,:/nonexistent:/bin/false

Dado que uno de los dos parámetros de búsqueda aparece en cada una de las tres líneas, todas ellas se muestran como resultado.

Sin embargo, si utilizamos el operador AND, obtendremos un resultado diferente usando los mismos parámetros de búsqueda.

# Operador AND  
## Expresiones Regulares

```bash
cry0l1t3@htb:~$ grep -E "(my.*false)" /etc/passwd

Salida:

mysql:x:116:120:MySQL Server,,:/nonexistent:/bin/false

Básicamente, lo que estamos indicando con este comando es que estamos buscando una línea donde aparezcan tanto my como false.

Una forma simplificada de hacer esto sería usar grep dos veces, de la siguiente manera:

cry0l1t3@htb:~$ grep -E "my" /etc/passwd | grep -E "false"

Salida:

mysql:x:116:120:MySQL Server,,:/nonexistent:/bin/false


# Ejercicios Opcionales con Expresiones Regulares

Aquí tienes algunas tareas opcionales para ayudarte a practicar expresiones regulares (RegEx) y mejorar tu habilidad para manejarlas de manera más efectiva. Estos ejercicios utilizarán el archivo `/etc/ssh/sshd_config` en tu instancia de **Pwnbox**, lo que te permitirá explorar aplicaciones reales de las expresiones regulares en un archivo de configuración. Al completar estas tareas, obtendrás experiencia práctica trabajando con patrones, búsqueda y manipulación de texto en escenarios reales.

## Ejercicios:

1. Mostrar todas las líneas que **no** contienen el carácter `#`.
2. Buscar todas las líneas que contengan una palabra que **comience con** `Permit`.
3. Buscar todas las líneas que contengan una palabra que **termine con** `Authentication`.
4. Buscar todas las líneas que contengan la palabra `Key`.
5. Buscar todas las líneas que **comiencen con** `Password` y contengan `yes`.
6. Buscar todas las líneas que **terminen con** `yes`.