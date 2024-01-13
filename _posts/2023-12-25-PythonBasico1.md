---
title: "PythonBasico1"
date: 2023-12-25
categories: [Programming_Language]
tags: [python]
image:
    path: /assets/PythonBasico/pythonbasico.jpg
---

# Python Básico (1/2): Conceptos


## Algunas reglas de Python:

**Palabras clave**: Los argumentos de palabras clave deben pasarse **después**
 de cualquier argumento posicional requerido.

**Escritura de números enteros**: No se puede ingresar un número de esta forma: **`11,111,111`** o `**11 111 111`.** Es claro que la separación hace que sea más fácil de leer, especialmente cuando el número tiene demasiados dígitos. Sin embargo, Python no acepta estas cosas, está **prohibido**. ¿Qué es lo que Python permite?. El uso de **guion bajo** en los literales numéricos.*

Por lo tanto, el número se puede escribir ya sea así: **`11111111`**, o como sigue: **`11_111_111`** y si es negativo: **`-11111111`**, o **`-11_111_111`**.

*Python 3.6 ha introducido el guion bajo en los literales numéricos, permitiendo colocar un guion bajo entre dígitos y después de especificadores de base para mejorar la legibilidad. Esta característica no está disponible en versiones anteriores de Python.*

**Escritura de números de coma flotante**: Se debe utilizar el punto para representar números flotantes y no coma ya que Python no lo aceptará, o (en casos poco probables) puede malinterpretar el número, debido a que la coma tiene su propio significado en Python.

En caso de escribir un número flotante con cero, eje: “4.0” o “0.4”, se podría escribir de la siguiente forma: “4.” y “.4”. Esto no cambiará su tipo ni su valor.

**Nombres de variables**: Si se desea **nombrar una variable**, se deben seguir las siguientes reglas:

- El nombre de la variable debe de estar compuesto por MAYÚSCULAS, minúsculas, dígitos, y el carácter _ (guion bajo)
- El nombre de la variable debe comenzar con una letra;
- El carácter guion bajo es considerado una letra;
- Las mayúsculas y minúsculas se tratan de forma distinta (un poco diferente que en el mundo real - *Alicia* y *ALICIA* son el mismo nombre, pero en Python son dos nombres de variable distintos, subsecuentemente, son dos variables diferentes);
- El nombre de las variables no pueden ser igual a alguna de las palabras reservadas de Python (las palabras clave - explicará más de esto pronto).

Nota que la misma restricción aplica a los nombres de funciones.

Python no impone restricciones en la longitud de los nombres de las variables, pero eso no significa que un nombre de variable largo sea mejor que uno corto.

Python te permite usar no solo letras latinas sino también caracteres específicos de idiomas que usan otros alfabetos.

- **Palabras clave**
    
    **Palabras clave**: Son llamadas palabras clave o (mejor dicho) palabras clave reservadas. Son reservadas porque no se deben utilizar como nombres: ni para variables, ni para funciones, ni para cualquier otra cosa que se desee crear: 
    
    **`['False', 'None', 'True', 'and', 'as', 'assert', 'break', 'class', 'continue', 'def', 'del', 'elif', 'else', 'except', 'finally', 'for', 'from', 'global', 'if', 'import', 'in', 'is', 'lambda', 'nonlocal', 'not', 'or', 'pass', 'raise', 'return', 'try', 'while', 'with', 'yield']`**
    
- **Recomendaciones**
    
    El [PEP 8 -- Style Guide for Python Code](https://www.python.org/dev/peps/pep-0008/) recomienda la siguiente convención de nomenclatura para variables y funciones en Python:
    
    - Los nombres de las variables deben estar en minúsculas, con palabras separadas por guiones bajos para mejorar la legibilidad (por ejemplo, var, my_variable)
    - Los nombres de las funciones siguen la misma convención que los nombres de las variables (por ejemplo, fun, my_function)
    - También es posible usar letras mixtas (por ejemplo, myVariable), pero solo en contextos donde ese ya es el estilo predominante, para mantener la compatibilidad retroactiva con la convención adoptada.

**Función range()**:

- Si el conjunto generado por la función range() está vacío, el bucle no ejecutará su cuerpo en absoluto.
- La función range() **solo acepta enteros como argumentos** y genera secuencias de enteros.
- El conjunto generado por range() debe ordenarse en **un orden ascendente**. No hay forma de forzar el range() para crear un conjunto en una forma diferente. Esto significa que el segundo argumento de range() debe ser mayor que el primero.

### Conceptos:

**IL (Instruction List)**: Un conjunto completo de comandos conocidos se llama lista de instrucciones, a veces abreviada IL (por sus siglas en inglés). Los diferentes tipos de computadoras pueden variar según el tamaño de sus IL y las instrucciones pueden ser completamente diferentes en diferentes modelos.

El IL es, de hecho, el **alfabeto de un lenguaje máquina**.

**Lenguajes naturales**: Ninguna computadora es actualmente capaz de crear un nuevo idioma o lenguaje. Sin embargo, eso puede cambiar pronto. Por otro lado, las personas también usan varios idiomas muy diferentes, pero estos idiomas se crearon ellos mismos. Además, todavía están evolucionando.

Cada día se crean nuevas palabras y desaparecen las viejas. Estos lenguajes se llaman **lenguajes naturales**.

**¿Qué compone a un lenguaje?**: Podemos decir que cada lenguaje (máquina o natural, no importa) consta de los siguientes elementos:

**Alfabeto**: un conjunto de símbolos utilizados para formar palabras de un determinado lenguaje (por ejemplo, el alfabeto latino para el inglés, el alfabeto cirílico para el ruso, el kanji para el japonés, y así sucesivamente)

**Léxico:** (también conocido como diccionario) un conjunto de palabras que el lenguaje ofrece a sus usuarios (por ejemplo, la palabra "computadora" proviene del diccionario en inglés, mientras que "cmoptrue" no; la palabra "chat" está presente en los diccionarios de inglés y francés, pero sus significados son diferentes)

**Sintaxis**: un conjunto de reglas (formales o informales, escritas o interpretadas intuitivamente) utilizadas para precisar si una determinada cadena de palabras forma una oración válida (por ejemplo, "Soy una serpiente" es una frase sintácticamente correcta, mientras que "Yo serpiente soy una" no lo es)

**Semántica:** un conjunto de reglas que determinan si una frase tiene sentido (por ejemplo, "Me comí una dona" tiene sentido, pero "Una dona me comió" no lo tiene)

**Lenguaje de alto nivel**: Son al menos algo similares a los naturales en que usan símbolos, palabras y convenciones legibles para los humanos. Estos lenguajes permiten a los humanos expresar comandos a las computadoras que son mucho más complejos que los que ofrecen las IL.

Un programa escrito en un lenguaje de programación de alto nivel se denomina código fuente ( en contraste con el código máquina ejecutado por las computadoras). Del mismo modo, el archivo que contiene el código fuente se denomina archivo fuente.

**Diferencia de lenguaje compilado y lenguaje interpretado**: La principal diferencia entre un lenguaje compilado y uno interpretado radica en el proceso de traducción del código fuente a lenguaje de máquina.

En un lenguaje compilado, el código fuente se traduce en su totalidad a lenguaje de máquina antes de ser ejecutado. Esto se realiza utilizando un programa llamado compilador, que analiza el código fuente y lo traduce a un archivo ejecutable o binario. Este archivo binario contiene el código de máquina directamente y es lo que se ejecuta directamente por la CPU. Ejemplos de lenguajes compilados incluyen C++, Java, y Swift.

Por otro lado, en un lenguaje interpretado, el código fuente no se traduce en su totalidad a lenguaje de máquina antes de ser ejecutado. En su lugar, se utiliza un programa llamado intérprete que lee el código fuente línea por línea y lo ejecuta de inmediato. Cada vez que se ejecuta una línea de código, el intérprete lo traduce a lenguaje de máquina y lo ejecuta en el momento. Ejemplos de lenguajes interpretados incluyen Python, Ruby y JavaScript.

- **Ventajas de los lenguaje compilados**
    - Mayor eficiencia en tiempo de ejecución: los programas compilados generalmente se ejecutan más rápido que los programas interpretados porque no hay una traducción en tiempo real de código fuente a código de máquina durante la ejecución.
    - Detección temprana de errores: el proceso de compilación verifica la sintaxis y la semántica del código y, por lo tanto, puede detectar errores antes de que se ejecute el programa.
    - Mayor control de memoria: los lenguajes compilados a menudo ofrecen un mayor control sobre la asignación y la liberación de memoria que los lenguajes interpretados, lo que puede ser útil en programas que manejan grandes cantidades de datos.
    - Mayor portabilidad: los programas compilados se pueden ejecutar en diferentes plataformas sin necesidad de instalar un intérprete o entorno específico.
- **Ventajas de los lenguajes interpretado**
    - Mayor facilidad de depuración: en los lenguajes interpretados, los errores de tiempo de ejecución a menudo muestran mensajes detallados y precisos que facilitan la depuración del código.
    - Mayor flexibilidad: los lenguajes interpretados a menudo son más flexibles que los compilados, lo que permite una mayor interacción con otros programas y sistemas operativos.
    - Mayor facilidad de aprendizaje: los lenguajes interpretados a menudo tienen una sintaxis más sencilla y un conjunto de características más pequeño que los lenguajes compilados, lo que los hace más fáciles de aprender para los principiantes.
    - Mayor rapidez en la edición y el desarrollo: en los lenguajes interpretados, se puede editar el código fuente en tiempo real y ver los resultados inmediatamente, lo que puede acelerar el proceso de desarrollo.

**¿De dónde proviene el nombre Python?** Aunque puede que conozcas a la pitón como una gran serpiente, el nombre del lenguaje de programación Python proviene de una vieja serie de comedia de la BBC llamada Monty Python's Flying Circus.

**¿Con qué lenguaje se creó Python?** Python es un lenguaje de programación de alto nivel que se diseñó y creó originalmente utilizando el lenguaje de programación ABC en 1989, y luego se reescribió en el lenguaje de programación "C" en la versión 2.0. La decisión de utilizar "C" para la implementación de Python se debió a que "C" es un lenguaje de bajo nivel que permite que el intérprete de Python se ejecute más rápido y se pueda portar a diferentes plataformas de manera más sencilla. Por lo tanto, aunque Python no se creó directamente con "C", la implementación actual de Python, que es la más utilizada, está escrita en "C".

**¿Qué es CPython?** CPython es la implementación principal de Python y se refiere a la versión del intérprete de Python escrita en el lenguaje de programación C. Fue desarrollada por el creador original de Python, Guido van Rossum, y su equipo en la Python Software Foundation (PSF).

CPython se utiliza comúnmente como el intérprete de Python predeterminado y se distribuye con la mayoría de las distribuciones de Python. Proporciona la base para muchas de las bibliotecas y módulos de Python que se utilizan en una amplia variedad de aplicaciones y proyectos.

Además, CPython es la implementación de referencia de Python y establece el estándar para otras implementaciones alternativas, como PyPy, Jython e IronPython. Como CPython es la implementación más utilizada de Python, muchas de las bibliotecas y herramientas de terceros se han creado específicamente para funcionar con CPython.

En resumen, CPython es la implementación de Python más popular y se utiliza ampliamente para desarrollar aplicaciones y programas en Python. Es una parte fundamental del ecosistema de Python y proporciona la base para muchas de las herramientas y bibliotecas que hacen de Python un lenguaje de programación versátil y poderoso.

**¿Qué es una función?** Una función (en este contexto) es una parte separada del código de computadora el cual es capaz de:

- **causar algún efecto** (por ejemplo, enviar texto a la terminal, crear un archivo, dibujar una imagen, reproducir un sonido, etc.); esto es algo completamente inaudito en el mundo de las matemáticas.
- **evaluar un valor** (por ejemplo, la raíz cuadrada de un valor o la longitud de un texto dado) y **devolverlo como el resultado de la función**; esto es lo que hace que las funciones de Python sean parientes de los conceptos matemáticos.

Además, muchas de las funciones de Python pueden hacer las dos cosas anteriores juntas.

La funciones pueden provenir de Python mismo, por ejemplo la función “print”, de otros módulos y del mismo código, es decir, funciones propias que crea el usuario, eje: “def funcion1()”.

**¿Qué es un argumento?** En Python, los argumentos de una función son valores que se pasan a la función para ser utilizados en su ejecución. Los argumentos son esencialmente los datos que se proporcionan a una función para que realice una tarea específica.

Hay dos tipos de argumentos en Python: los argumentos posicionales y los argumentos con palabras clave.

- Argumentos posicionales: estos son los argumentos que se pasan a la función en el orden en que se definen en la declaración de la función. Por ejemplo, si una función está definida como **`def suma(a, b):`**, los argumentos posicionales se pasarían en el orden **`a`** y luego **`b`**, como en **`suma(3, 5)`**.
- Argumentos con palabras clave: estos son los argumentos que se pasan a la función con su nombre correspondiente, seguido de un signo igual. Estos argumentos se pueden pasar en cualquier orden. Por ejemplo, si la misma función **`suma()`** se define como **`def suma(a, b, c=0):`**, el argumento **`c`** es un argumento con palabras clave con un valor predeterminado de **`0`**. Este argumento se puede omitir en la llamada de la función y se utilizará el valor predeterminado. Los argumentos con palabras clave también se pueden proporcionar en cualquier orden, por ejemplo: **`suma(a=3, b=5)`**.
    
    ## *En Python, no hay un límite máximo para el número de argumentos que se pueden pasar a una función. Se pueden pasar tantos argumentos como se necesiten, siempre y cuando la función esté diseñada para manejarlos.*
    

**¿Qué sucede cuando Python encuentra una invocación como la que está a continuación?** 

```python
function_name(argument)
```

Veamos:

- Primero, Python comprueba si el nombre especificado es **legal** (explora sus datos internos para encontrar una función existente del nombre; si esta búsqueda falla, Python aborta el código)
- En segundo lugar, Python comprueba si los requisitos de la función para el número de argumentos **le permiten invocar** la función de esta manera (por ejemplo, si una función específica exige exactamente dos argumentos, cualquier invocación que entregue solo un argumento se considerará errónea y abortará la ejecución del código)
- Tercero, Python **deja el código por un momento** y salta dentro de la función que se desea invocar; por lo tanto, también toma los argumento(s) y los pasa a la función;
- Cuarto, la función **ejecuta el código**, provoca el efecto deseado (si lo hubiera), evalúa el (los) resultado(s) deseado(s) y termina la tarea;
- Finalmente, Python **regresa al código** (al lugar inmediatamente después de la invocación) y reanuda su ejecución.

**¿Qué son los Literales?** Un literal se refiere a datos cuyos valores están determinados por el literal mismo.

Eje, Observa los siguientes dígitos:

`123`

¿Puedes adivinar qué valor representa? Claro que puedes - es *ciento veintitrés*.

Que tal este:

`c`

¿Representa algún valor? Tal vez. Puede ser el símbolo de la velocidad de la luz, por ejemplo. También puede representar la constante de integración. Incluso la longitud de una hipotenusa en el Teorema de Pitágoras. Existen muchas posibilidades.

No se puede elegir el valor correcto sin algo de conocimiento adicional.

Y esta es la pista: ”123” es un literal, y ”c” no lo es.

Se utilizan literales **para codificar datos y ponerlos dentro del código.**

- **Tipos de Literales**
    
    String
    
    ```python
    print("2")
    ```
    
    Entero
    
    ```python
    print(2)
    ```
    
    La función **`print()`** los muestra exactamente de la misma manera - Sin embargo, internamente, la memoria de la computadora los almacena de dos maneras completamente diferentes - La cadena existe como eso solo una cadena - una serie de letras.
    
    El número es convertido a una representación máquina (una serie de bits). La función print() es capaz de mostrar ambos en una forma legible para humanos.
    

## *Existe un literal especial más utilizado en Python: el literal None. Este literal es llamado un objeto de NoneType, y puede ser utilizado para representar la ausencia de un valor. Pronto se hablará más acerca de ello.*

**Números flotantes**: El punto decimal es esencialmente importante para reconocer números punto-flotantes en Python.

Por otro lado, no solo el punto hace que un número sea flotante. Se puede utilizar la letra **`e`**.

Cuando se desea utilizar números que son muy pequeños o muy grandes, se puede implementar la **notación científica**.

- **Ejemplos**
    
    Eje#1: La velocidad de la luz, expresada en *metros por segundo*. Escrita directamente se vería de la siguiente manera: **`300000000`**.
    
    Para evitar escribir tantos ceros, los libros de texto emplean la forma abreviada, la cual probablemente hayas visto: **`3 x 108`**.
    
    Se lee: tres por diez elevado a la octava potencia.
    
    En Python, el mismo efecto puede ser logrado de una manera similar - observa lo siguiente:
    
    ```
    3E8
    ```
    
    La letra **`E`** (también se puede utilizar la letra minúscula **`e`** - proviene de la palabra **exponente**) la cual significa *por diez a la n potencia*.
    
    Eje#2: Una constante de física denominada *La Constante de Planck* (denotada como *h*), de acuerdo con los libros de texto, tiene un valor de: **6.62607 x 10-34**.
    
    Si se quisiera utilizar en un programa, se debería escribir de la siguiente manera:
    
    6.62607E-34
    
    Nota: el hecho de que se haya escogido una de las posibles formas de codificación de un valor flotante no significa que Python lo presentará de la misma manera.
    
    Python podría en ocasiones elegir una **notación diferente**.
    
    Por ejemplo, supongamos que se ha elegido utilizar la siguiente notación:
    
    0.0000000000000000000001
    
    Cuando se corre en Python:
    
    *`print*(0.0000000000000000000001) **Output**`
    
    este es el resultado:
    
    `1e-22**Output**`
    
    Python siempre elige **la presentación más corta del número**, y esto se debe de tomar en consideración al crear literales.
    
    Nota:
    
    - El **exponente** (el valor después de la *E*) debe ser un valor entero;
    - La **base** (el valor antes de la *E*) puede ser un valor entero o flotante.

**Cadenas**: Para codificar un apóstrofe o una comilla dentro de una cadena se puede utilizar el carácter de escape, por ejemplo, **`'I\'m happy.'`**, o abrir y cerrar la cadena utilizando un conjunto de símbolos distintos al símbolo que se desea codificar, por ejemplo, **`"I'm happy."`** para codificar un apóstrofe, y **`'El dijo "Python", no "typhoon"'`** para codificar comillas.

**Valores Booleanos**: son dos objetos constantes **True** y **False** empleados para representar valores de verdad (en contextos numéricos 1 es True, mientras que 0 es False.

**Enlazado de lado izquierdo o derecho**: Quiere decir que la expresión será ejecutada de izquierda a derecha (enlazado izquierdo) o de derecha a izquierda (enlazado derecho).

**Operadores básicos**: Los **operadores** son símbolos especiales o palabras clave que son capaces de operar en los valores y realizar operaciones matemáticas, por ejemplo, el * multiplica dos valores: x * y.

- **TABLA: Jerarquía de prioridades**
    
    
    | Prioridad | Operador |  |
    | --- | --- | --- |
    | 1 | ** |  |
    | 2 | +, - (nota: los operadores unarios a la derecha del operador exponencial enlazan con mayor fuerza.) | unario |
    | 3 | *, /, //, % |  |
    | 4 | +, - | binario |
    | 5 | <<, >> |  |
    | 6 | <, <=, >, >= |  |
    | 7 | ==, != |  |
    | 8 | & |  |
    | 9 | | |  |
    | 10 | =, +=, -=, *=, /=, %=, &=, ^=, |=, >>=, <<= |  |
    - El operador ** (exponencial) tiene la prioridad más alta;
    - Posteriormente los operadores unarios + y - (nota: los operadores unarios a la derecha del operador exponencial enlazan con mayor fuerza, por ejemplo 4 ** -1 es igual a 0.25)
    - Después *, /, //, y %,
    - Finalmente, la prioridad más baja: los operadores binarios + y -.
    
    **Operadores y sus enlaces**
    
    El **enlace** de un operador determina el orden en que se computan las operaciones de los operadores con la misma prioridad, los cuales se encuentran dentro de una misma expresión.
    
    La mayoría de los operadores de Python tienen un enlazado hacia la izquierda, lo que significa que el cálculo de la expresión es realizado de izquierda a derecha.
    
    Ejemplo de una excepción:
    
    ```python
    print(2 ** 2 ** 3)
    
    Resultado
    256
    ```
    
    Los dos posibles resultados son:
    
    - `2 ** 2` → `4`; `4 ** 3` → `64`
    - `2 ** 3` → `8`; `2 ** 8` → `256`
    
    El resultado muestra claramente que **el operador de exponenciación utiliza enlazado del lado derecho**.
    
- Cuando **ambos** **`**`** argumentos son enteros, el resultado es entero, también;
- Cuando **al menos un `**`** argumento es flotante, el resultado también es flotante.
- El valor después de la **diagonal `/`** es el dividendo, el valor antes de la diagonal es el divisor.
    - **El resultado producido por el operador de división siempre es flotante**, sin importar si a primera vista el resultado es flotante: **`1 / 2`**, o si parece ser completamente entero: **`2 / 1`**.
- Un símbolo de **`//`** (doble diagonal) es un operador de **división entera**. Difiere del operador estándar **`/`** en dos detalles:
    - El resultado carece de la parte fraccionaria, está ausente (para los enteros), o siempre es igual a cero (para los flotantes); esto significa que **los resultados siempre son redondeados**;
    - Se ajusta a la regla *entero vs flotante*.
    - El resultado de la división entera siempre se redondea al valor entero inferior mas cercano del resultado de la división no redondeada.
    - La division entera también se le suele llamar en inglés **floor division**.
- El símbolo **`%`** sirve para obtener el **residuo que queda de una división entera**.
    - El operador en ocasiones también es denominado **módulo** en otros lenguajes de programación.
- En aplicaciones de resta **`-`**, el **operador de resta espera dos argumentos**: el izquierdo (un **minuendo** en términos aritméticos) y el derecho (un **sustraendo**).

**Diferencia de operadores unarios y binarios**: Los operadores unarios realizan una acción con un solo operando. Los operadores binarios realizan acciones con dos operandos.

eje: 

```python
Operador binario:
print(2. + 2)
print(-4 - 6)

Operador unario:
print(+2)
print(-1.1)
```

**Operadores abreviados**: 

- **Operadores abreviados**
    
    Es tiempo de explicar el siguiente conjunto de operadores que harán la vida del programador/desarrollador más fácil. Muy seguido, se desea utilizar la misma variable al lado derecho y al lado izquierdo del operador **`=`** operator.
    
    Por ejemplo, si se necesita calcular una serie de valores sucesivos de la potencia de 2, se puede usar el siguiente código:
    
    `12x = x * 2`
    
    También, puedes utilizar una expresión como la siguiente si no puedes dormir y estas tratando de resolverlo con alguno de los métodos tradicionales:
    
    `12sheep = sheep + 1`
    
    Python ofrece una manera más corta de escribir operaciones como estas, lo cual se puede codificar de la siguiente manera:
    
    `123x *= 2sheep += 1`
    
    A continuación se intenta presentar una descripción general para este tipo de operaciones. Si op es un operador de dos argumentos (esta es una condición muy importante) y el operador es utilizado en el siguiente contexto...:
    
    **`variable = variable op expresión`**
    
    ...entonces se puede simplificar y mostrar de la siguiente manera:
    
    **`variable op= expresión`**
    
    Observa los siguientes ejemplos. Asegúrate de entenderlos todos.
    
    | Expresión | Operador abreviado |
    | --- | --- |
    | i = i + 2 * j | i += 2 * j |
    | var = var / 2 | var /= 2 |
    | rem = rem % 10 | rem %= 10 |
    | j = j - (i + var + rem) | j -= (i + var + rem) |
    | x = x ** 2 | x **= 2 |
    

**Variable**: es una ubicación nombrada reservada para almacenar valores en la memoria. Una variable es creada o inicializada automáticamente cuando se le asigna un valor por primera vez.

Se les puede asignar valores nuevos a variables ya existentes utilizando el operador de asignación o un operador abreviado, por ejemplo:

```python
var = 2
print(var)
 
var = 3
print(var)
 
var += 1
print(var)

var1, var2, var3 = 5, 10, 15
print(var1 + var2 + var3)
```

**Comentarios**: Los comentarios pueden ser utilizados para colocar información adicional en el código. Son omitidos al momento de la ejecución. Dicha información es para los lectores que están manipulando el código. En Python, un comentario es un fragmento de texto que comienza con un #. El comentario se extiende hasta el final de la línea.

Es importante utilizar los comentarios para que los programas sean más fáciles de entender, además de emplear variables legibles y significativas en el código. Sin embargo, es igualmente importante **no utilizar** nombres de variables que sean confusos, o dejar comentarios que contengan información incorrecta.

Los comentarios pueden ser muy útiles cuando *tú* estás leyendo tu propio código después de un tiempo (es común que los desarrolladores olviden lo que su propio código hace), y cuando *otros* están leyendo tu código (les puede ayudar a comprender que es lo que hacen tus programas y como es que lo hacen).

**Función input()**: La función input() es capaz de leer datos que fueron introducidos por el usuario y pasar esos datos al programa en ejecución.

El **resultado de la función input() es una cadena**. Una cadena que contiene todos los caracteres que el usuario introduce desde el teclado. No es un entero ni un flotante, esto significa que **no se debe utilizar como un argumento para operaciones matemáticas**

**Función str()**: Se utiliza para convertir un número a una cadena.

**Función type()**: Se utiliza para conocer el tipo de dato que se está usando si es str, int, float, etc. eje:

```python
x = str(1)
y = float(2)
z = 3

print(type(x))
print(type(y))
print(type(z))

Resultado:
<class 'str'>
<class 'float'>
<class 'int'>
```

**Conversión de tipos**: Python ofrece dos simples funciones para especificar un tipo de dato y resolver este problema, aquí están: int() y float().

Sus nombres indican cual es su función:

- La función int() **toma un argumento** (por ejemplo, una cadena: int(string)) e intenta convertirlo a un valor entero; si llegase a fallar, el programa entero fallará también (existe una manera de solucionar esto, se explicará mas adelante);
- La función float() toma un argumento (por ejemplo, una cadena: float(string)) e intenta convertirlo a flotante (el resto es lo mismo).

Esto es muy simple y muy efectivo. Sin embargo, estas funciones se pueden invocar directamente pasando el resultado de la función input() directamente. No hay necesidad de emplear variables como almacenamiento intermedio.

```python
anything = float(input("Ingresa un número: "))
something = anything ** 2.0
print(anything, "a la potencia de 2 es", something)
```

**Operadores de cadena**: Es tiempo de regresar a estos dos operadores aritméticos: + y *.

Ambos tienen una función secundaría. Son capaces de hacer algo más que **sumar** y **multiplicar**.

El signo de + (más), al ser aplicado a dos cadenas, se convierte en **un operador de concatenación**:

```python
string + string
```

Simplemente **concatena** (junta) dos cadenas en una. Por supuesto, puede ser utilizado más de una vez en una misma expresión, y en tal contexto se comporta con enlazado del lado izquierdo.

En contraste con el operador aritmético, el operador de concatenación **no es conmutativo**, por ejemplo, "ab" + "ba" no es lo mismo que "ba" + "ab".

No olvides, si se desea que el signo + sea un **concatenador**, no un sumador, solo se debe asegurar que **ambos argumentos sean cadenas**.

No se pueden mezclar los tipos de datos aquí.

El signo de * (asterisco), cuando es aplicado a una cadena y a un número (o a un número y cadena) se convierte en un **operador de replicación**:

```python
string * number
number * string
```

Un número menor o igual a cero produce una **cadena vacía**.

- **Ejemplo de dibujo de rectángulo**
    
    ```python
    print("+" + 10 * "-" + "+")
    print(("|" + " " * 10 + "|\n") * 5, end="")
    print("+" + 10 * "-" + "+")
    ```
    

**Operadores de comparación**: Un programador escribe un programa y **el programa hace preguntas**.

Una computadora ejecuta el programa y **proporciona las respuestas**. El programa debe ser capaz de **reaccionar de acuerdo con las respuestas recibidas**.

Afortunadamente, las computadoras solo conocen dos tipos de respuestas:

- Si, es cierto.
- No, esto es falso.

- **Igualdad: El operador *igual a* (==)**
    
    Pregunta: ¿**son dos valores iguales**?
    
    Para hacer esta pregunta, se utiliza el **`==`** (igual igual) operador.
    
    No olvides esta importante distinción:
    
    - = es un **operador de asignación**, por ejemplo, a = b asigna a la varable a el valor de b;
    - == es una pregunta *¿Son estos valores iguales?* así que a == b **compara** a y b.
    
    Es un **operador binario con enlazado del lado izquierdo**. Necesita dos argumentos y **verifica si son iguales**.
    
    El operador == (igual a) compara los valores de dos operandos. Si son iguales, el resultado de la comparación es **`True`**. Si no son iguales, el resultado de la comparación es **`False`**.
    
    eje:
    
    ```python
    #Pregunta #1: ¿Cuál es el resultado de la siguiente comparación?
    print(2 == 2)
    # R/ True - por supuesto, 2 es igual a 2. Python responderá True
    
    #Pregunta #2: ¿Cuál es el resultado de la siguiente comparación?
    print(2 == 2.)
    # R/ Esta pregunta no es tan fácil como la primera. Afortunadamente, 
    # Python puede convertir el valor entero en su equivalente real y, en 
    # consecuencia, la respuesta es True.
    ```
    
- **Desigualdad: el operador *no es igual a* (!=)**
    
    El operador **`!=`** (no es igual a) también compara los valores de dos operandos. Aquí está la diferencia: si son iguales, el resultado de la comparación es False. Si no son iguales, el resultado de la comparación es True.
    
    Ahora echa un vistazo a la comparación de desigualdad a continuación - ¿puedes adivinar el resultado de esta operación?
    
    ```python
    var = 0  # Asignando 0 a var
    print(var != 0)
     
    var = 1  # Asignando 1 a var
    print(var != 0)
    ```
    
- **Comparación: mayor que (>)**
    
    También se puede hacer una pregunta de comparación usando el operador **`>`** (mayor que).
    
    Si deseas saber si hay más ovejas negras que blancas, puedes escribirlo de la siguiente manera:
    
    ```python
    black_sheep > white_sheep  # Mayor que
    ```
    
    **`True`** lo confirma; `**False**` lo niega.
    
- **Comparación: mayor o igual que (>=)**
    
    El operador *mayor que* tiene otra variante especial, una variante **no estricta**, pero se denota de manera diferente que la notación aritmética clásica: **`>=`** (mayor o igual que).
    
    Hay dos signos subsecuentes, no uno.
    
    Ambos operadores (estrictos y no estrictos), así como los otros dos que se analizan en la siguiente sección, son **operadores binarios con enlazado del lado izquierdo**, y su **prioridad es mayor que la mostrada por == y !=**.
    
    Si queremos saber si tenemos que usar un gorro o no, nos hacemos la siguiente pregunta:
    
    ```python
    centigrade_outside >= 0.0  # Mayor o igual que
    ```
    
- **Comparación: menor o igual que (<=)**
    
    Los operadores utilizados en este caso son: El operador **`<`** (menor que) y su hermano no estricto: **`<=`** (menor o igual que).
    
    Observa este ejemplo simple:
    
    ```python
    current_velocity_mph < 85  # Menor que
    current_velocity_mph <= 85  # Menor o igual que
    ```
    
    Vamos a comprobar si existe un riesgo de ser multados por la ley (la primera pregunta es estricta, la segunda no).
    
- **Tabla resumen**
    
    
    | Operador | Descripción | Ejemplo |
    | --- | --- | --- |
    | == | Devuelve True si los valores de los operandos son iguales, y False de lo contrario. | x == y  # False    x == z  # True     |
    | != | Devuelve True si los valores de los operandos no son iguales, y False de lo contrario. | x != y  # True    x != z  # False     |
    | > | Devuelve True si el valor del operando izquierdo es mayor que el valor del operando derecho, y False de lo contrario. | x > y  # False    y > z  # True     |
    | < | Devuelve True si el valor del operando izquierdo es menor que el valor del operando derecho, y False de lo contrario. | x < y  # True    y < z  # False     |
    | >= | Devuelve True si el valor del operando izquierdo es mayor o igual al valor del operando derecho, y False de lo contrario. | x >= y  # False    x >= z  # True    y >= z  # True     |
    | <= | Devuelve True si el valor del operando izquierdo es menor o igual al valor del operando derecho, y False de lo contrario. | x <= y  # True    x <= z  # True    y <= z  # False |

**Condicionales**: 

- **if**
    
    Para tomar tales decisiones, Python ofrece una instrucción especial. Debido a su naturaleza y su aplicación, se denomina **instrucción condicional** (o sentencia condicional).
    
    Existen varias variantes de la misma. Comenzaremos con la más simple, aumentando la dificultad lentamente.
    
    La primera forma de una sentencia condicional, que puede ver a continuación, está escrita de manera muy informal pero figurada:
    
    ```python
    if true_or_not:
        do_this_if_true
    ```
    
    Esta sentencia condicional consta de los siguientes elementos, estrictamente necesarios en este orden:
    
    - La palabra clave reservada if;
    - Uno o más espacios en blanco;
    - Una expresión (una pregunta o una respuesta) cuyo valor se interpretar únicamente en términos de True (cuando su valor no sea cero) y False (cuando sea igual a cero);
    - Unos **dos puntos** seguidos de una nuevalínea;
    - Una instrucción **con sangría** o un conjunto de instrucciones (se requiere absolutamente al menos una instrucción); la **sangría** se puede lograr de dos maneras: insertando un número particular de espacios (la recomendación es usar **cuatro espacios de sangría**), o usando el *tabulador*; nota: si hay mas de una instrucción en la parte con sangría, la sangría debe ser la misma en todas las líneas; aunque puede parecer lo mismo si se mezclan tabuladores con espacios, es importante que todas las sangrías **sean exactamente iguales** Python 3 **no permite mezclar espacios y tabuladores** para la sangría.
    
    ¿Cómo funciona esta sentencia?
    
    - Si la expresión true_or_not **representa la verdad** (es decir, su valor no es igual a cero), **las sentencias con sangría se ejecutarán**;
    - Si la expresión true_or_not **no representa la verdad** (es decir, su valor es igual a cero), **las sentencias con sangría se omitirán** (ignorado), y la siguiente instrucción ejecutada será la siguiente al nivel de la sangría original.
- **else**
    
    Comenzamos con una frase simple que decía: *Si el clima es bueno, saldremos a caminar*.
    
    Nota: no hay una palabra sobre lo que sucederá si el clima es malo. Solo sabemos que no saldremos al aire libre, pero no sabemos que podríamos hacer. Es posible que también queramos planificar algo en caso de mal tiempo.
    
    Podemos decir, por ejemplo: *Si el clima es bueno, saldremos a caminar, de lo contrario, iremos al cine*.
    
    Ahora sabemos lo que haremos **si se cumplen las condiciones**, y sabemos lo que haremos **si no todo sale como queremos**. En otras palabras, tenemos un "Plan B".
    
    Python nos permite expresar dichos planes alternativos. Esto se hace con una segunda forma, ligeramente mas compleja, de la sentencia condicional, la sentencia *if-else*:
    
    ```python
    if true_or_false_condition:
        perform_if_condition_true
    else:
        perform_if_condition_false
    ```
    
    Por lo tanto, hay una nueva palabra: else - esta es una **palabra clave reservada**.
    
    La parte del código que comienza con else dice que hacer si no se cumple la condición especificada por el if (observa los **dos puntos** después de la palabra).
    
    La ejecución de *if-else* es la siguiente:
    
    - Si la condición se evalúa como **True** (su valor no es igual a cero), la instrucción perform_if_condition_true se ejecuta, y la sentencia condicional llega a su fin;
    - Si la condición se evalúa como **False** (es igual a cero), la instrucción perform_if_condition_false se ejecuta, y la sentencia condicional llega a su fin.
- **if anidado**
    
    Considera el caso donde la instrucción **colocada después del if  es otro if**.
    
    Lee lo que hemos planeado para este Domingo. Si hay buen clima, saldremos a caminar. Si encontramos un buen restaurante, almorzaremos allí. De lo contrario, vamos a comer un sandwich. Si hay mal clima, iremos al cine. Si no hay boletos, iremos de compras al centro comercial más cercano.
    
    Escribamos lo mismo en Python. Considera cuidadosamente el código siguiente:
    
    ```python
    if the_weather_is_good:
        if nice_restaurant_is_found:
            have_lunch()
        else:
            eat_a_sandwich()
    else:
        if tickets_are_available:
            go_to_the_theater()
        else:
            go_shopping()
    ```
    
    Aquí hay dos puntos importantes:
    
    - Este uso de la sentencia if se conoce como **anidamiento**; recuerda que cada else se refiere al if que se encuentra **en el mismo nivel de sangría**; se necesita saber esto para determinar cómo se relacionan los *if*s y los *else*;
    - Considera como la sangría **mejora la legibilidad** y hace que el código sea más fácil de entender y rastrear.
- **elif**
    
    El segundo caso especial presenta otra nueva palabra clave de Python: **elif**. Como probablemente sospechas, es una forma más corta de **else if**.
    
    elif se usa para **verificar más de una condición**, y para **detener** cuando se encuentra la primera sentencia verdadera.
    
    Nuestro siguiente ejemplo se parece a la anidación, pero las similitudes son muy leves. Nuevamente, cambiaremos nuestros planes y los expresaremos de la siguiente manera: si hay buen clima, saldremos a caminar, de lo contrario, si obtenemos entradas, iremos al cine, de lo contrario, si hay mesas libres en el restaurante, vamos a almorzar; si todo falla, regresaremos a casa y jugaremos ajedrez.
    
    ¿Has notado cuantas veces hemos usado las palabras *de lo contrario*? Esta es la etapa en la que la palabra clave reservada elif desempeña su función.
    
    Escribamos el mismo escenario empleando Python:
    
    ```python
    if the_weather_is_good:
        go_for_a_walk()
    elif tickets_are_available:
        go_to_the_theater()
    elif table_is_available:
        go_for_lunch()
    else:
        play_chess_at_home()
    ```
    
    La forma de ensamblar las siguientes sentencias *if-elif-else* a veces se denomina **cascada**.
    
    Observa de nuevo como la sangría mejora la legibilidad del código.
    
    Se debe prestar atención adicional a este caso:
    
    - **No debes usar else sin un if precedente**;
    - else siempre es la **última rama de la cascada**, independientemente de si has usado elif o no;
    - else es una parte **opcional** de la cascada, y puede omitirse;
    - Si hay una rama else en la cascada, solo se ejecuta una de todas las ramas;
    - Si no hay una rama else, es posible que no se ejecute ninguna de las opciones disponibles.

**Función min() y max()**: Para encontrar el número más grande de todos, puede usar una función incorporada de Python llamada max(). Puedes usarlo con múltiples argumentos. Analiza el código de abajo:

```python
# Se leen tres números.
number1 = int(input("Ingresa el primer número: "))
number2 = int(input("Ingresa el segundo número: "))
number3 = int(input("Ingresa el tercer número: "))
 
# Verifica cuál de los números es el mayor
# y pásalo a la variable largest_number.
 
largest_number = max(number1, number2, number3)
 
# Imprime el resultado.
print("El número más grande es:", largest_number)
```

De la misma manera, puedes usar la función min() para devolver el número más pequeño.

**Diferencia de “while var != 0:” y “while var:”**:

En el primer caso, **`while counter != 0:`**, la condición es que la variable **`counter`** sea distinta de cero. Es decir, el ciclo se repetirá mientras **`counter`** tenga un valor diferente de cero.

En el segundo caso, **`while counter:`**, la condición es que la variable **`counter`** sea evaluada como verdadera. En Python, cualquier valor diferente de cero, una cadena no vacía, una lista no vacía, un objeto no nulo, entre otros, se considera verdadero. Es decir, el ciclo se repetirá mientras **`counter`** tenga un valor que sea considerado verdadero.

**Bucles con while y for**:

while: El bucle while ejecuta una sentencia o un conjunto de sentencias siempre que una condición booleana especificada sea verdadera

- **while (Explicación)**
    
    En general, en Python, un bucle se puede representar de la siguiente manera:
    
    ```python
    while
        instruction
    ```
    
    Si observas algunas similitudes con la instrucción *if*, está bien. De hecho, la diferencia sintáctica es solo una: usa la palabra while en lugar de la palabra if.
    
    La diferencia semántica es más importante: cuando se cumple la condición, *if* realiza sus sentencias **sólo una vez**; *while* **repite la ejecución siempre que la condición se evalúe como True**.
    
    Ahora, es importante recordar que:
    
    - si deseas ejecutar **más de una sentencia dentro de un while**, debes (como con if) **poner sangría** a todas las instrucciones de la misma manera.
    - una instrucción o conjunto de instrucciones ejecutadas dentro del while se llama el **cuerpo del bucle**.
    - si la condición es False (igual a cero) tan pronto como se compruebe por primera vez, el cuerpo no se ejecuta ni una sola vez (ten en cuenta la analogía de no tener que hacer nada si no hay nada que hacer).
    - el cuerpo debe poder cambiar el valor de la condición, porque si la condición es True al principio, el cuerpo podría funcionar continuamente hasta el infinito. Observa que hacer una cosa generalmente disminuye la cantidad de cosas por hacer.
    

for: El bucle for ejecuta un conjunto de sentencias muchas veces; se usa para iterar sobre una secuencia (por ejemplo, una lista, un diccionario, una tupla o un conjunto; pronto aprenderás sobre ellos) u otros objetos que son iterables (por ejemplo, cadenas). Puedes usar el bucle for para iterar sobre una secuencia de números usando la función incorporada range.

- **for (Explicación)**
    
    Otro tipo de bucle disponible en Python proviene de la observación de que a veces es más importante **contar los "giros o vueltas" del bucle** que verificar las condiciones.
    
    Imagina que el cuerpo de un bucle debe ejecutarse exactamente cien veces. Si deseas utilizar el bucle while para hacerlo, puede tener este aspecto:
    
    ```python
    i = 0
    while i < 100:
        # do_something()
        i += 1
    ```
    
    Sería bueno si alguien pudiera hacer esta cuenta aburrida por ti. ¿Es eso posible?
    
    Por supuesto que lo es - hay un bucle especial para este tipo de tareas, y se llama for.
    
    En realidad, el bucle for está diseñado para realizar tareas más complicadas, **puede "explorar" grandes colecciones de datos elemento por elemento**. Te mostraremos como hacerlo pronto, pero ahora presentaremos una variante más sencilla de su aplicación.
    
    Observa el fragmento de código:
    
    ```python
    for i in range(100):
        # do_something()
        pass
    ```
    
    Existen algunos elementos nuevos. Déjanos contarte sobre ellos:
    
    - la palabra reservada *for* abre el bucle for; nota - No hay condición después de eso; no tienes que pensar en las condiciones, ya que se verifican internamente, sin ninguna intervención.
    - cualquier variable después de la palabra reservada *for* es la **variable de control** del bucle; cuenta los giros del bucle y lo hace automáticamente.
    - la palabra reservada *in* introduce un elemento de sintaxis que describe el rango de valores posibles que se asignan a la variable de control.
    - la función range() (esta es una función muy especial) es responsable de generar todos los valores deseados de la variable de control; en nuestro ejemplo, la función creará (incluso podemos decir que **alimentará** el bucle con) valores subsiguientes del siguiente conjunto: 0, 1, 2 .. 97, 98, 99; nota: en este caso, la función range() comienza su trabajo desde 0 y lo finaliza un paso (un número entero) antes del valor de su argumento.
    - nota la palabra clave *pass* dentro del cuerpo del bucle - no hace nada en absoluto; es una **instrucción vacía** la colocamos aquí porque la sintaxis del bucle for exige al menos una instrucción dentro del cuerpo (por cierto - if, elif, else y while expresan lo mismo).
    
    La función range() también puede aceptar **tres argumentos**: Echa un vistazo al código del editor.
    
    ```python
    for i in range(2, 8, 3):
        print("El valor de i es", i)
    ```
    
    El primer argumento pasado a la función range() nos dice cual es el número **de inicio** de la secuencia (por lo tanto 2 en la output). El segundo argumento le dice a la función dónde **detener** la secuencia (la función genera números hasta el número indicado por el segundo argumento, pero no lo incluye). Finalmente, el tercer argumento indica el **paso**, que en realidad significa la diferencia entre cada número en la secuencia de números generados por la función.
    
    2 (número inicial) → 5 (2 incremento por 3 es igual a 5 - el número está dentro del rango de 2 a 8) → 8 (5 incremento por 3 es igual a 8 - el número no está dentro del rango de 2 a 8, porque el parámetro de parada no está incluido en la secuencia de números generados por la función).
    

**Las sentencias break y continue**:

Break: Puedes usar las sentencias break y continue para cambiar el flujo de un bucle.

continue: Utiliza continue para omitir la iteración actual, y continuar con la siguiente iteración.

- **Ejemplo #1**
    
    Hasta ahora, hemos tratado el cuerpo del bucle como una secuencia indivisible e inseparable de instrucciones que se realizan completamente en cada giro del bucle. Sin embargo, como desarrollador, podrías enfrentar las siguientes opciones:
    
    - parece que no es necesario continuar el bucle en su totalidad; se debe abstener de seguir ejecutando el cuerpo del bucle e ir más allá;
    - parece que necesitas comenzar el siguiente giro del bucle sin completar la ejecución del turno actual.
    
    Python proporciona dos instrucciones especiales para la implementación de estas dos tareas. Digamos por razones de precisión que su existencia en el lenguaje no es necesaria - un programador experimentado puede codificar cualquier algoritmo sin estas instrucciones. Tales adiciones, que no mejoran el poder expresivo del lenguaje, sino que solo simplifican el trabajo del desarrollador, a veces se denominan **dulces sintácticos** o azúcar sintáctica.
    
    Estas dos instrucciones son:
    
    - break - sale del bucle inmediatamente, e incondicionalmente termina la operación del bucle; el programa comienza a ejecutar la instrucción más cercana después del cuerpo del bucle.
    - continue - se comporta como si el programa hubiera llegado repentinamente al final del cuerpo; el siguiente turno se inicia y la expresión de condición se prueba de inmediato.
    
    Ambas palabras son **palabras clave reservadas**.
    
    ```python
    import time
    # break - ejemplo
    
    print("La instrucción break:")
    for i in range(1, 6):
        if i == 3:
            break
        print("Dentro del bucle.", i)
    print("Fuera del bucle.")
    
    # continue - ejemplo
    
    print("\nLa instrucción continue:")
    for i in range(1, 6):
        if i == 3:
            continue
        print("Dentro del bucle.", i)
        time.sleep(2)
    print("Fuera del bucle.")
    ```
    
- **Ejemplo #2**
    
    Regresemos a nuestro programa que reconoce el más grande entre los números ingresados. Lo convertiremos dos veces, usando las instrucciones de break y continue.
    
    Analiza el código y determina como las usarías.
    
    La variante empleando break es la siguiente:
    
    ```python
    largest_number = -99999999
    counter = 0
    
    while True:
        number = int(input("Ingresa un número o escribe -1 para finalizar el programa: "))
        if number == -1:
            break
        counter += 1
        if number > largest_number:
            largest_number = number
    
    if counter != 0:
        print("El número más grande es", largest_number)
    else:
        print("No has ingresado ningún número.")
    ```
    
    Y ahora la variante con continue:
    
    ```python
    largest_number = -99999999
    counter = 0
    
    number = int(input("Ingresa un número o escribe -1 para finalizar el programa: "))
    
    while number != -1:
        if number == -1:
            continue
        counter += 1
    
        if number > largest_number:
            largest_number = number
        number = int(input("Ingresa un número o escribe -1 para finalizar el programa: "))
    
    if counter:
        print("El número más grande es", largest_number)
    else:
        print("No has ingresado ningún número.")
    ```
    

**El bucle while y el bloque else**

- **Explicación**
    
    Ambos bucles while y for, tienen una característica interesante (y rara vez se usa).
    
    Te mostraremos como funciona - intenta juzgar por ti mismo si es utilizable.
    
    En otras palabras, trata de convencerte si la función es valiosa y útil, o solo es azúcar sintáctica.
    
    Echa un vistazo al fragmento en el editor. Hay algo extraño al final - la palabra reservada else.
    
    Como pudiste haber sospechado, los bucles **también pueden tener la rama else, como los if**.
    
    La rama else del bucle **siempre se ejecuta una vez, independientemente de si el bucle ha entrado o no en su cuerpo**.
    
    ¿Puedes adivinar la output? Ejecuta el programa para comprobar si tenías razón.
    
    ```python
    i = 1
    while i < 5:
        print(i)
        i += 1
    else:
        print("else:", i)
    ```
    
    Modifica el fragmento un poco para que el bucle no tenga oportunidad de ejecutar su cuerpo ni una sola vez:
    
    ```python
    i = 5
    while i < 5:
        print(i)
        i += 1
    else:
        print("else:", i)
    ```
    
    El estado del while es False al principio - ¿puedes verlo?
    
    Ejecuta y prueba el programa, y verifica si se ha ejecutado o no la rama else.
    

**El bucle for y el bloque else**

- **Explicación**
    
    Los bucles for se comportan de manera un poco diferente - echa un vistazo al fragmento en el editor y ejecútalo.
    
    ```python
    for i in range(5):
        print(i)
    else:
        print("else:", i)
    ```
    
    La output puede ser un poco sorprendente.
    
    La variable i conserva su último valor.
    
    Modifica el código un poco para realizar un experimento más.
    
    ```python
    i = 111
    for i in range(2, 1):
        print(i)
    else:
        print("else:", i)
    ```
    
    El cuerpo del bucle no se ejecutará aquí en absoluto. Nota: hemos asignado la variable i antes del bucle.
    
    Ejecuta el programa y verifica su output.
    
    Cuando el cuerpo del bucle no se ejecuta, la variable de control conserva el valor que tenía antes del bucle.
    
    ## *Si la variable de control no existe antes de que comience el bucle, no existirá cuando la ejecución llegue a la rama else branch.*
    

**Función range()**: La función range() genera una secuencia de números. Acepta enteros y devuelve objetos de rango. La sintaxis de range() tiene el siguiente aspecto: range(start, stop, step), donde:

- start es un parámetro opcional que especifica el número de inicio de la secuencia (por defecto) 0
- stop es un parámetro opcional que especifica el final de la secuencia generada (no está incluido).
- y step es un parámetro opcional que especifica la diferencia entre los números en la secuencia es (por defecto.) 1

**Lógica de computadoras**: Hemos utilizado la conjunción and (y), lo que significa que salir a caminar depende del cumplimiento simultáneo de estas dos condiciones. En el lenguaje de la lógica, tal conexión de condiciones se denomina **conjunción**. Y ahora otro ejemplo:

*Si tu estás en el centro comercial o yo estoy en el centro comercial, uno de nosotros le comprará un regalo a mamá.*

La aparición de la palabra or (o) significa que la compra depende de al menos una de estas condiciones. En lógica, este compuesto se llama una **disyunción**.

**Operador AND**: Un operador de conjunción lógica en Python es la palabra *and*. Es un operador binario **con una prioridad inferior a la expresada por los operadores de comparación**. Nos permite codificar condiciones complejas sin el uso de paréntesis como este:

```python
counter > 0 and value == 100
```

- **Tabla de verdad (AND)**
    
    
    | Argumento A | Argumento B | A and B |
    | --- | --- | --- |
    | False | False | False |
    | False | True | False |
    | True | False | False |
    | True | True | True |
    

**Operador OR**: Un operador de disyunción es la palabra or. Es un operador binario **con una prioridad más baja que and** (al igual que + en comparación con *).

- **Tabla de verdad (OR)**
    
    
    | Argumento A | Argumento B | A or B |
    | --- | --- | --- |
    | False | False | False |
    | False | True | True |
    | True | False | True |
    | True | True | True |
    

**Operador NOT**: Además, hay otro operador que se puede aplicar para condiciones de construcción. Es un **operador unario que realiza una negación lógica**. Su funcionamiento es simple: convierte la verdad en falso y lo falso en verdad.

Este operador se escribe como la palabra not, y su **prioridad es muy alta: igual que el unario + y -**.

- **Tabla de verdad (NOT)**
    
    
    | Argumento | not Argumento |
    | --- | --- |
    | False | True |
    | True | False |
    

**Expresiones Lógicas**: 

- **Explicación**
    
    Creemos una variable llamada var y asignémosle 1. Las siguientes condiciones son **equivalentes a pares**:
    
    ```python
    # Ejemplo 1:
    print(var > 0)
    print(not (var <= 0))
     
     
    # Ejemplo 2:
    print(var != 0)
    print(not (var == 0))
    ```
    
    Puedes estar familiarizado con las leyes de De Morgan. Dicen que:
    
    *La negación de una conjunción es la separación de las negaciones.*
    
    *La negación de una disyunción es la conjunción de las negaciones.*
    
    Escribamos lo mismo usando Python:
    
    ```python
    not (p and q) == (not p) or (not q)
    not (p or q) == (not p) and (not q)
    ```
    
    Observa como se han utilizado los paréntesis para codificar las expresiones - las colocamos allí para mejorar la legibilidad.
    
    Deberíamos agregar que ninguno de estos operadores de dos argumentos se puede usar en la forma abreviada conocida como op=. Vale la pena recordar esta excepción.
    

**Operadores Lógicos vs Bits Individuales**:

- **Operadores Lógicos**
    
    Los operadores lógicos toman sus argumentos como un todo, independientemente de cuantos bits contengan. Los operadores solo conocen el valor: cero (cuando todos los bits se restablecen) significa False; no cero (cuando se establece al menos un bit) significa True.
    
    El resultado de sus operaciones es uno de estos valores: False o True. Esto significa que este fragmento de código asignará el valor True a la variable j si i no es cero; de lo contrario, será False.
    
    ```python
    i = 1
    j = not not i
    ```
    
- **Operadores Bit a Bit**
    
    Sin embargo, hay cuatro operadores que le permiten **manipular bits de datos individuales**. Se denominan **operadores bit a bit**.
    
    Cubren todas las operaciones que mencionamos anteriormente en el contexto lógico, y un operador adicional. Este es el operador xor (significa **o exclusivo** ), y se denota como ^ (signo de intercalación).
    
    Aquí están todos ellos:
    
    - & (ampersand) ‒ conjunción a nivel de bits;
    - | (barra vertical) - disyunción a nivel de bits;
    - ~ (tilde) - negación a nivel de bits;
    - ^ (signo de intercalación) - o exclusivo a nivel de bits (xor).
    
    Operaciones bit a bit (&, |, y ^)
    
    | Argumento A | Argumento B | A & B |  A (pipe) B | A ^ B |
    | --- | --- | --- | --- | --- |
    | 0 | 0 | 0 | 0 | 0 |
    | 0 | 1 | 0 | 1 | 1 |
    | 1 | 0 | 0 | 1 | 1 |
    | 1 | 1 | 1 | 1 | 0 |
    
    Operaciones bit a bit (~)
    
    | Argumento | ~ Argumento |
    | --- | --- |
    | 0 | 1 |
    | 1 | 0 |
    
    Hagámoslo más fácil:
    
    - **&** requieres exactamente dos 1 s para proporcionar 1 como resultado;
    - **|** requiere al menos un 1 para proporcionar 1 como resultado;
    - **^** requiere exactamente un 1 para proporcionar 1 como resultado.
    
    Agreguemos un comentario importante: los argumentos de estos operadores **deben ser enteros**; no debemos usar flotantes aquí.
    
    La diferencia en el funcionamiento de los operadores lógicos y de bits es importante: **los operadores lógicos no penetran en el nivel de bits de su argumento**. Solo les interesa el valor entero final.
    
    Los operadores bit a bit son más estrictos: tratan con **cada bit por separado**. Si asumimos que la variable entera ocupa 64 bits (lo que es común en los sistemas informáticos modernos), puede imaginar la operación a nivel de bits como una evaluación de 64 veces del operador lógico para cada par de bits de los argumentos. Su analogía es obviamente imperfecta, ya que en el mundo real todas estas 64 operaciones se realizan al mismo tiempo (simultáneamente).
    

En resumen: Los **operadores lógicos** evalúan los valores booleanos de sus operandos y devuelven un resultado booleano basado en ellos (True o False). Pueden trabajar con diferentes tipos de valores (int, str, float, etc) y no tratan con bits individuales.

Por otro lado, los **operadores bit a bit** operan en cada bit individual de los operandos y realizan operaciones a nivel de bits. A diferencia de los operadores lógicos, solo pueden operar en números enteros y no en strings o floats. Estos operadores tratan con cada bit del número y devuelven un resultado entero basado en ellos. Los operadores bit a bit se expresan como "&" (and), "|" (or), "~" (not) y "^" (xor).

- **Operaciones lógicas vs operaciones bit a bit**
    
    Ahora te mostraremos un ejemplo de la diferencia en la operación entre las operaciones lógicas y de bit. Supongamos que se han realizado las siguientes tareas:
    
    ```python
    i = 15
    j = 22
    ```
    
    Si asumimos que los enteros se almacenan con 32 bits, la imagen a nivel de bits de las dos variables será la siguiente:
    
    ```python
    i: 00000000000000000000000000001111
    j: 00000000000000000000000000010110
    ```
    
    Se ejecuta la asignación:
    
    ```python
    log = i and j
    ```
    
    Estamos tratando con una conjunción lógica aquí. Vamos a trazar el curso de los cálculos. Ambas variables i y j no son ceros, por lo que se considerará que representan a True. Al consultar la tabla de verdad para el operador and, podemos ver que el resultado será True. No se realizan otras operaciones.
    
    ```python
    log: True
    ```
    
    Ahora la operación a nivel de bits - aquí está:
    
    ```python
    bit = i & j
    ```
    
    El operador & operará con cada par de bits correspondientes por separado, produciendo los valores de los bits relevantes del resultado. Por lo tanto, el resultado será el siguiente:
    
    | i | 00000000000000000000000000001111 |
    | --- | --- |
    | j | 00000000000000000000000000010110 |
    | bit = i & j | 00000000000000000000000000000110 |
    
    Estos bits corresponden al valor entero de seis.
    
    Veamos ahora los operadores de negación. Primero el lógico:
    
    ```python
    logneg = not i
    ```
    
    La variable logneg se establecerá en False - no es necesario hacer nada más.
    
    La negación a nivel de bits es así:
    
    ```python
    bitneg = ~i
    ```
    
    Puede ser un poco sorprendente: el valor de la variable bitneg es -16. Esto puede parecer extraño, pero no lo es en absoluto. Si deseas obtener más información, debes consultar el sistema de números binarios y las reglas que rigen los números de complemento de dos.
    
    [https://es.wikipedia.org/wiki/Complemento_a_dos](https://es.wikipedia.org/wiki/Complemento_a_dos)
    
    *En python simplemente se le suma 1 al valor entero y se le añade el signo negativo (-)*.
    
    | i | 00000000000000000000000000001111 |
    | --- | --- |
    | bitneg = ~i | 11111111111111111111111111110000 |
    
    ## *"-0b" es un prefijo que indica que el número que le sigue es un número binario negativo en Python.*
    
    *Por ejemplo, "-0b1010" representa el número binario negativo "1010" que en decimal es igual a -10.*
    
    *El prefijo "-0b" se utiliza en lugar del prefijo "-0x" que se utiliza para representar números hexadecimales negativos ("-0x" indica que el número es un número hexadecimal negativo).*
    
    Cada uno de estos operadores de dos argumentos se puede utilizar en **forma abreviada**. Estos son los ejemplos de sus notaciones equivalentes:
    
    | x = x & y | x &= y |
    | --- | --- |
    | x = x | y | x |= y |
    | x = x ^ y | x ^= y |
    
- **¿Cómo tratar con bits individuales?**
    
    Ahora te mostraremos para que puedes usar los operadores de bit a bit. Imagina que eres un desarrollador obligado a escribir una pieza importante de un sistema operativo. Se te ha dicho que puedes usar una variable asignada de la siguiente forma:
    
    ```python
    flag_register = 0x1234
    ```
    
    La variable almacena la información sobre varios aspectos de la operación del sistema. **Cada bit de la variable almacena un valor de si/no**. También se te ha dicho que solo uno de estos bits es tuyo - el tercero (recuerda que los bits se numeran desde cero y el número de bits cero es el más bajo, mientras que el más alto es el número 31). Los bits restantes no pueden cambiar, porque están destinados a almacenar otros datos. Aquí está tu bit marcado con la letra x:
    
    ```python
    flag_register = 0000000000000000000000000000x000
    ```
    
    Es posible que tengas que hacer frente a las siguientes tareas:
    
    1. **Comprobar el estado de tu bit** - deseas averiguar el valor de su bit; comparar la variable completa con cero no hará nada, porque los bits restantes pueden tener valores completamente impredecibles, pero puedes usar la siguiente propiedad de conjunción:
    
    ```python
    x & 1 = x
    x & 0 = 0
    ```
    
    Si aplicas la operación & a la variable flag_register junto con la siguiente imagen de bits:
    
    ```python
    00000000000000000000000000001000
    ```
    
    (observa el 1 en la posición de tu bit) como resultado, obtendrás una de las siguientes cadenas de bits:
    
    - 000000000000000000000000000**1**000 si tu bit se estableció en 1
    - 000000000000000000000000000**0**000 si tu bit se reseteo a 0
    
    Dicha secuencia de ceros y unos, cuya tarea es tomar el valor o cambiar los bits seleccionados, se denomina **máscara de bits**.
    
    Construyamos una máscara de bits para detectar el estado de tus bits. Debería apuntar a **el tercer bit**. Ese bit tiene el peso de 2^3=8 (dos elevado a 3 igual a 8). Se podría crear una máscara adecuada mediante la siguiente sentencia:
    
    ```python
    the_mask = 8
    ```
    
    También puedes hacer una secuencia de instrucciones dependiendo del estado de tu bit, aquí está:
    
    ```python
    if flag_register & the_mask:
        # Mi bit se estableció en 1.
    else:
        # Mi bit se restableció a 0.
    ```
    
    2. **Reinicia tu bit** - asigna un cero al bit, mientras que todos los otros bits deben permanecer sin cambios; usemos la misma propiedad de la conjunción que antes, pero usemos una máscara ligeramente diferente - exactamente como se muestra a continuación:
    
    ```python
    11111111111111111111111111110111
    ```
    
    Toma en cuenta que la máscara se creó como resultado de la negación de todos los bits de la variable the_mask. Restablecer el bit es simple, y se ve así (elige el que más te guste):
    
    ```python
    flag_register = flag_register & ~the_mask
    flag_register &= ~the_mask
    ```
    
    3. **Establece tu bit** - asigna un 1 a tu bit, mientras que todos los bits restantes deben permanecer sin cambios; usa la siguiente propiedad de disyunción:
    
    ```python
    x | 1 = 1
    x | 0 = x
    ```
    
    Ya estás listo para configurar su bit con una de las siguientes instrucciones:
    
    ```python
    flag_register = flag_register | the_mask
    flag_register |= the_mask
    ```
    
    4. **Niega tu bit** - reemplaza un 1 con un 0 y un 0 con un 1. Puedes utilizar una propiedad interesante del operador ~x:
    
    ```python
    x ^ 1 = ~x
    x ^ 0 = x
    ```
    
    Niega tu bit con las siguientes instrucciones:
    
    ```python
    flag_register = flag_register ^ the_mask
    flag_register ^= the_mask
    ```
    

****Desplazamiento binario a la izquierda y desplazamiento binario a la derecha (<<, >>)****:

- **Explicación**
    
    Python ofrece otra operación relacionada con los bits individuales: **shifting**. Esto se aplica solo a los valores de **número entero**, y no debe usar flotantes como argumentos para ello.
    
    Ya aplicas esta operación muy a menudo y muy inconscientemente. ¿Cómo multiplicas cualquier número por diez? Echa un vistazo:
    
    12345 × 10 = 123450
    
    Como puede ver, **multiplicar por diez es de hecho un desplazamiento** de todos los dígitos a la izquierda y llenar el vacío resultante con cero.
    
    ¿División entre diez? Echa un vistazo:
    
    12340 ÷ 10 = 1234
    
    Dividir entre diez no es más que desplazar los dígitos a la derecha.
    
    La computadora realiza el mismo tipo de operación, pero con una diferencia: como dos es la base para los números binarios (no 10), **desplazar un valor un bit a la izquierda corresponde a multiplicarlo por dos**; respectivamente, **desplazar un bit a la derecha es como dividir entre dos** (observa que se pierde el bit más a la derecha).
    
    Los **operadores de cambio** en Python son un par de **dígrafos**: < < y > >, sugiriendo claramente en qué dirección actuará el cambio.
    
    ```python
    valor << bits
    valor >> bits
    ```
    
    **El argumento izquierdo de estos operadores es un valor entero cuyos bits se desplazan. El argumento correcto determina el tamaño del desplazamiento.**
    
    Esto demuestra que esta operación ciertamente no es conmutativa
    
    La prioridad de estos operadores es muy alta. Los verás en la tabla de prioridades actualizada, que te mostraremos al final de esta sección.
    
    Echa un vistazo a los turnos en la ventana del editor.
    
    ```python
    var = 17
    var_right = var >> 1
    var_left = var << 2
    print(var, var_left, var_right)
    
    Resultado:
    17 68 8
    ```
    
    - **`17 >> 1`** → `**17 // 2**` (**17** dividido entre **2 a la potencia de 1**) → 8 (desplazarse hacia la derecha en un bit equivale a la división entera entre dos)
    - `**17 << 2**` → **`17 * 4`** (**17** multiplicado por **2 a la potencia de 2**) → 68 (desplazarse hacia la izquierda dos bits es lo mismo que multiplicar números enteros por cuatro)
    

**Listas**:

- **Explicación**
    
    La lista **es un tipo de dato** en Python que se utiliza para **almacenar múltiples objetos**. Es una **colección ordenada y mutable** de elementos separados por comas entre corchetes
    
    Hasta ahora, has aprendido como declarar variables que pueden almacenar exactamente un valor dado a la vez. Tales variables a veces se denominan **escalares** por analogía con las matemáticas. Todas las variables que has usado hasta ahora son realmente escalares.
    
    Piensa en lo conveniente que sería declarar una variable que podría **almacenar más de un valor**. Por ejemplo, cien, o mil o incluso diez mil. Todavía sería una y la misma variable, pero muy amplia y espaciosa. ¿Suena atractivo? Quizás, pero ¿cómo manejarías un contenedor así lleno de valores diferentes? ¿Cómo elegirías solo el que necesitas?
    
    Vamos a crear una variable llamada numbers; se le asigna no solo un número, sino que se llena con una lista que consta de cinco valores (nota: la lista **comienza con un corchete abierto y termina con un corchete cerrado**; el espacio entre los corchetes es llenado con cinco números separados por comas).
    
    ```python
    numbers = [10, 5, 7, 2, 1]
    ```
    
    Digamos lo mismo utilizando una terminología adecuada: **numbers es una lista que consta de cinco valores, todos ellos números**. También podemos decir que esta sentencia crea una lista de longitud igual a cinco (ya que contiene cinco elementos).
    
    Los elementos dentro de una lista **pueden tener diferentes tipos**. Algunos de ellos pueden ser enteros, otros son flotantes y otros pueden ser listas.
    
    Python ha adoptado una convención que indica que los elementos de una lista están **siempre numerados desde cero**. Esto significa que el elemento almacenado al principio de la lista tendrá el número cero. Como hay cinco elementos en nuestra lista, al último de ellos se le asigna el número cuatro.
    
    Antes de continuar con nuestra discusión, debemos indicar lo siguiente: nuestra lista **es una colección de elementos, pero cada elemento es un escalar**.
    

**Indexación de listas**:

- **Cambiar el valor de una lista**
    
    Vamos a **asignar un nuevo valor de 111 al primer elemento** en la lista. Lo hacemos de esta manera:
    
    ```python
    numbers = [10, 5, 7, 2, 1]
    print("Contenido de la lista:", numbers)  # Imprimiendo contenido de la lista original.
     
    numbers[0] = 111
    print("Nuevo contenido de la lista: ", numbers)  # Contenido actual de la lista.
    ```
    
- **Copiar el valor de una lista**
    
    Y ahora queremos **copiar el valor del quinto elemento al segundo elemento**
    
    ```python
    numbers = [10, 5, 7, 2, 1]
    print("Contenido de la lista original:", numbers)  # Imprimiendo contenido de la lista original.
     
    numbers[0] = 111
    print("\nPrevio contenido de la lista:", numbers)  # Imprimiendo contenido de la lista anterior.
     
    numbers[1] = numbers[4]  # Copiando el valor del quinto elemento al segundo elemento.
    print("Nuevo contenido de la lista:", numbers)  # Imprimiendo el contenido de la lista actual.
    ```
    
    El valor dentro de los corchetes que selecciona un elemento de la lista se llama un **índice**, mientras que la operación de seleccionar un elemento de la lista se conoce como **indexación**.
    
- **Acceso al contenido de la lista**
    
    Se puede acceder a cada uno de los elementos de la lista por separado. Por ejemplo, se puede imprimir:
    
    ```python
    print(numbers[0]) # Accediendo al primer elemento de la lista.
    ```
    
- **Imprimir la longitud de la lista**
    
    La longitud **de una lista** puede variar durante la ejecución. Se pueden agregar nuevos elementos a la lista, mientras que otros pueden eliminarse de ella. Esto significa que la lista es una entidad muy dinámica.
    
    Si deseas verificar la longitud actual de la lista, puedes usar una función llamada len() (su nombre proviene de *length - longitud*).
    
    La función toma **el nombre de la lista como un argumento y devuelve el número de elementos almacenados actualmente** dentro de la lista (en otras palabras - la longitud de la lista).
    
    ```python
    numbers = [10, 5, 7, 2, 1]
    print("\nLongitud de la lista:", len(numbers))  # Imprimiendo la longitud de la lista.
    ```
    
- **Imprimir la lista como un todo**
    
    La lista también puede imprimirse como un todo - como aquí:
    
    ```python
    print(numbers) # Imprimiendo la lista completa.
    ```
    
- **Anidar listas**
    
    Las listas pueden estar **anidadas**, por ejemplo:
    
    ```python
    my_list = [1, 'a', ["lista", 64, [0, 1], False]]
    ```
    

- **Eliminando elementos de una lista**
    
    Cualquier elemento de la lista puede ser **eliminado** en cualquier momento - esto se hace con una instrucción llamada del (eliminar). Nota: es una **instrucción**, no una función.
    
    Tienes que apuntar al elemento que quieres eliminar - desaparecerá de la lista y la longitud de la lista se reducirá en uno.
    
    ```python
    del numbers[1]
    print(len(numbers))
    print(numbers)
    
    del numbers # borra la lista entera
    ```
    
- **índices negativos**
    
    Puede parecer extraño, pero los índices negativos son válidos, y pueden ser muy útiles.
    
    Un elemento con un índice igual a -1 es **el último en la lista**.
    
    ```python
    numbers = [111, 7, 2, 1]
    print(numbers[-2])
    ```
    
    El código del ejemplo mostrará 1. Ejecuta el programa y comprueba.
    
    Del mismo modo, el elemento con un índice igual a -2 es **el anterior al último en la lista**.
    
    El último elemento accesible en nuestra lista es numeros[-4] (el primero)
    

- **Utilidades con el bucle “for”**
    
    **Primer aspecto de for:**
    
    El bucle for tiene una variante muy especial que puede **procesar las listas** de manera muy efectiva - echemos un vistazo a eso.
    
    ```python
    my_list = [10, 1, 8, 3, 5]
    total = 0
    
    for i in range(len(my_list)):
        total += my_list[i]
    
    print(total)
    ```
    
    Supongamos que deseas **calcular la suma de todos los valores almacenados en la lista my_list**.
    
    Necesitas una variable cuya suma se almacenará y se le asignará inicialmente un valor de 0 - su nombre será total. (Nota: no la vamos a nombrar sum ya que Python utiliza el mismo nombre para una de sus funciones integradas: sum(). **Utilizar ese nombre sería considerado una mala práctica**.) Luego agrega todos los elementos de la lista usando el bucle for. Echa un vistazo al fragmento en el editor.
    
    Comentemos este ejemplo:
    
    - a la lista se le asigna una secuencia de cinco valores enteros;
    - la variable i toma los valores 0, 1,2,3, y 4, y luego indexa la lista, seleccionando los elementos siguientes: el primero, segundo, tercero, cuarto y quinto;
    - cada uno de estos elementos se agrega junto con el operador += a la variable suma, dando el resultado final al final del bucle;
    - observa la forma en que se ha empleado la función len() - hace que el código sea independiente de cualquier posible cambio en el contenido de la lista.
    
    **Segundo aspecto de for:**
    
    Pero el bucle for puede hacer mucho más. Puede ocultar todas las acciones conectadas a la indexación de la lista y entregar todos los elementos de la lista de manera práctica.
    
    Este fragmento modificado muestra como funciona:
    
    ```python
    my_list = [10, 1, 8, 3, 5]
    total = 0
     
    for i in my_list:
        total += i
     
    print(total)
    ```
    
    ¿Qué sucede aquí?
    
    - la instrucción for especifica la variable utilizada para navegar por la lista (i) seguida de la palabra clave in y el nombre de la lista siendo procesado (my_list).
    - a la variable i se le asignan los valores de todos los elementos de la lista subsiguiente, y el proceso ocurre tantas veces como hay elementos en la lista;
    - esto significa que usa la variable i como una copia de los valores de los elementos, y no necesita emplear índices;
    - la función len() tampoco es necesaria aquí.
    
- **Invertir valores en listas**
    
    Dejemos de lado las listas por un breve momento y veamos un tema intrigante.
    
    Imagina que necesitas reorganizar los elementos de una lista, es decir, revertir el orden de los elementos: el primero y el quinto, así como el segundo y cuarto elementos serán intercambiados. El tercero permanecerá intacto.
    
    Pregunta: ¿cómo se pueden intercambiar los valores de dos variables?
    
    Echa un vistazo al fragmento:
    
    ```python
    variable_1 = 1
    variable_2 = 2
     
    variable_2 = variable_1
    variable_1 = variable_2
    ```
    
    Si haces algo como esto, **perderás el valor previamente almacenado** en variable_2. Cambiar el orden de las tareas no ayudará. Necesitas una tercera variable **que sirva como almacenamiento auxiliar**.
    
    Así es como puedes hacerlo:
    
    ```python
    variable_1 = 1
    variable_2 = 2
     
    auxiliary = variable_1
    variable_1 = variable_2
    variable_2 = auxiliary
    ```
    
    Python ofrece una forma más conveniente de hacer el intercambio - echa un vistazo:
    
    ```python
    variable_1 = 1
    variable_2 = 2
     
    variable_1, variable_2 = variable_2, variable_1
    ```
    
    Ahora puedes **intercambiar** fácilmente los elementos de la lista para **revertir su orden**:
    
    ```python
    my_list = [10, 1, 8, 3, 5]
     
    my_list[0], my_list[4] = my_list[4], my_list[0]
    my_list[1], my_list[3] = my_list[3], my_list[1]
     
    print(my_list)
    ```
    
    Ejecuta el fragmento. Su output debería verse así:
    
    ```python
    [5, 3, 8, 1, 10]
    ```
    
    Se ve bien con cinco elementos.
    
    ¿Seguirá siendo aceptable con una lista que contenga 100 elementos? No, no lo hará.
    
    ¿Puedes usar el bucle for para hacer lo mismo automáticamente, independientemente de la longitud de la lista? Si, si puedes.
    
    Así es como lo hemos hecho:
    
    ```python
    for i in range(length // 2):
        my_list[i], my_list[length - i - 1] = my_list[length - i - 1], my_list[i]
     
    print(my_list)
    ```
    
    Nota:
    
    - hemos asignado la variable length a la longitud de la lista actual (esto hace que nuestro código sea un poco más claro y más corto)
    - hemos preparado el bucle for para que se ejecute su cuerpo length // 2 veces (esto funciona bien para listas con longitudes pares e impares, porque cuando la lista contiene un número impar de elementos, el del medio permanece intacto)
    - hemos intercambiado el elemento i (desde el principio de la lista) por el que tiene un índice igual a (length - i - 1) (desde el final de la lista); en nuestro ejemplo, for i igual a 0 a la (length - i - 1) da 4; for i igual a 3, da 3 - esto es exactamente lo que necesitábamos.
    
    Las listas son extremadamente útiles y las encontrarás muy a menudo.
    
- **Invertir valores en listas sencillo (método)**
    
    También hay un método de lista llamado reverse(), que puedes usar para invertir la lista, por ejemplo:
    
    ```python
    lst = [5, 3, 1, 2, 4]
    print(lst)
     
    lst.reverse()
    print(lst)  # output: [4, 2, 1, 3, 5]
    ```
    

****Funciones vs métodos:**

- **Explicación**
    
    Un método **es un tipo específico de función** - se comporta como una función y se parece a una función, pero difiere en la forma en que actúa y en su estilo de invocación.
    
    Una función **no pertenece a ningún dato** - obtiene datos, puede crear nuevos datos y (generalmente) produce un resultado.
    
    Un método hace todas estas cosas, pero también puede **cambiar el estado de una entidad seleccionada**.
    
    **Un método es propiedad de los datos para los que trabaja, mientras que una función es propiedad de todo el código**.
    
    Esto también significa que invocar un método requiere alguna especificación de los datos a partir de los cuales se invoca el método.
    
    En general, una invocación de función típica puede tener este aspecto:
    
    ```python
    result = function(arg)
    ```
    
    La función toma un argumento, hace algo, y devuelve un resultado.
    
    Una invocación de un método típico usualmente se ve así:
    
    ```python
    result = data.method(arg)
    ```
    
    Nota: el nombre del método está precedido por el nombre de los datos que posee el método. A continuación, se agrega un **punto**, seguido del **nombre del método** y un par de **paréntesis que encierran los argumentos**.
    
    El método se comportará como una función, pero puede hacer algo más - puede **cambiar el estado interno de los datos** a partir de los cuales se ha invocado.
    

**Agregando elementos a una lista: append() y insert()**:

- **Explicación**
    
    Un nuevo elemento puede ser *añadido* al final de la lista existente:
    
    ```python
    list.append(value)
    ```
    
    Dicha operación se realiza mediante un método llamado **append()**. Toma el valor de su argumento y lo coloca **al final de la lista** que posee el método.
    
    La longitud de la lista aumenta en uno.
    
    El método **insert()** es un poco más inteligente - puede agregar un nuevo elemento **en cualquier lugar de la lista**, no solo al final.
    
    ```python
    list.insert(location, value)
    ```
    
    Toma dos argumentos:
    
    - el primero muestra la ubicación requerida del elemento a insertar; nota: todos los elementos existentes que ocupan ubicaciones a la derecha del nuevo elemento (incluido el que está en la posición indicada) se desplazan a la derecha, para hacer espacio para el nuevo elemento;
    - el segundo es el elemento a insertar.
    
- **Ejemplo**
    
    Observa el código en el editor. Ve como usamos los métodos append() e insert(). Presta atención a lo que sucede después de usar insert(): el primer elemento anterior ahora es el segundo, el segundo el tercero, y así sucesivamente.
    
    ```python
    numbers = [111, 7, 2, 1]
    print(len(numbers))
    print(numbers)
    
    ###
    
    numbers.append(4)
    
    print(len(numbers))
    print(numbers)
    
    ###
    
    numbers.insert(0, 222)
    print(len(numbers))
    print(numbers)
    
    #
    
    numbers.insert(1, 333)
    print(len(numbers))
    print(numbers)
    ```
    
    Puedes **iniciar la vida de una lista creándola vacía** (esto se hace con un par de corchetes vacíos) y luego agregar nuevos elementos según sea necesario.
    
    ```python
    my_list = []  # Creando una lista vacía.
    
    for i in range(5):
        my_list.append(i + 1)
    
    print(my_list)
    ```
    
    Será una secuencia de números enteros consecutivos del 1 (luego agrega uno a todos los valores agregados) hasta 5.
    
    Hemos modificado un poco el fragmento:
    
    ```python
    my_list = []  # Creando una lista vacía.
     
    for i in range(5):
        my_list.insert(0, i + 1)
     
    print(my_list)
    ```
    
    Deberías obtener la misma secuencia, pero en **orden inverso** (este es el mérito de usar el método insert()).
    

**Ordenamiento burbuja**:

- **Explicación**
    
    Se han inventado muchos algoritmos de clasificación, que difieren mucho en velocidad, así como en complejidad. Vamos a mostrar un algoritmo muy simple, fácil de entender, pero desafortunadamente, tampoco es muy eficiente. Se usa muy raramente, y ciertamente no para listas extensas.
    
    Digamos que una lista se puede ordenar de dos maneras:
    
    - ascendente (o más precisamente - no descendente) - si en cada par de elementos adyacentes, el primer elemento no es mayor que el segundo;
    - descendente (o más precisamente - no ascendente) - si en cada par de elementos adyacentes, el primer elemento no es menor que el segundo.
    
    En las siguientes secciones, ordenaremos la lista en orden ascendente, de modo que los números se ordenen de menor a mayor.
    
    Aquí está la lista:
    
    | 8 | 10 | 6 | 2 | 4 |
    | --- | --- | --- | --- | --- |
    
    Intentaremos utilizar el siguiente enfoque: tomaremos el primer y el segundo elemento y los compararemos; si determinamos que están en el orden incorrecto (es decir, el primero es mayor que el segundo), los intercambiaremos; si su orden es válido, no haremos nada. Un vistazo a nuestra lista confirma lo último - los elementos 01 y 02 están en el orden correcto, así como 8<10.
    
    Ahora observa el segundo y el tercer elemento. Están en las posiciones equivocadas. Tenemos que intercambiarlos:
    
    | 8 | 6 | 10 | 2 | 4 |
    | --- | --- | --- | --- | --- |
    
    Vamos más allá y observemos los elementos tercero y cuarto. Una vez más, esto no es lo que se supone que es. Tenemos que intercambiarlos:
    
    | 8 | 6 | 2 | 10 | 4 |
    | --- | --- | --- | --- | --- |
    
    Ahora comprobemos los elementos cuarto y quinto. Si, ellos también están en las posiciones equivocadas. Ocurre otro intercambio:
    
    | 8 | 6 | 2 | 4 | 10 |
    | --- | --- | --- | --- | --- |
    
    El primer paso a través de la lista ya está terminado. Todavía estamos lejos de terminar nuestro trabajo, pero algo curioso ha sucedido mientras tanto. El elemento más grande, 10, ya ha llegado al final de la lista. Ten en cuenta que este es el **lugar deseado** para el. Todos los elementos restantes forman un lío pintoresco, pero este ya está en su lugar.
    
    Ahora, por un momento, intenta imaginar la lista de una manera ligeramente diferente - es decir, de esta manera:
    
    | 10 |
    | --- |
    | 4 |
    | 2 |
    | 6 |
    | 8 |
    
    Observa - El 10 está en la parte superior. Podríamos decir que flotó desde el fondo hasta la superficie, al igual que las burbujas **en una copa de champán**. El método de clasificación deriva su nombre de la misma observación - se denomina **ordenamiento burbuja**.
    
    Ahora comenzamos con el segundo paso a través de la lista. Miramos el primer y el segundo elemento - es necesario un intercambio:
    
    | 6 | 8 | 2 | 4 | 10 |
    | --- | --- | --- | --- | --- |
    
    Es hora del segundo y tercer elemento: también tenemos que intercambiarlos:
    
    | 6 | 2 | 8 | 4 | 10 |
    | --- | --- | --- | --- | --- |
    
    Ahora el tercer y cuarto elementos, y la segunda pasada, se completa, ya que 8 ya está en su lugar:
    
    | 6 | 2 | 4 | 8 | 10 |
    | --- | --- | --- | --- | --- |
    
    Comenzamos el siguiente pase inmediatamente. Observa atentamente el primer y el segundo elemento - se necesita otro cambio:
    
    | 2 | 6 | 4 | 8 | 10 |
    | --- | --- | --- | --- | --- |
    
    Ahora 6 necesita ir a su lugar. Cambiamos el segundo y el tercer elemento:
    
    | 2 | 4 | 6 | 8 | 10 |
    | --- | --- | --- | --- | --- |
    
    La lista ya está ordenada. No tenemos nada más que hacer. Esto es exactamente lo que queremos.
    
    Como puedes ver, la esencia de este algoritmo es simple: **comparamos los elementos adyacentes y, al intercambiar algunos de ellos, logramos nuestro objetivo**.
    
    Codifiquemos en Python todas las acciones realizadas durante un solo paso a través de la lista, y luego consideraremos cuántos pases necesitamos para realizarlo. No hemos explicado esto hasta ahora, pero lo haremos pronto.
    
- **Solución**
    
    ¿Cuántos pases necesitamos para ordenar la lista completa?
    
    Resolvamos este problema de la siguiente manera: **introducimos otra variable**, su tarea es observar si se ha realizado algún intercambio durante el pase o no. Si no hay intercambio, entonces la lista ya está ordenada, y no hay que hacer nada más. Creamos una variable llamada swapped, y le asignamos un valor de False para indicar que no hay intercambios. De lo contrario, se le asignará True.
    
    ```python
    my_list = [8, 10, 6, 2, 4]  # lista a ordenar
     
    for i in range(len(my_list) - 1):  # necesitamos (5 - 1) comparaciones
        if my_list[i] > my_list[i + 1]:  # compara elementos adyacentes
            my_list[i], my_list[i + 1] = my_list[i + 1], my_list[i]  # Si terminamos aquí, tenemos que intercambiar elementos.
    ```
    
    Deberías poder leer y comprender este programa sin ningún problema:
    
    ```python
    my_list = [8, 10, 6, 2, 4]  # lista a ordenar
    swapped = True  # Lo necesitamos verdadero (True) para ingresar al bucle while.
     
    while swapped:
        swapped = False  # no hay intercambios hasta ahora
        for i in range(len(my_list) - 1):
            if my_list[i] > my_list[i + 1]:
                swapped = True  # ¡ocurrió el intercambio!
                my_list[i], my_list[i + 1] = my_list[i + 1], my_list[i]
     
    print(my_list)
    ```
    
- **Solución (vers. Interactiva)**
    
    En el editor, puedes ver un programa completo, enriquecido por una conversación con el usuario, y que permite ingresar e imprimir elementos de la lista: **El ordenamiento burbuja - versión final interactiva**.
    
    ```python
    my_list = []
    swapped = True
    num = int(input("¿Cuántos elementos deseas ordenar?: "))
    
    for i in range(num):
        val = float(input("Ingresa un elemento de la lista: "))
        my_list.append(val)
    
    while swapped:
        swapped = False
        for i in range(len(my_list) - 1):
            if my_list[i] > my_list[i + 1]:
                swapped = True
                my_list[i], my_list[i + 1] = my_list[i + 1], my_list[i]
    
    print("\nOrdenada:")
    print(my_list)
    ```
    
- **Solución sencilla (método)**
    
    Python, sin embargo, tiene sus propios mecanismos de clasificación. Nadie necesita escribir sus propias clases, ya que hay un número suficiente de **herramientas listas-para-usar**.
    
    Te explicamos este sistema de clasificación porque es importante aprender como procesar los contenidos de una lista y mostrarte como puede funcionar la clasificación real.
    
    Si quieres que Python ordene tu lista, puedes hacerlo de la siguiente manera:
    
    ```python
    my_list = [8, 10, 6, 2, 4]
    my_list.sort()
    print(my_list)
    ```
    
    Es tan simple como eso.
    
    La output del fragmento es la siguiente:
    
    ```python
    [2, 4, 6, 8, 10]
    ```
    
    Como puedes ver, todas las listas tienen un método denominado sort(), que las ordena lo más rápido posible. Ya has aprendido acerca de algunos de los métodos de lista anteriormente, y pronto aprenderás más sobre otros.
    

**La vida al interior de las listas**:

- **Explicación**
    
    Ahora queremos mostrarte una característica importante y muy sorprendente de las listas, que las distingue de las variables ordinarias.
    
    Queremos que lo memorices - ya que puede afectar tus programas futuros y causar graves problemas si se olvida o se pasa por alto.
    
    Echa un vistazo al fragmento en el editor.
    
    ```python
    list_1 = [1]
    list_2 = list_1
    list_1[0] = 2
    print(list_2)
    ```
    
    El programa:
    
    - crea una lista de un elemento llamada list_1;
    - la asigna a una nueva lista llamada list_2;
    - cambia el único elemento de list_1;
    - imprime la list_2;
    
    La parte sorprendente es el hecho de que el programa mostrará como resultado: [2], no [1], que parece ser la solución obvia.
    
    Las listas (y muchas otras entidades complejas de Python) se almacenan de diferentes maneras que las variables ordinarias (escalares).
    
    Se podría decir que:
    
    - el nombre de una variable ordinaria es el **nombre de su contenido**;
    - el nombre de una lista es el nombre de una ubicación de memoria **donde se almacena la lista**.
    
    Lee estas dos líneas una vez más - la diferencia es esencial para comprender de que vamos a hablar a continuación.
    
    La asignación: list_2 = list_1 copia el nombre del arreglo no su contenido. En efecto, los dos nombres (list_1 y list_2) identifican la misma ubicación en la memoria de la computadora. Modificar uno de ellos afecta al otro, y viceversa.
    
    ¿Cómo te las arreglas con eso?
    
- **Rebanadas poderosas**
    
    Afortunadamente, la solución está al alcance de tu mano - su nombre es **rebanada**.
    
    Una rebanada es un elemento de la sintaxis de Python que permite **hacer una copia nueva de una lista, o partes de una lista**.
    
    En realidad, copia el contenido de la lista, no el nombre de la lista.
    
    Esto es exactamente lo que necesitas. Echa un vistazo al fragmento de código a continuación:
    
    ```python
    list_1 = [1]
    list_2 = list_1[:]
    list_1[0] = 2
    print(list_2)
    ```
    
    Su output es [1].
    
    Esta parte no visible del código descrito como [:] puede producir una lista completamente nueva.
    
    Una de las formas más generales de la rebanada es la siguiente:
    
    ```python
    my_list[inicio:fin]
    ```
    
    Como puedes ver, se asemeja a la indexación, pero los dos puntos en el interior hacen una gran diferencia.
    
    Una rebanada de este tipo **crea una nueva lista (de destino), tomando elementos de la lista de origen - los elementos de los índices desde el start hasta el fin fin - 1**.
    
    Nota: no hasta el fin sino hasta fin-1. Un elemento con un índice igual a fin es el primer elemento el cual **no participa en la segmentación**.
    
    Es posible utilizar valores negativos tanto para el inicio como para el fin(al igual que en la indexación).
    
    Echa un vistazo al fragmento:
    
    ```python
    my_list = [10, 8, 6, 4, 2]
    new_list = my_list[1:3]
    print(new_list)
    ```
    
    La lista new_list contendrá fin - inicio (3 - 1 = 2) elementos ‒ los que tienen índices iguales a 1 y 2 (pero no 3).
    
    La output del fragmento es: [8, 6]
    
    Ejecuta el código en el editor para ver cómo Python copia la lista completa y un fragmento de la lista.
    
    ```python
    # Copiando la lista completa.
    list_1 = [1]
    list_2 = list_1[:]
    list_1[0] = 2
    print(list_2)
    
    # Copiando parte de la lista.
    my_list = [10, 8, 6, 4, 2]
    new_list = my_list[1:3]
    print(new_list)
    ```
    
- **Rebanadas índices negativos**
    
    Observa el fragmento de código a continuación:
    
    ```python
    my_list[start:end]
    ```
    
    Para confirmar:
    
    - start es el índice del primer elemento **incluido en la rebanada**.
    - end es el índice del primer elemento **no incluido en la rebanada**.
    
    Así es como **los índices negativos** funcionan en las rebanadas:
    
    ```python
    my_list = [10, 8, 6, 4, 2]
    new_list = my_list[1:-1]
    print(new_list)
    ```
    
    El resultado del fragmento es:
    
    ```python
    [8, 6, 4]
    ```
    
    Si start especifica un elemento que se encuentra más allá del descrito por end (desde el punto de vista inicial de la lista), la rebanada estará **vacía**:
    
    ```python
    my_list = [10, 8, 6, 4, 2]
    new_list = my_list[-1:1]
    print(new_list)
    ```
    
    La output del fragmento es:
    
    ```python
    []
    ```
    
    Si omites el start en tu rebanada, se supone que deseas obtener un segmento que comienza en el elemento con índice 0.
    
    En otras palabras, la rebanada sería de esta forma:
    
    ```python
    my_list[:end]
    ```
    
    es un equivalente más compacto de:
    
    ```python
    my_list[0:end]
    ```
    
    Observa el fragmento de código a continuación:
    
    ```python
    my_list = [10, 8, 6, 4, 2]
    new_list = my_list[:3]
    print(new_list)
    ```
    
    Es por esto que su output es: [10, 8, 6].
    
    Del mismo modo, si omites el end en tu rebanada, se supone que deseas que el segmento termine en el elemento con el índice len(my_list).
    
    En otras palabras, la rebanada sería de esta forma:
    
    ```python
    my_list[start:]
    ```
    
    es un equivalente más compacto de:
    
    ```python
    my_list[start:len(my_list)]
    ```
    
    Observa el siguiente fragmento de código:
    
    ```python
    my_list = [10, 8, 6, 4, 2]
    new_list = my_list[3:]
    print(new_list)
    ```
    
    Por lo tanto, la output es: [4, 2].
    
    Como hemos dicho anteriormente, el omitir el inicio y fin crea una copia **de toda la lista**:
    
    ```python
    my_list = [10, 8, 6, 4, 2]
    new_list = my_list[:]
    print(new_list)
    ```
    
    El resultado del fragmento es: [10, 8, 6, 4, 2].
    
- **Más sobre la instrucción “del”**
    
    La instrucción del descrita anteriormente puede **eliminar más de un elemento de la lista a la vez - también puede eliminar rebanadas**:
    
    ```python
    my_list = [10, 8, 6, 4, 2]
    del my_list[1:3]
    print(my_list)
    ```
    
    Nota: En este caso, la rebanada **¡no produce ninguna lista nueva!**
    
    La output del fragmento es: [10, 4, 2].
    
    También es posible eliminar **todos los elementos** a la vez:
    
    ```python
    my_list = [10, 8, 6, 4, 2]
    del my_list[:]
    print(my_list)
    ```
    
    La lista se queda vacía y la output es: [].
    
    Al eliminar la rebanada del código, su significado cambia dramáticamente.
    
    Echa un vistazo:
    
    ```python
    my_list = [10, 8, 6, 4, 2]
    del my_list
    print(my_list)
    ```
    
    La instrucción del **eliminará la lista, no su contenido**.
    
    La función print() de la última línea del código provocará un error de ejecución.
    
- **Resumen**
    
    Si tienes una lista list_1, y la siguiente asignación: list_2 = list_1 esto no hace una copia de la lista list_1, pero hace que las variables list_1 y list_2 **apunten a la misma lista en la memoria**. Por ejemplo:
    
    ```python
    vehicles_one = ['coche', 'bicicleta', 'motor']
    print(vehicles_one) # output: ['coche', 'bicicleta', 'motor']
     
    vehicles_two = vehicles_one
    del vehicles_one[0] # elimina 'coche'
    print(vehicles_two) # output: ['bicicleta', 'motor']
    ```
    
    Si deseas copiar una lista o parte de la lista, puedes hacerlo haciendo uso de **rebanadas**:
    
    ```python
    colors = ['rojo', 'verde', 'naranja']
     
    copy_whole_colors = colors[:]  # copia la lista entera
    copy_part_colors = colors[0:2]  # copia parte de la lista
    ```
    
    También puede utilizar **índices negativos** para hacer uso de rebanadas. Por ejemplo:
    
    ```python
    sample_list = ["A", "B", "C", "D", "E"]
    new_list = sample_list[2:-1]
    print(new_list)  # output: ['C', 'D']
    ```
    
    Los parámetros start y end son **opcionales** al partir en rebanadas una lista: list[start:end], por ejemplo:
    
    ```python
    my_list = [1, 2, 3, 4, 5]
    slice_one = my_list[2: ]
    slice_two = my_list[ :2]
    slice_three = my_list[-2: ]
     
    print(slice_one)  # output: [3, 4, 5]
    print(slice_two)  # output: [1, 2]
    print(slice_three)  # output: [4, 5]
    ```
    
    Puedes **eliminar rebanadas** utilizando la instrucción del:
    
    ```python
    my_list = [1, 2, 3, 4, 5]
    del my_list[0:2]
    print(my_list)  # output: [3, 4, 5]
     
    del my_list[:]
    print(my_list)  # elimina el contenido de la lista, genera: []
    ```
    
    Puedes probar si algunos elementos **existen en una lista o no** utilizando las palabras clave in y not in, por ejemplo:
    
    ```python
    my_list = ["A", "B", 1, 2]
     
    print("A" in my_list)  # output: True
    print("C" not in my_list)  # output: True
    print(2 not in my_list)  # output: Fals
    ```
    

**Los operadores in y not in**:

Python ofrece dos operadores muy poderosos, capaces de **revisar la lista para verificar si un valor específico está almacenado dentro de la lista o no**.

Estos operadores son:

```python
elem in my_list
elem not in my_list
```

El primero de ellos (in) verifica si un elemento dado (el argumento izquierdo) está actualmente almacenado en algún lugar dentro de la lista (el argumento derecho) - el operador devuelve True en este caso.

El segundo (not in) comprueba si un elemento dado (el argumento izquierdo) está ausente en una lista - el operador devuelve True en este caso.

Observa el código en el editor. El fragmento muestra ambos operadores en acción. ¿Puedes adivinar su output? Ejecuta el programa para comprobar si tenías razón.

```python
my_list = [0, 3, 12, 8, 2]

print(5 in my_list)
print(5 not in my_list)
print(12 in my_list)
```

**Listas - Algunos programas simples**:

- **Explicación**
    
    El concepto es bastante simple - asumimos temporalmente que el primer elemento es el más grande y comparamos la hipótesis con todos los elementos restantes de la lista.
    
    El código da como resultado el 17 (como se espera).
    
    El código puede ser reescrito para hacer uso de la forma recién introducida del bucle for:
    
    ```python
    my_list = [17, 3, 11, 5, 1, 9, 7, 15, 13]
    largest = my_list[0]
     
    for i in my_list:
        if i > largest:
            largest = i
     
    print(largest)
    ```
    
    El programa anterior realiza una comparación innecesaria, cuando el primer elemento se compara consigo mismo, pero esto no es un problema en absoluto.
    
    El código da como resultado el 17 también (nada inusual).
    
    Si necesitas ahorrar energía de la computadora, puedes usar una rebanada:
    
    ```python
    my_list = [17, 3, 11, 5, 1, 9, 7, 15, 13]
    largest = my_list[0]
     
    for i in my_list[1:]:
        if i > largest:
            largest = i
     
    print(largest)
    ```
    
    La pregunta es: ¿Cuál de estas dos acciones consume más recursos informáticos - solo una comparación o rebanar casi todos los elementos de una lista?
    
    Ahora encontremos la ubicación de un elemento dado dentro de una lista:
    
    ```python
    my_list = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
    to_find = 5
    found = False
     
    for i in range(len(my_list)):
        found = my_list[i] == to_find
        if found:
            break
     
    if found:
        print("Elemento encontrado en el índice", i)
    else:
        print("ausente")
    ```
    
    Nota:
    
    - el valor buscado se almacena en la variable to_find;
    - el estado actual de la búsqueda se almacena en la variable found (True/False).
    - cuando found se convierte en True, se sale del bucle for.
    
    Supongamos que has elegido los siguientes números en la lotería: 3, 7, 11, 42, 34, 49.
    
    Los números que han salido sorteados son: 5, 11, 9, 42, 3, y 49.
    
    La pregunta es: ¿A cuántos números le has atinado?
    
    Este programa te dará la respuesta:
    
    ```python
    drawn = [5, 11, 9, 42, 3, 49]
    bets = [3, 7, 11, 42, 34, 49]
    hits = 0
     
    for number in bets:
        if number in drawn:
            hits += 1
     
    print(hits)
    ```
    
    Nota:
    
    - la lista drawn almacena todos los números sorteados;
    - La lista bets almacena los números con que se juega;
    - la variable hits cuenta tus aciertos.
    
    la output del programa es: 4.
    

**Operaciones avanzadas de listas**:

- **Listas dentro de listas**
    
    Las listas pueden constar de escalares (es decir, números) y elementos de una estructura mucho más compleja (ya has visto ejemplos como cadenas, booleanos o incluso otras listas en las lecciones del Resumen de la Sección anterior). Veamos más de cerca el caso en el que **los elementos de una lista son listas**.
    
    A menudo encontramos estos **arreglos** en nuestras vidas. Probablemente el mejor ejemplo de esto sea un **tablero de ajedrez**.
    
    Un tablero de ajedrez está compuesto de filas y columnas. Hay ocho filas y ocho columnas. Cada columna está marcada con las letras de la A a la H. Cada línea está marcada con un número del uno al ocho.
    
    La ubicación de cada campo se identifica por pares de letras y dígitos. Por lo tanto, sabemos que la esquina inferior derecha del tablero (la que tiene la torre blanca) es A1, mientras que la esquina opuesta es H8.
    
    Supongamos que podemos usar los números seleccionados para representar cualquier pieza de ajedrez. También podemos asumir que **cada fila en el tablero de ajedrez es una lista**.
    
    Observa el siguiente código:
    
    ```python
    row = []
     
    for i in range(8):
        row.append(WHITE_PAWN)
    ```
    
    Crea una lista que contiene ocho elementos que representan la segunda fila del tablero de ajedrez: la que está llena de peones (supon que WHITE_PAWN es un **símbolo predefinido** que representa un peón blanco).
    
    ### Comprensión de listas
    
    El mismo efecto se puede lograr mediante una **comprensión de lista**, la sintaxis especial utilizada por Python para completar o llenar listas masivas.
    
    Una comprensión de lista es en realidad una lista, pero **se creó sobre la marcha durante la ejecución del programa, y no se describe de forma estática**.
    
    Echa un vistazo al fragmento:
    
    ```python
    row = [WHITE_PAWN for i in range(8)]
    ```
    
    La parte del código colocada dentro de los paréntesis especifica:
    
    - los datos que se utilizarán para completar la lista (WHITE_PAWN)
    - la cláusula que especifica cuántas veces se producen los datos dentro de la lista (for i in range(8))
    
- **Ejemplos de Comprensión de listas**
    
    Permítenos mostrarte otros **ejemplos de comprensión de lista**:
    
    **Ejemplo #1:**
    
    ```python
    squares = [x ** 2 for x in range(10)]
    ```
    
    El fragmento de código genera una lista de diez elementos y la rellena con cuadrados de diez números enteros que comienzan desde cero (0, 1, 4, 9, 16, 25, 36, 49, 64, 81)
    
    **Ejemplo #2:**
    
    ```python
    twos = [2 ** i for i in range(8)]
    ```
    
    El fragmento crea un arreglo de ocho elementos que contiene las primeras ocho potencias del numero dos (1, 2, 4, 8, 16, 32, 64, 128)
    
    **Ejemplo #3:**
    
    ```python
    odds = [x for x in squares if x % 2 != 0 ]
    ```
    
    El fragmento crea una lista con solo los elementos impares de la lista squares.
    
- **Arreglo de dos dimensiones**
    
    Supongamos también que un **símbolo predefinido** denominado EMPTY designa un campo vacío en el tablero de ajedrez.
    
    Entonces, si queremos crear una lista de listas que representan todo el tablero de ajedrez, se puede hacer de la siguiente manera:
    
    ```python
    board = []
     
    for i in range(8):
        row = [EMPTY for i in range(8)]
        board.append(row)
    ```
    
    Nota:
    
    - la parte interior del bucle crea una fila que consta de ocho elementos (cada uno de ellos es igual a EMPTY) y lo agrega a la lista del board;
    - la parte exterior se repite ocho veces;
    - en total, la lista board consta de 64 elementos (todos iguales a EMPTY).
    
    Este modelo imita perfectamente el tablero de ajedrez real, que en realidad es una lista de elementos de ocho elementos, todos ellos en filas individuales. Resumamos nuestras observaciones:
    
    - los elementos de las filas son campos, ocho de ellos por fila;
    - los elementos del tablero de ajedrez son filas, ocho de ellos por tablero de ajedrez.
    
    La variable board ahora es un **arreglo bidimensional**. También se le llama, por analogía a los términos algebraicos, una **matriz**.
    
    Como las listas de comprensión puede ser **anidadas**, podemos acortar la creación del tablero de la siguiente manera:
    
    ```python
    board = [[EMPTY for i in range(8)] for j in range(8)]
    ```
    
    La parte interna crea una fila, y la parte externa crea una lista de filas.
    
    El acceso al campo seleccionado del tablero requiere dos índices - el primero selecciona la fila; el segundo - el número del campo dentro de la fila, el cual es un número de columna.
    
    Echa un vistazo al tablero de ajedrez. Cada campo contiene un par de índices que se deben dar para acceder al contenido del campo:
    
    ![Untitled](/assets/PythonBasico/Untitled.png)
    
    Echando un vistazo a la figura que se muestra arriba, coloquemos algunas piezas de ajedrez en el tablero. Primero, agreguemos todas las torres:
    
    ```python
    board[0][0] = ROOK
    board[0][7] = ROOK
    board[7][0] = ROOK
    board[7][7] = ROOK
    ```
    
    Si deseas agregar un caballo a C4, hazlo de la siguiente manera:
    
    ```python
    board[4][2] = KNIGHT
    ```
    
    Y ahora un peón a E5:
    
    ```python
    board[3][4] = PAWN
    ```
    
    Eje:
    
    ```python
    board = []
    EMPTY = []
    
    for i in range(8):
        row = [EMPTY for i in range(8)]
        board.append(row)
        
    board[0][0] = "ROOK" # Coloca la torre en la primer fila en la primer columna
    board[0][7] = "ROOK" # Coloca la torre en la primer fila en la septima columna
    board[7][0] = "ROOK" # Coloca la torre en la septima fila en la primer columna
    board[7][7] = "ROOK" # Coloca la torre en la septima fila en la septima columna
    board[0][1] = "KNIGHT"
    board[0][6] = "KNIGHT"
    board[0][2] = "BISHOP"
    board[0][5] = "BISHOP"
    board[0][3] = "QUEEN"
    board[0][4] = "KING"
    print(board)
    ```
    
- ****Naturaleza multidimensional de las listas: aplicaciones avanzadas****
    
    Profundicemos en la naturaleza multidimensional de las listas. Para encontrar cualquier elemento de una lista bidimensional, debes usar dos *coordenadas*:
    
    - Una vertical (número de fila).
    - Una horizontal (número de columna).
    
    Imagina que desarrollas una pieza de software para una estación meteorológica automática. El dispositivo registra la temperatura del aire cada hora y lo hace durante todo el mes. Esto te da un total de 24 × 31 = 744 valores. Intentemos diseñar una lista capaz de almacenar todos estos resultados.
    
    Primero, debes decidir qué tipo de datos sería adecuado para esta aplicación. En este caso, sería mejor un float, ya que este termómetro puede medir la temperatura con una precisión de 0.1 ℃.
    
    Luego tomarás la decisión arbitraria de que las filas registrarán las lecturas cada hora exactamente (por lo que la fila tendrá 24 elementos) y cada una de las filas se asignará a un día del mes (supongamos que cada mes tiene 31 días, por lo que necesita 31 filas). Aquí está el par apropiado de comprensiones (h es para las horas, d para el día):
    
    ```python
    temps = [[0.0 for h in range(24)] for d in range(31)]
    ```
    
    Toda la matriz está llena de ceros ahora. Puede suponer que se actualiza automáticamente utilizando agentes de hardware especiales. Lo que tienes que hacer es esperar a que la matriz se llene con las mediciones.
    
    Ahora es el momento de determinar la temperatura promedio mensual del mediodía. Suma las 31 lecturas registradas al mediodía y divida la suma por 31. Puedes suponer que la temperatura de medianoche se almacena primero. Aquí está el código:
    
    ```python
    temps = [[0.0 for h in range(24)] for d in range(31)]
    #
    # La matriz se actualiza aquí.
    #
     
    total = 0.0
     
    for day in temps:
        total += day[11]
     
    average = total / 31
     
    print("Temperatura promedio al mediodía:", average)
    ```
    
    Nota: La variable day utilizada por el bucle for no es un escalar - cada paso a través de la matriz temps lo asigna a la siguiente fila de la matriz; Por lo tanto, es una lista. Se debe indexar con 11 para acceder al valor de temperatura medida al mediodía.
    
    Ahora encuentra la temperatura más alta durante todo el mes - analiza el código:
    
    ```python
    Note:
    
    la variable day itera en todas las filas de la matriz temps;
    la variable temp itera a través de todas las mediciones tomadas en un día.
    ```
    
    Ahora cuenta los días en que la temperatura al mediodía fue de al menos 20 ℃:
    
    ```python
    temps = [[0.0 for h in range(24)] for d in range(31)]
    #
    # La matriz se actualiza aquí.
    #
     
    hot_days = 0
     
    for day in temps:
        if day[11] > 20.0:
            hot_days += 1
     
    print(hot_days, "fueron los días calurosos.")
    ```
    
    Python no limita la profundidad de la inclusión lista en lista. Aquí puedes ver un ejemplo de un arreglo tridimensional:
    
    ```python
    rooms = [[[False for r in range(20)] for f in range(15)] for t in range(3)]
    ```
    
    Imagina un hotel. Es un hotel enorme que consta de tres edificios, de 15 pisos cada uno. Hay 20 habitaciones en cada piso. Para esto, necesitas un arreglo que pueda recopilar y procesar información sobre las habitaciones ocupadas/libres.
    
    Primer paso - El tipo de elementos del arreglo. En este caso, sería un valor Booleano (True/False).
    
    Paso dos - análisis de la situación. Resume la información disponible: tres edificios, 15 pisos, 20 habitaciones.
    
    Ahora puedes crear el arreglo:
    
    ```python
    rooms = [[[False for r in range(20)] for f in range(15)] for t in range(3)]
    ```
    
    El primer índice (0 a 2) selecciona uno de los edificios; el segundo (0 a 14) selecciona el piso, el tercero (0 a 19) selecciona el número de habitación. Todas las habitaciones están inicialmente desocupadas.
    
    Ahora ya puedes reservar una habitación para dos recién casados: en el segundo edificio, en el décimo piso, habitación 14:
    
    ```python
    rooms[1][9][13] = True
    ```
    
    y desocupar el segundo cuarto en el quinto piso ubicado en el primer edificio:
    
    ```python
    rooms[0][4][1] = False
    ```
    
    Verifica si hay disponibilidad en el piso 15 del tercer edificio:
    
    ```python
    vacancy = 0
     
    for room_number in range(20):
        if not rooms[2][14][room_number]:
            vacancy += 1
    ```
    
    La variable vacancy contiene 0 si todas las habitaciones están ocupadas, o en dado caso el número de habitaciones disponibles.
    
- **Resumen**
    
    1. **La comprensión de listas** te permite crear nuevas listas a partir de las existentes de una manera concisa y elegante. La sintaxis de una comprensión de lista es la siguiente:
    
    ```python
    [expresión para el elemento en la lista si es condicional]
    ```
    
    El cual es un equivalente del siguiente código:
    
    ```python
    for element in list:
        if conditional:
            expression
    ```
    
    Este es un ejemplo de una comprensión de lista - el código siguiente crea una lista de cinco elementos con los primeros cinco números naturales elevados a la potencia de 3:
    
    ```python
    cubed = [num ** 3 for num in range(5)]
    print(cubed) # output: [0, 1, 8, 27, 64]
    ```
    
    2. Puedes usar **listas anidadas** en Python para crear **matrices** (es decir, listas bidimensionales). Por ejemplo:
    
    ![Untitled](/assets/PythonBasico/Untitled1.png)
    
    ```python
    # Una tabla de cuatro columnas y cuatro filas: un arreglo bidimensional (4x4)
     
    table = [[":(", ":)", ":(", ":)"],
             [":)", ":(", ":)", ":)"],
             [":(", ":)", ":)", ":("],
             [":)", ":)", ":)", ":("]]
     
    print(table)
    print(table[0][0])  # output: ':('
    print(table[0][3])  # output: ':)'
    ```
    
    3. Puedes anidar tantas listas en las listas como desee y, por lo tanto, crear listas n-dimensionales, por ejemplo, arreglos de tres, cuatro o incluso sesenta y cuatro dimensiones. Por ejemplo:
    
    ![Untitled](/assets/PythonBasico/Untitled2.png)
    
    ```python
    # Cubo - un arreglo tridimensional (3x3x3)
     
    cube = [[[':(', 'x', 'x'],
             [':)', 'x', 'x'],
             [':(', 'x', 'x']],
     
            [[':)', 'x', 'x'],
             [':(', 'x', 'x'],
             [':)', 'x', 'x']],
     
            [[':(', 'x', 'x'],
             [':)', 'x', 'x'],
             [':)', 'x', 'x']]]
     
    print(cube)
    print(cube[0][0][0])  # output: ':('
    print(cube[2][2][0])  # output: ':)'
    ```
    

**Funciones**:

- **¿Por qué se necesitan?**
    
    Hasta ahorita has implementado varias veces el uso de **funciones**, pero solo se han visto algunas de sus ventajas. Solo se han invocado funciones para utilizarlas como herramientas, con el fin de hacer la vida más fácil, y para simplificar tareas tediosas y repetitivas.
    
    Cuando se desea mostrar o imprimir algo en consola se utiliza print(). Cuando se desea leer el valor de una variable se emplea input(), combinados posiblemente con int() o float().
    
    También se ha hecho uso de algunos **métodos**, los cuales también son funciones, pero declarados de una manera muy específica.
    
    Ahora aprenderás a escribir tus propias funciones, y como utilizarlas. Escribiremos varias de ellas juntos, desde muy sencillas hasta algo complejas. Se requerirá de tu concentración y atención.
    
    Muy a menudo ocurre que un cierto fragmento de código **se repite muchas veces en un programa**. Se repite de manera literal o, con algunas modificaciones menores, empleando algunas otras variables dentro del programa. También ocurre que un programador ha comenzado a copiar y pegar ciertas partes del código en más de una ocasión en el mismo programa.
    
    Puede ser muy frustrante percatarse de repente que existe un error en el código copiado. El programador tendrá que escarbar bastante para encontrar todos los lugares en el código donde hay que corregir el error. Además, existe un gran riesgo de que las correcciones produzcan errores adicionales.
    
    Definamos la primera condición por la cual es una buena idea comenzar a escribir funciones propias: **si un fragmento de código comienza a aparecer en más de una ocasión, considera la posibilidad de aislarlo en la forma de una función** invocando la función desde el lugar en el que originalmente se encontraba.
    
    Puede suceder que el algoritmo que se desea implementar sea tan complejo que el código comience a crecer de manera incontrolada y, de repente, ya no se puede navegar por él tan fácilmente.
    
    Se puede intentar solucionar este problema comentando el código, pero pronto te darás cuenta que esto empeorará la situación - **demasiados comentarios hacen que el código sea más difícil de leer y entender**. Algunos dicen que **una función bien escrita debe ser comprensible con tan solo una mirada**.
    
    Un buen desarrollador **divide el código** (o mejor dicho: el problema) en piezas aisladas, y **codifica cada una de ellas en la forma de una función**.
    
    ![Untitled](/assets/PythonBasico/Untitled3.png)
    
    Esto simplifica considerablemente el trabajo del programa, debido a que cada pieza se codifica por separado, y consecuentemente se prueba por separado. A este proceso se le llama comúnmente **descomposición**.
    
    Existe una segunda condición: **si un fragmento de código se hace tan extenso que leerlo o entenderlo se hace complicado, considera dividirlo en pequeños problemas, por separado, e implementa cada uno de ellos como una función independiente.**.
    
    Esta descomposición continúa hasta que se obtiene un conjunto de funciones cortas, fáciles de comprender y probar.
    
- **Descomposición**
    
    ![Untitled](/assets/PythonBasico/Untitled4.png)
    
    Es muy común que un programa sea tan largo y complejo que no puede ser asignado a un solo desarrollador, y en su lugar un **equipo de desarrolladores** trabajarán en el. El problema, debe ser dividido entre varios desarrolladores de una manera en que se pueda asegurar su eficiencia y cooperación.
    
    Es inconcebible que más de un programador deba escribir el mismo código al mismo tiempo, por lo tanto, el trabajo debe de ser dividido entre todos los miembros del equipo.
    
    Este tipo de descomposición tiene diferentes propósitos, no solo se trata de **compartir el trabajo**, sino también de **compartir la responsabilidad** entre varios desarrolladores.
    
    Cada uno debe escribir un conjunto bien definido y claro de funciones, las cuales al ser **combinadas dentro de un módulo** (esto se clarificara un poco más adelante) nos dará como resultado el producto final.
    
    Esto nos lleva directamente a la tercera condición: si se va a dividir el trabajo entre varios programadores, **se debe descomponer el problema para permitir que el producto sea implementado como un conjunto de funciones escritas por separado empacadas juntas en diferentes módulos**.
    
- **¿De dónde provienen las funciones?**
    
    En general, las funciones provienen de al menos tres lugares:
    
    De **Python** mismo: varias funciones (como print()) son una **parte integral de Python**, y siempre están disponibles sin algún esfuerzo adicional del programador; se les llama a estas **funciones integradas**. Puedes ver una lista completa de las funciones integradas de Python en la siguiente liga [https://docs.python.org/3/library/functions.html](https://docs.python.org/3/library/functions.html).
    
    De los **módulos preinstalados** de Python: muchas de las funciones, las cuales comúnmente son menos utilizadas que las integradas, están disponibles en módulos instalados juntamente con Python; para poder utilizar estas funciones el programador debe realizar algunos pasos adicionales (se explicará acerca de esto en un momento).
    
    **Directamente del código**: tu puedes escribir tus propias funciones, colocarlas dentro del código, y usarlas libremente.
    
    **?**: Existe una posibilidad más, las funciones lambda (aprenderás acerca de ellas en el curso *Fundamentos de Python 2*).
    
- **Ejemplos**
    
    Observa el fragmento de código en el editor.
    
    ```python
    print("Ingresa un valor: ")
    a = int(input())
    
    print("Ingresa un valor: ")
    b = int(input())
    
    print("Ingresa un valor: ")
    c = int(input())
    ```
    
    Es bastante sencillo, es un ejemplo de como **transformar una parte de código que se está repitiendo en una función**.
    
    El mensaje enviado a la consola por la función print() es siempre el mismo. El código es funcional y no contiene errores, sin embargo imagina que tendrías que hacer si tu jefe pidiera cambiar el mensaje para que fuese más cortés, por ejemplo, que comience con la frase "Por favor,".
    
    Tendrías que tomar algo de tiempo para cambiar el mensaje en todos los lugares donde aparece (podrías hacer uso de copiar y pegar, pero eso no lo haría más sencillo). Es muy probable que cometas errores durante el proceso de corrección, eso traería frustración a ti y a tu jefe.
    
    ¿Es posible separar ese código *repetido*, darle un nombre y hacerlo reutilizable? Significaría que **el cambio hecho en un solo lugar será propagado a todos los lugares donde se utilice**.
    
    Para que esto funcione, dicho código debe ser invocado cada vez que se requiera.
    
    Es posible, esto es exactamente para lo que existen las funciones.
    
    Se necesita **definirla**. Aquí, la palabra *definir* es significativa.
    
    Así es como se ve la definición más simple de una función:
    
    ```python
    def function_name():
        cuerpo de la función
    ```
    
    - Siempre comienza con la **palabra reservada def** (que significa *definir*)
    - después de def va el **nombre de la función** (las reglas para darle nombre a las funciones son las mismas que para las variables)
    - después del nombre de la función, hay un espacio para un par de **paréntesis** (ahorita no contienen algo, pero eso cambiará pronto)
    - la línea debe de terminar con **dos puntos**;
    - la línea inmediatamente después de def marca el comienzo del **cuerpo de la función** - donde varias o (al menos una) **instrucción anidada**, será ejecutada cada vez que la función sea invocada; nota: la **función termina donde el anidamiento termina**, se debe ser cauteloso.
        
        
    
    A continuación se **definirá** la función. Se llamará message ‒ aquí está:
    
    ```python
    def message():
        print("Ingresar valor: ")
    ```
    
    La función es muy sencilla, pero completamente **utilizable**. Se ha nombrado message, pero eso es opcional, tu puedes cambiarlo. Hagamos uso de ella.
    
    El código ahora contiene la definición de la función:
    
    ```python
    def message():
        print("Ingresar valor: ")
     
    print("Inicia aqui.")
    print("Termina aqui.")
    ```
    
    Nota: no se está utilizando la función - no se está **invocando** en el código.
    
    Al correr el programa, se mostrará lo siguiente:
    
    ```python
    Inicia aqui.
    Termina aqui.
    ```
    
    Esto significa que Python lee la definición de la función y la recuerda, pero no la ejecuta sin tu permiso.
    
    Se ha modificado el código - se ha insertado la **invocación de la función** entre los dos mensajes:
    
    ```python
    def message():
        print("Ingresar valor: ")
     
    print("Inicia aqui.")
    message()
    print("Termina aqui.")
    ```
    
    La salida ahora se ve diferente:
    
    ```python
    Inicia aqui.
    Ingresar valor:
    Termina aqui.
    ```
    
- **¿Cómo funcionan las funciones?**
    
    Observa la imagen.
    
    ![https://skillsforall.com/content/pe1/1.0/m4/course/es-XL/assets/e96e512543eaa49bbdaf70b79b1488b1da38804d.png](https://skillsforall.com/content/pe1/1.0/m4/course/es-XL/assets/e96e512543eaa49bbdaf70b79b1488b1da38804d.png)
    
    Intenta mostrarte el proceso completo:
    
    - cuando se **invoca** una función, Python recuerda el lugar donde esto ocurre y *salta* hacia dentro de la función invocada;
    - el cuerpo de la función es entonces **ejecutado**;
    - al llegar al final de la función, Python **regresa** al lugar inmediato después de donde ocurrió la invocación.
    
    Existen dos consideraciones muy importantes, la primera de ella es:
    
    **No se debe invocar una función antes de que se haya definido.**
    
    Recuerda - Python lee el código de arriba hacia abajo. No va a adelantarse en el código para determinar si la función invocada está definida más adelante, (el lugar "correcto" para definirla es "antes de ser invocada".)
    
    Se ha insertado un error en el siguiente código - ¿puedes notar la diferencia?
    
    ```python
    print("Inicia aqui.")
    message()
    print("Termina aqui.")
     
     
    def message():
        print("Ingresar valor: ")
    ```
    
    Se ha movido la función al final del código. ¿Podrá Python encontrarla cuando la ejecución llegue a la invocación?
    
    No, no podrá. El mensaje de error dirá:
    
    ```python
    NameError: name 'message' is not defined
    ```
    
    No intentes forzar a Python a encontrar funciones que no están definidas en el lugar correcto.
    
    La segunda consideración es más sencilla:
    
    **Una función y una variable no pueden compartir el mismo nombre**.
    
    El siguiente fragmento de código es erróneo:
    
    ```python
    def message():
        print("Ingresar valor: ")
     
    message = 1
    ```
    
    El asignar un valor al nombre message causa que Python olvide su rol anterior. La función con el nombre de message ya no estará disponible.
    
    Afortunadamente, es posible **combinar o mezclar el código con las funciones** - no es forzoso colocar todas las funciones al inicio del archivo fuente.
    
    Observa el siguiente código:
    
    ```python
    print("Comienza aqui.")
     
     
    def message():
        print("Ingresar valor: ")
     
    message()
     
    print("Termina aqui.")
    ```
    
    Puede verse extraño, pero es completamente correcto, y funciona como se necesita.
    
    Regresemos al ejemplo inicial para implementar la función de manera correcta:
    
    ```python
    def message():
        print("Ingresar valor: ")
     
    message()
    a = int(input())
    message()
    b = int(input())
    message()
    c = int(input())
    ```
    
    El modificar el mensaje de entrada es ahora sencillo - se puede hacer con solo **modificar el código una única vez** - dentro del cuerpo de la función.
    

**Funciones - ¿Cómo se comunican las funciones en su entorno?**

- **Funciones Parametrizadas**
    
    El potencial completo de una función se revela cuando puede ser equipada con una interface que es capaz de aceptar datos provenientes de la invocación. Dichos datos pueden modificar el comportamiento de la función, haciéndola más flexible y adaptable a condiciones cambiantes.
    
    Un parámetro es una variable, pero existen dos factores que hacen a un parámetro diferente:
    
    - **los parámetros solo existen dentro de las funciones en donde han sido definidos**, y el único lugar donde un parámetro puede ser definido es entre los paréntesis después del nombre de la función, donde se encuentra la palabra clave reservada def;
    - **la asignación de un valor a un parámetro de una función se hace en el momento en que la función se manda llamar o se invoca**, especificando el argumento correspondiente.
    
    ```python
    def function(parameter):
        ###
    ```
    
    Recuerda que:
    
    - **los parámetros solo existen dentro de las funciones** (este es su entorno natural)
    - **los argumentos existen fuera de las funciones**, y son los que pasan los valores a los parámetros correspondientes.
    
    Existe una clara división entre estos dos mundos.
    
    Enriquezcamos la función anterior agregándole un parámetro - se utilizará para mostrar al usuario el valor de un número que la función pide.
    
    Se tendrá que modificar la definición def de la función - así es como se ve ahora:
    
    ```python
    def message(number):
        ###
    ```
    
    Esta definición especifica que nuestra función opera con un solo parámetro con el nombre de number. Se puede utilizar como una variable normal, pero **solo dentro de la función** - no es visible en otro lugar.
    
    Ahora hay que mejorar el cuerpo de la función:
    
    ```python
    def message(number):
        print("Ingresa un número:", number)
    ```
    
    Se ha hecho buen uso del parámetro. Nota: No se le ha asignado al parámetro algún valor. ¿Es correcto?
    
    Si, lo es.
    
    Un valor para el parámetro llegará del entorno de la función.
    
    Recuerda: **especificar uno o más parámetros en la definición de la función** es un requerimiento, y se debe de cumplir durante la invocación de la misma. Se debe **proveer el mismo número de argumentos como haya parámetros definidos**.
    
    El no hacerlo provocará un error.
    
    Intenta ejecutar el código en el editor.
    
    ```python
    def message(number):
        print("Ingresa un número:", number)
    
    message()
    ```
    
    Esto es lo que aparecerá en consola:
    
    ```python
    TypeError: message() missing 1 required positional argument: 'number'
    ```
    
    Aquí está ya de manera correcta:
    
    ```python
    def message(number):
        print("Ingresa un número:", number)
     
    message(1)
    ```
    
    De esta manera ya está correcto. El código producirá la siguiente salida:
    
    ```python
    	Ingresa un número: 1
    ```
    
    ¿Puedes ver cómo funciona? El valor del argumento utilizado durante la invocación (1) ha sido pasado a la función, dándole un valor inicial al parámetro con el nombre de number.
    
    ---
    
    Existe una circunstancia importante que se debe mencionar.
    
    Es posible tener una **variable con el mismo nombre del parámetro de la función**.
    
    El siguiente código muestra un ejemplo de esto:
    
    ```python
    def message(number):
        print("Ingresa un número:", number)
     
    number = 1234
    message(1)
    print(number)
    ```
    
    Una situación como la anterior activa un mecanismo denominado **sombreado**:
    
    - el parámetro x sombrea cualquier variable con el mismo nombre, pero...
    - ... solo dentro de la función que define el parámetro.
    
    El parámetro llamado number es una entidad completamente diferente de la variable llamada number.
    
    Esto significa que el código anterior producirá la siguiente salida:
    
    ```python
    Ingresa un número: 1
    1234
    ```
    
    Una función puede tener **tantos parámetros como se desee**, pero entre más parámetros, es más difícil memorizar su rol y propósito.
    
    Modifiquemos la función- ahora tiene **dos parámetros**:
    
    ```python
    def message(what, number):
        print("Ingresa", what, "número", number)
    ```
    
    Esto significa que para invocar la función se necesitan **dos argumentos**.
    
    El primer parámetro va a contener el nombre del valor deseado.
    
    ![Untitled](/assets/PythonBasico/Untitled5.png)
    
    Aquí está:
    
    ```python
    def message(what, number):
        print("Ingresa", what, "número", number)
     
    message("teléfono", 11)
    message("precio", 5)
    message("número", "number")
    ```
    
    Esta es la salida del código anterior:
    
    ```python
    Ingresa teléfono número 11
    Ingresa precio número 5
    Ingresa número número number
    ```
    
- **Paso de parámetros posicionales**
    
    La técnica que asigna cada argumento al parámetro correspondiente, es llamada **paso de parámetros posicionales**, los argumentos pasados de esta manera son llamados **argumentos posicionales**.
    
    Ya se ha utilizado, pero Python ofrece mucho más. Se abordará este tema a continuación.
    
    ```python
    def my_function(a, b, c):
        print(a, b, c)
     
    my_function(1, 2, 3)
    ```
    
    Nota: el paso de parámetros posicionales es usado de manera intuitiva por las personas en muchas situaciones. Por ejemplo, es generalmente aceptado que cuando nos presentamos mencionamos primero nuestro nombre(s) y después nuestro apellido, por ejemplo, "Me llamo Juan Pérez."
    
    Sin embargo, En Hungría se hace al revés.
    
    ---
    
    ```python
    def introduction(first_name, last_name):
        print("Hola, mi nombre es", first_name, last_name)
     
    introduction("Luke", "Skywalker")
    introduction("Jesse", "Quick")
    introduction("Clark", "Kent")
    ```
    
    ¿Puedes predecir la salida? Ejecuta el código y verifícalo por ti mismo.
    
    Ahora imaginemos que la función esta siendo utilizada en Hungría. En este caso, el código sería de la siguiente manera:
    
    ```python
    def introduction(first_name, last_name):
        print("Hola, mi nombre es", first_name, last_name)
     
    introduction("Skywalker", "Luke")
    introduction("Quick", "Jesse")
    introduction("Kent", "Clark")
    ```
    
- **Paso de argumentos de palabra clave**
    
    Python ofrece otra manera de pasar argumentos, donde **el significado del argumento está definido por su nombre**, no su posición - a esto se le denomina **paso de argumentos con palabra clave**.
    
    Observa el siguiente código:
    
    ```python
    def introduction(first_name, last_name):
        print("Hola, mi nombre es", first_name, last_name)
     
    introduction(first_name = "James", last_name = "Bond")
    introduction(last_name = "Skywalker", first_name = "Luke")
    ```
    
    El concepto es claro - los valores pasados a los parámetros son precedidos por el nombre del parámetro al que se le va a pasar el valor, seguido por el signo de =.
    
    La posición no es relevante aquí - cada argumento conoce su destino con base en el nombre utilizado.
    
    Debes de poder predecir la salida. Ejecuta el código y verifica tu respuesta.
    
    ---
    
    Por supuesto que **no se debe de utilizar el nombre de un parámetro que no existe**.
    
    El siguiente código provocará un error de ejecución:
    
    ```python
    def introduction(first_name, last_name):
        print("Hola, mi nombre es", first_name, last_name)
     
    introduction(surname="Skywalker", first_name="Luke")
    ```
    
    Esto es lo que Python arrojará:
    
    ```python
    TypeError: introduction() got an unexpected keyword argument 'surname'
    ```
    
- **Mezclando argumentos posicionales y de palabra clave**
    
    Es posible combinar ambos tipos si se desea - solo hay una regla inquebrantable: se deben colocar primero los **argumentos posicionales y después los de palabra clave**.
    
    Piénsalo por un momento, entenderás el porque.
    
    Para mostrarte como funciona, se utilizará la siguiente función de tres parámetros:
    
    ```python
    def adding(a, b, c):
        print(a, "+", b, "+", c, "=", a + b + c)
    ```
    
    Su propósito es el de evaluar y presentar la suma de todos sus argumentos.
    
    La función, al ser invocada de la siguiente manera:
    
    ```python
    adding(1, 2, 3)
    ```
    
    dará como salida:
    
    ```python
    1 + 2 + 3 = 6
    ```
    
    Hasta ahorita es un ejemplo solo de **argumentos posicionales**.
    
    ---
    
    También, se puede reemplazar la invocación actual por una con palabras clave, como la siguiente:
    
    ```python
    adding(c = 1, a = 2, b = 3)
    ```
    
    El programa dará como salida lo siguiente:
    
    ```python
    2 + 3 + 1 = 6
    ```
    
    Ten presente el orden de los valores.
    
    ---
    
    Ahora intentemos mezclar ambas formas.
    
    Observa la siguiente invocación de la función:
    
    ```python
    adding(3, c = 1, b = 2)
    ```
    
    Vamos a analizarla:
    
    - el argumento (3) para el parámetro a es pasado utilizando la forma posicional;
    - los argumentos para c y b son especificados con palabras clave.
    
    Esto es lo que se verá en la consola:
    
    ```python
    3 + 2 + 1 = 6
    ```
    
    Sé cuidadoso, ten cuidado de no cometer errores. Si se intenta pasar mas de un valor a un argumento, ocurrirá un error y se mostrará lo siguiente.
    
    Observa la siguiente invocación - se le esta asignando dos veces un valor al parámetro a:
    
    ```python
    adding(3, a = 1, b = 2)
    ```
    
    La respuesta de Python es:
    
    ```python
    TypeError: adding() got multiple values for argument 'a'
    ```
    
    Observa el siguiente código. Es un código completamente correcto y funcional, pero no tiene mucho sentido:
    
    ```python
    adding(4, 3, c = 2)
    ```
    
    Todo es correcto, pero el dejar solo un argumento con palabra clave es algo extraño - ¿Qué es lo que opinas?
    
- ****Funciones parametrizadas - más detalles****
    
    En ocasiones ocurre que algunos valores de ciertos argumentos son más utilizados que otros. Dichos argumentos tienen **valores predefinidos** los cuales pueden ser considerados cuando los argumentos correspondientes han sido omitidos.
    
    Uno de los apellidos más comunes en Latinoamérica es *González*. Tomémoslo para el ejemplo.
    
    El valor por predeterminada para el parámetro se asigna de la siguiente manera:
    
    ```python
    def introduction(first_name, last_name="Smith"):
         print("Hola, mi nombre es", first_name, last_name)
    ```
    
    Solo se tiene que colocar el nombre del parámetro seguido del signo de = y el valor por predeterminada.
    
    Invoquemos la función de manera normal:
    
    ```python
    introduction("Jorge", "Pérez")
    ```
    
    ¿Puedes predecir la salida del programa? Ejecútalo y revisa si era lo esperado.
    
    ¿Y? No parece haber cambiado algo, pero cuando se invoca la función de una manera inusual, como esta:
    
    ```python
    introduction("Enrique")
    ```
    
    o así:
    
    ```python
    introduction(first_name="Guillermo")
    ```
    
    no habrá errores, ambas invocaciones funcionarán, la consola mostrará los siguientes resultados:
    
    ```python
    Hola, mi nombre es Enrique González
    Hola, mi nombre es Guillermo González
    ```
    
    Puedes hacerlo con más parámetros, si te resulta útil. Ambos parámetros tendrán sus valores por predeterminada, observa el siguiente código:
    
    ```python
    def introduction(first_name="Juan", last_name="González"):
        print("Hola, mi nombre es", first_name, last_name)
    ```
    
    Esto hace que la siguiente invocación sea completamente valida:
    
    ```python
    introduction()
    ```
    
    Y esta es la salida esperada:
    
    ```python
    Hola, mi nombre es Juan González
    ```
    
    Si solo se especifica un argumento de palabra clave, el restante tomará el valor por predeterminada:
    
    ```python
    introduction(last_name="Rodríguez")
    ```
    
    La salida es:
    
    ```python
    Hola, mi nombre es Juan Rodríguez
    ```
    
- **Resumen**
    
    1. Se puede pasar información a las funciones utilizando parámetros. Las funciones pueden tener tantos parámetros como sean necesarios.
    
    Un ejemplo de una función con un parámetro:
    
    ```python
    def hi(name):
        print("Hola,", name)
     
    hi("Greg")
    ```
    
    Un ejemplo de una función de dos parámetros:
    
    ```python
    def hi_all(name_1, name_2):
        print("Hola,", name_2)
        print("Hola,", name_1)
     
    hi_all("Sebastián", "Konrad")
    ```
    
    Un ejemplo de una función de tres parámetros:
    
    ```python
    def address(street, city, postal_code):
        print("Tu dirección es:", street,"St.,", city, postal_code)
     
    s = input("Calle: ")
    p_c = input("Código Postal: ")
    c = input("Ciudad: ")
    address(s, c, p_c)
    ```
    
    2. Puedes pasar argumentos a una función utilizando las siguientes técnicas:
    
    - **paso de argumentos posicionales** en la cual el orden de los parámetros es relevante (Ejemplo 1)
    - **paso de argumentos con palabras clave** en la cual el orden de los argumentos es irrelevante (Ejemplo 2)
    - una mezcla de argumentos posicionales y con palabras clave (Ejemplo 3).
    
    ```python
    Ejemplo 1
    def subtra(a, b):
        print(a - b)
     
    subtra(5, 2) # salida: 3
    subtra(2, 5) # salida: -3
     
     
    Ejemplo 2
    def subtra(a, b):
        print(a - b)
     
    subtra(a=5, b=2) # salida: 3
    subtra(b=2, a=5) # salida: 3
     
    Ex. 3
    def subtra(a, b):
        print(a - b)
     
    subtra(5, b=2) # salida: 3
    subtra(5, 2) # salida: 3
    ```
    
    Es importante recordar que **primero se especifican los argumentos posicionales y después los de palabras clave**. Es por esa razón que si se intenta ejecutar el siguiente código:
    
    ```python
    def subtra(a, b):
        print(a - b)
     
    subtra(5, b=2) # salida: 3
    subtra(a=5, 2) # Syntax Error
    ```
    
    Python no lo ejecutará y marcará un error de sintaxis SyntaxError.
    
    3. Se puede utilizar la técnica de argumentos con palabras clave para asignar valores **predefinidos** a los argumentos:
    
    ```python
    def name(first_name, last_name="Pérez"):
        print(first_name, last_name)
     
    name("Andy") # salida: Andy Pérez
    name("Bety", "Rodríguez") # salida: Bety Rodríguez (el argumento de palabra clave es reemplazado por "Rodríguez")
    ```
    

**Funciones - Retornando el resultado de una función**

- ****Efectos y resultados: la instrucción return****
    
    Todas las funciones presentadas anteriormente tienen algún tipo de efecto - producen un texto y lo envían a la consola.
    
    Por supuesto, las funciones - al igual que las funciones matemáticas - pueden tener resultados.
    
    Para lograr que las **funciones devuelvan un valor** (pero no solo para ese propósito) se utiliza la instrucción return (regresar o retornar).
    
    Esta palabra nos da una idea completa de sus capacidades. Nota: es una **palabra clave reservada** de Python.
    
    La instrucción return tiene **dos variantes diferentes** - considerémoslas por separado.
    
    ## *return* sin una expresión
    
    Consideremos la siguiente función:
    
    ```python
    def happy_new_year(wishes = True):
        print("Tres...")
        print("Dos...")
        print("Uno...")
        if not wishes:
            return
     
        print("¡Feliz año nuevo!")
    ```
    
    Cuando se invoca sin ningún argumento:
    
    ```python
    happy_new_year()
    ```
    
    La función produce un poco de ruido - la salida se verá así:
    
    ```python
    Tres...
    Dos...
    Uno...
    ¡Feliz año nuevo!
    ```
    
    Al proporcionar False como argumento:
    
    ```python
    happy_new_year(False)
    ```
    
    Se modificará el comportamiento de la función - la instrucción return provocará su terminación justo antes de los deseos - esta es la salida actualizada:
    
    ```python
    Tres...
    Dos...
    Uno...
    ```
    
    ## *return* con una expresión
    
    La segunda variante de return está **extendida con una expresión**:
    
    ```python
    def function():
        return expression
    ```
    
    Existen dos consecuencias de usarla:
    
    - provoca la **terminación inmediata de la ejecución de la función** (nada nuevo en comparación con la primer variante)
    - además, la función **evaluará el valor de la expresión y lo devolverá (de ahí el nombre una vez más) como el resultado de la función**.
    
    Si, este ejemplo es sencillo:
    
    ```python
    def boring_function():
        return 123
     
    x = boring_function()
     
    print("La función boring_function ha devuelto su resultado. Es:", x)
    ```
    
    El fragmento de código escribe el siguiente texto en la consola:
    
    ```python
    La función boring_function ha devuelto su resultado. Es: 123
    ```
    
    Vamos a investigarlo.
    
    Analiza la siguiente figura:
    
    ![Untitled](Untitled6.png)
    
    La instrucción return, enriquecida con la expresión (la expresión es muy simple aquí), "transporta" el valor de la expresión al lugar donde se ha invocado la función.
    
    El resultado se puede usar libremente aquí, por ejemplo, para ser asignado a una variable.
    
    También puede ignorarse por completo y perderse sin dejar rastro.
    
    Tenga en cuenta que no estamos siendo demasiado educados aquí: la función devuelve un valor y lo ignoramos (no lo usamos de ninguna manera):
    
    ```python
    def boring_function():
        print("'Modo aburrimiento' ON.")
        return 123
     
    print("¡Esta lección es interesante!")
    boring_function()
    print("Esta lección es aburrida...")
    ```
    
    El programa produce el siguiente resultado:
    
    ```python
    ¡Esta lección es interesante!
    'Modo aburrimiento' ON.
    Esta lección es aburrida...
    ```
    
    ¿Está mal? De ninguna manera.
    
    La única desventaja es que el resultado se ha perdido irremediablemente.
    
    No olvides:
    
    - siempre se te **permite ignorar el resultado de la función** y estar satisfecho con el efecto de la función (si la función tiene alguno)
    - si una función intenta devolver un resultado útil, debe contener la segunda variante de la instrucción return.
    
    Espera un segundo - ¿significa esto que también hay resultados inútiles? Sí, en cierto sentido.
    
- **Unas pocas palabras sobre None**
    
    Permítenos presentarte un valor muy curioso (para ser honestos, un valor que es ninguno) llamado None.
    
    Sus datos no representan valor razonable alguno - en realidad, no es un valor en lo absoluto; por lo tanto, **no debe participar en ninguna expresión**.
    
    Por ejemplo, un fragmento de código como el siguiente:
    
    ```python
    print(None + 2)
    ```
    
    Causará un error de tiempo de ejecución, descrito por el siguiente mensaje de diagnóstico:
    
    ```python
    TypeError: unsupported operand type(s) for +: 'NoneType' and 'int'
    ```
    
    Nota: None es una **palabra clave reservada**.
    
    Solo existen dos tipos de circunstancias en las que None se puede usar de manera segura:
    
    - cuando **se le asigna a una variable** (o se devuelve como **el resultado de una función**)
    - cuando **se compara con una variable** para diagnosticar su estado interno.
    
    Al igual que aquí:
    
    ```python
    value = None
    if value is None:
        print("Lo siento, no contienes ningún valor")
    ```
    
    No olvides esto: si una función no devuelve un cierto valor utilizando una cláusula de expresión return, se asume que **devuelve implícitamente None**.
    
    Vamos a probarlo.
    
    Echa un vistazo al código en el editor.
    
    ```python
    def strange_function(n):
        if(n % 2 == 0):
            return True
    ```
    
    Es obvio que la función strange_Function retorna True cuando su argumento es par.
    
    ¿Qué es lo que retorna de otra manera?
    
    Podemos usar el siguiente código para verificarlo:
    
    ```python
    print(strange_function(2))
    print(strange_function(1))
    ```
    
    Esto es lo que vemos en la consola:
    
    ```python
    True
    None
    ```
    
    No te sorprendas la próxima vez que veas None como el resultado de la función - puede ser el síntoma de un error sutil dentro de la función.
    
- ****Efectos y resultados: listas y funciones****
    
    Existen dos preguntas adicionales que deben responderse aquí.
    
    La primera es: **¿Se puede enviar una lista a una función como un argumento?**
    
    ¡Por supuesto que se puede! Cualquier entidad reconocible por Python puede desempeñar el papel de un argumento de función, aunque debes asegurarte de que la función sea capaz de hacer uso de él.
    
    Entonces, si pasas una lista a una función, la función tiene que manejarla como una lista.
    
    Una función como la siguiente:
    
    ```python
    def list_sum(lst):
        s = 0
     
        for elem in lst:
            s += elem
     
        return s
    ```
    
    y se invoca así:
    
    ```python
    print(list_sum([5, 4, 3]))
    ```
    
    retornará 12 como resultado, pero habrá problemas si la invocas de esta manera riesgosa:
    
    ```python
    print(list_sum(5))
    ```
    
    La respuesta de Python será la siguiente:
    
    ```python
    TypeError: 'int' object is not iterable
    ```
    
    Esto se debe al hecho de que el **bucle for no puede iterar un solo valor entero.**
    
    La segunda pregunta es: **¿Puede una lista ser el resultado de una función?**
    
    ¡Si, por supuesto! Cualquier entidad reconocible por Python puede ser un resultado de función.
    
    Observa el código en el editor. La salida del programa será así:
    
    ```python
    def strange_list_fun(n):
        strange_list = []
        
        for i in range(0, n):
            strange_list.insert(0, i)
        
        return strange_list
    
    print(strange_list_fun(5))
    ```
    
    Observa el código en el editor. La salida del programa será así:
    
    ```python
    [4, 3, 2, 1, 0]
    ```
    
    Ahora puedes escribir funciones con y sin resultados.
    
    Vamos a profundizar un poco más en los problemas relacionados con las variables en las funciones. Esto es esencial para crear funciones efectivas y seguras.
    

- **Resumen**
    
    1. Puedes emplear la palabra clave reservada return para decirle a una función que devuelva algún valor. La instrucción return termina la función, por ejemplo:
    
    ```python
    def multiply(a, b):
        return a * b
     
    print(multiply(3, 4)) # salida: 12
     
     
    def multiply(a, b):
        return
     
    print(multiply(3, 4)) # salida: None
    ```
    
    2. El resultado de una función se puede asignar fácilmente a una variable, por ejemplo:
    
    ```python
    def wishes():
        return "¡Felíz Cumpleaños!"
     
    w = wishes()
     
    print(w) # salida:¡Felíz Cumpleaños!
    ```
    
    Observa la diferencia en la salida en los siguientes dos ejemplos:
    
    ```python
    # Ejemplo 1
    def wishes():
        print("Mis deseos")
        return "Felíz Cumpleaños"
     
    wishes() # salida: Mis deseos
     
     
    # Ejemplo 2
    def wishes():
        print("Mis deseos")
        return "Felíz Cumpleaños"
     
    print(wishes())
     
    # salida: Mis deseos
    # Felíz Cumpleaños
    ```
    
    3. Puedes usar una lista como argumento de una función, por ejemplo:
    
    ```python
    def hi_everybody(my_list):
        for name in my_list:
            print("Hola,", name)
     
    hi_everybody(["Adán", "Juan", "Lucía"])
    ```
    
    4. Una lista también puede ser un resultado de función, por ejemplo:
    
    ```python
    def create_list(n):
        my_list = []
        for i in range(n):
            my_list.append(i)
        return my_list
     
    print(create_list(5))
    ```
    

**Alcances en Python**:

- **Funciones y alcances**
    
    Comencemos con una definición:
    
    *El **alcance de un nombre** (por ejemplo, el nombre de una variable) es la parte del código donde el nombre es reconocido correctamente.*
    
    Por ejemplo, el alcance del parámetro de una función es la función en sí. El parámetro es inaccesible fuera de la función.
    
    Vamos a revisarlo. Observa el código en el editor. ¿Qué ocurrirá cuando se ejecute?
    
    ```python
    def scope_test():
        x = 123
    
    scope_test()
    print(x)
    ```
    
    El programa no correrá. El mensaje de error dirá:
    
    ```python
    NameError: name 'x' is not defined
    ```
    
    Esto era de esperarse.
    
    Vamos a conducir algunos experimentos para mostrar como es que Python define los alcances, y como los puedes utilizar para tu beneficio.
    
    Comencemos revisando si una variable creada fuera de una función es visible dentro de una función. En otras palabras, ¿El nombre de la variable se propaga dentro del cuerpo de la función?
    
    Observa el código en el editor. Ahí esta nuestro conejillo de indias.
    
    ```python
    def my_function():
        print("¿Conozco a la variable?", var)
    
    var = 1
    my_function()
    print(var)
    ```
    
    El resultado de la prueba es positivo - el código da como salida:
    
    ```python
    ¿Conozco a la variable? 1
    1
    ```
    
    La respuesta es: **una variable que existe fuera de una función tiene alcance dentro del cuerpo de la función**.
    
    Esta regla tiene una excepción muy importante. Intentemos encontrarla.
    
    Hagamos un pequeño cambio al código:
    
    ```python
    def my_function():
        var = 2
        print("¿Conozco a la variable?", var)
     
     
    var = 1
    my_function()
    print(var)
    ```
    
    El resultado ha cambiado también - el código arroja una salida con una ligera diferencia:
    
    ```python
    ¿Conozco a la variable? 2
    1
    ```
    
    ¿Qué es lo que ocurrió?
    
    - la variable var creada dentro de la función no es la misma que cuando se define fuera de ella; parece que hay dos variables diferentes con el mismo nombre;
    - además, la variable de la función es una sombra de la variable fuera de la función.
    
    La regla anterior se puede definir de una manera más precisa y adecuada:
    
    **Una variable que existe fuera de una función tiene un alcance dentro del cuerpo de la función, excluyendo a aquellas que tienen el mismo nombre.**
    
    También significa que **el alcance de una variable existente fuera de una función solo se puede implementar dentro de una función cuando su valor es leído**. El asignar un valor hace que la función cree su propia variable.
    
    Asegúrate bien de entender esto correctamente y de realizar tus propios experimentos.
    
- **Funciones y alcances: la palabra clave global**
    
    Al llegar a este punto, debemos hacernos la siguiente pregunta: ¿Una función es capaz de modificar una variable que fue definida fuera de ella? Esto sería muy incomodo.
    
    Afortunadamente, la respuesta es *no*.
    
    Existe un método especial en Python el cual puede **extender el alcance de una variable incluyendo el cuerpo de las funciones** (para poder no solo leer los valores de las variables sino también modificarlos).
    
    Este efecto es causado por la palabra clave reservada llamada global:
    
    ```python
    global name
    global name1, name2, ...
    ```
    
    El utilizar la palabra reservada dentro de una función con el nombre o nombres de las variables separados por comas, obliga a Python a abstenerse de crear una nueva variable dentro de la función - se empleará la que se puede acceder desde el exterior.
    
    En otras palabras, este nombre se convierte en global (tiene un **alcance global**, y no importa si se esta leyendo o asignando un valor).
    
    Observa el código en el editor.
    
    ```python
    def my_function():
        global var
        var = 2
        print("¿Conozco a aquella variable?", var)
    
    var = 1
    my_function()
    print(var)
    ```
    
    Se ha agregado la palabra global a la función.
    
    El código ahora da como salida:
    
    ```python
    ¿Conozco a aquella variable?
    2
    ```
    
    Esto debe de ser suficiente evidencia para mostrar lo que la palabra clave reservada global puede hacer.
    
- ****Cómo interactúa la función con sus argumentos****
    
    Ahora descubramos como la función interactúa con sus argumentos.
    
    El código en el editor nos enseña algo. Como puedes observar, la función cambia el valor de su parámetro. ¿Este cambio afecta el argumento?
    
    ```python
    def my_function(n):
        print("Yo recibí", n)
        n += 1
        print("Ahora tengo", n)
    
    var = 1
    my_function(var)
    print(var)
    ```
    
    Ejecuta el programa y verifícalo.
    
    La salida del código es:
    
    ```python
    Yo recibí 1
    Ahora tengo 2
    1
    ```
    
    La conclusión es obvia - **al cambiar el valor del parámetro este no se propaga fuera de la función** (más específicamente, no cuando la variable es un valor escalar, como en el ejemplo).
    
    Esto también significa que una función recibe el **valor del argumento**, no el argumento en sí. Esto es cierto para los valores escalares.
    
    Vale la pena revisar cómo funciona esto con las listas (¿Recuerdas las peculiaridades de asignar rebanadas de listas en lugar de asignar la lista entera?)
    
    ---
    
    El siguiente ejemplo arrojará luz sobre el asunto:
    
    ```python
    def my_function(my_list_1):
        print("Print #1:", my_list_1)
        print("Print #2:", my_list_2)
        my_list_1 = [0, 1]
        print("Print #3:", my_list_1)
        print("Print #4:", my_list_2)
     
     
    my_list_2 = [2, 3]
    my_function(my_list_2)
    print("Print #5:", my_list_2)
    ```
    
    La salida del código es:
    
    ```python
    Print #1: [2, 3]
    Print #2: [2, 3]
    Print #3: [0, 1]
    Print #4: [2, 3]
    Print #5: [2, 3]
    ```
    
    Parece ser que se sigue aplicando la misma regla.
    
    ---
    
    Finalmente, la diferencia se puede observar en el siguiente ejemplo:
    
    ```python
    def my_function(my_list_1):
        print("Print #1:", my_list_1)
        print("Print #2:", my_list_2)
        del my_list_1[0] # Presta atención a esta línea.
        print("Print #3:", my_list_1)
        print("Print #4:", my_list_2)
     
     
    my_list_2 = [2, 3]
    my_function(my_list_2)
    print("Print #5:", my_list_2)
    ```
    
    No se modifica el valor del parámetro my_list_1 (ya se sabe que no afectará el argumento), en lugar de ello se modificará la lista identificada por el.
    
    El resultado puede ser sorprendente. Ejecuta el código y verifícalo:
    
    ```python
    Print #1: [2, 3]
    Print #2: [2, 3]
    Print #3: [3]
    Print #4: [3]
    Print #5: [3]
    ```
    
    ¿Lo puedes explicar?
    
    Intentémoslo:
    
    - si el argumento es una lista, el cambiar el valor del parámetro correspondiente no afecta la lista (recuerda: las variables que contienen listas son almacenadas de manera diferente que las escalares)
    - pero si se modifica la lista identificada por el parámetro (nota: ¡la lista, no el parámetro!), la lista reflejará el cambio.
- **Resumen**
    
    1. Una variable que existe fuera de una función tiene alcance dentro del cuerpo de la función. (Ejemplo 1) al menos que la función defina una variable con el mismo nombre. (Ejemplo 2, y Ejemplo 3), por ejemplo:
    
    **Ejemplo 1:**
    
    ```python
    var = 2
     
     
    def mult_by_var(x):
        return x * var
     
     
    print(mult_by_var(7)) # salida: 14
    ```
    
    **Ejemplo 2:**
    
    ```python
    def mult(x):
        var = 5
        return x * var
     
     
    print(mult(7)) # salida: 35
    ```
    
    **Ejemplo 3:**
    
    ```python
    def mult(x):
        var = 7
        return x * var
     
     
    var = 3
    print(mult(7)) # salida: 49
    ```
    
    2. Una variable que existe dentro de una función tiene un alcance solo dentro del cuerpo de la función (Ejemplo 4), por ejemplo:
    
    **Ejemplo 4:**
    
    ```python
    def adding(x):
        var = 7
        return x + var
     
     
    print(adding(4)) # salida: 11
    print(var) # NameError
    ```
    
    3. Se puede emplear la palabra clave reservada global seguida por el nombre de una variable para que el alcance de la variable sea global, por ejemplo:
    
    ```python
    var = 2
    print(var) # salida: 2
     
     
    def return_var():
        global var
        var = 5
        return var
     
     
    print(return_var()) # salida: 5
    print(var) # salida: 52
    ```
    

**Creación de funciones multiparámetro**:

- ****Ejemplos de funciones: Cálculo del IMC****
    
    Definamos una función que calcula el Índice de Masa Corporal (IMC).
    
    Como puedes observar, la formula ocupa dos valores:
    
    - peso (originalmente en kilogramos)
    - altura (originalmente en metros)
    
    La nueva función tendrá **dos parámetros**. Su nombre será bmi, pero si prefieres utilizar otro nombre, adelante.
    
    ![Untitled](Untitled7.png)
    
    Codifiquemos la función.
    
    ```python
    def bmi(weight, height):
        return weight / height ** 2
     
     
    print(bmi(52.5, 1.65))
    ```
    
    El resultado del ejemplo anterior es el siguiente:
    
    ```python
    19.283746556473833
    ```
    
    La función hace lo que deseamos, pero es un poco sencilla - asume que los valores de ambos parámetros son significativos. Se debe comprobar que son confiables.
    
    Vamos a comprobar ambos y regresar None si cualquiera de los dos es incorrecto.
    
    ## Calcular el IMC y convertir unidades del Sistema Inglés al Sistema Métrico
    
    Observa el código en el editor. Hay dos cosas a las cuales hay que prestar atención.
    
    ```python
    def bmi(weight, height):
        if height < 1.0 or height > 2.5 or \
        weight < 20 or weight > 200:
            return None
    
        return weight / height ** 2
    
    print(bmi(352.5, 1.65))
    ```
    
    Primero, se asegura que los datos que sean ingresados sean correctos - de lo contrario la salida será:
    
    ```python
    None
    ```
    
    Segundo, observa como el símbolo de **diagonal invertida** (\) es empleado. Si se termina una línea de código con el, Python entenderá que la línea continua en la siguiente.
    
    Esto puede ser útil cuando se tienen largas líneas de código y se desea que sean más legibles.
    
    Sin embargo, hay algo que omitimos - las medias en sistema inglés. La función no es útil para personas que utilicen libras, pies y pulgadas.
    
    ¿Qué podemos hacer por ellos?
    
    Escribimos dos funciones sencillas para **convertir unidades del sistema inglés al sistema métrico**. Comencemos con las pulgadas.
    
    Es bien conocido que 1 lb = 0.45359237 kg. Esto lo emplearemos en nuestra nueva función.
    
    Esta función se llamará **`lb_to_kg`**:
    
    ```python
    def lb_to_kg(lb):
        return lb * 0.45359237
     
     
    print(lb_to_kg(1))
    ```
    
    El resultado de la prueba es el siguiente:
    
    ```python
    0.45359237
    ```
    
    Haremos lo mismo ahora con los pies y pulgadas: 1 ft = 0.3048 m, y 1 in = 2.54 cm = 0.0254 m.
    
    La función se llamará **`ft_and_inch_to_m`**:
    
    ```python
    def ft_and_inch_to_m(ft, inch):
        return ft * 0.3048 + inch * 0.0254
     
     
    print(ft_and_inch_to_m(1, 1))
    ```
    
    El resultado de una prueba rápida es:
    
    ```python
    0.3302
    ```
    
    Resulta como esperado.
    
    Nota: queríamos nombrar el segundo parámetro solo in, no inch, pero no pudimos. ¿Sabes por qué?
    
    in es una **palabra clave reservada** de Python ‒ no se puede usar como nombre.
    
    Vamos a convertir *seis pies* a metros:
    
    ```python
    print(ft_and_inch_to_m(6, 0))
    ```
    
    Esta es la salida:
    
    ```python
    1.8288000000000002
    ```
    
    Es muy posible que en ocasiones se desee utilizar solo pies sin pulgadas. ¿Python nos ayudará? Por supuesto que si.
    
    Se ha modificado el código un poco:
    
    ```python
    def ft_and_inch_to_m(ft, inch = 0.0):
        return ft * 0.3048 + inch * 0.0254
     
     
    print(ft_and_inch_to_m(6))
    ```
    
    Ahora el parámetro inch tiene como valor predeterminado el 0.0.
    
    El código produce la siguiente salida - esto es lo que se esperaba:
    
    ```python
    1.8288000000000002
    ```
    
    Finalmente, el código es capaz de responder a la pregunta: ¿Cuál es el IMC de una persona que tiene 5'7" de altura y un peso de 176 lbs?
    
    Este es el código que hemos construido:
    
    ```python
    def ft_and_inch_to_m(ft, inch = 0.0):
        return ft * 0.3048 + inch * 0.0254
     
     
    def lb_to_kg(lb):
        return lb * 0.4535923
     
     
    def bmi(weight, height):
        if height < 1.0 or height > 2.5 or weight < 20 or weight > 200:
            return None
     
        return weight / height ** 2
     
     
    print(bmi(weight = lb_to_kg(176), height = ft_and_inch_to_m(5, 7)))
    ```
    
    La respuesta es:
    
    ```python
    27.565214082533313
    ```
    
- ****Ejemplos de funciones: Triángulos****
    
    ![Untitled](/assets/PythonBasico/Untitled8.png)
    
    Ahora trabajaremos con triángulos. Comenzaremos con una función que verifique si tres lados de ciertas longitudes pueden formar un triángulo.
    
    En la escuela aprendimos que *la suma arbitraria de dos lados tiene que ser mayor que la longitud del tercer lado*.
    
    No será algo difícil. La función tendrá **tres parámetros** - uno para cada lado.
    
    Regresará True si todos los lados pueden formar un triángulo, y False de lo contrario. En este caso, is_a_triangle es un buen nombre para dicha función.
    
    Observa el código en el editor. Ahí se encuentra la función. Ejecuta el programa.
    
    ```python
    def is_a_triangle(a, b, c):
        if a + b <= c:
            return False
        if b + c <= a:
            return False
        if c + a <= b:
            return False
        return True
    
    print(is_a_triangle(1, 1, 1))
    print(is_a_triangle(1, 1, 3))
    ```
    
    Parece que funciona perfectamente - estos son los resultados:
    
    ```python
    True
    False
    ```
    
    ¿Se podrá hacer más compacta? Parece tener demasiadas palabras.
    
    Esta es la versión más compacta:
    
    ```python
    def is_a_triangle(a, b, c):
        if a + b <= c or b + c <= a or c + a <= b:
            return False
        return True
     
     
    print(is_a_triangle(1, 1, 1))
    print(is_a_triangle(1, 1, 3))
    ```
    
    ¿Se puede compactar aun más?
    
    Sí, por supuesto - observa:
    
    ```python
    def is_a_triangle(a, b, c):
        return a + b > c and b + c > a and c + a > b
     
     
    print(is_a_triangle(1, 1, 1))
    print(is_a_triangle(1, 1, 3))
    ```
    
    Se ha negado la condición (se invirtieron los operadores relacionales y se reemplazaron los ors con ands, obteniendo una **expresión universal para probar triángulos**).
    
    Coloquemos la función en un programa más grande. Se le pedirá al usuario los tres valores y se hará uso de la función.
    
    ## Triángulos y el Teorema de Pitágoras
    
    Observa el código en el editor. Le pide al usuario tres valores. Después hace uso de la función is_a_triangle. El código esta listo para correrse.
    
    ```python
    def is_a_triangle(a, b, c):
        return a + b > c and b + c > a and c + a > b
    
    a = float(input('Ingresa la longitud del primer lado: '))
    b = float(input('Ingresa la longitud del segundo lado: '))
    c = float(input('Ingresa la longitud del tercer lado: '))
    
    if is_a_triangle(a, b, c):
        print('Si, si puede ser un triángulo.')
    else:
        print('No, no puede ser un triángulo.')
    ```
    
    En el segundo paso, intentaremos verificar si un triángulo es un **triángulo rectángulo**.
    
    Para ello haremos uso del **Teorema de Pitágoras**:
    
    **c2 = a2 + b2**
    
    ¿Cómo saber cual de los tres lados es la hipotenusa?
    
    **La hipotenusa es el lado más largo**.
    
    Aquí esta el código:
    
    ```python
    def is_a_triangle(a, b, c):
        return a + b > c and b + c > a and c + a > b
     
     
    def is_a_right_triangle(a, b, c):
        if not is_a_triangle(a, b, c):
            return False
        if c > a and c > b:
            return c ** 2 == a ** 2 + b ** 2 if a > b and a > c:
        if a > b and a > c:
            return a ** 2 == b ** 2 + c ** 2
    print(is_a_right_triangle(5, 3, 4))
    print(is_a_right_triangle(1, 3, 4))
    ```
    
    Observa como se establece la relación entre la hipotenusa y los dos catetos - se eligió el lado más largo y se aplico el **Teorema de Pitágoras** para verificar que todo estuviese en orden. Esto requiere tres revisiones en total.
    
    ## Evaluando el área de un triángulo
    
    También es posible evaluar el área de un triángulo. **La Formula de Heron** será útil aquí:
    
    ![Untitled](/assets/PythonBasico/Untitled9.png)
    
    Vamos a emplear el operador de exponenciación para calcular la raíz cuadrada - puede ser extraño, pero funciona.
    
    ![Untitled](/assets/PythonBasico/Untitled10.png)
    
    Este es el código resultante:
    
    ```python
    def is_a_triangle(a, b, c):
        return a + b > c and b + c > a and c + a > b
     
     
    def heron(a, b, c):
        p = (a + b + c) / 2
        return (p * (p - a) * (p - b) * (p - c)) ** 0.5
     
     
    def area_of_triangle(a, b, c):
        if not is_a_triangle(a, b, c):
            return None
        return heron(a, b, c)
     
     
    print(area_of_triangle(1., 1., 2. ** .5))
    ```
    
    Lo probaremos con un triángulo rectángulo la mitad de un cuadrado y con un lado igual a 1. Esto significa que su área debe ser igual a 0.5.
    
    Es extraño - pero el código produce la siguiente salida:
    
    ```python
    0.49999999999999983
    ```
    
    Es muy cercano a 0.5, pero no es exactamente 0.5. ¿Qué significa? ¿Es un error?
    
    No, no lo es. Son solo los **cálculos de valores punto flotantes**. Pronto se discutirá el tema.
    
- ****Ejemplos de funciones: Factoriales****
    
    La siguiente función a definir calcula **factoriales**. ¿Recuerdas cómo se calcula un factorial?
    
    0! = 1 (¡Si!, es verdad) 1! = 1 2! = 1 * 2 3! = 1 * 2 * 3 4! = 1 * 2 * 3 * 4 : : n! = 1 * 2 ** 3 * 4 * ... * n-1 * n
    
    Se expresa con un **signo de exclamación**, y es igual al **producto** de todos los números naturales previos al argumento o número dado.
    
    Escribamos el código. Creemos una función con el nombre factorial_function. Aquí esta el código:
    
    ```python
    def factorial_function(n):
        if n < 0:
            return None
        if n < 2:
            return 1
     
        product = 1
        for i in range(2, n + 1):
            product *= i
        return product
     
     
    for n in range(1, 6): # probando
        print(n, factorial_function(n))
    ```
    
    Observa como se sigue el procedimiento matemático, y como se emplea el bucle for para **encontrar el producto**.
    
    Estos son los resultados obtenidos del código de prueba:
    
    ```python
    1 1
    2 2
    3 6
    4 24
    5 120
    ```
    
- ****Números Fibonacci****
    
    ¿Estás familiarizado con la serie **Fibonacci**?
    
    Son una **secuencia de números enteros** los cuales siguen una regla sencilla:
    
    - el primer elemento de la secuencia es igual a uno (**Fib1 = 1**)
    - el segundo elemento también es igual a uno (**Fib2 = 1**)
    - cada número después de ellos son la suman de los dos números anteriores (**Fibi = Fibi-1 + Fibi-2**)
    
    Aquí están algunos de los primeros números en la serie Fibonacci:
    
    fib_1 = 1 fib_2 = 1 fib_3 = 1 + 1 = 2 fib_4 = 1 + 2 = 3 fib_5 = 2 + 3 = 5 fib_6 = 3 + 5 = 8 fib_7 = 5 + 8 = 13
    
    ¿Que opinas acerca de **implementarlo como una función**?
    
    Creemos nuestra propia función fib y probémosla, aquí esta:
    
    ```python
    def fib(n):
        if n < 1:
            return None
        if n < 3:
            return 1
     
        elem_1 = elem_2 = 1
        the_sum = 0
        for i in range(3, n + 1):
            the_sum = elem_1 + elem_2
            elem_1, elem_2 = elem_2, the_sum
        return the_sum
     
     
    for n in range(1, 10): # probando
        print(n, "->", fib(n))
    ```
    
    Analiza el codigo del bucle for cuidadosamente, descifra como se **mueven las variables elem_1 y elem_2 a través de los números subsecuentes de la serie Fibonacci**.
    
    Al probar el código, se generan los siguientes resultados:
    
    ```python
    1 -> 1
    2 -> 1
    3 -> 2
    4 -> 3
    5 -> 5
    6 -> 8
    7 -> 13
    8 -> 21
    9 -> 34
    ```
    
- ****Recursividad****
    
    Existe algo más que se desea mostrar: es la **recursividad**.
    
    Este termino puede describir muchos conceptos distintos, pero uno de ellos, hace referencia a la programación computacional.
    
    Aquí, la recursividad es una **técnica donde una función se invoca a si misma**.
    
    Tanto el factorial como la serie Fibonacci, son las mejores opciones para ilustrar este fenómeno.
    
    **La serie de Fibonacci es un claro ejemplo de recursividad**. Ya te dijimos que:
    
    **Fibi = Fibi-1 + Fibi-2**
    
    El número i se refiere al número i-1, y así sucesivamente hasta llegar a los primeros dos.
    
    ¿Puede ser empleado en el código? Por supuesto que puede. Puede hacer el código más corto y claro.
    
    La segunda versión de la función fib() hace uso directo de la recursividad:
    
    ```python
    def fib(n):
        if n < 1:
            return None
        if n < 3:
            return 1
        return fib(n - 1) + fib(n - 2)
    ```
    
    El código es mucho más claro ahora.
    
    ¿Pero es realmente seguro? ¿Implica algún riesgo?
    
    Si, existe algo de riesgo. **Si no se considera una condición que detenga las invocaciones recursivas, el programa puede entrar en un bucle infinito**. Se debe ser cuidadoso.
    
    El factorial también tiene un lado **recursivo**. Observa:
    
    n! = 1 × 2 × 3 × ... × n-1 × n
    
    Es obvio que:
    
    1 × 2 × 3 × ... × n-1 = (n-1)!
    
    Entonces, finalmente, el resultado es:
    
    n! = (n-1)! × n
    
    Esto se empleará en nuestra nueva solución.
    
    Aquí esta:
    
    ```python
    def factorial_function(n):
        if n < 0:
            return None
        if n < 2:
            return 1
        return n * factorial_function(n - 1)
    ```
    
    ¿Funciona? Claro que si. Pruébalo por ti mismo.
    
    Nuestro viaje *funcional* esta por terminar. La siguiente sección abordara dos tipos de datos en Python: tuplas y diccionarios.
    
    ```python
    def fib(n):
        if n < 1:
             return None
        if n < 3:
            return 1
    
        elem_1 = elem_2 = 1
        the_sum = 0
        for i in range(3, n + 1):
            the_sum = elem_1 + elem_2
            elem_1, elem_2 = elem_2, the_sum
        return the_sum
    
    for n in range(1, 10):
        print(n, "->", fib(n))
    ```
    
- **Resumen**
    
    1. Una función puede invocar otras funciones o incluso a sí misma. Cuando una función se invoca a si misma, se le conoce como **recursividad**, y la función que se invoca a si misma y contiene una condición de terminación (la cual le dice a la función que ya no siga invocándose a si misma) es llamada una función **recursiva**.
    
    2. Se pueden emplear funciones recursivas en Python para crear funciones **limpias, elegantes, y dividir el código en trozos más pequeños**. Sin embargo, se debe tener mucho cuidado ya que es **muy fácil cometer un error y crear una función la cual nunca termine**. También se debe considerar que **las funciones recursivas consumen mucha memoria**, y por lo tanto pueden ser en ocasiones ineficientes.
    
    Al emplear la recursividad, se deben de tomar en cuenta tanto sus ventajas como desventajas.
    
    La función factorial es un ejemplo clásico de como se puede implementar el concepto de recursividad:
    
    ```python
    # Implementación recursiva de la función factorial.
     
    def factorial(n):
        if n == 1: # El caso base (condición de terminación).
            return 1
        else:
            return n * factorial(n - 1)
     
     
    print(factorial(4)) # 4 * 3 * 2 * 1 = 24
    ```
    

**Tuplas y diccionarios**:

- ****Tipos de secuencia y mutabilidad****
    
    Antes de comenzar a hablar acerca de **tuplas** y **diccionarios**, se deben introducir dos conceptos importantes: **tipos de secuencia** y **mutabilidad**.
    
    Un **tipo de secuencia es un tipo de dato en Python el cual es capaz de almacenar más de un valor (o ninguno si la secuencia esta vacía), los cuales pueden ser secuencialmente (de ahí el nombre) examinados**, elemento por elemento.
    
    Debido a que el bucle for es una herramienta especialmente diseñada para iterar a través de las secuencias, podemos definirlas de la siguiente manera: **una secuencia es un tipo de dato que puede ser escaneado por el bucle for**.
    
    Hasta ahora, has trabajado con una secuencia en Python - la lista. La lista es un clásico ejemplo de una secuencia de Python. Aunque existen otras secuencias dignas de mencionar, las cuales se presentaran a continuación.
    
    La segunda noción - **la mutabilidad** - es una propiedad de cualquier tipo de dato en Python que describe su disponibilidad para poder cambiar libremente durante la ejecución de un programa. Existen dos tipos de datos en Python: **mutables** e **inmutables**.
    
    **Los datos mutables pueden ser actualizados libremente en cualquier momento** - a esta operación se le denomina "in situ".
    
    *In situ* es una expresión en Latín que se traduce literalmente como *en posición, en el lugar o momento*. Por ejemplo, la siguiente instrucción modifica los datos "in situ":
    
    ```python
    list.append(1)
    ```
    
    **Los datos inmutables no pueden ser modificados de esta manera**.
    
    Imagina que una lista solo puede ser asignada y leída. No podrías adjuntar ni remover un elemento de la lista. Si se agrega un elemento al final de la lista provocaría que la lista se cree desde cero.
    
    Se tendría que crear una lista completamente nueva, la cual contenga los elementos ya existentes más el nuevo elemento.
    
    El tipo de datos que se desea tratar ahora se llama **tupla**. **Una tupla es una secuencia inmutable**. Se puede comportar como una lista pero no puede ser modificada en el momento.
    
- **Tuplas**
    
    Lo primero que distingue una lista de una tupla es la sintaxis empleada para crearlas - las **tuplas utilizan paréntesis**, mientras que las listas usan corchetes, aunque también es **posible crear una tupla tan solo separando los valores por comas**.
    
    Observa el ejemplo:
    
    ```python
    tuple_1 = (1, 2, 4, 8)
    tuple_2 = 1., .5, .25, .125
    ```
    
    Se definieron dos tuplas, ambas contienen **cuatro elementos**.
    
    A continuación se imprimen en consola:
    
    ```python
    tuple_1 = (1, 2, 4, 8)
    tuple_2 = 1., .5, .25, .125
     
    print(tuple_1)
    print(tuple_2)
    ```
    
    Esto es lo que se muestra en consola:
    
    ```python
    (1, 2, 4, 8)
    (1.0, 0.5, 0.25, 0.125)
    ```
    
    Nota: **cada elemento de una tupla puede ser de distinto tipo** (punto flotante, entero, cadena, o cualquier otro tipo de dato).
    
    ## ¿Cómo crear una tupla?
    
    ¿Es posible crear una tupla vacía? Si, solo se necesitan unos paréntesis:
    
    ```python
    empty_tuple = ()
    ```
    
    Si se desea crear una tupla de **un solo elemento**, se debe de considerar el hecho de que, debido a la sintaxis (una tupla debe de poder distinguirse de un valor entero ordinario), se debe de colocar una coma al final:
    
    ```python
    one_element_tuple_1 = (1, )
    one_element_tuple_2 = 1.,
    ```
    
    El quitar las comas no arruinará el programa en el sentido sintáctico, pero serán dos variables, no tuplas.
    
    ## ¿Cómo utilizar un tupla?
    
    Si deseas leer los elementos de una tupla, lo puedes hacer de la misma manera que se hace con las listas.
    
    Observa el código en el editor.
    
    ```python
    my_tuple = (1, 10, 100, 1000)
    
    print(my_tuple[0])
    print(my_tuple[-1])
    print(my_tuple[1:])
    print(my_tuple[:-2])
    
    for elem in my_tuple:
        print(elem)
    ```
    
    El programa debe de generar la siguiente salida, ejecútalo y comprueba:
    
    ```python
    1
    1000
    (10, 100, 1000)
    (1, 10)
    1
    10
    100
    1000
    ```
    
    Las similitudes pueden ser engañosas - **no intentes modificar el contenido de la tupla** ¡No es una lista!
    
    Todas estas instrucciones (con excepción de primera) causarán un error de ejecución:
    
    ```python
    my_tuple = (1, 10, 100, 1000)
     
    my_tuple.append(10000)
    del my_tuple[0]
    my_tuple[1] = -10
    ```
    
    Este es el mensaje que Python arrojará en la ventana de consola:
    
    ```python
    AttributeError: 'tuple' object has no attribute 'append'
    ```
    
    ¿Qué más pueden hacer las tuplas?
    
    - la función len() acepta tuplas, y regresa el número de elementos contenidos dentro;
    - el operador + puede unir tuplas (ya se ha mostrado esto antes)
    - el operador * puede multiplicar las tuplas, así como las listas;
    - los operadores in y not in funcionan de la misma manera que en las listas.
    
    El fragmento de código en el editor presenta todo esto.
    
    ```python
    my_tuple = (1, 10, 100)
    
    t1 = my_tuple + (1000, 10000)
    t2 = my_tuple * 3
    
    print(len(t2))
    print(t1)
    print(t2)
    print(10 in my_tuple)
    print(-10 not in my_tuple)
    ```
    
    La salida es la siguiente:
    
    ```python
    9
    (1, 10, 100, 1000, 10000)
    (1, 10, 100, 1, 10, 100, 1, 10, 100)
    True
    True
    ```
    
    Una de las propiedades de las tuplas más útiles es que pueden **aparecer en el lado izquierdo del operador de asignación**. Este fenómeno ya se vio con anterioridad, cuando fue necesario encontrar una manera de intercambiar los valores entre dos variables.
    
    Observa el siguiente fragmento de código:
    
    ```python
    var = 123
     
    t1 = (1, )
    t2 = (2, )
    t3 = (3, var)
     
    t1, t2, t3 = t2, t3, t1
     
    print(t1, t2, t3)
    ```
    
    Muestra tres tuplas interactuando - en efecto, los valores almacenados en ellas "circulan" entre ellas - t1 se convierte en t2, t2 se convierte en t3, y t3 se convierte en t1.
    
    Nota: el ejemplo presenta un importante hecho mas: los **elementos de una tupla pueden ser variables**, no solo literales. Además, pueden ser expresiones si se encuentran en el lado derecho del operador de asignación.
    
- **Diccionarios**
    
    El **diccionario** es otro tipo de estructura de datos de Python. No **es una secuencia** (pero puede adaptarse fácilmente a un procesamiento secuencial) y además es **mutable**.
    
    Para explicar lo que es un diccionario en Python, es importante comprender de manera literal lo que es un diccionario.
    
    ![https://skillsforall.com/content/pe1/1.0/m4/course/es-XL/assets/bde274bd50374fc4df624312833234aa51237378.png](https://skillsforall.com/content/pe1/1.0/m4/course/es-XL/assets/bde274bd50374fc4df624312833234aa51237378.png)
    
    ## ¿Cómo crear un diccionario?
    
    Si deseas asignar algunos pares iniciales a un diccionario, utiliza la siguiente sintaxis:
    
    ```python
    dictionary = {"gato": "chat", "perro": "chien", "caballo": "cheval"}
    phone_numbers = {'jefe': 5551234567, 'Suzy': 22657854310}
    empty_dictionary = {}
     
    print(dictionary)
    print(phone_numbers)
    print(empty_dictionary)
    ```
    
    En este primer ejemplo, el diccionario emplea claves y valores las cuales ambas son cadenas. En el segundo, las claves con cadenas pero los valores son enteros. El orden inverso (claves → números, valores → cadenas) también es posible, así como la combinación número a número.
    
    La lista de todos los pares es **encerrada con llaves**, mientras que los pares son **separados por comas**, y las **claves y valores por dos puntos**.
    
    El primer diccionario es muy simple, es un diccionario Español-Francés. El segundo es un directorio telefónico muy pequeño.
    
    Los diccionarios vacíos son construidos por **un par vacío de llaves** - nada inusual.
    
    Un diccionario en Python funciona de la misma manera que **un diccionario bilingüe**. Por ejemplo, se tiene la palabra en español "gato" y se necesita su equivalente en francés. Lo que se haría es buscar en el diccionario para encontrar la palabra "gato". Eventualmente la encontrarás, y sabrás que la palabra equivalente en francés es "chat".
    
    En el mundo de Python, la palabra que se esta buscando se denomina key. La palabra que se obtiene del diccionario es denominada value.
    
    Esto significa que un diccionario es un conjunto de pares de **key y value**. Nota:
    
    - cada clave debe de ser **única** - no es posible tener una clave duplicada;
    - una clave puede ser **un de dato de cualquier tipo** - puede ser un número (entero o flotante), incluso una cadena, pero no una lista;
    - un diccionario no es una lista - una lista contiene un conjunto de valores numerados, mientras que un **diccionario almacena pares de valores**;
    - la función len() aplica también para los diccionarios - regresa la cantidad de pares (clave-valor) en el diccionario;
    - un diccionario es **una herramienta de un solo sentido** - si fuese un diccionario español-francés, podríamos buscar en español para encontrar su contraparte en francés más no viceversa.
    
    A continuación veamos algunos ejemplos:
    
    El diccionario entero se puede imprimir con una invocación a la función print(). El fragmento de código **puede** producir la siguiente salida:
    
    ```python
    {'perro': 'chien', 'caballo': 'cheval', 'gato': 'chat'}
    {'Suzy': 5557654321, 'jefe': 5551234567}
    {}
    ```
    
    ¿Has notado que el orden de los pares impresos es diferente a la asignación inicial? ¿Qué significa esto?
    
    Primeramente, recordemos que **los diccionarios no son listas** - no guardan el orden de sus datos, el orden no tiene significado (a diferencia de los diccionarios reales). El orden en que un diccionario **almacena sus datos esta fuera de nuestro control**. Esto es normal. (*)
    
    **Nota**
    
    (*) En Python 3.6x los diccionarios se han convertido en colecciones **ordenadas** de manera predeterminada. Tu resultado puede variar dependiendo en la versión de Python que se este utilizando.
    
    ## ¿Cómo utilizar un diccionario?
    
    Analiza el código en el editor:
    
    ```python
    dictionary = {"gato": "chat", "perro": "chien", "caballo": "cheval"}
    phone_numbers = {'jefe' : 5551234567, 'Suzy' : 22657854310}
    empty_dictionary = {}
    
    # Imprimir valores aquí.
    ```
    
    Si deseas obtener cualquiera de los valores, debes de proporcionar una clave válida:
    
    ```python
    print(dictionary['gato'])
    print(phone_numbers['Suzy'])
    ```
    
    El obtener el valor de un diccionario es semejante a la indexación, gracias a los corchetes alrededor del valor de la clave.
    
    Nota:
    
    - si una clave es una cadena, se tiene que especificar como una cadena;
    - **las claves son sensibles a las mayúsculas y minúsculas**: 'Suzy' sería diferente a 'suzy'.
    
    El fragmento de código da las siguientes salidas:
    
    ```python
    chat
    5557654321
    ```
    
    Ahora algo muy importante: **No se puede utilizar una clave que no exista**. Hacer algo como lo siguiente:
    
    ```python
    print(phone_numbers['president'])
    ```
    
    Provocará un error de ejecución. Inténtalo.
    
    Afortunadamente, existe una manera simple de evitar dicha situación. El operador in, junto con su acompañante, not in, pueden salvarnos de esta situación.
    
    El siguiente código busca de manera segura palabras en francés:
    
    ```python
    dictionary = {"cat": "gato", "perro": "chien", "caballo": "cheval"}
    words = ['gato', 'león', 'caballo']
     
    for word in words:
        if word in dictionary:
            print(word, "->", dictionary[word])
        else:
            print(word, "no está en el diccionario")
    ```
    
    La salida del código es la siguiente:
    
    ```python
    gato -> chat
    león no está en el diccionario
    caballo -> cheval
    ```
    
    **Nota:** Cuando escribes una expresión grande o larga, puede ser una buena idea mantenerla alineada verticalmente. Así es como puede hacer que el código sea más legible y más amigable para el programador, por ejemplo:
    
    ```python
    # Ejemplo 1:
    dictionary = {
                  "gato": "chat",
                  "perro": "chien",
                  "caballo": "cheval"
    }
    # Ejemplo 2:
    phone_numbers = {'jefe': 5551234567,
                  'Suzy': 22657854310
    }
    ```
    
    Este tipo de formato se llama **sangría francesa**.
    
- ****Métodos y funciones de los diccionarios****
    
    ## El método *keys()*
    
    ¿Pueden los diccionarios ser **examinados** utilizando el bucle for, como las listas o tuplas?
    
    No y si.
    
    No, porque un diccionario **no es un tipo de dato secuencial** - el bucle for no es útil aquí.
    
    Si, porque hay herramientas simples y muy efectivas que pueden **adaptar cualquier diccionario a los requerimientos del bucle for** (en otras palabras, se construye un enlace intermedio entre el diccionario y una entidad secuencial temporal).
    
    El primero de ellos es un método denominado keys(), el cual es parte de todo diccionario. El método **retorna o regresa una lista de todas las claves dentro del diccionario**. Al tener una lista de claves se puede acceder a todo el diccionario de una manera fácil y útil.
    
    A continuación se muestra un ejemplo:
    
    ```python
    dictionary = {"gato": "chat", "perro": "chien", "caballo": "cheval"}
     
    for key in dictionary.keys():
        print(key, "->", dictionary[key]
    ```
    
    El código produce la siguiente salida:
    
    ```python
    caballo -> cheval
    perro -> chien
    gato -> chat
    ```
    
    Otra manera de hacerlo es utilizar el método items(). Este método **retorna una lista de tuplas** (este es el primer ejemplo en el que las tuplas son mas que un ejemplo de si mismas) **donde cada tupla es un par de cada clave con su valor**.
    
    Así es como funciona:
    
    ```python
    dictionary = {"gato": "chat", "perro": "chien", "caballo": "cheval"}
     
    for spanish, french in dictionary.items():
        print(spanish, "->", french)
    ```
    
    Nota la manera en que la tupla ha sido utilizada como una variable del bucle for.
    
    El ejemplo imprime lo siguiente:
    
    ```python
    gato -> chat
    perro -> chien
    caballo -> cheval
    ```
    
    ## Modificar, agregar y eliminar valores
    
    El asignar un nuevo valor a una clave existente es sencillo - debido a que los diccionarios son completamente **mutables**, no existen obstáculos para modificarlos.
    
    Se va a reemplazar el valor "chat" por "minou", lo cual no es muy adecuado, pero funcionará con nuestro ejemplo.
    
    Observa:
    
    ```python
    dictionary = {"gato": "chat", "perro": "chien", "caballo": "cheval"}
     
    dictionary['gato'] = 'minou'
    print(dictionary)
    ```
    
    La salida es:
    
    ```python
    {'gato': 'minou', 'perro': 'chien', 'caballo': 'cheval'}
    ```
    
    ## La función *sorted()*
    
    ¿Deseas que la salida este **ordenada**? Solo hay que agregar al bucle for lo siguiente:
    
    ```python
    for key in sorted(dictionary.keys()):
    ```
    
    También existe un método denominado values(), funciona de manera muy similar al de keys(), pero **regresa una lista de valores**.
    
    Este es un ejemplo sencillo:
    
    ```python
    dictionary = {"gato": "chat", "perro": "chien", "caballo": "cheval"}
     
    for french in dictionary.values():
        print(french)
    ```
    
    Como el diccionario no es capaz de automáticamente encontrar la clave de un valor dado, el rol de este método es algo limitado.
    
    ## Agregando nuevas claves
    
    El agregar una nueva clave con su valor a un diccionario es tan simple como cambiar un valor. Solo se tiene que asignar un valor a una nueva **clave que no haya existido antes**.
    
    Nota: este es un comportamiento muy diferente comparado a las listas, las cuales no permiten asignar valores a índices no existentes.
    
    A continuación se agrega un par nuevo al diccionario - un poco extraño pero válido:
    
    ```python
    dictionary = {"gato": "chat", "perro": "chien", "caballo": "cheval"}
     
    dictionary['cisne'] = 'cygne'
    print(dictionary)
    ```
    
    El ejemplo muestra como salida:
    
    ```python
    {'gato': 'chat', 'perro': 'chien', 'caballo': 'cheval', 'cisne': 'cygne'}
    ```
    
    **Nota:** También es posible insertar un elemento al diccionario utilizando el método update(), por ejemplo:
    
    ```python
    dictionary = {"gato": "chat", "perro": "chien", "caballo": "cheval"}
     
    dictionary.update({"pato": "canard"})
    print(dictionary)
    ```
    
    ## Eliminado una clave
    
    ¿Puedes deducir como eliminar una clave de un diccionario?
    
    Nota: al eliminar la clave también se **removerá el valor asociado**. **Los valores no pueden existir sin sus claves**.
    
    Esto se logra con la instrucción del.
    
    A continuación un ejemplo:
    
    ```python
    dictionary = {"gato": "chat", "perro": "chien", "caballo": "cheval"}
     
    del dictionary['perro']
    print(dictionary)
    ```
    
    Nota: **el eliminar una clave no existente, provocará un error**.
    
    El ejemplo da como salida:
    
    ```python
    {'gato': 'chat', 'caballo': 'cheval'}
    ```
    
    **EXTRA:** Para eliminar el ultimo elemento de la lista, se puede emplear el método popitem():
    
    ```python
    dictionary = {"gato": "chat", "perro": "chien", "caballo": "cheval"}
     
    dictionary.popitem()
    print(dictionary) # salida: {'gato': 'chat', 'perro': 'chien'}
    ```
    
    En versiones anteriores de Python, por ejemplo, antes de la 3.6.7, el método popitem() elimina un elemento al azar del diccionario.
    
- **Resumen**
    
    ## **Puntos Clave: tuplas**
    
    1. Las **Tuplas** son colecciones de datos ordenadas e inmutables. Se puede pensar en ellas como listas inmutables. Se definen con paréntesis:
    
    ```python
    my_tuple = (1, 2, True, "una cadena", (3, 4), [5, 6], None)
    print(my_tuple)
     
    my_list = [1, 2, True, "una cadena", (3, 4), [5, 6], None]
    print(my_list)
    ```
    
    Cada elemento de la tupla puede ser de un tipo de dato diferente (por ejemplo, enteros, cadenas, boleanos, etc.). Las tuplas pueden contener otras tuplas o listas (y viceversa).
    
    2. Se puede crear una tupla vacía de la siguiente manera:
    
    ```python
    empty_tuple = ()
    print(type(empty_tuple)) # salida: <class 'tuple'>
    ```
    
    3. La tupla de un solo elemento se define de la siguiente manera:
    
    ```python
    one_elem_tuple_1 = ("uno", ) # Paréntesis y una coma.
    one_elem_tuple_2 = "uno", # Sin paréntesis, solo la coma.
    ```
    
    Si se elimina la coma, Python creará una **variable** no una tupla:
    
    ```python
    my_tuple_1 = 1,
    print(type(my_tuple_1)) # salida: <class 'tuple'>
     
    my_tuple_2 = 1 # Esto no es una tupla.
    print(type(my_tuple_2)) # salida: <class 'int'>
    ```
    
    4. Se pueden acceder los elementos de la tupla al indexarlos:
    
    ```python
    my_tuple = (1, 2.0, "cadena", [3, 4], (5, ), True)
    print(my_tuple[3]) # salida: [3, 4]
    ```
    
    5. Las tuplas son **immutable**, lo que significa que no se puede agregar, modificar, cambiar o quitar elementos. El siguiente fragmento de código provocará una excepción:
    
    ```python
    my_tuple = (1, 2.0, "cadena", [3, 4], (5, ), True)
    my_tuple[2] = "guitarra" # Se genera la excepción TypeError.
    ```
    
    Sin embargo, se puede eliminar la tupla completa:
    
    ```python
    my_tuple = 1, 2, 3,
    del my_tuple
    print(my_tuple) # NameError: name 'my_tuple' is not defined
    ```
    
    6. Puedes iterar a través de los elementos de una tupla con un bucle (Ejemplo 1), verificar si un elemento o no esta presente en la tupla (Ejemplo 2), emplear la función len() para verificar cuantos elementos existen en la tupla (Ejemplo 3), o incluso unir o multiplicar tuplas (Ejemplo 4):
    
    ```python
    # Ejemplo 1
    tuple_1 = (1, 2, 3)
    for elem in tuple_1:
        print(elem)
     
    # Ejemplo 2
    tuple_2 = (1, 2, 3, 4)
    print(5 in tuple_2)
    print(5 not in tuple_2)
     
    # Ejemplo 3
    tuple_2 = (1, 2, 3, 4)
    print(len(tuple_3))
    print(5 not in tuple_2)
    # Ejemplo 4
    tuple_4 = tuple_1 + tuple_2
    tuple_5 = tuple_3 * 2
     
    print(tuple_4)
    print(tuple_5)
    ```
    
    **EXTRA:** También se puede crear una tupla utilizando la función integrada de Python tuple(). Esto es particularmente útil cuando se desea convertir un iterable (por ejemplo, una lista, rango, cadena, etcétera) en una tupla:
    
    ```python
    my_tuple = tuple((1, 2, "cadena"))
    print(my_tuple)
     
    my_list = [2, 4, 6]
    print(my_list) # salida: [2, 4, 6]
    print(type(my_list)) # salida: <class 'list'>
    tup = tuple(my_list)
    print(tup) # salida: (2, 4, 6)
    print(type(tup)) # salida: <class 'tuple'>
    ```
    
    De la misma manera, cuando se desea convertir un iterable en una lista, se puede emplear la función integrada de Python denominada list():
    
    ```python
    tup = 1, 2, 3,
    my_list = list(tup)
    print(type(my_list)) # salida: <class 'list'>
    ```
    
    ## **Puntos clave: diccionarios**
    
    1. Los diccionarios son *****colecciones indexadas de datos, mutables y desordenadas. (*****En Python 3.6x los diccionarios están ordenados de manera predeterminada.
    
    Cada diccionario es un par de *clave:valor*. Se puede crear empleando la siguiente sintaxis:
    
    ```python
    my_dictionary = {
        key1: value1,
        key2: value2,
        key3: value3,
    }
    ```
    
    2. Si se desea acceder a un elemento del diccionario, se puede hacer haciendo referencia a su clave colocándola dentro de corchetes (Ejemplo 1) o utilizando el método get() (Ejemplo 2):
    
    ```python
    pol_esp_dictionary = {
        "kwiat": "flor",
        "woda": "agua",
        "gleba": "tierra"
        }
     
    item_1 = pol_esp_dictionary["gleba"] # Ejemplo 1
    print(item_1) # salida: tierra
     
    item_2 = pol_esp_dictionary.get("woda") # Ejemplo 2
    print(item_2) # salida: agua
    ```
    
    3. Si se desea cambiar el valor asociado a una clave específica, se puede hacer haciendo referencia a la clave del elemento, a continuación se muestra un ejemplo:
    
    ```python
    pol_esp_dictionary = {
        "zamek": "castillo",
        "woda": "agua",
        "gleba": "tierra"
        }
     
    pol_esp_dictionary["zamek"] = "cerradura"
    item = pol_esp_dictionary["zamek"]
    print(item) # salida: cerradura
    ```
    
    4. Para agregar o eliminar una clave (junto con su valor asociado), emplea la siguiente sintaxis:
    
    ```python
    phonebook = {} # un diccionario vacío
     
    phonebook["Adán"] = 3456783958 # crear/agregar un par clave-valor
    print(phonebook) # salida: {'Adán': 3456783958}
     
    del phonebook["Adán"]
    print(phonebook) # salida: {}
    ```
    
    Además, se puede insertar un elemento a un diccionario utilizando el método update(), y eliminar el ultimo elemento con el método popitem(), por ejemplo:
    
    ```python
    pol_esp_dictionary = {"kwiat": "flor"}
     
    pol_esp_dictionary.update({"gleba": "tierra"})
    print(pol_esp_dictionary) # salida: {'kwiat': 'flor', 'gleba': 'tierra'}
     
    pol_esp_dictionary.popitem()
    print(pol_esp_dictionary) # salida: {'kwiat': 'flor'}
    ```
    
    5. Se puede emplear el bucle for para iterar a través del diccionario, por ejemplo:
    
    ```python
    pol_esp_dictionary = {
        "zamek": "castillo",
        "woda": "agua",
        "gleba": "tierra"
        }
     
    for item in pol_esp_dictionary:
        print(item)
    #          zamek
    #          woda
    #          gleba
    ```
    
    6. Si deseas examinar los elementos (claves y valores) del diccionario, puedes emplear el método items(), por ejemplo:
    
    ```python
    pol_esp_dictionary = {
        "zamek": "castillo",
        "woda": "agua",
        "gleba": "tierra"
        }
     
    for key, value in pol_esp_dictionary.items():
        print("Pol/Esp ->", key, ":", value)
    ```
    
    7. Para comprobar si una clave existe en un diccionario, se puede emplear la palabra clave reservada in:
    
    ```python
    pol_esp_dictionary = {
        "zamek": "castillo",
        "woda": "agua",
        "gleba": "tierra"
        }
     
    if "zamek" in pol_esp_dictionary:
       print("Si")
    else:
       print("No")
    ```
    
    8. Se puede emplear la palabra reservada del para eliminar un elemento, o un diccionario entero. Para eliminar todos los elementos de un diccionario se debe emplear el método clear():
    
    ```python
    pol_esp_dictionary = {
        "zamek": "castillo",
        "woda": "agua",
        "gleba": "tierra"
        }
     
    print(len(pol_esp_dictionary)) # salida: 3
    del pol_esp_dictionary["zamek"] # eliminar un elemento
    print(len(pol_esp_dictionary)) # salida: 2
     
    pol_esp_dictionary.clear() # eliminar todos los elementos
    print(len(pol_esp_dictionary)) # salida: 0
     
    del pol_esp_dictionary # elimina el diccionario
    ```
    
    9. Para copiar un diccionario, emplea el método copy():
    
    ```python
    pol_esp_dictionary = {
        "zamek": "castillo",
        "woda": "agua",
        "gleba": "tierra"
        }
     
    copy_dictionary = pol_esp_dictionary.copy()
    ```
    

**Excepciones**:

- ****Errores - el pan diario del desarrollador****
    
    ![https://skillsforall.com/content/pe1/1.0/m4/course/es-XL/assets/f136b0b029f4fcfb2c5bd65038eb996bff9d2ae5.png](https://skillsforall.com/content/pe1/1.0/m4/course/es-XL/assets/f136b0b029f4fcfb2c5bd65038eb996bff9d2ae5.png)
    
    Parece indiscutible que todos los programadores (incluso tú) quieren escribir código libre de errores y hacen todo lo posible para lograr este objetivo. Desafortunadamente, nada es perfecto en este mundo y el software no es una excepción. Presta atención a la palabra **excepción**, ya que la veremos de nuevo muy pronto en un significado que no tiene nada que ver con el común.
    
    Errar es humano, es imposible no cometer errores y es imposible escribir código sin errores. No nos malinterpretes, no queremos convencerte de que escribir programas desordenados y defectuosos es una virtud. Más bien queremos explicar que incluso el programador más cuidadoso no puede evitar defectos menores o mayores. Sólo aquellos que no hacen nada no cometen errores.
    
    Paradójicamente, aceptar esta difícil verdad puede convertirte en un mejor programador y mejorar la calidad de tu código.
    
    Te preguntarás: "¿Cómo podría ser esto posible?".
    
    Intentaremos mostrártelo.
    
    ## Errores en los datos frente a errores en el código
    
    El lidiar con errores de programación tiene (al menos) dos partes. La primera es cuando te metes en problemas porque tu código, aparentemente correcto, se alimenta con datos incorrectos. Por ejemplo, esperas que se ingrese al código un valor entero, pero tu usuario descuidado ingresa algunas letras al azar.
    
    Puede suceder que tu código termine en ese momento y el usuario se quede solo con un mensaje de error conciso y a la vez ambiguo en la pantalla. El usuario estará insatisfecho y tu también deberías estarlo.
    
    Te mostraremos cómo proteger tu código de este tipo de fallas y cómo no provocar la ira del usuario.
    
    La segunda parte de lidiar con errores de programación se revela cuando ocurre un comportamiento no deseado del programa debido a errores que se cometieron cuando se estaba escribiendo el código. Este tipo de error se denomina comúnmente "bug" (bicho en inglés), que es una manifestación de una creencia bien establecida de que, si un programa funciona mal, esto debe ser causado por bichos maliciosos que viven dentro del hardware de la computadora y causan cortocircuitos u otras interferencias.
    
    Esta idea no es tan descabellada como puede parecer: incidentes de este tipo eran comunes en tiempos en que las computadoras ocupaban grandes pasillos, consumían kilovatios de electricidad y producían enormes cantidades de calor. Afortunadamente, o no, estos tiempos se han ido para siempre y los únicos errores que pueden estropear tu código son los que tú mismo sembraste en el código. Por lo tanto, intentaremos mostrarte cómo encontrar y eliminar tus errores, en otras palabras, cómo depurar tu código.
    
    Comencemos el viaje por la tierra de los errores y las fallas o bugs.
    
- **Cuando los datos no son lo que deberían ser**
    
    Escribamos un fragmento de código extremadamente trivial - leerá un número natural (un entero no negativo) e imprimirá su recíproco. De esta forma, 2 se convertirá en 0.5 (1/2) y 4 en 0.25 (1/4). Aquí está el programa:
    
    ```python
    value = int(input('Ingresa un número natural: '))
    print('El recíproco de', value, 'es', 1/value)
    ```
    
    ¿Hay algo que pueda salir mal? El código es tan breve y compacto que no parece que vayamos a encontrar ningún problema allí.
    
    Parece que ya sabes hacia dónde vamos. Sí, tienes razón - ingresar datos que no sean un número entero (que también incluye ingresar nada) arruinará completamente la ejecución del programa. Esto es lo que verá el usuario del código:
    
    ```python
    Traceback (most recent call last):
       File "code.py", line 1, in
        value = int(input('Ingresa un número natural: '))
    ValueError: invalid literal for int() with base 10: ''
    ```
    
    Todas las líneas que muestra Python son significativas e importantes, pero la última línea parece ser la más valiosa. La primera palabra de la línea es el nombre de la **excepción** la cual provoca que tu código se detenga. Su nombre aquí es ValueError. El resto de la línea es solo una breve explicación que especifica con mayor precisión la causa de la excepción ocurrida.
    
    ¿Cómo lo afrontas? ¿Cómo proteges tu código de la terminación abrupta, al usuario de la decepción y a ti mismo de la insatisfacción del usuario?
    
    La primera idea que se te puede ocurrir es verificar si los datos proporcionados por el usuario son válidos y negarte a cooperar si los datos son incorrectos. En este caso, la verificación puede basarse en el hecho de que esperamos que la cadena de entrada contenga solo dígitos.
    
    Ya deberías poder implementar esta verificación y escribirla tu mismo, ¿no es así? También es posible comprobar si la variable value es de tipo int (Python tiene un medio especial para este tipo de comprobaciones - es un operador llamado is. La revisión en sí puede verse de la siguiente manera:
    
    ```python
    type(value) is int
    ```
    
    Su resultado es true si el valor actual de la variable es del tipo int.
    
    Perdónanos si no dedicamos más tiempo a esto ahora - encontrarás explicaciones más detalladas sobre el operador is en un módulo del curso dedicado a la programación orientada a objetos.
    
    Es posible que te sorprendas al saber que no queremos que realices ninguna validación preliminar de datos. ¿Por qué? Porque esta no es la forma que Python recomienda.
    
- **La rama try-except**
    
    En el mundo de Python, hay una regla que dice: *"Es mejor pedir perdón que pedir permiso"*.
    
    Detengámonos aquí por un momento. No nos malinterpretes - no queremos que apliques la regla en tu vida diaria. No tomes el automóvil de nadie sin permiso, con la esperanza de que puedas ser tan convincente que evites la condena por lo ocurrido. La regla se trata de otra cosa.
    
    En realidad, la regla dice: *"es mejor manejar un error cuando ocurre que tratar de evitarlo"*.
    
    *"De acuerdo,"* puedes decir, *"pero ¿cómo debo pedir perdón cuando el programa finaliza y no queda nada que más por hacer?"* Aquí es donde algo llamado **excepción** entra en escena.
    
    Observa el código en el editor:
    
    ```python
    try:
    	# Es un lugar donde
    	# tu puedes hacer algo 
        # sin pedir permiso.
    except:
    	# Es un espacio dedicado  
        # exclusivamente para pedir perdón.
    ```
    
    Puedes ver dos bloques aquí:
    
    - el primero, comienza con la palabra clave reservada try este es el lugar donde se coloca el código que se sospecha que es riesgoso y puede terminar en caso de un error; nota: este tipo de error lleva por nombre **excepción**, mientras que la ocurrencia de la excepción se le denomina **generar** - podemos decir que se genera (o se generó) una excepción;
    - el segundo, la parte del código que comienza con la palabra clave reservada except esta parte fue diseñada para manejar la excepción; depende de ti lo que quieras hacer aquí: puedes limpiar el desorden o simplemente puede barrer el problema debajo de la alfombra (aunque se prefiere la primera solución)
    
    Entonces, podríamos decir que estos dos bloques funcionan así:
    
    - la palabra clave reservada try marca el lugar donde intentas hacer algo sin permiso;
    - la palabra clave reservada except comienza un lugar donde puedes mostrar tu talento para disculparte o pedir perdón.
    
    Como puedes ver, este enfoque acepta errores (los trata como una parte normal de la vida del programa) en lugar de intensificar los esfuerzos para evitarlos por completo.
    
- **La excepción confirma la regla**
    
    Reescribamos el código para adoptar el enfoque de Python para la vida:
    
    ```python
    try:
        value = int(input('Ingresa un número natural: '))
        print('El recíproco de', value, 'es', 1/value)        
    except:
        print('No se que hacer con', value)
    ```
    
    Resumamos lo que hemos hablado:
    
    - cualquier fragmento de código colocado entre try y except se ejecuta de una manera muy especial: cualquier error que ocurra aquí dentro **no terminará la ejecución del programa**. En cambio, el control saltará inmediatamente a la primera línea situada después de la palabra clave reservada except, y no se ejecutará ninguna otra línea del bloque try;
    - el código en el bloque except se activa solo cuando se ha encontrado una excepción dentro del bloque try. No hay forma de llegar por ningún otro medio;
    - cuando el bloque try o except se ejecutan con éxito, el control vuelve al proceso normal de ejecución y cualquier código ubicado más allá en el archivo fuente se ejecuta como si no hubiera pasado nada.
    
    Ahora queremos hacerte una pregunta: ¿Es ValueError la única forma en que el control podría caer dentro del bloque except?
    
    ¡Analiza el código cuidadosamente y piensa en tu respuesta!
    
- ****Cómo lidiar con más de una excepción****
    
    La respuesta obvia es "no" - hay más de una forma posible de plantear una excepción. Por ejemplo, un usuario puede ingresar cero como entrada - ¿puedes predecir lo que sucederá a continuación?
    
    Sí, tienes razón - la división colocada dentro de la invocación de la función print() generará la excepción ZeroDivisionError. Como es de esperarse, el comportamiento del código será el mismo que en el caso anterior - el usuario verá el mensaje *"No se que hacer con..."*, lo que parece bastante razonable en este contexto, pero también es posible que desees manejar este tipo de problema de una manera un poco diferente.
    
    ¿Es posible? Por supuesto que lo es. Hay al menos dos enfoques que puedes implementar aquí.
    
    El primero de ellos es simple y complicado al mismo tiempo: puedes agregar dos bloques try por separado, uno que incluya la invocación de la función input() donde se puede generar la excepción ValueError y el segundo dedicado a manejar posibles problemas inducidos por la división. Ambos bloques try tendrían su propio except y de esa manera, tendrías un control total sobre dos errores diferentes.
    
    Esta solución es buena, pero es un poco larga - el código se hincha innecesariamente. Además, no es el único peligro que te espera. Toma en cuenta que dejar el primer bloque try-except deja mucha incertidumbre - tendrás que agregar código adicional para asegurarte de que el valor que ingresó el usuario sea seguro para usar en la división. Así es como una solución aparentemente simple se vuelve demasiado complicada.
    
    Afortunadamente, Python ofrece una forma más sencilla de afrontar este tipo de desafíos.
    
    ## Dos excepciones después de un *try*
    
    Observa el código en el editor. Como puedes ver, acabamos de agregar un segundo except. Esta no es la única diferencia - toma en cuenta que ambos except tienen **nombres** de excepción específicos. En esta variante, cada una de las excepciones esperadas tiene su propia forma de manejar el error, pero debe enfatizarse en que **solo una** de todas puede interceptar el control - **si se ejecuta una, todas las demás permanecen inactivas**.
    
    ```python
    try:
        value = int(input('Ingresa un número natural: '))
        print('El recíproco de', value, 'es', 1/value)        
    except ValueError:
        print('No se que hacer con', value)    
    except ZeroDivisionError:
        print('La división entre cero no está permitida en nuestro Universo.')
    ```
    
    Además, la cantidad de except no está limitada - puedes especificar tantas o tan pocas como necesites, pero no se te olvide que **ninguna de las excepciones se puede especificar más de una vez**.
    
    Pero esta todavía no es la última palabra de Python sobre excepciones.
    
- ****La excepción predeterminada y cómo usarla****
    
    El código ha cambiado de nuevo - ¿puedes ver la diferencia?
    
    ```python
    try:
        value = int(input('Ingresa un número natural: '))
        print('El recíproco de', value, 'es', 1/value)        
    except ValueError:
        ('No se que hacer con.')    
    except ZeroDivisionError:
        print('La división entre cero no está permitida en nuestro Universo.')    
    except:
        print('Ha sucedido algo extraño, ¡lo siento!')
    ```
    
    Hemos agregado un tercer except, pero esta vez **no tiene un nombre de excepción específico** – podemos decir que es **anónimo** o (lo que está más cerca de su función real) es el por **defecto**. Puedes esperar que cuando se genere una excepción y no haya un except dedicado a esa excepción, esta será manejada por la excepción por defecto.
    
    **Nota:** ¡el except por defecto debe ser el último except! ¡Siempre!
    
- ****Algunas excepciones útiles****
    
    Analicemos con más detalle algunas excepciones útiles (o más bien, las más comunes) que puedes llegar a experimentar.
    
    ## ZeroDivisionError
    
    Esta aparece cuando intentas forzar a Python a realizar cualquier operación que provoque una división en la que el divisor es cero o no se puede distinguir de cero. Toma en cuenta que hay más de un operador de Python que puede hacer que se genere esta excepción. ¿Puedes adivinarlos todos?
    
    Si, estos son: /, //, y %.
    
    ## ValueError
    
    Espera esta excepción cuando estás manejando valores que pueden usarse de manera inapropiada en algún contexto. En general, esta excepción se genera cuando una función (como int() o float()) recibe un argumento de un tipo adecuado, pero su valor es inaceptable.
    
    ## TypeError
    
    Esta excepción aparece cuando intentas aplicar un dato cuyo tipo no se puede aceptar en el contexto actual. Mira el ejemplo:
    
    ```python
    short_list = [1]
    one_value = short_list[0.5]
    ```
    
    No está permitido usar un valor flotante como índice de una lista (la misma regla también se aplica a las tuplas). TypeError es un nombre adecuado para describir el problema y una excepción adecuada a generar.
    
    ## AttributeError
    
    Esta excepción llega - entre otras ocasiones - cuando intentas activar un método que no existe en un elemento con el que se está tratando. Por ejemplo:
    
    ```python
    short_list = [1]
    short_list.append(2)
    short_list.depend(3)
    ```
    
    La tercera línea de nuestro ejemplo intenta hacer uso de un método que no está incluido en las listas. Este es el lugar donde se genera la excepción AttributeError.
    
    ## SyntaxError
    
    Esta excepción se genera cuando el control llega a una línea de código que viola la gramática de Python. Puede sonar extraño, pero algunos errores de este tipo no se pueden identificar sin ejecutar primero el código. Este tipo de comportamiento es típico de los lenguajes interpretados - el intérprete siempre trabaja con prisa y no tiene tiempo para escanear todo el código fuente. Se conforma con comprobar el código que se está ejecutando en el momento. Muy pronto se te presentará un ejemplo de esta categoría.
    
    Es una mala idea manejar este tipo de excepciones en tus programas. Deberías producir código sin errores de sintaxis, en lugar de enmascarar las fallas que has causado.
    
- ****Por qué no puedes evitar probar tu código****
    
    Aunque vamos a resumir nuestras consideraciones *excepcionales* aquí, no creas que es todo lo que Python puede ofrecer para ayudarte a suplicar perdón. La maquinaria de excepciones de Python es mucho más compleja y sus capacidades te permiten desarrollar estrategias de manejo de errores expandidas. Volveremos a estos temas - lo prometemos. No dudes en realizar tus experimentos y sumergirte en las excepciones por ti mismo.
    
    Ahora queremos contarte sobre el segundo lado de la lucha interminable contra los errores - el destino inevitable de la vida de un desarrollador. Como no puedes evitar la creación de errores en tu código, siempre debes estar listo para buscarlos y destruirlos. No entierres la cabeza en la arena - ignorar los errores no los hará desaparecer.
    
    Un deber importante para los desarrolladores es probar el código recién creado, pero no debes olvidar que las pruebas no son una forma de demostrar que el código está libre de errores. Paradójicamente, lo único que las pruebas determinan, es que tu código contiene errores. No creas que puedes relajarte después de una prueba exitosa.
    
    El segundo aspecto importante de las pruebas de software es estrictamente psicológico. Es una verdad conocida desde hace años que los autores - incluso aquellos que son confiables y conscientes de sí mismos - **no pueden evaluar y verificar objetivamente sus trabajos**.
    
    Es por eso por lo que cada novelista necesita un editor y cada programador necesita un tester. Algunos dicen - con un poco de rencor, pero con sinceridad - que los desarrolladores prueban su código para mostrar su perfección, no para encontrar problemas que puedan frustrarlos. Los testers o probadores están libres de tales dilemas, y es por eso por lo que su trabajo es más efectivo y rentable.
    
    Por supuesto, esto no te exime de estar atento y cauteloso. Prueba tu código lo mejor que puedas. No facilites demasiado el trabajo a los probadores.
    
    Su deber principal es **asegurarse de haber verificado todas las rutas o caminos de ejecución** por las que puede pasar tu código. ¿Suena misterioso? ¡Por supuesto que no!
    
    ## Rastreando las rutas de ejecución
    
    Supón que acabas de terminar de escribir este fragmento de código:
    
    ```python
    temperature = float(input('Ingresa la temperatura actual:'))
    
    if temperature > 0:
        print("Por encima de cero")
    elif temperature < 0:
        print("Por debajo de cero")
    else:
        print("Cero")
    ```
    
    Existen tres rutas o caminos de ejecución independientes en el código - ¿puedes verlas? Están determinadas por las sentencias if-elif-else. Por supuesto, las rutas de ejecución pueden construirse mediante muchas otras sentencias como bucles, o incluso bloques try-except.
    
    Si vas a probar tu código de manera justa y quieres dormir profundamente y soñar sin pesadillas (las pesadillas sobre errores pueden ser devastadoras para el rendimiento de un desarrollador), estás obligado a preparar un conjunto de datos de prueba que obligará a tu código a negociar todos los posibles caminos.
    
    En nuestro ejemplo, el conjunto debe contener al menos tres valores flotantes: uno positivo, uno negativo y cero.
    
- ****Cuando Python cierra los ojos****
    
    Tal prueba es crucial. Queremos mostrarte por qué no debes omitirlo. Observa el siguiente código:
    
    ```python
    temperature = float(input('Ingresa la temperatura actual:'))
    
    if temperature > 0:
        print("Por encima de cero")
    elif temperature < 0:
        prin("Por debajo de cero")
    else:
        print("Cero")
    ```
    
    Introdujimos intencionalmente un error en el código - esperamos que tus ojos atentos lo noten de inmediato. Sí, eliminamos solo una letra y, en efecto, la invocación válida de la función print() se convierte en la obviamente inválida invocación "prin()". No existe tal función como "prin()" en el alcance de nuestro programa, pero ¿es realmente obvio para Python?
    
    Ejecuta el código e ingresa un 0.
    
    Como puedes ver, el código finaliza su ejecución sin ningún obstáculo.
    
    ¿Cómo es eso posible? ¿Por qué Python **pasa por alto** un error de desarrollador tan evidente?
    
    ¿Puedes encontrar las respuestas a estas preguntas fundamentales?
    
- ****Pruebas, pruebas y más pruebas****
    
    La respuesta es más simple de lo esperado y también un poco decepcionante. Python - como seguramente sabes - es un lenguaje **interpretado**. Esto significa que el código fuente se analiza y ejecuta al mismo tiempo. En consecuencia, es posible que Python no tenga tiempo para analizar las líneas de código que no están sujetas a ejecución. Como dice un antiguo dicho de los desarrolladores: *"es una característica, no un error"* (no utilices esta frase para justificar el comportamiento extraño de tu código).
    
    ¿Entiendes ahora por qué el pasar por todos los caminos de ejecución es tan vital e inevitable?
    
    Supongamos que terminas tu código y que las pruebas que has realizado son exitosas. Entregas tu código a los probadores y - ¡afortunadamente! - encontraron algunos errores en él. Estamos usando la palabra *"afortunadamente"* de manera completamente consciente. Debes aceptar que, en primer lugar, los probadores son los mejores amigos del desarrollador - no debes tratar a los errores que ellos encuentran como una ofensa o una malignidad; y, en segundo lugar, cada error que encuentran los probadores es un error que no afectará a los usuarios. Ambos factores son valiosos y merecen tu atención.
    
    Ya sabes que tu código contiene un error o errores (lo segundo es más probable). Ahora, ¿cómo los localizas y cómo arreglas tu código?
    
    ## Error frente a depuración (Bug vs. debug)
    
    La medida básica que un desarrollador puede utilizar contra los errores es - como era de esperarse - un **depurador**, mientras que el proceso durante el cual se eliminan los errores del código se llama **depuración**. Según un viejo chiste, la depuración es un complicado juego de misterio en el que eres simultáneamente el asesino, el detective y - la parte más dolorosa de la intriga - la víctima. ¿Estás listo para interpretar todos estos roles? Entonces debes armarte con un depurador.
    
    Un depurador es un software especializado que puede controlar cómo se ejecuta tu programa. Con el depurador, puedes ejecutar tu código línea por línea, inspeccionar todos los estados de las variables y cambiar sus valores en cualquier momento sin modificar el código fuente, detener la ejecución del programa cuando se cumplen o no ciertas condiciones, y hacer muchas otras tareas útiles.
    
    ![https://skillsforall.com/content/pe1/1.0/m4/course/es-XL/assets/58d35007ddf39a88e5356f4586d14a3e187b60e9.png](https://skillsforall.com/content/pe1/1.0/m4/course/es-XL/assets/58d35007ddf39a88e5356f4586d14a3e187b60e9.png)
    
    Podemos decir que todo IDLE está equipado con un depurador más o menos avanzado. Incluso IDLE tiene uno, aunque puedes encontrar su manejo un poco complicado y problemático. Si deseas utilizar el depurador integrado de IDLE, debes activarlo mediante la opción "Debug" en la barra de menú de la ventana principal de IDLE. Es el punto de partida para la depuración.
    
    [Haz clic aquí para ver las capturas de pantalla](https://www.cs.uky.edu/~keen/help/debug-tutorial/debug.html) que muestran el depurador IDLE durante una sesión de depuración simple. (¡Gracias, Universidad de Kentucky!)
    
    Puedes ver cómo el depurador visualiza las variables y los valores de los parámetros, y observa la pila de llamadas que muestra la cadena de invocaciones que van desde la función actualmente ejecutada hasta el nivel del intérprete.
    
    Si deseas saber más sobre el depurador IDLE, consulta [la documentación IDLE](https://docs.python.org/3/library/idle.html).
    
- **Print debugging (depuración)**
    
    ![https://skillsforall.com/content/pe1/1.0/m4/course/es-XL/assets/f0c57e27d43f5e74b8641ec2dc2e501be0fdbbc4.png](https://skillsforall.com/content/pe1/1.0/m4/course/es-XL/assets/f0c57e27d43f5e74b8641ec2dc2e501be0fdbbc4.png)
    
    Esta forma de depuración, que se puede aplicar a tu código mediante cualquier tipo de depurador, a veces se denomina **depuración interactiva**. El significado del término se explica por sí mismo - el proceso necesita su interacción (la del desarrollador) para que se lleve a cabo.
    
    Algunas otras técnicas de depuración se pueden utilizar para cazar errores. Es posible que no puedas o no quieras usar un depurador (las razones pueden variar). ¿Estás entonces indefenso? ¡Absolutamente no!
    
    Puedes utilizar una de las tácticas de depuración más simples y antiguas (pero aún útil) conocida como la **depuración por impresión**. El nombre habla por sí mismo - simplemente insertas varias invocaciones print() adicionales dentro de tu código para generar datos que ilustran la ruta que tu código está negociando actualmente. Puedes imprimir los valores de las variables que pueden afectar la ejecución.
    
    Estas impresiones pueden generar texto significativo como *"Estoy aquí"*, *"Ingresé a la función foo()"*, *"El resultado es 0"*, o pueden contener secuencias de caracteres que solo tu puedes leer. Por favor, no uses palabras obscenas o indecentes para ese propósito, aunque puedas sentir una fuerte tentación - tu reputación puede arruinarse en un momento si estas payasadas se filtran al público.
    
    Como puedes ver, este tipo de depuración no es realmente interactiva en lo absoluto, o es interactiva solo en pequeña medida, cuando decides aplicar la función input() para detener o retrasar la ejecución del código.
    
    Una vez que se encuentran y eliminan los errores, las impresiones adicionales pueden comentarse o eliminarse - tu decides. No permitas que se ejecuten en el código final - pueden confundir tanto a los probadores como a los usuarios, y traer mal karma sobre ti.
    
- ****Algunos consejos útiles****
    
    Aquí hay algunos consejos que pueden ayudarte a encontrar y eliminar errores. Ninguno de ellos es definitivo. Úsalos de manera flexible y confía en tu intuición. No te creas a ti mismo - comprueba todo dos veces.
    
    1. **Intenta decirle a alguien** (por ejemplo, a tu amigo o compañero de trabajo) qué es lo que se espera que haga tu código y cómo se espera que se comporte. Sé concreto y no omitas detalles. Responde todas las preguntas que te hagan. Es probable que te des cuenta de la causa del problema mientras cuentas tu historia, ya que el hablar activa esas partes de tu cerebro que permanecen inactivas mientras codificas. Si ningún humano puede ayudarte con el problema, usa un patito amarillo de goma en su lugar. No estamos bromeando - consulta el artículo de Wikipedia para obtener más información sobre esta técnica de uso común: [Rubber Duck Debugging](https://en.wikipedia.org/wiki/Rubber_duck_debugging).
    2. **Intenta aislar el problema.** Puedes extraer la parte de tu código que se sospecha que es responsable de tus problemas y ejecutarla por separado. Puedes comentar partes del código para ocultar el problema. Asigna valores concretos a las variables en lugar de leerlos desde la consola. Prueba tus funciones aplicando valores de argumentos predecibles. Analiza el código cuidadosamente. Léelo en voz alta.
    3. Si el error apareció recientemente y no había aparecido antes, **analiza todos los cambios que has introducido en tu código** - uno de ellos puede ser la razón.
    4. **Tómate un descanso**, bebe una taza de café, toma a tu perro y sal a caminar, lee un buen libro, incluso dos, haz una llamada telefónica a tu mejor amigo - te sorprenderás de la frecuencia con la que esto ayuda.
    5. **Se optimista** eventualmente encontrarás el error; te lo prometemos.
- ****Pruebas unitarias - un mayor nivel de codificación****
    
    También existe una técnica de programación importante y ampliamente utilizada que tendrás que adoptar tarde o temprano durante tu carrera de desarrollador - se llama prueba unitaria. El nombre puede ser un poco confuso, ya que no se trata solo de probar el software, sino también (y, sobre todo) de cómo se escribe el código.
    
    Para resumir la historia - las pruebas unitarias asumen que las pruebas son partes inseparables del código y la preparación de los datos de prueba es una parte inseparable de la codificación. Esto significa que cuando escribes una función o un conjunto de funciones cooperativas, también estás obligado a crear un conjunto de datos para los cuales el comportamiento de tu código es predecible y conocido.
    
    Además, debes equipar a tu código con una interfaz que pueda ser utilizada por un entorno de pruebas automatizado. En este enfoque, cualquier enmienda realizada al código (incluso la menos significativa) debe ir seguida de la ejecución de todas las pruebas unitarias que acompañan al código fuente.
    
    Para estandarizar este enfoque y facilitar su aplicación, Python proporciona un módulo dedicado llamado unittest. No vamos a discutirlo aquí - es un tema amplio y complejo.
    
    Por lo tanto, hemos preparado un curso y una ruta de certificación independiente para este tema. Se llama ”Testing Essentials with Python, y ahora te invitamos a participar en él.
    
    ¡Nos vemos pronto!
    
- **Resumen**
    
    1. En Python, existe una distinción entre dos tipos de errores:
    
    - **errores de sintaxis** (errores de análisis), que ocurren cuando el analizador encuentra una sentencia de código que no es correcta. Por ejemplo:
    
    El intentar ejecutar la siguiente línea:
    
    ```python
    print("Hola, ¡Mundo!)
    ```
    
    Provocará un error del tipo *SyntaxError*, y da como resultado el siguiente (o similar) mensaje que se muestra en la consola:
    
    Presta atención a la flecha - indica el lugar donde el analizador de Python ha tenido problemas. En este caso, la doble comilla es la que falta. ¿Lo notaste?
    
    ```python
    File "main.py", line 1
     
        print("Hola, ¡Mundo!)
                            ^
    SyntaxError: EOL while scanning string literal
    ```
    
    - **excepciones**, ocurren incluso cuando una sentencia o expresión es sintácticamente correcta. Estos son los errores que se detectan durante la ejecución, cuando tu código da como resultado un error que no es *incondicionalmente fatal*. Por ejemplo:
    
    El intentar ejecutar la siguiente línea:
    
    ```python
    print(1/0)
    ```
    
    Provocará una excepción *ZeroDivisionError*, y da como resultado el siguiente (o similar) mensaje que se muestra en la consola:
    
    ```python
    Traceback (most recent call last):
      File "main.py", line 1, in
        print(1/0)
    ZeroDivisionError: division by zero
    ```
    
    Presta atención a la última línea del mensaje de error - en realidad, te dice lo que sucedió. Existen muchos diferentes tipos de excepciones, como *ZeroDivisionError*, *NameError*, *TypeError*, y muchas mas; y esta parte del mensaje te informa qué tipo de excepción se ha generado. Las líneas anteriores muestran el contexto en el que ha ocurrido la excepción.
    
    2. Puedes "capturar" y manejar excepciones en Python usando el bloque *try-except*. Por lo tanto, si tienes la sospecha de que cualquier fragmento de código en particular puede generar una excepción, puedes escribir el código que la manejará con elegancia y no interrumpirá el programa. Observa el ejemplo:
    
    ```python
    while True:
        try:
            number = int(input("Ingresa un número entero: "))
            print(number/2)
            break
        except:
            print("Advertencia: el valor ingresado no es un número válido. Intenta de nuevo...")
    ```
    
    El código anterior le pide al usuario que ingrese un valor hasta que el valor ingresado sea un número entero válido. Si el usuario ingresa un valor que no se puede convertir a un int, el programa imprimirá en la consola Advertencia: el valor ingresado no es un número válido. Intenta de nuevo..., y pide al usuario que ingrese un número nuevamente. Veamos que sucede en dicho caso?
    
    1. El programa entra en el bucle *while*.
    2. El bloque try se ejecuta. El usuario ingresa un valor incorrecto, por ejemplo: ¡hola!.
    3. Se genera una excepción, y el resto del código del bloque try se omite. El programa salta al bloque except, lo ejecuta, y luego se sigue al código que se encuentra después del bloque *try-except*.
    
    Si el usuario ingresa un valor correcto y no se genera ninguna excepción, las instrucciones subsiguientes al bloque *try*, son ejecutadas. En este caso, los excepts no se ejecutan.
    
    3. Puedes manejar múltiples excepciones en tu bloque de código. Analiza los siguientes ejemplos:
    
    ```python
    while True:
        try:
            number = int(input("Ingresa un número entero: "))
            print(5/number)
            break
        except ValueError:
            print("Valor incorrecto.")
        except ZeroDivisionError:
            print("Lo siento. No puedo dividir entre cero.")
        except:
            print("No se que hacer...")
    ```
    
    Puedes utilizar varios bloques *except* dentro de una sentencia *try*, y especificar nombres de excepciones. Si se ejecuta alguno de los except, los otros se omitirán. Recuerda: puedes especificar una excepción integrada solo una vez. Además, no olvides que la excepción por **defecto** (o genérica), es decir, a la que no se le especifica nombre, debe ser colocada **al final** (utiliza las excepciones más específicas primero, y las más generales al último).
    
    También puedes especificar y manejar múltiples excepciones integradas dentro de un solo bloque *except*:
    
    ```python
    while True:
        try:
            number = int(input("Ingresa un número entero: "))
            print(5/number)
            break
        except (ValueError, ZeroDivisionError):
            print("Valor incorrecto o se ha roto la regla de división entre cero.")
        except:
            print("Lo siento, algo salió mal...")
    ```
    
    4. Algunas de las excepciones integradas más útiles de Python son: *ZeroDivisionError*, *ValueError*, *TypeError*, *AttributeError*, y *SyntaxError*. Una excepción más que, en nuestra opinión, merece tu atención es la excepción *KeyboardInterrupt*, que se genera cuando el usuario presiona la tecla de interrupción (CTRL-C o Eliminar). Ejecuta el código anterior y presiona la combinación de teclas para ver qué sucede.
    
    Para obtener más información sobre las excepciones integradas de Python, consulta la documentación oficial de Python [aquí.](https://docs.python.org/3/library/exceptions.html)
    
    5. Por último, pero no menos importante, debes recordar cómo probar y depurar tu código. Utiliza técnicas de depuración como depuración de *impresión*; si es posible - pide a alguien que lea tu código y te ayude a encontrar errores o mejorarlo; intenta aislar el fragmento de código que es problemático y susceptible a errores: **prueba tus funciones** aplicando valores de argumento predecibles, y trata de **manejar** las situaciones en las que alguien ingresa valores incorrectos; **comenta** las partes del código que ocultan el problema. Finalmente, toma descansos y vuelve a tu código después de un tiempo con un par de ojos nuevos.
