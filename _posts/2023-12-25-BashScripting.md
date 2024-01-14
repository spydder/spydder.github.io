---
title: "BashScripting"
date: 2023-12-25
categories: [Programming_Language]
tags: [bash]
image:
    path: /assets/BashScripting/bashscripting.jpg
---



# Bash Scripting

Comandos y conceptos útiles para bash scripting.


1. **Subir repositorio o script a GitHub**:

      ```bash
      # Primero: Crear un repositorio en Github
  
      # Segundo: Estando en el directorio donde se encuentra el archivo, ingresar
      git init # Inicializar un repositorio Git en vació.
      git add <dirección_del_script> # Añadir el script en en el repositorio Git.
      git commit -n <”Descripción de herramienta”> # Añadirá una descripción al commit.
      git remote add origin <dirección URL del repositorio en github> # Añadirá el script al repositorio creado en GitHub.
      git push -u origin master # Ingresar usuario y contraseña de GitHub.
      ```
    
    
2. **Manejar archivos con nombres especiales**: 
    
    Archivos con “-” al inicio:
    
    ```bash
    # Crear el archivo
    touch -- -file_name
    
    # Eliminar el archivo
    rm -- -file_name
    
    # Abrir el archivo
    cat < -file_name
    cat ./-
    cat /home/path/-file_name
    $(pwd)/-file_name
    ```
    
    Archivos ocultos:
    
    ```bash
    # Crear el archivo
    touch .hidden_file
    
    # Ver archivos ocultos
    ls -al
    ls -a
    find .
    ```
    
    Archivos con espacios:
    
    ```bash
    cat "spaces in this file"
    cat spaces\ in\ this\ file
    copy "c:\my file name" "d:\my new file name"
    ```
    
3. **Formas de aplicar filtros en archivos**:
    
    ```bash
    1. [GREP]
    		cat data.txt | grep "millionth"
    		grep "millionth" data.txt
    
    		#Mostrar líneas por debajo de un string:
    		grep "Name" -A <number>
    
    		#Mostrar líneas por encima de un string:
    		grep "Name" -B <number>
    
    		#Mostrar líneas por encima y debajo de un string:
    		grep "Name" -C <number>
    
    		#Eliminar un string:
    		cat data.txt | grep -v "Name"
    
    		#Eliminar multiples string:
    		cat data.txt | grep -v -E "Name|Surname"
    
    		#Filtrar string que coincidan con el inicio del mismo:
    		grep "**^Name**" data.txt
    
    		#Filtrar string que coincida con el final del mismo:
    		grep "Name$" data.txt
    
    		#Mostrar el número de línea del string especificado:
    		grep -n "Name" data.txt
    
    		#Filtrar en modo case insensitive:
    		grep -i "Name" data.txt
    
    		#Filtrar un dígito con un rango de dígitos mínimos y máximos:
    		cat <file> | grep -oP '**\d{1,3}**'
    		cat <file> | grep -oP '**\d{2,5}**'
    
    		#Filtrar cierta cantidad de caracteres:
    		cat <file> | grep -E "^.{3}$"
    
    		#Filtrar string que contengan cualquier cadena después o antes de él:
    		cat <file> | grep "a.*"
    
    		#Resetear punto de filtrado con \K
    		cat <file> | grep -oP 'text\K[^.*]+'
    
    2. [AWK]
    		cat data.txt | awk '/millionth/'
    		awk '/millionth/' data.txt
    
    		#Filtrar argumento:
    		cat data.txt | awk '{print $1}'
    		cat data.txt | awk '{print $2}'
    			#Filtrar argumento y especificar un criterio delimitador:
    			cat data.txt | awk '{print 1}' FS=":"
    			cat data.txt | awk '{print 2}' FS="-"
    			cat data.txt | awk -F':' '{print 3}'
    
    		#Filtrar string y el último argumento:
    		awk '/millionth/' data.txt | awk 'NF{print $NF}'
    
    		#Mostrar un número de línea en específico:
    		cat data.txt | awk 'NR==2'
    		awk 'NR==2' data.txt
    
    		#Filtrar string de cierta cantidad de caracteres:
    		cat data.txt | grep "test" | awk 'length($1)>10'
    
    		#Filtrar líneas impares
    		awk 'NR%2==1' data.txt
    		cat data.txt | awk 'NR%2==1'
    	
    		#Filtrar la última línea
    		cat data.txt | awk 'END {print}'
    
    		#Meter una variable en el comando awk
    		variable=$(cat data.txt | wc -l) | awk -v n=$variable 'NR==n'
    		awk -v n=$variable
    		
    
    4. [TR]
    		#Reemplazar string en un archivo:
    		tr '<string>' '<NEWstring>'
    		cat data.txt | tr ' ' '_'
    			#Remplazar string por salto de línea
    			cat data.txt | tr 'string' '\n'
    
    		#Eliminar string:
    		tr -d '<string>'
    
    5. [SED]
    		#Reemplazar string en un archivo de forma general:
    		cat data.txt | sed '/<string>/<newstring>/g'
    
    		#Eliminar todo lo que empiece por un espacio hasa el final:
    		cat data.txt | sed '/^\s*$/d'
    
    		#Convertir un string en retorno de carro
    		cat data.txt | sed '/,/\r/g'
    
    		#Eliminar la última línea del archivo
    		cat data.txt | sed '$d'
    
    		#Sustituir una línea en específico
    		cat data.txt | sed '1s/linea1/Como/'
    		cat data.txt | sed '2s/linea2/estas/'
    
    		#Especificar multiples comandos sed con ";"
    		cat data.txt | sed '1s/Hola/Hi;2s/Esto/This/'
    
    		#Desactivar la impresión automática de cada línea procesada
    		cat data.txt | sed -n '1p;2p;3p'
    
    		#Mostrar líneas específicas del patrón de espacio de sed
    		cat data.txt | sed '1p;2p;3p;5p'
    
    6. [CUT]
    		#Tomar un argumento estableciendo un delimitador
    		cat file | cut -d ' ' -f 1
    		cat file | cut -d ' ' -f1,2,3
    
    7. [HEAD]
    		#Mostrar las primeras dos líneas:
    		head -n 2
    		cat data.txt | head -n 2
    
    		#Eliminar las últimas dos líneas:
    		head -n -2
    		cat data.txt | head -n -2
    		cat data.txt | head -2
    
    8. [TAIL]
    		#Mostrar las últmas dos líneas:
    		tail -n 2 data.txt
    		cat data.txt | tail -n 2
    
    9. [REV]
    		# Mostrar el contenido de forma inversa:
    		cat <file> | rev
    		rev <file>
    
    10. [SORT]
    		#Ordenar resultados alfabeticamente
    		cat <file> | sort
    
    		#Eliminar elementos repetidos:
    		cat <file> | sort -u
    
    		# Ordenar elementos numéricos (ascendente)
    		cat <file> | sort -n
    
    		# Ordenar elementos numéricos (descendente)
    		cat <file> | sort -nr
    
    		#Ordenar los archivos por orden de tamaño
    		ls | sort -k5 -rn
    
    11. [SPONGE]
    		#Sobreescribir el mismo archivo con el resultado del comando:
    		#[No sirve rescribir el mismo archivo 
    		#con redirectories como > o >>. 
    		#Eje: cat data.txt | grep -v "test" > data.txt]
    		cat data.txt | grep -v "test" | sponge data.txt
    
    12. [TEE]
    		#Crear un archivo con el resultado del comando
    		#[No sirve para sobreescribir archivos]
    		echo "Test" | tee data.txt
    
    13. [PASTE]
    		#Compactar archivos, es decir, unirlos:
    		paste user password
    			#Compactar archivos usando un delimitador de ambos contenidos:
    			paste user password -d ':'
    
    14. [TAC]
    		# Darle vuelta al contenido del archivo:
    		cat data.txt | tac
    
    15. [UNIQ]
    		#Nota: El archivo debe estar "sorted" para que este comando funcione
    
    		# Mostrar cadenas únicas:
    		uniq -u data.txt
    
    		# Mostrar cadenas repetidas:
    		uniq -d data.txt
    
    		# Contabilizar repeticiones de una línea:
    		uniq -c data.txt
    
    16. [STRINGS]
    		# Mostrar cadenas legibles:
    		strings data.txt
    
    		# Mostrar cadenas legibles con un mínimo de caracteres
    		strings -n 5 data.txt
    
    17. [DIFF]
    		# Encontrar diferencias entre archivos ("<" = archivo quitado y ">" = archivo ingresado)
    		diff data.txt data2.txt
    ```
    
4. **Concatenar comandos con xargs**: Permite trabajar sobre el output de un comando anterior:
    
    Ejemplo:
    
    ```bash
    find inhere/ -size 1033c
    ```
    
    Muestra como resultado: “inhere/maybehere07/.file2”.
    
    Ahora se ejecuta el mismo comando pero con un pipe para hacerle un cat
    
    ```bash
    find inhere/ -size 1033c | xargs cat
    ```
    
    Esto dará el contenido del archivo “.file2”.
    
    Este comando también permite formatear un output que contenga saltos de línea o espacios:
    
    ```bash
    find inhere/ -type f -size 1033c | xargs cat | xargs
    ```
    
5. **Poner procesos en segundo plano**: En bash se pueden poner procesos en segundo plano agregando un “&” al final del comando:
    
    ```bash
    firefox > /dev/null 2>&1 &
    ```
    
    Los procesos pueden tomar roles que dependen de otros, por ejemplo, al ejecutar en segundo plano **`firefox &`** este se convierte en un proceso “hijo” del proceso “padre” que sería la consola, por lo que si se cierra la consola (padre), se cierra también el firefox (hijo) ya que este último depende del otro. Existe un comando que independiza al proceso hijo del proceso padre: **`disown -a`** o **`disown`**, de esta forma se independizan las tareas que se ejecutan en segundo plano y no se cerrarán al cerrar el proceso padre.
    
6. **Crear “one-liner” sin hacer un archivo script**: 
    
    Ejemplo:
    
    ```bash
    contador=1; cat data.txt | base64 -d | while read line; do echo "Esta es el número de línea de la contraseña:" $contador: $line; let contador+=1 ; done | sed -e 's/The password is//’
    ```
    
7. **Decodificar y codificar base64**:
    
    ```bash
    base64 file.txt # Codifica el archivo a base64
    echo "Hola, soy Bryan" | base64 # Codifica la cadena a base64
    base64 -d "SG9sYSwgc295IEJyeWFu" # Decodifica la cadena
    ```
    
8. **Codificar y decodificar ROT13**:
    
    ```bash
    # Codificar Palabra a codificar "Hola"
    echo “Hola” | tr '[a-zA-Z]' '[n-za-mN-ZA-M]'
    echo “Hola” | tr '[a-z][A-Z]' '[n-za-m][N-ZA-M]'
    
    # Decodificar
    echo “Ubyn” | tr '[n-za-mN-ZA-M]' '[a-zA-Z]'
    echo “Ubyn" | tr '[A-Za-z]' '[N-Zn-z]'
    ```
    
9. **Convertir las mayúsculas a minúsculas y viceversa**: 
    
    ```bash
    # Convertir "hola" en "HOLA".
    echo "hola" | tr '[a-z]' '[A-Z]'
    ```
    
10. **Descomprimir archivos**:
    
    **`7z`**: Con 7z es posible descomprimir todo tipo de comprimidos, ya que es universal. Antes de descomprimir un archivo, puedo ver cuál será el archivo resultado o descomprimido con **`7z l data.gzip`**esto me listará los archivos que me dará el archivo comprimido antes de descomprimirlo y para descomprimirlo: **`7z x data.gzip`.**
    
    **`gzip -d`**: El comando "gzip -d" en Linux se utiliza para descomprimir archivos que han sido comprimidos previamente con el comando "gzip". El comando "gzip" utiliza el algoritmo de compresión de Lempel-Ziv-Markov (LZ77) para reducir el tamaño de los archivos, mientras que "gzip -d" se utiliza para descomprimir esos archivos comprimidos y recuperar su tamaño original.
    
    El parámetro "-d" es una opción o "flag" que se utiliza con el comando "gzip" en Linux. Cuando se utiliza "-d", indica al comando "gzip" que descomprima el archivo especificado en lugar de comprimirlo. En otras palabras, "-d" significa "descomprimir" y es utilizado para recuperar el archivo original desde un archivo comprimido con gzip. Es importante mencionar que gzip solo puede descomprimir archivos comprimidos con gzip, no con otros formatos de compresión.
    
    **`zcat`**: Muestra el contenido de los archivos GZIP.
    
    **`gunzip`**: El comando "gunzip" es una alternativa al comando "gzip -d" y sirve para descomprimir archivos que han sido comprimidos previamente con el comando "gzip". Es decir, "gunzip" es una forma más corta y sencilla de escribir "gzip -d" y tiene el mismo propósito. La principal diferencia entre los dos comandos es que "gunzip" es más fácil de escribir y recordar, mientras que "gzip -d" es más preciso. Ambos comandos son equivalentes y puedes usar el que te sea más cómodo.
    
    **`unzip`** "unzip" es una herramienta de línea de comando utilizada para descomprimir archivos en formato "zip". Es compatible con los formatos de compresión más comunes, como Deflate y PKWARE. El comando "unzip" se utiliza para extraer el contenido de un archivo zip, permitiendo acceder a los archivos y carpetas dentro de él. Por ejemplo, el comando "unzip file.zip”
    
    El parámetro "-d" es utilizado con el comando "unzip" para especificar el directorio en el que se descomprimirán los archivos del archivo zip.
    
11. **Los lenguajes booleanos “&&” (and) y “||” (or)**: Funcionan de la siguiente manera:
    
    ```bash
    whoami && ls # (AND) El comando "ls" funcionará solo si el comando whoami es correcto
    whoami -a ls # (AND) El comando "ls" funcionará solo si el comando whoami es correcto
    whoami || ls # (OR) Funcionará si alguno de los comandos es correcto
    ```
    
12. **Mostrar ruta por la que inició un proceso**:
    
    ```bash
    pwdx <PID>
    ```
    
13. **Mostrar comandos en ejecución**:
    
    ```bash
    ps -eo command # Muestra todos los comandos ejecutados
    ps -eo user,command # Muestra el nombre del usuario el comando que ejecutó
    ```
    
14. **Netcat**: Sirve para gestionar conexiones de red por TCP o UDP.
    
    ```bash
    nc -nlvp <portnumber> # Sirve para poner un puerto en escucha
    ```
    
15. **Ejecutar un comando cada cierto tiempo**:
    
    ```bash
    watch -n 5 whoami # Ejecutará whoami cada 5 segundos
    ```
    
16. **Bucle “while” con código de estado**:
    
    Este script se ejecutará hasta que el código de estado sea “0”:
    
    ```bash
    false; while [ $(echo $?) != "0" ]; do cat /free/* ; done 2>/dev/null
    ```
    
    Este script de bash ejecuta un bucle "while" que seguirá ejecutándose mientras el resultado de la salida del comando "echo $?" sea diferente de "0". Dentro del bucle, se ejecuta el comando "cat" para mostrar el contenido de todos los archivos en el directorio "/free/". El comando seguido de "2>/dev/null" redirige toda la salida de error a "/dev/null" para evitar mostrar mensajes de error en la salida.
    
    El comando "false" es utilizado para forzar una salida de error en el script. La primera línea "false" se ejecuta antes del bucle while y su salida es el código de error 1, lo que hace que la condición en la línea del bucle while se cumpla y el bucle se ejecute. Sin la línea "false", el bucle no haría nada ya que nunca se cumpliría la condición del bucle ya que no habría una salida previa que pudiera ser evaluada.
    
17. **Crear archivos temporales**:
    
    ```bash
    mktemp # Crear un archivo temporal
    mktemp -d # Crear un directorio temporal
    ```
    
18. **Crear una lista de números**:
    
    ```bash
    for i in {0000..9999}; do echo "Número $i"; done
    ```
    
    Esto mostrará todos los números que van del 0000 hasta el 9999.
    
19. **Comandos GIT**:
    
    ```bash
    git log -p # Ver los cambios que se hicieron en los repositorios de github.
    git branch # Ver el branch en el que se está actualmente.
    git branch -r # Listará todos los branchs disponibles.
    git checkout <branch_name> # Cambiará al branch especificado.
    git tag # Listar las tags del proyecto git.
    git show <tag_name> # Ver el contenido de la tag especificada.
    git add <file_name> # Añadir archivo al proyecto git.
    										# Luego se hace un git commit -m ”Frase explicativa” para 
    										# subir los proyectos agregados al proyecto.
    git push -u origin master # Se usa para cargar contenido del repositorio local a un repositorio remoto.
    ```
    
    El archivo .gitignore sirve para ignorar el agregado de cualquier tipo de archivo al proyecto según lo especificado en su contenido.
    
20. **Mostrar el tipo de shell que se está ejecutando**:
    
    ```bash
    echo $0
    $0 # Abrirá una shell
    ```
    
21. **Mostrar el nombre del fichero de la terminal de entrada estándar**:
    
    ```bash
    tty
    ```
    

22. **Comandos de configuración de nanorc**:
    
    ```bash
    set tabsize <value>
    	# Ejemplo
    	set tabsize 4
    set autoindent # Sirve para que se auto indente cuando se haga un salto de línea.
    set smooth # Esto sirve para que al bajar, no se haga un AV pág (avanzar o retroceder una página completa) sino se forma “suave” o despacio.
    set linenumbers # Mostrará los números de línea.
    ```
    
23. **Controlar el flujo del programa al hacer “Ctrl+C”**:
    
    ```bash
    function ctrl_c(){
    	echo -e "\n${redColour}[!] Exiting...\n${endColour}”
    }
    
    trap ctrl_c INT
    ```
    
24. **Colores en el script**: Para utilizar colores, primero se crean las dos variables con caracteres especiales para colores y para utilizarlos, se sigue la estructura “{redName}Texto{endColour}” esto para llamar a la variable del mismo nombre que contiene la combinación de colores para rojo”, luego se le debe especificar el “endColour” al final del texto.
    
    ```bash
    redColour="\e[0;31m\033[1m”
    endColour="\033[0m\e[0m”
    
    function ctrl_c(){
    	echo -e "\n${redColour}[!] Exiting...\n${endColour}”
    ```
    
25. **Ocultar y mostrar cursos**:
    
    ```bash
    tput civis # Ocultar el cursos
    tput cnorm # Mostrar el cursor
    ```
    
26. Establecer un delay:
    
    ```bash
    sleep 10 # Espera 10 segundos antes de seguir con la ejecución del programa
    ```
    
27. **Estructura GETOPTS**:
    
    ```bash
    while "ab:cd:" arg; do # Los dos puntos ":" indican que el parámetro tomará argumentos
    	case $arg in
    		a) functionName;; # Se pueden llamar funciones
    		b) let var1+=1;; # Se puede cambiar el valor de variables
    		c) var2="$OPTARG";; # OPTARG es una función especial que toma el argumento dado
    											  # después del parámetro. eje: ./test.sh -c argumento1
    		d) ;; # Se puede dejar vacío
    	esac
    done
    ```
    
    Ejemplo #2:
    
    ```bash
    parameter_counter=0; while getopts “e:h:” arg; do
    	case $arg in
    		e) exploration_mode=$OPTARG; let parameter_counter+=1;
    		h) helpPanel;;
    	esac
    done
    
    if [ $parameter_counter -eq 0 ]: then
    	helpPanel
    fi
    ```
    
28. **Eliminar saltos de línea**: El parámetro -n indica que no se quieren hacer saltos de línea, sino mostrar en la misma línea.
    
    Eje:
    
    ```bash
    for i in $(seq 1 80); do echo -n “?”
    ```
    
    Esto nos mostrará ochenta signos “?” en la misma línea.
    
29. **Comando CURL**: El comando **`curl`** sirve para enviar una solicitud HTTP a una URL y recibir un resultado
    
    **`-s`:** Silencia u oculta el “verbose” creado por curl en un resultado.
    
    El comando **`curl -XPUT <URL>`** enviaría una solicitud PUT HTTP a la URL especificada. PUT es un método HTTP que se utiliza para actualizar recursos existentes en un servidor. La solicitud PUT incluiría los datos que se utilizarían para actualizar el recurso especificado en la URL. Es importante mencionar que el recurso en la URL debe existir previamente, ya que el método PUT se utiliza para actualizar y no crear recursos.
    
    Cuando utilizas el comando **`curl -XPUT <URL>`**, puedes incluir datos en la solicitud PUT mediante varias opciones. Algunas de las opciones comunes incluyen:
    
    - Utilizando el parámetro “d” o “-data” seguido de una cadena de datos, por ejemplo, **`curl -XPUT <URL> -d '{"nombre":"nuevo nombre"}'`**
    - Utilizando el parámetro “d” o “-data” seguido de un archivo, por ejemplo, **`curl -XPUT <URL> --data @data.json`**
    - Utilizando el parámetro “H” o “-header” para agregar cabeceras personalizadas a la solicitud, por ejemplo, **`curl -XPUT <URL> -H "Content-Type: application/json" -d '{"nombre":"nuevo nombre"}'`**
        
        Enviar una solicitud PUT vacía podría tener diferentes resultados dependiendo de la implementación del servidor al que se está enviando la solicitud.
        
        En algunos casos, el servidor podría interpretar una solicitud PUT vacía como una petición para actualizar un recurso con datos vacíos o nulos, eliminando cualquier información previamente almacenada en el recurso.
        
        En otros casos, el servidor podría rechazar la solicitud PUT vacía con un código de respuesta HTTP 4xx o 5xx, ya que espera recibir datos específicos para actualizar un recurso.
        
        Es importante tener en cuenta que, antes de enviar una solicitud PUT vacía, debes comprender cómo se espera que el servidor maneje esa situación y si es lo que realmente se desea.
        
    
    **`curl -A <URL>`**: Es una solicitud HTTP GET a la URL especificada. La opción -A o --user-agent permite enviar una etiqueta de agente de usuario específica en la solicitud HTTP. Una etiqueta de agente de usuario es una cadena de texto que se envía en el encabezado de una solicitud HTTP que proporciona información sobre el cliente que está realizando la solicitud. Puede incluir información como el nombre del navegador, la versión del sistema operativo, el dispositivo, entre otros. El servidor utiliza esta información para proporcionar contenido específico para el cliente que está solicitando. Por ejemplo, puede enviar una versión móvil de un sitio web si detecta que la solicitud proviene de un dispositivo móvil.
    
    Diferencia de “curl -A” y “curl -H”:
    
    El comando **`curl -A veronica <URL>`** es equivalente a **`curl -H 'User-Agent: veronica' <URL>`**.
    
    El parámetro -A o --user-agent permite establecer una etiqueta de agente de usuario específica, que se enviará en el encabezado de la solicitud HTTP, especificando el nombre de agente de usuario "veronica".
    
    Por otro lado, el parámetro -H o --header permite enviar un encabezado personalizado en la solicitud HTTP, especificando el encabezado "User-Agent" y el valor "veronica"
    
    En ambos casos, el resultado es el mismo, el servidor recibirá una etiqueta de agente de usuario "veronica" en la solicitud HTTP.
    
30. **Modo “pagined”**: 
    
    ```bash
    cat file.txt | less -S
    ```
    
31. **Muestra el HTML en formato de texto**:
    
    ```bash
    html2text
    ```
    
32. **Deshabilitar auto-indentado**: Esc + i.
    
    
33. **Crear tablas**: La siguiente dirección contiene código necesario para formatear información en tablas (desde la línea 66 hasta la 152): [https://github.com/s4vitar/htbExplorer/blob/master/htbExplorer](https://github.com/s4vitar/htbExplorer/blob/master/htbExplorer)
    
    A la hora de crear filas, se debe especificar “_” para indicar hasta dónde llega la fila
    
34. **Pasar variables a funciones**:
    
    ```bash
    var1=2
    
    function example(){
    	var1=$1
    	echo $((var1+2))
    }
    
    example $var1
    ```
    
35. **Crear una sumatoria de cada línea de un archivo**: Se crea una variable o contador y se establece en 0 en este caso “totalMoney”, luego se le hace un cat para mostrar el contenido. El siguiente script lo que hará es que mientras lee cada línea del archivo “money” (1), deja que el contador se sume con cada línea del archivo (2), luego el total del contador lo mete en un archivo, en este caso “money.tmp” (3).
    
    
    ```bash
    (1) totalMoney=0; cat money | while read money_in_line; do
    (2)      let totalMoney+=$money_in_line
    (3)      echo $totalMoney > money.tmp
          done; cat money.tmp
    ```
    
36. **Representar números con espacios**: **`printf “%’d.\n” <número>`** Esto muestra el número separado. Otra alternativa es: **`echo "\$$(printf "%'d\n" $(cat money.tmp))”`**Es lo mismo que el primero solo que esta vez está tomando el valor desde un fichero.

37. **Controlar el id de usuario**: Al ejecutar **`id -u`**, indica el usuario que se está usando: “0” corresponde a usuario Root y “1000” corresponde a un usuario normal.
    
    
38. **Paleta de colores para bash**:
    
    ```bash
    #Colours
    greenColour="\e[0;32m\033[1m"
    endColour="\033[0m\e[0m"
    redColour="\e[0;31m\033[1m"
    blueColour="\e[0;34m\033[1m"
    yellowColour="\e[0;33m\033[1m"
    purpleColour="\e[0;35m\033[1m"
    turquoiseColour="\e[0;36m\033[1m"
    grayColour="\e[0;37m\033[1m"
    ```
    
39. **Optimización de getopts con declare -i**: Al usar getopts, se puede controlar el flujo del script con una variable establecida en cero, eje: (parameter_counter=0; while getopts “:a:h:” arg; do), esto a nivel de reserva de espacio en memoria, se puede optimizar la forma en la que almacena ese “0” de la siguiente manera: (declare -i parameter_counter=0; while getopts “:a:h:” arg; do), ya que al declararle que es un número entero (-i), estos ocupan menos espacios en memoria.
    
    
40. **Terminar una función con un std específico:** Se puede especificar el tipo de std que devolverá una función en un script poniéndole al final un “exit <#std>”, eje “exit 2” esto terminará la función con un stderr.
    
    
41. **Quitar el modo interactivo de algún programa durante su instalación**: **`export DEBIAN_FRONTEND=noninteractive`**al hacer esto, responderá con las respuestas puestas por default.
    
    
42. **Herramienta para establecer una interfaz gráfica**: **`whiptail`.**
    
    
43. **Verificar ruta absoluta de un programa**: **`which <program_name>`**, mostrará la ubicación del programa ejecutable. La alternativa es usar el comando **`command -v <program_name>`**
    
    
44. **Verificar dependencias para el programa**:
    
    Con condicionales: Utilizando condicionales junto con el comando “which”, por ejemplo: se le puede decir al programa que si el comando “which <program_name>” devuelve un resultado, se comprobaría que el programa está instalado, sin embargo, existen otras opciones más óptimas.
    
    Con vectores: Utilizando un vector, eje : dependencies=(aircrack-ng macchanger) se crea una “variable” (dependencies) y se le inserta como “valor” los dos programas que se quieren comprobar (aircrack-ng y macchanger), esto posteriormente, permite ir recorriendo cada elemento del vector en bucle.
    
    Lo que hará el siguiente script es que irá iterando en cada uno de los elementos en la variable “dependencies” (1), cada uno de los valores se irá almacenando en la variable “program” y los mostrará en pantalla (2). El arroba ([@]) es la forma en la que se le dice al programa que recorra todos los elementos de la variable#
    
    Ejemplo de cómo se hace un recorrido por el vector establecido:
    
    ```bash
    dependencies=(aircrack-ng macchanger)
    (1) for program in ${dependencies[@]}; do
     (2) echo $program
    		done
    ```
    
45. **Verificar acceso a un archivo**: **`test -f`** esto verificará el acceso del archivo y devolverá un bit de estado 0 (el archivo existe) o 1 (el archivo no existe), eje: **`test -f /usr/bin/aircrack-ng`.**
    
    
46. **Ocultar todo el flujo de algún comando (stdin) al dev/null**, eje: **`apt install macchanger > /dev/null`**adicionalmente, los errores se pueden convertir en stdin para que vayan al /dev/null, eje: 2>&1.
    
    
47. **File WifiHack.sh is being edited (by root with nano 2.4.2, PID xxxx); continue?**
    
    Esto se puede deber a que puede haber algún proceso utilizando el archivo, se puede verificar usando ps o htop.
    
    Se puede deber a que hay un archivo oculto en el mismo directorio, eje: .WifiHack.sh.swp Esto se soluciona eliminando el archivo.
    
48. **Ejecutar un comando con un intervalo de tiempo específico**: **`timeout 5 bash -c "ping google.com"`**, esto ejecutará un ping de 5 segundos a google.com.
    
    
49. **Hostear un comando en una nueva ventana**: **`xterm hold -e airodump-ng wlan0`**airodump-ng se ejecutará en nueva ventana.
    
    
50. Ejecutar un comando en segundo plano: **`firefox &`** Al agregar “&” ejecutará el programa firefox en segundo plano.
    
    
51. **Hacer que el usuario ingrese strings a una variable**, eje:
    
    Se establece una variable llamada “Nombre” el cual su valor será ingresado por el usuario al ejecutarse el script, posteriormente, se mostrará en el stdout.
    
    Se puede usar el parámetro -r para admitir espacios, eje read -r Nombre
    
    ```bash
    echo “Ingrese su nombre: ”read Nombre
    echo “Su nombre es: $Nombre”
    ```
    
52. **Obtener el PID del programa en segundo plano ejecutado**: Ejecutar el programa en segundo plano y crear una variable que almacene su PID: process_PID=$!. #La variable solo capturará PID del proceso en segundo plano#
    
    
53. **Hacer que un comando espere la conclusión de otro comando para ejecutarse**: wait firefox El programa esperará la conclusión de firefox para ejecutar el siguiente comando especificado.
    
    
54. **Establecer una variable en modo solo lectura**: Esto sirve para optimizar los recursos utilizados y se declara de la siguiente manera: declare -r my_variable=<variable_value>
    
    
55. **Crear arrays**:
    
    declarar un array: declare -a my_variable
    
    Ingresar un valor a un array: my_variable+=($path) donde $path es una variable que contiene un valor. Esto ingresará el valor de $path en “my_variable”.
    
    Contabilizar los elementos de un array: echo ${#my_variable[@]} o echo ${#my_variable[*]}
    
    El “@”: Iterará sobre cada elemento.
    
    El “*”: englobará todos los elementos.
    
56. **Utilizar una condicional para validar la existencia de un comando con “-x”**: if [ -x /usr/bin/whoami ]; then echo “Existe”; else echo “No existe”; fi
    
    
57. El parámetro “break” se utiliza para **salir de cualquier ciclo o bucle**.
    
    
58. **Trazas de depuración (debugging)**: También puede ser útil habilitar las trazas de depuración para obtener más información sobre el error. Para habilitar las trazas de depuración, añade el siguiente código al principio del script: set -x
    
    Al ejecutar el script de nuevo, se verá cómo se imprimen todas las líneas de código ejecutadas y sus respectivos valores de retorno. Esto puede ayudar a identificar el problema más fácilmente.
    

59. **Mostrar las variables de entorno**: **`printenv`**
    
    
60. **“Crackear” una ruta para descubrir un archivo**:
    
    Situación: La usuaria isabel ha dejado su password en un fichero en la carpeta /etc/xdg pero no recuerda el nombre, sin embargo tiene dict.txt que puede ayudarle a recordar.
    
    El archivo dict.txt contiene una lista con muchos nombres de archivos posibles en los cuales uno contiene la contraseña de isabel.
    
    Scripts que se usará: `while HI= read -r line; do readlink -e /etc/xdg/$line ; done <` dict.txt
    
    Este script de bash lee cada línea de un archivo llamado "dict.txt" y utiliza cada línea como un argumento para el comando "readlink -e /etc/xdg/". El comando "readlink -e" se utiliza para seguir un enlace simbólico y devolver la ruta absoluta del archivo al que apunta. En este caso, se está utilizando la variable "line" para especificar un archivo dentro del directorio "/etc/xdg/" y el comando "readlink -e" se utiliza para seguir el enlace simbólico y devolver la ruta absoluta del archivo al que apunta. El bucle while se utiliza para repetir este proceso para cada línea del archivo "dict.txt".
    
    El comando "read -r" se utiliza en Bash para leer una línea de un archivo o una entrada de un canal de comunicación y almacenar el resultado en una variable. La opción "-r" se utiliza para evitar que se interpreten caracteres especiales en la línea leída, como el carácter de nueva línea. Sin esta opción, los caracteres especiales se interpretarían y podrían causar problemas en el script.
    
    En este script en particular, se esta utilizando para leer cada línea del archivo dict.txt y almacenando en una variable llamada "line" y esta se usa como argumento para el comando "readlink -e /etc/xdg/$line".
    
    El parámetro "-e" de readlink es utilizado para mostrar el enlace simbólico original en lugar de seguirlo. Es decir, si el enlace simbólico apunta a otro enlace simbólico, con el parámetro -e se mostrará el primer enlace simbólico y no se seguirá el enlace a otro enlace.
    Sin el parámetro -e, si el enlace simbólico apunta a otro enlace simbólico, readlink mostraría la ruta del archivo o directorio al que apunta el segundo enlace simbólico.
    
    En resumen, el parámetro -e permite obtener la ruta del enlace simbólico en sí, no la ruta del archivo o directorio al que apunta.
    
61. **Bruteforce al loging de super-usuario**:
    
    El siguiente script utiliza un diccionario para intentar descifrar la contraseñe de un usuario logueandose con el comando “su”, eje: **`su bryan`**, Password: 123.
    
    **`while read -r line; do echo $line | timeout 5 su lola 2>/dev/null; if [ $(echo $?) == "0" ]; then echo $line; break; fi; done < /tmp/passwd.txt`**
    
    Este código es un script de shell que lee cada línea de un archivo llamado "/tmp/passwd.txt" y ejecuta un comando en cada línea. El comando es "timeout 5 su lola 2>/dev/null". El "timeout 5" significa que el comando se detendrá después de 5 segundos si no se ha completado. El "su lola" significa que el comando se ejecutará como el usuario "lola". El "2>/dev/null" significa que se redirigirán todos los errores del comando a /dev/null (el "tubo de la basura" del sistema). Si el comando se ejecuta con éxito (es decir, si el código de salida es 0), entonces se imprimirá la línea del archivo y se detendrá el script.
    
62. **Crackear el logueo por ssh**: El siguiente comando intentará crackear el logueo del ssh utilizando Hydra: **`hydra -l <remote_user> -P <wordlist> ssh://<Domain/IP>:<port>`**
    
    
63. **Operadores de redirección “<” y “<<<”**: 
    
    La diferencia entre el operador "<" y "<<<" es la forma en que se utilizan para pasar entrada a un comando o script.
    
    El operador "<" se utiliza para redirigir la entrada de un archivo a un comando o script. Por ejemplo, en el script anterior se utiliza "done < "$file"" para redirigir la entrada del archivo específico que se esta recorriendo en ese momento dentro del bucle for al bucle while.
    
    Por otro lado, el operador "<<<" se utiliza para pasar una cadena de texto como entrada a un comando o script. Por ejemplo, en el script anterior se utiliza "done <<< "$file_content"" para pasar el contenido de la variable "file_content" como entrada al bucle while.
    En resumen, el operador "<" se utiliza para redirigir la entrada de un archivo a un comando o script, mientras que el operador "<<<" se utiliza para pasar una cadena de texto como entrada a un comando o script.
    
    eje:
    
    ```bash
    for file in file1.txt file2.txt file3.txt; do
    	file_content=$(<"$file")
    	while IFS= read -r line; do readlink -e /etc/xdg/"$line"
    	done <<< "$file_content"
    done
    ```
    
64. **Ejercicio: Mostrar valores impares del 1 al 99**: **`for i=1; i≤100; i+=2; do echo $i; done`**

	- La primera expresión entre paréntesis **`((i=1;`** establece el valor inicial de la variable "i" en 1.
	- La segunda expresión **`i<100;`** es la condición de parada del bucle, en este caso, el bucle se ejecutará mientras "i" sea menor a 100.
	- La tercera expresión **`i+=2))`** es el paso que se da en cada iteración, en este caso se le suma 2 a "i" en cada iteración
	- El comando **`echo $i`** imprime el valor de "i" en cada iteración
	- El "done" al final indica el fin del bucle.

		En resumen, este código imprimirá todos los números impares desde 1 hasta 99.

		El uso de doble paréntesis en este caso es para indicar que se está utilizando una sintaxis especial de Bash llamada "arithmetic expansion". Esta sintaxis permite realizar operaciones aritméticas dentro del bucle "for" utilizando una sintaxis similar a la utilizada en otros lenguajes de programación como C o Java.

		Sin esta sintaxis, se tendría que hacer uso de las funciones aritméticas de Bash y se vería como : **`for i in {1..99..2}; do echo $i; done`** la sintaxis de doble paréntesis es más legible y fácil de entender para aquellos que estén familiarizados con otros lenguajes de programación.

		¿Por qué “+=2” y no “+2”?

		En el código anterior, **`i+=2`** se utiliza para incrementar el valor de la variable "i" en 2 unidades en cada iteración del bucle, mientras que si se utilizara **`i+2`**, se estaría creando una nueva variable con el valor de "i" más 2 y no se estaría incrementando el valor de "i" en sí.

		La notación **`+=`** es una abreviación de "incrementar en" o "sumarle a", mientras que **`+`** es simplemente una operación matemática de suma.
		En resumen, **`i+=2`** es utilizado para incrementar el valor actual de "i" en 2, mientras que **`i+2`** es utilizado para obtener un nuevo valor que es 2 unidades mayor que el valor actual de "i"

		**Establecer dos variables dentro del bucle “for” y “while”**:

		FOR

		Es posible establecer dos o más variables dentro de un bucle "for" en Bash utilizando la sintaxis de "arithmetic expansion". Por ejemplo:

		```bash
		for ((i=1, j=10; i<=5; i++, j--)) do
			echo "i: $i, j: $j"
		done
		```

		En este ejemplo, se establecen dos variables, "i" y "j", en el inicio del bucle, y se les asigna un valor inicial respectivo. Luego, en cada iteración del bucle, se incrementa el valor de "i" en 1 y se decrementa el valor de "j" en 1, imprimiendo ambos valores en cada iteración.

		Es importante mencionar que las variables deben ser separadas por comas y las operaciones que se realizan en cada iteración del bucle también deben estar separadas por comas.

		Si desea incrementar ambas variables en 1 unidad en cada iteración del bucle, puede hacerlo de la siguiente manera:

		```bash
		for ((i=1, j=10; i<=5; i++, j++)) do
			echo "i: $i, j: $j"
		done
		```

		En este ejemplo se establece dos variables "i" y "j" con un valor inicial respectivo, y en cada iteración se le suma 1 a ambas variables, imprimiendo ambos valores en cada iteración.

		Otra forma de hacerlo sería de esta manera:

		```bash
		for ((i=1; i<=5; i++)) do
			j=$((j+1))
			echo "i: $i, j: $j"
		done
		```

		Aquí, se establece una variable "i" y se incrementa en 1 en cada iteración del bucle, y se establece una variable "j" con un valor inicial y se incrementa en 1 en cada iteración del bucle dentro del cuerpo del mismo.

		En ambos casos se logra incrementar ambas variables en 1 unidad en cada iteración del bucle.

		WHILE

		Para establecer dos o más variables dentro de un bucle "while" en Bash, puede hacerlo utilizando la sintaxis de "arithmetic expansion" y asignándoles un valor inicial antes del bucle.

		```bash
		i=1
		j=10
		while ((i<=5)) do
			echo "i: $i, j: $j"
			((i++))
			((j--))
		done
		```

		En este ejemplo, se establecen dos variables, "i" y "j", con valores iniciales antes del bucle. En cada iteración del bucle, se incrementa el valor de "i" en 1 y se decrementa el valor de "j" en 1 y se imprimen ambos valores en cada iteración.

		También se puede hacerlo de esta manera:

		```bash
		i=1
		j=10
		while [ $i -le 5 ] do
			echo "i: $i, j: $j"
			i=$((i+1))
			j=$((j-1))
		done
		```

		Aquí se establece una variable "i" y "j" con un valor inicial antes del bucle, y en cada iteración del bucle se incrementa el valor de "i" en 1 y se decrementa el valor de "j" en 1 dentro del cuerpo del mismo y se imprimen ambos valores en cada iteración.

		Es importante mencionar que se pueden hacer operaciones aritméticas más complejas en cada iteración dentro del cuerpo del bucle.

		En ambos casos se logra establecer dos variables dentro de un bucle "while" y realizar operaciones con ellas en cada iteración.

65. **Ejemplo de script que toma valores ingresados por el usuario y los use para operaciones aritméticas**:
    
    ```bash
    echo "Ingrese el primer valor: "
    read valor1
    echo "Ingrese el segundo valor: "
    read valor2
    
    suma=$((valor1 + valor2))
    resta=$((valor1 - valor2))
    
    echo "La suma de $valor1 y $valor2 es $suma"
    echo "La resta de $valor1 y $valor2 es $resta"
    ```
    
    Es importante mencionar que si se desea hacer operaciones con decimales se debe utilizar una herramienta como "bc" para poder realizar operaciones con un mayor rango de precisión:
    
    **`echo "La division de $valor1 y $valor2 es $(echo "scale=2;$valor1/$valor2" | bc)”`**
    
    "scale=2;" es una opción utilizada en la herramienta "bc" para establecer el número de decimales que se desea utilizar en las operaciones aritméticas.
    
    Cuando se utiliza "bc" para realizar operaciones con números decimales, el resultado por defecto es un número con un número ilimitado de decimales. Sin embargo, a veces es necesario limitar el número de decimales en el resultado. La opción "scale=2" establece que se desean 2 decimales en el resultado de la operación.
    
    Por ejemplo, si se realiza una división entre dos números y se utiliza "scale=2", el resultado será un número con solo 2 decimales. Sin esta opción, el resultado podría tener un número ilimitado de decimales.
    
    En resumen, "scale=2" es una opción utilizada en bc para establecer el número de decimales que se desea utilizar en las operaciones aritméticas y controlar la precisión del resultado.
    
    **`bc -l`**: "bc -l" es una utilidad para calcular expresiones matemáticas en línea de comandos. La opción "-l" se utiliza para habilitar la compatibilidad con números de punto flotante con precisión ilimitada. Esto permite que bc realice cálculos con números con decimales de manera más precisa. No se utiliza "bc -l" cuando no se requiere de las funciones matemáticas avanzadas, ya que solo se realizan operaciones aritméticas básicas como sumar y dividir. Además, el uso de "bc -l" también aumentaría la complejidad del código. Sin embargo, en caso de requerir operaciones matemáticas más complejas, es recomendable utilizar "bc -l".
    
    Funciones matemáticas avanzadas se refiere como punto decimal, como raíces cuadradas, exponenciales, logaritmos, etc. También pueden ser operaciones matemáticas con números grandes, por ejemplo, con números de varios dígitos. "bc -l" permite realizar estas operaciones con una mayor precisión y es útil en escenarios donde se requieren cálculos más precisos o complejos.
    
66. **Condicionales IF, ELIF, ELSE**: La estructura de control de flujo "if-elif-else" en Bash Scripting se utiliza para ejecutar un conjunto de instrucciones si se cumple una determinada condición. La sintaxis básica es la siguiente:
    
    ```bash
    if [condición]
    then
    	instrucciones
    elif [condición]
     instrucciones
    else
     instrucciones
    fi
    ```
    
    - "if" se utiliza para evaluar una condición. Si la condición se cumple, se ejecutan las instrucciones que se encuentran después de "then"
    - "elif" se utiliza para evaluar una segunda condición en caso que la primera condición evaluada en "if" no se cumpla. Si esta segunda condición se cumple, se ejecutan las instrucciones que se encuentran después de "elif"
    - "else" se utiliza para ejecutar un conjunto de instrucciones si ninguna de las condiciones anteriores se han cumplido.
    - "fi" indica el fin de la estructura de control de flujo "if-elif-else"
    
67. **Operadores de comparación MATEMÁTICOS**: En Bash Scripting, se utilizan los operadores de comparación para comparar valores y determinar si un valor es mayor o menor que otro. A continuación se presentan algunos ejemplos de operadores de comparación:
    - Mayor que (greater than): **`valor1 -gt valor2`**
    - Menor que (less than): **`valor1 -lt valor2`**
    - Mayor o igual que (greater or equal to): **`valor1 -ge valor2`**
    - Menor o igual que (less or equal to): **`valor1 -le valor2`**
    - Igual a (equal to): **`valor1 -eq valor2`**
    - Diferente a (different to): **`valor1 -ne valor2`**
    
    También se pueden utilizar los operadores de comparación "==", ">", "<" para comparar valores. Sin embargo, es importante mencionar que el uso de estos operadores es menos común y no es recomendado, ya que pueden causar problemas en algunas situaciones y es mejor utilizar los operadores mencionados anteriormente.
    
    Es importante mencionar que el uso de los operadores "==", ">", "<" es menos común y no es recomendado, ya que pueden causar problemas en algunas situaciones, como por ejemplo, cuando se utilizan como operadores de comparación en una expresión dentro de una secuencia de comandos dentro de los corchetes "[]", ya que Bash interpreta estos operadores como operadores de redirección. Por esta razón, se recomienda usar los operadores de comparación mencionados anteriormente, ya que son más claros y menos propensos a causar problemas.
    
    Además, es importante mencionar que al utilizar estos operadores, es necesario utilizar espacios en blanco alrededor de ellos, de lo contrario, pueden generar errores en el script. como por ejemplo: if [ $valor1>$valor2 ] # sin espacios en blanco, causará un error
    
    En resumen, es posible utilizar los operadores "==", ">", "<" en Bash Scripting para comparar valores, pero se recomienda utilizar los operadores de comparación "-" mencionados anteriormente para evitar problemas y tener un código más legible y fácil de entender.
    

68. **Operadores de comparación de CADENA o STRING**: 
    
    En Bash Scripting, para comparar strings se utilizan los mismos operadores de comparación que se utilizan para comparar números, pero en lugar de usar los operadores matemáticos, se utilizan los operadores de comparación de cadenas. A continuación se presentan algunos ejemplos de operadores de comparación de cadenas:
    
    - Igual a: **`cadena1 = cadena2`** o **`cadena1 == cadena2`**
    - Diferente a: **`cadena1 != cadena2`**
    - Vacío: **`-z cadena`**
    - No vacío: **`-n cadena`**
    
    Ejemplo #1:
    
    ```bash
    cadena1="Hola"
    cadena2="Mundo"
    
    if [ $cadena1 = $cadena2 ]
    then
    	echo "Las cadenas son iguales"
    else
    	echo "Las cadenas son diferentes"
    
    fi
    ```
    
    Ejemplo #2
    
    ```bash
    cadena=" "
    
    if [ -z $cadena ]
    then
    	echo "La cadena está vacía"
    else
    	echo "La cadena no está vacía"
    
    fi
    ```
    
    El parámetro -z verifica si la cadena está vacía.
    
    ```bash
    if [[ -n "$cadena" && ${#cadena} -eq 5 ]]; then
    	echo "La variable cadena tiene una longitud de 5 caracteres"
    else
    	echo "La variable cadena no tiene una longitud de 5 caracteres"
    fi
    ```
    
    El parámetro "-n" en el comando "test" o "[[ ]]" es utilizado para verificar si una cadena de caracteres tiene una longitud determinada. Por ejemplo, si queremos verificar si una variable llamada "cadena" tiene una longitud de 5 caracteres
    
    Es importante que al comparar strings se debe tener en cuenta las mayúsculas y minúsculas, ya que Bash las distingue.
    
69. **Diferencia de corchetes: “[  ]” y “[[  ]]”**:
    
    En Bash scripting, hay dos tipos de corchetes que se utilizan para evaluar condiciones: "[]" y "[[ ]]".
    
    Los corchetes "[]" se utilizan para evaluar expresiones regulares, es decir, para evaluar si una cadena cumple con un patrón específico. Estos corchetes se utilizan para comparaciones simples, como comparar si una variable es igual a un valor específico, verificar si un archivo existe, entre otras.
    
    Por otro lado, los corchetes "[[ ]]" se utilizan para evaluar expresiones complejas, es decir, para evaluar múltiples condiciones al mismo tiempo, utilizando operadores lógicos como "and" (y), "or" (o) y "not" (no) entre otras funciones. Estos corchetes se utilizan para comparaciones más complejas, como verificar si una variable cumple con varios patrones al mismo tiempo, verificar si un archivo tiene ciertos permisos, entre otras.
    
    La principal ventaja de usar "[[ ]]" en lugar de "[]" es que ofrecen una sintaxis más clara y fácil de entender, ya que permiten escribir expresiones más complejas de manera más legible. Además, algunas funciones solo están disponibles al usar "[[ ]]", como por ejemplo, el uso de operadores lógicos.
    
    En resumen, los corchetes "[]" se utilizan para evaluar expresiones regulares y para comparaciones simples, mientras que los corchetes "[[ ]]" se utilizan para evaluar expresiones complejas y para comparaciones más complejas, permitiendo una sintaxis más clara y legible.
    
70. **Traer un proceso en segundo plano al primer plano**:
    
    Más info: [https://tldp.org/LDP/abs/html/x9644.html#:~:text=The fg command switches a,upon the currently running job](https://tldp.org/LDP/abs/html/x9644.html#:~:text=The%20fg%20command%20switches%20a,upon%20the%20currently%20running%20job).
    
    ```bash
    fg %<número del "job">
    ```
    
71. **Verificar si el input es un número o string**: 
    
    Verificar número:
    
    Para indicar una variable que tome un input del usuario y solo acepte números, se puede utilizar una estructura de control de flujo "while-do" y la función "expr" para verificar si el input es un número.
    
    Por ejemplo:
    
    ```bash
    while true; do
    	read -p "Ingrese un número: " num
    if [[ $(expr $num + 0 2> /dev/null) = $num ]]; then
    	break
    else
    	echo "Por favor ingrese solo números"
    fi
    done
    ```
    
    En este ejemplo, se utiliza el comando "read" para recibir el input del usuario y almacenarlo en la variable "num". Luego, se utiliza la función "expr" para verificar si el input es un número, si es un número se sale del ciclo y si no se imprime un mensaje "Por favor ingrese solo números" y se vuelve a preguntar al usuario.
    
    La función expr es utilizada para evaluar expresiones matemáticas, en este caso se esta evaluando si $num + 0 es un valor numérico, si es un valor numérico, expr retorna el valor de la operación, en caso contrario retorna un error, el cual se redirige a /dev/null.
    
    Así se garantiza que la variable solo tomará números como valor y se puede continuar con el script sin problemas.
    
    El parámetro "-p" en el comando "read" es utilizado para especificar un prompt o mensaje que se mostrará al usuario antes de recibir el input.
    
    Si se quiere comparar varios valores numéricos con expresiones regulares, se puede hacer de la siguiente forma utilizando “=~” y “[[ ]]” sin utilizar el “expr”:
    
    ```bash
    read num1; read num2; read num3; read num4
    if [[ $num1 =~ ^[0-9]\+$ && $num2 =~ ^[0-9]\+$ && $num3 =~ ^[0-9]\+$ && $num4 =~ ^[0-9]\+$ ]]; then
    	suma=$(echo "$num1+$num2+$num3+$num4" | bc); printf "\n%0.3f\n" $(echo "$suma/4" | bc)
    else
    	echo "ERROR"
    fi
    ```
    
    Verificar string:
    
    ejemplos:
    
    ```bash
    if [[ $var =~ ^[a-zA-Z]+$ ]]; then
    	echo "La variable es un string"
    else
    	echo "La variable no es un string"
    fi
    ```
    
    Se puede usar la expresión regular **`[[ $var =~ ^[a-zA-Z]+$ ]]`**
     para verificar si una variable es un string. El patrón **`^[a-zA-Z]+$`**
     indica que la variable debe comenzar con una o más letras (minúsculas o mayúsculas), y debe terminar en una o más letras también.
    
    ```bash
    if expr "$var" : '.*' > /dev/null; then
        echo "La variable es un string"
    else
        echo "La variable no es un string"
    fi
    ```
    
    En una expresión regular, '.' se utiliza para buscar cualquier caracter (cualquier cantidad de veces) hasta encontrar el siguiente caracter o conjunto de caracteres especificado.
    Por ejemplo, si se busca en una cadena de texto "Hola mundo" la expresión regular "Hola.", encontraría la coincidencia completa "Hola mundo". El punto (.) representa cualquier caracter y el asterisco (*) indica que se repita cualquier cantidad de veces.
    
72. **Ejecutar comandos como otro usuario con `doas`**: doas (“dedicated openbsd application subexecutor”) is a program to execute commands as another user
. The system administrator can configure it to give specified users privileges to execute specified commands. ex: **`doas -u <user> <command_to_execute>`**
    
    
73. **Comando PRINTF**:
    
    El comando **`printf`** es utilizado para imprimir una cadena de formateo en la consola. El primer parámetro de **`printf`** es la cadena de formateo, que especifica cómo se deben imprimir los valores.
    
    En este caso, la cadena de formateo es "%.3f\n", que contiene dos partes:
    
    - "%.3f": Este es el formato de impresión para un número de punto flotante. El símbolo **`%`** indica que se trata de un formato de impresión, seguido por un número (3) que indica el número de dígitos decimales a imprimir, y el carácter **`f`** que indica que se trata de un número de punto flotante.
    - "\n": El carácter de nueva línea, indica el final de la línea de impresión.
    
    En resumen, el comando **`printf "%.3f\n"`** indica que se debe imprimir un número de punto flotante con 3 dígitos decimales seguido de un salto de línea.
    
    El punto (.) después del símbolo de porcentaje (%), en el formato de impresión "%.3f", indica el número de dígitos decimales a imprimir para un número de punto flotante.
    
    En este caso, el número 3 después del punto indica que se deben imprimir 3 dígitos decimales. Esto significa que el número se redondeará para que solo tenga 3 dígitos decimales después del punto.
    
    Por ejemplo, si el número es 3.14159, el comando **`printf "%.3f\n" 3.14159`** imprimiría "3.142" (redondeado a 3 decimales).
    
    Sin el punto, el formato de impresión "%.3f" no tendría sentido ya que no se indica cuantos decimales se desea imprimir.
    
    Parámetros
    
    - “%d”: Imprimir un número entero.
    - “%f”: Imprimir un número con coma flotante.
    - “%”: Se indica que se quiere imprimir un string.
    - “\n”: Indica que se quiere hacer un salto de línea.
    - “%s”: imprime una cadena de caracteres (string)
    - “%c”: imprime un caractér
    - “%x”: imprime un número entero en notación hexadecimal
    - "%o": imprime un número en octal
    - “%e” o “%E”: imprime un número flotante en notación científica
    - "%g" o "%G": imprime un número en notación general
    - "%%": imprime un carácter de porcentaje
    - %p: imprime un puntero, es decir, una dirección de memoria. En C, los punteros son variables que almacenan direcciones de memoria, y se utilizan para acceder a los valores almacenados en esas direcciones. El formato %p se utiliza para imprimir las direcciones de memoria de las variables de puntero.
    - %ld: imprime un entero largo, es decir, un número entero de mayor tamaño que el tipo int estándar. En C, los enteros largos se suelen almacenar en 64 bits, y su rango de valores es mayor que el de los enteros estándar. El formato %ld se utiliza para imprimir variables de tipo long int.
    - %lu: imprime un entero largo sin signo, es decir, un número entero de mayor tamaño que el tipo unsigned int estándar. El formato %lu se utiliza para imprimir variables de tipo unsigned long int.
    - %lld: imprime un entero largo largo, es decir, un número entero de mayor tamaño que el tipo long int. En C, los enteros largos largos se suelen almacenar en 64 bits, y su rango de valores es mayor que el de los enteros largos. El formato %lld se utiliza para imprimir variables de tipo long long int.
    - %llu: imprime un entero largo largo sin signo, es decir, un número entero de mayor tamaño que el tipo unsigned long int. El formato %llu se utiliza para imprimir variables de tipo unsigned long long int.
    - %u: imprime un entero sin signo, es decir, un número entero que no tiene signo. Los enteros sin signo solo pueden almacenar valores positivos, y su rango de valores es mayor que el de los enteros con signo. El formato %u se utiliza para imprimir variables de tipo unsigned int.
    
    Además, se pueden agregar especificaciones de formato adicionales, como la precisión y el ancho de campo. Por ejemplo, %.2f imprimiría un número flotante con solo dos dígitos después del punto decimal, y %5d imprimiría un número entero con un ancho de campo de 5 caracteres (rellenando con espacios en blanco si es necesario).
    
    También se pueden especificar la cantidad de caracteres mínima que debe tener el valor impreso y el carácter de relleno (si el valor es más corto) con el formato : "%m.n", donde m es el caractér de relleno y n es el número mínimo de caracteres.
    
    eje: **`read num; printf "%05d\n" $(echo $num)`**
    

	Diferencia de “echo” y “printf”:

	"echo" y "printf" son dos comandos diferentes con funciones similares: ambos son utilizados para imprimir texto en la consola. Sin embargo, hay algunas diferencias entre ellos.

	"echo" es un comando más simple que se utiliza para imprimir texto directamente en la consola. Es más fácil de usar, pero tiene menos funciones avanzadas. Por ejemplo, no tiene la capacidad de especificar un formato de impresión para los números.

	"printf", por otro lado, es un comando más avanzado que se utiliza para imprimir texto con un formato específico. Se utiliza principalmente para imprimir números con un formato específico, tales como números con un número específico de decimales o para imprimir texto con un ancho específico. Además, permite especificar el formato de impresión para las variables.

74. **Encriptar y desencriptar archivos con OpenSSL**:
    
    Desencriptar:
    
    **`openssl rsautl -decrypt -inkey id_rsa.pem -in pass.enc`**: 
    
    Este código utiliza el comando OpenSSL "rsautl" para descifrar un archivo cifrado.
    
    El comando "rsautl" es utilizado para realizar operaciones RSA sobre archivos. El parámetro "-decrypt" especifica que se desea descifrar el archivo.
    
    El parámetro "-inkey" especifica la clave privada RSA que se utilizará para descifrar el archivo. En este caso, la clave privada se encuentra en un archivo con nombre "id_rsa.pem"
    
    El parámetro "-in" especifica el archivo cifrado que se desea descifrar. En este caso, el archivo cifrado se llama "pass.enc".
    
    En resumen, este código utiliza el comando OpenSSL "rsautl" para descifrar un archivo cifrado "pass.enc" con una clave privada RSA almacenada en "id_rsa.pem".
    
    Encriptar:
    
    **`openssl rsautl -encrypt -pubin -inkey id_rsa.pub.pem -in mensaje.txt -out mensaje_encriptado.txt`:**
    
    El comando OpenSSL también puede utilizarse para encriptar archivos. El comando "rsautl" es utilizado para realizar operaciones RSA sobre archivos, pero tiene diferentes opciones para encriptar y desencriptar.
    
    Para encriptar un archivo con RSA, se puede usar la opción "-encrypt" en lugar de "-decrypt". Es necesario especificar la clave pública RSA a utilizar con el parámetro "-pubin", y el archivo a encriptar con el parámetro "-in".
    
    En el ejemplo, código encripta un archivo "mensaje.txt" con una clave pública RSA en "id_rsa.pub.pem”
    
    "-inkey" se utiliza para especificar la clave pública RSA que se utilizará para encriptar el archivo.
    
    El parámetro "-pubin" es utilizado para especificar que se está utilizando una clave pública RSA para encriptar el archivo. Es necesario usarlo en conjunto con el parámetro "-inkey" para especificar el archivo que contiene la clave pública RSA.
    
    En cambio, el parámetro "-inkey" es utilizado para especificar el archivo que contiene la clave RSA (pública o privada) a utilizar. Sin el parámetro "-pubin", se asume que se está utilizando una clave privada RSA para desencriptar el archivo.
    
    En resumen, "-inkey" se utiliza para especificar el archivo que contiene la clave RSA (pública o privada) a utilizar, y "-pubin" se utiliza para especificar que se está utilizando una clave pública RSA para encriptar el archivo.
    
75. **Expresiones regulares**:
    
    ejemplos: 
    
    ```bash
    if ! [[ $num1 =~ ^[0-9]+([.][0-9]+)?$ ]]; then
    	echo "num1 no es un número válido"
    	exit 1
    fi
    ```
    
    ```bash
    if expr "$num1" : "[0-9]\+" > /dev/null; then
    	echo "num1 es un número valido"
    else
    	echo "num1 no es un número válido"
    	exit 1
    fi
    ```
    
    El script valida si una variable especificada, en este caso "num1", es un número válido. Utiliza la expresión regular "[0-9]+" para buscar uno o más dígitos numéricos en el valor de la variable. Si se encuentran dígitos numéricos, el comando "expr" devuelve el valor 0, lo que se considera verdadero. Si no se encuentran dígitos numéricos, el comando "expr" devuelve un valor distinto de cero, lo que se considera falso.
    
    En el script, se utiliza el comando "if" para ejecutar un bloque de código si el resultado de "expr" es verdadero, y otro bloque de código si es falso. El comando "expr" se utiliza para comparar la variable "num1" con la expresión regular "[0-9]+". El símbolo ":" después de la variable especifica que se esta comparando con una expresión regular.
    
    En este caso, no se usan "[[ ]]" porque la sintaxis "if expr" es más antigua y es compatible con versiones anteriores de Bash, mientras que "[[ ]]" se introdujo en versiones más recientes y no está disponible en todas las versiones de Bash. Además, la sintaxis "if expr" es más simple y directa para esta tarea específica de validar si una cadena es un número entero.
    
    La sintaxis "[[ ]]" se utiliza para comparar y hacer operaciones con cadenas de texto en Bash, mientras que "expr" es un comando utilizado para evaluar expresiones aritméticas y de comparación. En el caso del ejemplo que me diste, se está utilizando "expr" para evaluar si la variable "num1" cumple con una expresión regular específica, es decir, si contiene solo números. Por eso no se utiliza "[[ ]]" en este caso. Sin embargo, también se podría utilizar "[[ ]]" para evaluar una expresión regular, pero se tendría que utilizar una sintaxis diferente.
    
    eje:
    
    ```bash
    num1="123"
    if expr "$num1" : "[0-9]\+" > /dev/null && [[ $num1 -lt 1000 ]]; then
    	echo "num1 es un número valido menor a 1000"
    else
    	echo "num1 no es un número válido o es mayor a 1000"
    	exit 1
    fi
    ```
    
    - **`^`** indica el inicio de la línea
    - **`[0-9]`** indica un dígito numérico del rango 0 al 9
    - **`+`** indica que debe haber uno o más dígitos numéricos y si se escapa \+ indica que buscará uno o más caracteres consecutivos.
    - **`(`** abre un grupo de captura
    - **`[.][0-9]`** indica un punto seguido de un dígito numérico
    - **`+`** indica que debe haber uno o más dígitos numéricos consecutivos después del punto
    - **`)`** cierra el grupo de captura
    - **`?`** indica que el grupo de captura anterior es opcional
    - **`$`** indica el fin de la línea
    - **`'.*'`** En una expresión regular, '.' se utiliza para buscar cualquier caracter (cualquier cantidad de veces) hasta encontrar el siguiente caracter o conjunto de caracteres especificado.
    Por ejemplo, si se busca en una cadena de texto "Hola mundo" la expresión regular "Hola.", encontraría la coincidencia completa "Hola mundo". El punto (.) representa cualquier caracter y el asterisco (*) indica que se repita cualquier cantidad de veces.
    - El operador **`=~`**es utilizado en combinación con el comando **`if`** y permite comparar una cadena de texto con una expresión regular. La comparación se realiza mediante el uso de un operador de asignación, es decir, se asigna el resultado de la comparación a una variable para luego ser evaluada en una expresión booleana. Si la comparación es verdadera, el comando **`if`** ejecutará las instrucciones dentro de su cuerpo.
    - El parámetro “:”  indica que se quiere buscar alguna coincidencia entre el valor de la variable y la expresión regular.
    - El parámetro “=~” indica que se busca una coincidencia exacta del valor de la variable y la expresión regular.
    - El parámetro “/” indica que se quieren escapar caracteres especiales para interpretarlos de forma literal.
    - El "!" después del “if” es un negador, lo que significa que la condición se evalúa como verdadera si el resultado es falso. En este caso, si el resultado de la comparación es verdadero, significa que la variable no cumple con el patrón especificado por la expresión regular, por lo que se ejecuta el código dentro del bloque del if.
    
    ```bash
    read num1; read num2; read num3; read num4
    if expr "$num1" =~ "^[0-9]\+$" && "num2" =~ "^[0-9]\+$" && "num3" =~ "^[0-9]\+$" && "num4" =~ "^[0-9]\+$"; then
    	suma=$(echo "$num1+$num2+$num3+$num4" | bc -l)) && printf "\n%0.3f\n" $(echo "$suma/4" | bc -l)
    else echo "ERROR"
    	exit 1
    fi
    ```
    

76. **Capturar llamadas de procesos**: **`ltrace`**, por ejemplo: se tiene un archivo ejecutable llamado “check” el cual pide una contraseña, al utilizar el comando **`ltrace ./check`**, supervisará las llamadas de “check”, ejemplo, si se ingresa un comando mal, mostrará con el string ingresado y el string con el que lo está comparando (la contraseña real). En resumen, ltrace es un comando que sirve para hacer “debugging” y “troubleshooting” de procesos.
    
    
77. **Diagnóstico de red**: El comando fping es una herramienta de diagnóstico de red que permite realizar un ping a varias direcciones IP del segmento al mismo tiempo, eje: **`fping -aqg 10.10.0.0/24`**
    
    Los argumentos son:
    
    -a: Este argumento le indica a fping que muestre solo las direcciones IP que respondieron al ping.
    
    -q: Este argumento hace que fping funcione en modo silencioso, es decir, que no muestre información en la salida excepto los resultados.
    
    -g: Este argumento permite generar secuencias de direcciones IP aleatorias dentro de la subred especificada.
    
    -c: Permite especificar el número de paquetes que se deben enviar a cada dirección.
    
    #*Cuando se realiza el ping de manera secuencial, se hace ping a cada dirección IP en orden consecutivo, desde la dirección IP más baja hasta la más alta. Esto puede ser útil en situaciones donde se desea obtener una visión general de la capacidad de respuesta de una subred, o para verificar si todas las direcciones IP dentro de la subred están en línea.*
    
    *Por otro lado, cuando se realiza el ping de manera aleatoria con el argumento "-g", se hace ping a una secuencia aleatoria de direcciones IP dentro de la subred. Esto puede ser útil en situaciones donde se desea evitar una congestión de tráfico en una subred específica o cuando se desea evaluar la capacidad de respuesta de una subred de manera aleatoria.*#
    
78. **Comando** **`wget`**:
    
    Sirve para obtener archivos de una página web.
    
    Parámetros:
    
    **`-r`**: Descarga archivos de forma recursiva o repetida.
    
    **`-np`**: No descarga “parent directories”.
    
    ********`-nH`********: No crea el directorio del host.
    
    **********`--cut-dirs=2`**********: Omite los dos anteriores directorios y pone el tercer directorio en la ubicación actual.
    
    ****`-R`****: Excluye archivos, eje: -R ‘index.html*’ esto lo que hará es excluir todos los archivos que empiecen por “index.html”.
    
79. **Metacaracteres**:
    
    “*”: El asterisco reemplaza cualquier secuencia de
    caracteres en el nombre de un archivo. eje: **`ls file*`** esto mostrará todos los archivos que comiencen con “file”
    
    “?”: El signo de interrogación reemplaza cualquier
    caracter (uno solo) en el nombre de un archivo. Eje: **`ls ? ?? ???`** esto listará todos los archivos del directorio actual cuyos nombres tengan uno, dos y tres caracteres, respectivamente.
    
    “[ ]”: Los caracteres o los rangos mencionados entre
    corchetes reemplazan a un caracter en el nombre de
    un archivo. Son válidos, por ejemplo, los rangos : 0-9 o
    a-z. Eje: **`ls file.[vcz]`** esto se referirá a: file.v, file.c y file.z
    

80. **Crear dos directorios al mismo tiempo**: **`mkdir -p /directory/;/directory2/`**
    
    El comando "mkdir -p /directory/;/directory2/" crea dos directorios, "/directory/" y "/directory2/" en la raíz del sistema de archivos en un solo comando.
    
    La opción "-p" le dice a mkdir que cree los directorios padre si no existen. Esto significa que si la ruta "/directory/" o "/directory2/" no existen en el sistema, mkdir los creará junto con los subdirectorios necesarios para llegar a ellos.
    
    La sintaxis "/directory/;/directory2/" es una forma de especificar dos rutas de directorio diferentes en un solo comando. El punto y coma se utiliza para separar las rutas y crear varios directorios al mismo tiempo.
    
    Eje: Supongamos que queremos crear dos directorios llamados "proyecto" y "documentos" en la ruta "/home/usuario/". Podemos usar el comando "**`mkdir -p /home/usuario/proyecto;/home/usuario/documentos/`**" para crear ambos directorios al mismo tiempo. Este comando creará los directorios "proyecto" y "documentos" en la ruta "/home/usuario/". La opción "-p" asegura que los directorios "usuario" y "home" también se creen si aún no existen. El punto y coma se utiliza para separar las dos rutas de directorio.
    

81. **Guardar salida**: Se puede guardar la entrada de texto en un archivo de la siguiente forma:
    
    ```bash
    cat > archivo.txt
    ```
    
    Cuando se ejecute, mostrará solamente el cursor, permitiendo ingresar texto por el usuario, una vez terminado de ingresar el texto, se termina la acción con “ctrl + d”.
    

82. **Delimitador cat**; Otra forma de mostrar texto en pantalla es delimitando el texto de la siguiente forma:
    
    ```bash
    cat << nombre_delimitador
    texto
    de prueba
    nombre_delimitador
    ```
    
    Al ejecutarlo, mostrará el texto que se encuentra dentro del “nombre_delimitador”.
    

83. **Diferencia de (( )), ( ), [[ ]] y [ ]**:
    - (( ))
        
        (( )): Los paréntesis dobles, también conocidos como "paréntesis aritméticos", se utilizan para evaluar expresiones aritméticas. Cualquier expresión matemática dentro de los paréntesis se evaluará y se interpretará como un número. Por ejemplo:
        
        ```bash
        resultado=$((3 + 4))
        echo $resultado  # Imprimirá 7
        ```
        
        Los paréntesis dobles son especialmente útiles para realizar cálculos aritméticos y comparaciones numéricas dentro de scripts de Bash.
        
    - ( )
        
        ( ): Los paréntesis simples se utilizan para crear un subproceso (subshell). Un subproceso es una instancia separada del intérprete de comandos de Bash, que se ejecuta en paralelo con el proceso principal. Esto significa que las variables y los cambios en el entorno dentro de un subproceso no afectarán al proceso principal ni viceversa. Por ejemplo:
        
        ```bash
        (cd /ruta/al/directorio; ls)  # Cambia de directorio solo dentro del subproceso y lista los archivos allí
        ```
        
        Además, los paréntesis simples también se pueden utilizar para agrupar comandos y establecer su precedencia. Por ejemplo:
        
        ```bash
        (comando1; comando2)  # Los comandos se ejecutan en el subproceso en orden
        ```
        
    - [[ ]]
        
        [[ ]]: Los corchetes dobles son una extensión de Bash y se utilizan para realizar comparaciones condicionales. Proporcionan una sintaxis más flexible y poderosa que los corchetes simples ([ ]). Puedes utilizar los corchetes dobles para realizar comparaciones de cadenas, comparaciones numéricas y otras operaciones de comparación más complejas. Por ejemplo:
        
        ```bash
        if [[ $variable == "valor" ]]; then
            echo "La variable es igual a 'valor'"
        fi
        ```
        
        Los corchetes dobles también admiten operadores lógicos como && (y), || (o), etc.
        
    - [ ]
        
        [ ]: Los corchetes simples, también conocidos como "paréntesis cuadrados", son un comando incorporado de Bash que se utiliza para evaluar expresiones condicionales. Los corchetes simples son equivalentes al comando "test". Se utilizan principalmente en estructuras condicionales, como instrucciones "if" y bucles "while". Por ejemplo:
        
        ```bash
        if [ -f archivo.txt ]; then
            echo "El archivo.txt existe"
        fi
        ```
        
        Los corchetes simples admiten una variedad de operadores y opciones de prueba para realizar comparaciones de archivos, cadenas y valores numéricos.
        
    
    En resumen, los diferentes tipos de paréntesis en Bash scripting tienen usos específicos: los paréntesis dobles se utilizan para evaluaciones aritméticas, los paréntesis simples crean subprocesos y establecen precedencia, los corchetes dobles se utilizan para comparaciones condicionales más flexibles y los corchetes simples se utilizan para evaluaciones condicionales en estructuras de control.
    

84. **¿Cómo funciona “case”?**
    - Explicación
        
        En Bash scripting, la estructura de control "case" se utiliza para realizar múltiples comparaciones en una sola estructura y ejecutar diferentes bloques de código según el resultado de esas comparaciones. Es una forma más concisa y legible de manejar múltiples opciones que el uso repetitivo de instrucciones "if-else".
        
        La sintaxis básica de la estructura "case" es la siguiente:
        
        ```bash
        case valor in
            patron1)
                # Código a ejecutar si el valor coincide con el patrón1
                ;;
            patron2)
                # Código a ejecutar si el valor coincide con el patrón2
                ;;
            patron3)
                # Código a ejecutar si el valor coincide con el patrón3
                ;;
            *)
                # Código a ejecutar si no coincide con ningún patrón anterior
                ;;
        esac
        ```
        
        Aquí hay una explicación de cómo funciona cada parte de la estructura "case":
        
        - "valor" es la variable o valor que se va a comparar con los diferentes patrones.
        - Los "patrones" son las opciones posibles que se evaluarán para encontrar una coincidencia. Pueden ser expresiones regulares o cadenas de texto.
        - El código a ejecutar para cada patrón se coloca entre paréntesis y se termina con la instrucción ";;". Cada bloque de código se ejecutará si el "valor" coincide con el patrón correspondiente.
        - Si ninguno de los patrones coincide con el "valor", se puede proporcionar un caso comodín utilizando el asterisco (*) como patrón. El bloque de código asociado al asterisco se ejecutará si no se encuentra ninguna coincidencia anterior.
        
        Aquí hay un ejemplo de uso de la estructura "case":
        
        ```bash
        fruta="manzana"
        
        case $fruta in
            manzana)
                echo "Has elegido una manzana."
                ;;
            naranja|mandarina)
                echo "Has elegido una naranja o una mandarina."
                ;;
            banana|plátano)
                echo "Has elegido una banana o un plátano."
                ;;
            *)
                echo "No has elegido ninguna fruta conocida."
                ;;
        esac
        ```
        
        En este ejemplo, se evalúa el valor de la variable "fruta" y se ejecuta el bloque de código correspondiente según la coincidencia. Si el valor es "manzana", se imprimirá "Has elegido una manzana.". Si el valor es "naranja" o "mandarina", se imprimirá "Has elegido una naranja o una mandarina.". Si el valor es "banana" o "plátano", se imprimirá "Has elegido una banana o un plátano.". Si el valor no coincide con ninguno de los patrones anteriores, se imprimirá "No has elegido ninguna fruta conocida.".
        
        La estructura "case" es útil cuando se necesitan realizar múltiples comparaciones y se desea evitar una serie de instrucciones "if-else" largas y anidadas. Simplifica y organiza el código, haciéndolo más legible y mantenible.
        
        **Case no funciona para hacer comparaciones aritméticas.**
        
    

85. **¿Cómo funciona “until”?**
    - Explicación
        
        La función "until" es una estructura de control en Bash scripting que se utiliza para crear bucles que se ejecutan repetidamente hasta que se cumple una condición determinada. A diferencia del bucle "while" que se ejecuta mientras una condición sea verdadera, "until" se ejecuta mientras una condición sea falsa.
        
        La sintaxis básica del bucle "until" es la siguiente:
        
        ```bash
        until condición
        do
            # Código a ejecutar mientras la condición sea falsa
        done
        ```
        
        Aquí tienes una explicación de cada parte del bucle "until":
        
        - "condición" es la expresión booleana que se evalúa antes de cada iteración del bucle. Si la condición es falsa, el bloque de código dentro del bucle se ejecuta. Si la condición es verdadera, el bucle se detiene y la ejecución continúa después del bloque "done".
        - El "bloque de código" dentro del bucle es el conjunto de instrucciones que se ejecutan repetidamente mientras la condición sea falsa.
        
        El flujo de ejecución del bucle "until" es el siguiente:
        
        1. Se evalúa la condición.
        2. Si la condición es falsa, se ejecuta el bloque de código.
        3. Después de ejecutar el bloque de código, se vuelve al paso 1 y se evalúa nuevamente la condición.
        4. Este proceso se repite hasta que la condición sea verdadera, momento en el cual el bucle se detiene y la ejecución continúa después del bloque "done".
    - Ejemplo
        
        Aquí tienes un ejemplo que ilustra cómo se utiliza el bucle "until":
        
        ```bash
        contador=0
        
        until [ $contador -eq 5 ]
        do
            echo "El contador es: $contador"
            contador=$((contador + 1))
        done
        
        echo "Fin del bucle"
        ```
        
        En este ejemplo, el bucle "until" se ejecuta hasta que el valor de la variable "contador" sea igual a 5. En cada iteración del bucle, se imprime el valor actual del contador y luego se incrementa en 1. El bucle se repetirá hasta que el contador alcance el valor 5. Después de eso, la condición se vuelve verdadera y el bucle se detiene. Luego se imprime "Fin del bucle".
        
        La función "until" es útil cuando se necesita ejecutar un bloque de código repetidamente hasta que se cumpla una condición específica. Proporciona una forma sencilla de implementar bucles con una condición de salida negativa.
        
    - Ejemplo #2
        
        ```bash
        echo "Ingresa un número:"
        read number
        
        until [[ number -le 10 ]]; do
        	echo $number
        	let number+=1
        done
        ```
        
        ```bash
        number=1
        until [ $number -ge 10 ]; do
        	echo $number
        	number=$((number + 1))
        done
        ```
        

86. **Argumentos posicionales**:
    
    En Bash scripting, los argumentos son valores que se pasan a un script o a una función cuando se invoca desde la línea de comandos. Estos argumentos se almacenan en variables especiales llamadas "variables de posición" o "parámetros posicionales". Los argumentos se numeran secuencialmente a partir de 1, y se accede a ellos utilizando la notación "$n", donde "n" es el número del argumento.
    
    Aquí tienes una explicación de los argumentos más comunes en Bash:
    
    - $0: Contiene el nombre del script en sí mismo o el nombre del comando utilizado para invocar el script.
    - $1, $2, $3, ...: Representan los argumentos pasados al script o función. $1 hace referencia al primer argumento, $2 al segundo argumento, y así sucesivamente.
    - $*: Contiene todos los argumentos pasados al script como una única cadena de texto. Se puede iterar sobre ellos utilizando un bucle "for".
    - $@: Al igual que $*, contiene todos los argumentos pasados al script, pero cada argumento se trata como una cadena separada. Esto permite mantener las diferencias entre los argumentos pasados.
    - $#: Contiene el número total de argumentos pasados al script.
    - $?: Contiene el estado de salida del comando o script anterior. Normalmente, un valor de 0 indica que se ejecutó correctamente, mientras que un valor distinto de 0 indica un error.
    - $$: Contiene el PID de la shell actual.
    - $!: Contiene el PID del último proceso en segundo plano ejecutado.
    - $-: Contiene las banderas o opciones de la línea de comandos establecidas durante la invocación del script.
    - !!: Hace alusión a todo el comando anterior incluyendo los argumentos.
    - Ejemplos
        
        Aquí hay un ejemplo para ilustrar el uso de los argumentos en Bash:
        
        ```bash
        #!/bin/bash
        
        echo "El nombre del script es: $0"
        echo "El primer argumento es: $1"
        echo "El segundo argumento es: $2"
        echo "Todos los argumentos: $*"
        echo "Número total de argumentos: $#"
        echo "Estado de salida del comando anterior: $?"
        echo "ID del proceso actual: $$"
        echo "PID del último proceso en segundo plano: $!"
        echo "Las banderas de la línea de comandos: $-"
        echo "Comando y argumento utilizado anteriormente: !!"
        ```
        
        Si este script se llama **`mi_script.sh`** y se ejecuta con los argumentos **`foo bar`**, el resultado sería:
        
        ```bash
        El nombre del script es: mi_script.sh
        El primer argumento es: foo
        El segundo argumento es: bar
        Todos los argumentos: foo bar
        Número total de argumentos: 2
        Estado de salida del comando anterior: 0
        ID del proceso actual: 12345  # Un número de PID real
        PID del último proceso en segundo plano: 6789  # Un número de PID real
        Las banderas de la línea de comandos: hB
        Comando y argumento utilizado anteriormente: mi_script.sh foo bar
        ```
        
        Los argumentos en Bash son una forma de pasar información al script o función cuando se invoca desde la línea de comandos, lo que permite una mayor flexibilidad y personalización de los programas.
        
	- **Más variables especiales**
	    - $OLDPWD: Contiene el directorio de trabajo anterior (Old Print Working Directory). Se utiliza para hacer referencia al directorio en el que estabas antes de cambiar al directorio actual.
	    - $_: Contiene el último argumento del comando anterior ejecutado o el último parámetro de una función.
	    - !$: Hace referencia al último argumento del comando anterior. Al igual que $!:!, puedes utilizarlo para evitar tener que volver a escribir el último argumento en una nueva línea de comandos.
	    - !^: Hace referencia al primer argumento del comando anterior. Es útil cuando deseas utilizar solo el primer argumento del comando anterior en una nueva línea de comandos.
	    - !-n: Hace referencia al comando "n" en el historial del shell. Puedes utilizarlo para recuperar comandos previos y ejecutarlos nuevamente.
	    - $IFS: Contiene el separador de campos internos (Internal Field Separator). Se utiliza para dividir cadenas en campos separados.
	    - $PIPESTATUS: Un arreglo que contiene los estados de salida de los comandos separados por tuberías (pipes) en un comando compuesto. El índice 0 corresponde al estado de salida del primer comando, el índice 1 al segundo comando, y así sucesivamente.
	    - $BASH_VERSION: Contiene la versión de Bash actualmente en uso.
	    - $RANDOM: Genera un número aleatorio cada vez que se accede a él.
	    - $LINENO: Contiene el número de línea actual en el script.
	    - $SECONDS: Contiene el número de segundos transcurridos desde que el script se inició o desde que se reinició la variable $SECONDS.
	    - $HISTCMD: Contiene el número de comando actual en el historial del shell.
	    - $HOSTNAME: Contiene el nombre del host de la máquina.
	    - $HOME: Contiene la ruta al directorio de inicio del usuario actual.
	    - $USER: Contiene el nombre del usuario actual.
	    - $UID: Contiene el ID de usuario del usuario actual.
	    - $EUID: Contiene el ID de usuario efectivo (effective user ID) del usuario actual.

87. **STDIN, STDERR y STDOUT**:
    - [stdin] Ejemplo #1
        
        ```bash
        while read line; do
              echo $line
        done < "${1:-/dev/stdin}"
        ```
        
        1. **`< "${1:-/dev/stdin}"`**: Redirige la entrada del bucle "while" desde el primer argumento pasado al script. "${1}" se refiere al primer argumento, mientras que "${1:-/dev/stdin}" significa que si no se proporciona ningún argumento, se utiliza "/dev/stdin" como entrada estándar. Esto permite leer líneas de un archivo si se proporciona como argumento, o de la entrada estándar si no se proporciona ningún argumento.
        
        En resumen, este comando lee líneas de entrada y las imprime en la salida estándar. Puede ser utilizado para procesar líneas de texto o archivos línea por línea en un bucle, ya sea leyendo desde un archivo especificado o desde la entrada estándar.
        
        El guion "-" en el comando **`read line`** es una convención comúnmente utilizada para indicar la lectura desde la entrada estándar. En este caso, significa que **`read`** leerá las líneas de entrada desde la entrada estándar.
        
        Por otro lado, **`/dev/stdin`** es un archivo especial en sistemas basados en Unix que representa la entrada estándar. Es un descriptor de archivo que permite acceder a la entrada estándar como si fuera un archivo regular. El descriptor de archivo estándar para la entrada es el número 0.
        
        En el comando que mencionaste, **`< "${1:-/dev/stdin}"`** está redirigiendo la entrada del bucle "while" desde el primer argumento pasado al script. Si se proporciona un argumento al script, se utilizará como nombre de archivo y se redirigirá la entrada del bucle "while" desde ese archivo. Si no se proporciona ningún argumento, se utilizará **`/dev/stdin`** como entrada estándar.
        
        En resumen, **`"/dev/stdin"`** es una forma de referirse a la entrada estándar en Bash. En el comando específico que mencionaste, se utiliza como opción predeterminada si no se proporciona ningún argumento de archivo al script. Esto permite que el script lea líneas de entrada tanto desde un archivo como desde la entrada estándar.
        
    - [stdout, stderr] Ejemplo #1
        
        ```bash
        # STDOUT
        ls > stdout.txt
        ls 1> stdout.txt
        
        # STDERR
        ls -asd 2>stderr.txt
        
        # STDOUT y STDERR
        ls 1> stdout.txt 2> stderr.txt
        ls > stdout_stderr.txt 2>&1
        ls >& stdout_stderr.txt
        ```
        
        1. **`ls > stdout.txt`**: Este comando ejecuta el comando "ls" para listar los archivos y directorios en el directorio actual. La salida estándar (stdout) del comando "ls" se redirige al archivo "stdout.txt". Esto significa que la lista de archivos se guardará en el archivo "stdout.txt" en lugar de mostrarse en la pantalla.
        2. **`ls 1> stdout.txt`**: Este comando es esencialmente lo mismo que el anterior. Utiliza el descriptor de archivo 1 para representar la salida estándar. Redirige la salida estándar del comando "ls" al archivo "stdout.txt". El número 1 en "1>" se refiere al descriptor de archivo de salida estándar.
        3. **`ls -asd 2> stderr.txt`**: En este comando, se ejecuta el comando "ls" con la opción "-asd", que mostrará un error ya que no es un argumento válido. La salida estándar de "ls” (stderr) se redirige al archivo "stderr.txt". Esto significa que cualquier mensaje de error o advertencia generado por el comando "ls" se guardará en el archivo "stderr.txt" en lugar de mostrarse en la pantalla.
        4. **`ls 1> stdout.txt 2> stderr.txt`**: Este comando combina la redirección de la salida estándar y de error. La salida estándar del comando "ls" se redirige al archivo "stdout.txt", y la salida de error se redirige al archivo "stderr.txt". Esto significa que tanto la salida estándar como la salida de error se guardarán en archivos separados en lugar de mostrarse en la pantalla.
        5. **`ls > stdout_stderr.txt 2>&1`**: En este comando, se redirige la salida estándar del comando "ls" al archivo "stdout_stderr.txt". Luego, **`2>&1`** redirige la salida de error (stderr) al mismo destino que la salida estándar, es decir, al archivo "stdout_stderr.txt". Como resultado, tanto la salida estándar como la salida de error se guardan en el mismo archivo.
        6. **`ls >& stdout_stderr.txt`**: Este comando es una forma más concisa de lograr lo mismo que el comando anterior. **`>&`** redirige tanto la salida estándar como la salida de error al archivo "stdout_stderr.txt". De esta manera, ambas salidas se fusionan en un solo archivo.
        

88. **Diferencia de “wc -c” y “wc -m”**:
    - Explicación
        
        En Bash, el comando **`wc`** se utiliza para contar el número de bytes, caracteres, palabras y líneas en un archivo o en la entrada proporcionada. La opción **`-c`** y **`-m`** son dos formas de especificar qué se debe contar:
        
        - **`wc -c`**: Esta opción cuenta el número de bytes en un archivo o en la entrada proporcionada. Incluye todos los caracteres, incluidos los caracteres de control y los caracteres de nueva línea.
        - **`wc -m`**: Esta opción cuenta el número de caracteres en un archivo o en la entrada proporcionada. Los caracteres de nueva línea también se cuentan como caracteres individuales.
        
        La diferencia principal entre **`-c`** y **`-m`** radica en cómo se cuentan los caracteres de nueva línea:
        
        - Con **`c`**, los caracteres de nueva línea también se cuentan como bytes individuales, lo que significa que cada carácter de nueva línea se suma al total de bytes contados.
        - Con **`m`**, los caracteres de nueva línea también se cuentan como caracteres individuales, pero no se cuentan como bytes adicionales. En otras palabras, los caracteres de nueva línea no contribuyen al recuento de bytes, solo al recuento de caracteres.
        
        Aquí tienes un ejemplo para ilustrar la diferencia:
        
        Supongamos que tenemos un archivo llamado "texto.txt" con el siguiente contenido:
        
        ```
        
        Hola
        Mundo
        
        ```
        
        Si ejecutamos el comando **`wc -c texto.txt`**, el resultado será **`12`**. Esto se debe a que hay 12 bytes en total, contando los caracteres **`H`**, **`o`**, **`l`**, **`a`**, **`\n`**, **`M`**, **`u`**, **`n`**, **`d`**, **`o`** y dos caracteres de nueva línea **`\n`**.
        
        Por otro lado, si ejecutamos el comando **`wc -m texto.txt`**, el resultado será **`11`**. Esto se debe a que hay 11 caracteres en total, contando cada carácter individualmente, incluyendo los dos caracteres de nueva línea **`\n`**.
        
    
    En resumen, la opción **`-c`** cuenta el número total de bytes, mientras que la opción **`-m`** cuenta el número total de caracteres, incluyendo los caracteres de nueva línea, pero sin contarlos como bytes adicionales. La elección entre **`-c`** y **`-m`** depende de cómo desees contar y representar los datos en tu caso específico.
    

89. **Exportar variables**:
    
    Para exportar variables y utilizarlas entre varios scripts en Bash, puedes seguir estos pasos:
    
    1. En el script donde defines la variable que deseas exportar, utiliza el comando **`export`** seguido del nombre de la variable. Por ejemplo, supongamos que tienes un script llamado "script1.sh" y quieres exportar la variable **`MY_VARIABLE`**:
        
        ```bash
        # script1.sh
        export MY_VARIABLE="Hola, mundo"
        ```
        
        Al utilizar **`export`**, le estás indicando a Bash que haga que la variable **`MY_VARIABLE`** esté disponible para otros scripts.
        
    2. En el script donde deseas utilizar la variable exportada, puedes acceder a ella simplemente referenciándola por su nombre. Por ejemplo, supongamos que tienes otro script llamado "script2.sh" y deseas utilizar la variable **`MY_VARIABLE`** exportada desde "script1.sh":
        
        ```bash
        # script2.sh
        echo "El valor de MY_VARIABLE es: $MY_VARIABLE"
        ```
        
        Al ejecutar "script2.sh", la variable **`MY_VARIABLE`** estará disponible y se imprimirá su valor en la salida.
        
    3. Asegúrate de ejecutar los scripts en el mismo proceso o sesión de Bash para que las variables exportadas estén disponibles. Si ejecutas los scripts en procesos separados, las variables exportadas no estarán disponibles en los otros scripts.
        
        Por ejemplo, si ejecutas "script1.sh" para exportar la variable y luego ejecutas "script2.sh" en una nueva sesión de Bash, la variable exportada no estará disponible en "script2.sh".
        
        Para ejecutar los scripts en la misma sesión de Bash, puedes usar el punto (**`.`**) o el comando **`source`** para cargar el contenido de un script en el contexto actual. Por ejemplo:
        
        ```bash
        . script1.sh
        # o
        source script1.sh
        ```
        
        Al cargar "script1.sh" de esta manera, las variables exportadas estarán disponibles en el entorno actual y podrán ser utilizadas por otros scripts.
        
    
    Con estos pasos, puedes exportar variables en un script y luego utilizarlas en otros scripts, siempre y cuando se ejecuten en la misma sesión de Bash y se importe o se ejecute el script que las exporta correctamente.
    

90. **Manipular de cadenas**:
    - Mayúsculas a minúsculas y viceversa
        1. Convertir a minúsculas:
            - **`${variable,,}`**: Convierte todo el contenido de la variable a minúsculas:
                
                ```bash
                variable="Hello World"
                echo "${variable,,}"  # Salida: hello world
                ```
                
            - **`${variable,}`**: Convierte solo el primer carácter del contenido de la variable a minúscula:
                
                ```bash
                variable="HELLO"
                echo "${variable,}"  # Salida: hELLO
                ```
                
        2. Convertir a mayúsculas:
            - **`${variable^^}`**: Convierte todo el contenido de la variable a mayúsculas:
                
                ```bash
                variable="hello world"
                echo "${variable^^}"  # Salida: HELLO WORLD
                ```
                
            - **`${variable^}`**: Convierte solo el primer carácter del contenido de la variable a mayúscula:
                
                ```bash
                variable="world"
                echo "${variable^}"  # Salida: World
                ```
                
        3. Conversión selectiva:
            - **`${variable^^pattern}`**: Convierte todas las coincidencias del patrón en el contenido de la variable a mayúsculas. El patrón puede ser un carácter o una expresión regular:
                
                ```bash
                variable="hello world"
                echo "${variable^^o}"  # Salida: hellO wOrld
                ```
                
            - **`${variable,,pattern}`**: Convierte todas las coincidencias del patrón en el contenido de la variable a minúsculas. El patrón puede ser un carácter o una expresión regular:
                
                ```bash
                variable="HELLO WORLD"
                echo "${variable,,WORLD}"  # Salida: HELLO world
                ```
                
            
            Algunos ejemplos adicionales:
            
            - **`${variable^^[aeiou]}`**: Convierte todas las vocales en el contenido de la variable a mayúsculas:
                
                ```bash
                variable="hello"
                echo "${variable^^[aeiou]}"  # Salida: hEllO
                ```
                
            - **`${variable,,[aeiou]}`**: Convierte todas las vocales en el contenido de la variable a minúsculas:
                
                ```bash
                variable="HELLO"
                echo "${variable,,[aeiou]}"  # Salida: hEllO
                ```
                
        
        Es importante tener en cuenta que estas construcciones para manipular cadenas en Bash están disponibles a partir de la versión 4 de Bash. Si estás utilizando una versión anterior, es posible que no estén disponibles. Puedes verificar la versión de Bash que estás utilizando ejecutando el comando **`bash --version`** en tu terminal.
        
    - Notación de slicing
        
        La notación de slicing de Bash te permite extraer una parte específica de una cadena utilizando un rango de índices. La sintaxis general para el slicing de cadenas en Bash es la siguiente:
        
        ```bash
        ${variable:start:count}
        ```
        
        Donde:
        
        - **`variable`** es el nombre de la variable que contiene la cadena.
        - **`start`** es el índice de inicio (basado en cero) desde donde se extraerá la porción de la cadena. Si es negativo, cuenta desde el final de la cadena.
        - **`count`** es el número opcional de caracteres a extraer. Si se omite, se extraen todos los caracteres a partir del índice de inicio.
        
        Aquí tienes algunos ejemplos para ilustrar el uso de la notación de slicing de Bash:
        
        1. Extraer un segmento de una cadena:
            
            ```bash
            variable="Hello, World!"
            echo "${variable:7:5}"  # Salida: World
            ```
            
            En este ejemplo, se extraen 5 caracteres a partir del índice 7 de la cadena, lo que resulta en "World".
            
        2. Extraer una porción desde el inicio:
            
            ```bash
            variable="Hello, World!"
            echo "${variable:0:5}"  # Salida: Hello
            ```
            
            En este caso, se extraen los primeros 5 caracteres de la cadena, obteniendo "Hello".
            
        3. Extraer una porción desde el final utilizando un índice negativo:
            
            ```bash
            variable="Hello, World!"
            echo "${variable: -6}"  # Salida: World!
            ```
            
            Aquí, se extraen todos los caracteres desde el índice -6 hasta el final de la cadena, lo que resulta en "World!".
            
        4. Extraer una porción hasta el final:
            
            ```bash
            variable="Hello, World!"
            echo "${variable:7}"  # Salida: World!
            ```
            
            En este ejemplo, se extraen todos los caracteres desde el índice 7 hasta el final de la cadena, obteniendo "World!".
            
        
        La notación de slicing de Bash es muy útil para extraer porciones específicas de una cadena según tus necesidades. Puedes utilizarla en combinación con otros comandos y operaciones para realizar manipulaciones más complejas en Bash.
        

91. **Operaciones aritméticas**:
    - Explicación
        
        ```bash
        # Primer conjunto
        echo $(( x + y ))
        echo $(( x - y ))
        echo $(( x * y ))
        echo $(( x / y ))
        echo $(( x % y ))
        
        # Segundo conjunto
        echo $(expr $x + $y )
        echo $(expr $x - $y )
        echo $(expr $x \* $y )
        echo $(expr $x / $y )
        echo $(expr $x % $y )
        ```
        
        Ambos conjuntos de comandos se utilizan en Bash para realizar operaciones aritméticas. Sin embargo, hay algunas diferencias en la sintaxis y el comportamiento entre los dos conjuntos.
        
        Primer conjunto de comandos:
        
        ```bash
        echo $(( x + y ))
        echo $(( x - y ))
        echo $(( x * y ))
        echo $(( x / y ))
        echo $(( x % y ))
        ```
        
        - Estos comandos utilizan la expansión de aritmética de Bash, que se indica mediante la doble paréntesis **`$((...))`**.
        - Realizan operaciones aritméticas básicas, como suma (**`+`**), resta (**``**), multiplicación (**``**), división (**`/`**) y módulo (**`%`**).
        - Los valores **`x`** y **`y`** son tratados como números enteros en estas operaciones.
        
        Segundo conjunto de comandos:
        
        ```bash
        echo $(expr $x + $y )
        echo $(expr $x - $y )
        echo $(expr $x \* $y )
        echo $(expr $x / $y )
        echo $(expr $x % $y )
        ```
        
        - Estos comandos utilizan el comando **`expr`** para realizar las operaciones aritméticas.
        - La sintaxis es **`$()`**, que se utiliza para ejecutar el comando **`expr`** y capturar su salida.
        - Se requiere un espacio antes y después de cada operador aritmético (**`+`**, **``**, **``**, **`/`**, **`%`**).
        - Los valores **`x`** y **`y`** son tratados como argumentos para el comando **`expr`** y se evalúan como expresiones aritméticas.
        
        La principal diferencia entre los dos conjuntos de comandos es la sintaxis utilizada y cómo se evalúan las expresiones aritméticas.
        
        El primer conjunto de comandos utiliza la expansión de aritmética de Bash (**`$((...))`**) y permite realizar operaciones aritméticas directamente en la línea de comandos sin la necesidad de comandos adicionales.
        
        El segundo conjunto de comandos utiliza el comando **`expr`** y se utiliza cuando se desea realizar operaciones aritméticas más complejas o cuando se necesita compatibilidad con versiones antiguas de Bash que no tienen la expansión de aritmética.
        
    
    En general, si estás utilizando una versión reciente de Bash, se recomienda el uso del primer conjunto de comandos (**`$((...))`**) debido a su simplicidad y facilidad de uso. Sin embargo, si necesitas compatibilidad con versiones antiguas de Bash, puedes utilizar el segundo conjunto de comandos con el comando **`expr`**.
    
    - **¿Qué relación tiene “expr” con los strings?**
        
        El segundo conjunto de comandos puede tener relación con las cadenas en ciertos casos. A diferencia del primer conjunto de comandos, que se enfoca en operaciones aritméticas numéricas, el segundo conjunto utiliza el comando **`expr`** que también puede operar en cadenas en algunos escenarios específicos.
        
        El comando **`expr`** en el segundo conjunto de comandos evalúa expresiones aritméticas, pero también puede manejar cadenas de texto en ciertos contextos. Aquí tienes un ejemplo:
        
        ```bash
        x="Hello"
        y="World"
        
        concatenated=$(expr "$x" : '\(.*\)')$y
        echo $concatenated  # Salida: HelloWorld
        ```
        
        En este ejemplo, se utiliza **`expr`** junto con la sintaxis **`$(expr ...)`** para capturar la salida del comando. La expresión **`"$x" : '\(.*\)'`** utiliza la función de coincidencia de patrones de **`expr`** para capturar todo el contenido de la variable **`x`** y luego se concatena con la variable **`y`**. El resultado es la concatenación de las cadenas **`"Hello"`** y **`"World"`**, lo que da como resultado **`"HelloWorld"`**.
        
        Es importante tener en cuenta que el uso de **`expr`** para operaciones de cadena es más limitado en comparación con herramientas como **`awk`**, **`sed`** o **`grep`**, que están especialmente diseñadas para manipular y trabajar con cadenas de texto. Si necesitas realizar operaciones más avanzadas con cadenas, es posible que desees considerar el uso de estas herramientas más especializadas.
        
    - **Resultado con coma flotante en Bash**
        
        En Bash, el manejo de números de coma flotante puede ser un poco limitado, ya que Bash está diseñado principalmente para trabajar con números enteros. Sin embargo, puedes usar herramientas externas como **`bc`** (calculadora de precisión arbitraria) para realizar cálculos con números de coma flotante. Aquí tienes un ejemplo:
        
        ```bash
        x=5.2
        y=3.7
        
        result=$(echo "scale=2; $x + $y" | bc)
        echo $result  # Salida: 8.9
        ```
        
        En este ejemplo, se utiliza el comando **`echo`** junto con la tubería **`|`** para pasar una expresión aritmética con números de coma flotante a **`bc`**. La opción **`scale=2`** establece la precisión decimal a 2, lo que significa que el resultado tendrá 2 decimales. La expresión **`"$x + $y"`** realiza la suma de los valores de coma flotante.
        
        El resultado se almacena en la variable **`result`**, y luego se muestra en la salida estándar.
        
        Ten en cuenta que al utilizar **`bc`**, debes especificar explícitamente la escala decimal (**`scale`**) y considerar las limitaciones de precisión inherentes al trabajar con números de coma flotante en Bash. Si necesitas realizar cálculos más complejos o con mayor precisión, es posible que desees considerar el uso de herramientas más especializadas, como Python o awk, que ofrecen un soporte más avanzado para cálculos de coma flotante.
        
    - **Operaciones aritméticas utilizando Python y awk**
        
        Ejemplo con **`awk`**:
        
        ```bash
        x=5.2
        y=3.7
        
        result=$(awk -v x="$x" -v y="$y" 'BEGIN { printf "%.2f", x + y }')
        echo $result  # Salida: 8.90
        ```
        
        En este ejemplo, **`awk`** se utiliza en el modo **`BEGIN`** para realizar el cálculo de suma (**`x + y`**). La opción **`-v`** se usa para pasar las variables **`x`** y **`y`** a **`awk`**. El formato **`%.2f`** se utiliza para especificar la precisión decimal de 2 en el resultado.
        
        Ejemplo con Python:
        
        ```bash
        x=5.2
        y=3.7
        
        result=$(python -c "print('%.2f' % ($x + $y))")
        echo $result  # Salida: 8.90
        ```
        
        En este ejemplo, se utiliza el comando **`python`** para ejecutar una expresión en Python. La expresión **`"%.2f" % ($x + $y)`** realiza la suma de los valores de coma flotante y formatea el resultado con una precisión decimal de 2 utilizando **`%.2f`**.
        
        Ambos ejemplos te permiten realizar cálculos con números de coma flotante y obtener el resultado con la precisión decimal deseada. Sin embargo, ten en cuenta que para utilizar Python, debes asegurarte de que esté instalado en tu sistema.
        
    - **Más información sobre BEGIN y '%.2f' %**
        
        En **`awk`**, la cláusula **`BEGIN`** se utiliza para ejecutar bloques de código antes de que se procesen las líneas de entrada. Se puede utilizar para inicializar variables, realizar cálculos iniciales o configurar el estado antes de procesar los datos. Sin embargo, no es necesario utilizar **`BEGIN`** para todas las operaciones matemáticas en **`awk`**. **`BEGIN`** se utiliza cuando deseas realizar operaciones antes de procesar las líneas de entrada, pero si deseas realizar operaciones en función de los datos de entrada, puedes utilizar otras partes de **`awk`** como patrones y acciones.
        
        En Python, el formato **`%.2f`** es una cadena de formato que especifica cómo se debe formatear un número de coma flotante. Veamos su funcionamiento:
        
        - **`%`** es un operador de formateo en Python que se utiliza para insertar valores en una cadena de formato.
        - **`.2f`** es una especificación de formato que indica que se debe formatear el valor como un número de coma flotante con 2 decimales después del punto decimal.
        
        En el ejemplo **`"%.2f" % ($x + $y)`**, el resultado de la expresión **`$x + $y`** se formatea como un número de coma flotante con 2 decimales utilizando la cadena de formato **`%.2f`**. El operador **`%`** se utiliza para insertar el valor formateado en la cadena.
        
        En resumen, **`%`** en Python se utiliza para formatear valores en una cadena de acuerdo con una especificación de formato, y **`%.2f`** especifica un número de coma flotante con 2 decimales.
        
        El operador **`%`** se utiliza dos veces en la expresión **`"%.2f" % ($x + $y)`** en Python para realizar el formateo de cadena. Permíteme explicarlo con más detalle:
        
        En Python, el operador **`%`** se utiliza para realizar el formateo de cadenas utilizando una cadena de formato y una lista de valores. La sintaxis general es la siguiente:
        
        ```bash
        cadena_de_formato % valores
        ```
        
        En este caso, **`"%.2f"`** es la cadena de formato y **`($x + $y)`** es el valor que se va a formatear.
        
        Cuando utilizas el operador **`%`** con una cadena de formato y un solo valor, como en este caso, se espera que el valor sea una tupla o una lista. Los elementos de la tupla o lista se insertan en la cadena de formato en el orden en que aparecen.
        
        En nuestro ejemplo, solo tenemos un valor **`($x + $y)`**, pero aún así se debe pasar como una tupla o lista. Esto se debe a que el operador **`%`** en Python espera una tupla o lista como el segundo argumento para poder realizar el formateo correctamente.
        
        Entonces, en resumen, el operador **`%`** se utiliza dos veces en este caso:
        
        1. El primer **`%`** se utiliza como el operador de formateo en sí.
        2. El segundo **`%`** se utiliza para proporcionar la tupla o lista de valores que se insertarán en la cadena de formato.
        
        Es importante tener en cuenta que esta sintaxis específica con **`%`** para el formateo de cadenas está presente en las versiones anteriores de Python (Python 2.x). A partir de Python 3.6, se introdujo una nueva sintaxis de formateo de cadenas llamada f-strings, que ofrece una sintaxis más legible y más poderosa.
        

92. **Declare**:
    
    En Bash, el comando **`declare`** se utiliza para declarar variables y establecer sus atributos. Proporciona varias opciones que permiten controlar el comportamiento y las propiedades de las variables. Aquí te explico algunas de las opciones más comunes:
    
    1. **`declare -a`**: Crea una variable como un arreglo (array). Puedes asignar múltiples valores a la variable utilizando la sintaxis **`variable=(valor1 valor2 ...)`**.
        
        Ejemplo:
        
        ```bash
        declare -a my_array=(1 2 3 4 5)
        echo ${my_array[2]}  # Salida: 3
        ```
        
    2. **`declare -i`**: Crea una variable como un entero. La asignación de un valor no numérico a una variable declarada con **`i`** se interpretará como cero.
        
        Ejemplo:
        
        ```bash
        declare -i my_integer=10
        my_integer="hello"
        echo $my_integer  # Salida: 0
        ```
        
    3. **`declare -r`**: Crea una variable como de solo lectura (read-only). No se puede modificar su valor una vez establecido.
        
        Ejemplo:
        
        ```bash
        declare -r my_readonly_var="This is read-only"
        my_readonly_var="New value"  # Error: Intento de modificar una variable de solo lectura
        ```
        
    4. **`declare -x`**: Crea una variable como una variable de entorno (export). La variable se convierte en una variable que está disponible para todos los procesos secundarios.
        
        Ejemplo:
        
        ```bash
        declare -x my_env_var="This is an environment variable"
        echo $my_env_var  # Salida: This is an environment variable
        ```
        
    
    Estas son solo algunas de las opciones más comunes del comando **`declare`**. Hay más opciones disponibles, como **`declare -p`** para mostrar los atributos de una variable y **`declare -f`** para mostrar las funciones definidas. Puedes obtener más información sobre las opciones del comando **`declare`** consultando la documentación de Bash utilizando el comando **`help declare`**.
    

93. **Listas o arrays**: En Bash, las listas se pueden implementar utilizando matrices (arrays). Una matriz en Bash es una variable que puede contener múltiples elementos, donde cada elemento se identifica mediante un índice numérico. Las matrices en Bash son de tamaño dinámico y pueden contener elementos de diferentes tipos.
    - Ejemplos #1
        1. Declarar una matriz:
        Puedes declarar una matriz en Bash utilizando la siguiente sintaxis:
            
            ```bash
            nombre_de_la_matriz=(valor1 valor2 valor3 ...)
            ```
            
            Ejemplo:
            
            ```bash
            my_array=(apple banana cherry)
            ```
            
        2. Acceder a elementos de la matriz:
        Puedes acceder a elementos específicos de la matriz utilizando el índice correspondiente dentro de corchetes **`[ ]`**.
            
            ```bash
            echo ${nombre_de_la_matriz[indice]}
            ```
            
            Ejemplo:
            
            ```bash
            echo ${my_array[0]}  # Salida: apple
            ```
            
        3. Longitud de la matriz:
        Puedes obtener la longitud de la matriz utilizando la variable especial **`${#nombre_de_la_matriz[@]}`**.
            
            ```bash
            echo ${#nombre_de_la_matriz[@]}
            ```
            
            Ejemplo:
            
            ```bash
            echo ${#my_array[@]}  # Salida: 3
            ```
            
        4. Recorrer una matriz:
        Puedes recorrer una matriz utilizando bucles **`for`** y accediendo a cada elemento de la matriz.
            
            ```bash
            for elemento in "${nombre_de_la_matriz[@]}"; do
                echo $elemento
            done
            ```
            
            Ejemplo:
            
            ```bash
            for fruit in "${my_array[@]}"; do
                echo $fruit
            done
            ```
            
        5. Añadir elementos a una matriz:
        Puedes agregar elementos a una matriz utilizando la sintaxis de asignación y corchetes.
            
            ```bash
            nombre_de_la_matriz+=(nuevo_elemento)
            ```
            
            Ejemplo:
            
            ```bash
            my_array+=(orange)
            ```
            
        6. Eliminar elementos de una matriz:
        Puedes eliminar elementos de una matriz utilizando el comando **`unset`** junto con el índice del elemento.
            
            ```bash
            unset nombre_de_la_matriz[indice]
            ```
            
            Ejemplo:
            
            ```bash
            unset my_array[1]
            ```
            
        7. Eliminar una matriz completa:
        Puedes eliminar una matriz completa utilizando el comando **`unset`** seguido del nombre de la matriz.
            
            ```bash
            unset nombre_de_la_matriz
            ```
            
            Ejemplo:
            
            ```bash
            unset my_array
            ```
            
        8. Copiar una matriz:
        Puedes copiar una matriz existente en otra matriz utilizando la expansión **`"${nombre_de_la_matriz[@]}"`** y asignando el resultado a una nueva matriz.
            
            ```bash
            nueva_matriz=("${nombre_de_la_matriz[@]}")
            ```
            
            Ejemplo:
            
            ```bash
            new_array=("${my_array[@]}")
            ```
            
        9. Filtrar elementos de una matriz:
        Puedes filtrar elementos de una matriz utilizando comandos como **`grep`** y expresiones regulares para seleccionar los elementos que cumplen ciertos criterios.
            
            ```bash
            nueva_matriz=($(printf "%s\n" "${nombre_de_la_matriz[@]}" | grep "patrón"))
            ```
            
            Ejemplo:
            
            ```bash
            new_array=($(printf "%s\n" "${my_array[@]}" | grep "a"))  # Filtra elementos que contienen la letra "a"
            ```
            
        10. Ordenar una matriz:
        Puedes ordenar los elementos de una matriz utilizando el comando **`sort`** junto con la expansión **`"${nombre_de_la_matriz[@]}"`**.
            
            ```bash
            nueva_matriz=($(printf "%s\n" "${nombre_de_la_matriz[@]}" | sort))
            ```
            
            Ejemplo:
            
            ```bash
            new_array=($(printf "%s\n" "${my_array[@]}" | sort))  # # Ordena los elementos alfabéticamente
            ```
            
    - Ejemplos #2
        
        ```bash
        # Mostrar un elemento en específico
        echo "${list[2]}"
        
        # Mostrar el primer y último elemento
        echo "${list[0]}"
        echo "${list[-1]}"
        echo "${list[${#list[@]}-1]}"
        
        # Agregar y reemplazar elementos
        list[3]="jane"
        list["${#list[@]}"]="kim"
        
        # Eliminar elementos
        unset list[-1]
        ```
        

94. **Funciones**: En Bash, puedes definir y utilizar funciones para agrupar un conjunto de comandos relacionados en un bloque lógico y reutilizable. Las funciones permiten estructurar y modularizar tu código en Bash, lo que facilita la escritura, lectura y mantenimiento de scripts más grandes.
    - **Explicación**
        1. Declarar una función:
        Para declarar una función en Bash, puedes utilizar la siguiente sintaxis:
            
            ```bash
            nombre_de_la_funcion() {
                # Comandos
            }
            
            # 2da opción:
            function nombre_de_la_funcion() {
                # Comandos
            }
            ```
            
            Ejemplo:
            
            ```bash
            saludar() {
                echo "Hola, ¿cómo estás?"
            }
            
            # 2da opción:
            function saludar() {
                echo "Hola, ¿cómo estás?"
            }
            ```
            
        2. Llamar a una función:
        Para llamar a una función en Bash, simplemente escribe el nombre de la función.
            
            ```bash
            nombre_de_la_funcion
            ```
            
            Ejemplo:
            
            ```bash
            saludar  # Llama a la función "saludar" y muestra el mensaje "Hola, ¿cómo estás?"
            ```
            
        3. Pasar argumentos a una función:
        Puedes pasar argumentos a una función en Bash, que se pueden utilizar dentro de la función como variables especiales. Los argumentos se acceden mediante **`$1`**, **`$2`**, **`$3`**, y así sucesivamente, dependiendo de la posición del argumento.
            
            ```bash
            nombre_de_la_funcion() {
                echo "El primer argumento es: $1"
                echo "El segundo argumento es: $2"
            }
            ```
            
            Ejemplo:
            
            ```bash
            mostrar_argumentos() {
                echo "El número de argumentos es: $#"
                echo "Los argumentos pasados son: $@"
            }
            mostrar_argumentos 1 2 3  # Muestra el número de argumentos y los argumentos pasados
            ```
            
        4. Devolver valores desde una función:
        Puedes devolver un valor desde una función en Bash utilizando el comando **`return`**. El valor de retorno se puede acceder utilizando la variable especial **`$?`** después de llamar a la función.
            
            ```bash
            nombre_de_la_funcion() {
                # Comandos
                return valor_de_retorno
            }
            ```
            
            Ejemplo:
            
            ```bash
            sumar() {
                local resultado=$(( $1 + $2 ))
                return $resultado
            }
            sumar 5 3
            echo "El resultado de la suma es: $?"  # Muestra el valor de retorno de la func
            ```
            
            Si tienes varias funciones en Bash y quieres llamar una función específica para obtener su valor de retorno, puedes almacenar el valor de retorno en una variable y luego usar esa variable según sea necesario. Aquí tienes un ejemplo de cómo hacerlo:
            
            ```bash
            funcion1() {
                # Comandos
                return 10
            }
            
            funcion2() {
                # Comandos
                return 20
            }
            
            # Llamar a la función1 y almacenar el valor de retorno en una variable
            resultado_funcion1=$(funcion1)
            
            # Llamar a la función2 y almacenar el valor de retorno en otra variable
            resultado_funcion2=$(funcion2)
            
            # Utilizar los valores de retorno almacenados en las variables
            echo "El valor de retorno de la función1 es: $resultado_funcion1"
            echo "El valor de retorno de la función2 es: $resultado_funcion2"
            ```
            
            En el ejemplo anterior, las funciones **`funcion1`** y **`funcion2`** devuelven valores de retorno diferentes. Al llamar a cada función y almacenar su valor de retorno en una variable separada, puedes usar esas variables más adelante en tu script según sea necesario.
            
            Recuerda que el valor de retorno de una función en Bash se puede acceder utilizando la variable especial **`$?`**, pero al almacenarlo en una variable explícita, puedes evitar posibles conflictos o sobrescrituras de **`$?`** en otras partes de tu script.
            
    - **Variables en funciones**
        
        En Bash, existen dos tipos principales de variables que se utilizan en las funciones: las variables locales y las variables globales.
        
        1. Variables locales:
        Las variables locales son aquellas que están definidas y son accesibles solo dentro del ámbito de una función en particular. Esto significa que no se pueden acceder a ellas desde fuera de la función ni desde otras funciones. Las variables locales se declaran utilizando la palabra clave **`local`** seguida del nombre de la variable y su valor opcional.
            
            Ejemplo:
            
            ```bash
            mi_funcion() {
                local variable_local="Hola, soy una variable local"
                echo $variable_local
            }
            mi_funcion  # Llama a la función y muestra el valor de la variable local
            
            echo $variable_local  # Esto dará como resultado un error, ya que la variable_local es local a la función y no está disponible fuera de ella
            ```
            
        2. Variables globales:
        Las variables globales son aquellas que están definidas fuera de las funciones o en el ámbito global del script. Estas variables son accesibles desde cualquier parte del script, incluidas las funciones. Si se modifica el valor de una variable global dentro de una función, ese cambio será visible fuera de la función.
            
            Ejemplo:
            
            ```bash
            variable_global="Hola, soy una variable global"
            
            mi_funcion() {
                variable_global="Hola, he sido modificada"
                echo $variable_global
            }
            
            echo $variable_global  # Muestra el valor original de la variable global
            mi_funcion  # Llama a la función y modifica la variable global
            echo $variable_global  # Muestra el nuevo valor de la variable global
            ```
            
        
        Es importante tener en cuenta que si una variable local en una función tiene el mismo nombre que una variable global fuera de la función, la variable local tendrá prioridad dentro de la función. La variable global no se modifica a menos que se haga explícitamente dentro de la función utilizando la palabra clave **`global`**.
        
        Las variables en las funciones te permiten almacenar y manipular datos de forma local o global según sea necesario. Utilizar variables locales dentro de las funciones ayuda a encapsular los datos y evitar posibles conflictos con otras partes del script. Mientras que las variables globales proporcionan una forma de compartir datos entre funciones y el ámbito global del script.
        

95. **Verificar archivos y directorios**:
    
    Los siguientes scripts son ejemplos de comandos en Bash para verificar la existencia de directorios y archivos, y realizar operaciones relacionadas con ellos. Aquí está una explicación de cada uno de los scripts:
    
    1. Verificar directorio:
    Este script solicita al usuario que ingrese un nombre de directorio y luego verifica si el directorio existe utilizando la condición **`d`**. Si el directorio existe, muestra un mensaje indicando su existencia; de lo contrario, muestra un mensaje indicando que el directorio no existe.
        
        ```bash
        # Verificar directorio
        echo "Ingresa el nombre del directorio:"
        read directory
        
        if [ -d $directory ]; then
             echo "El directorio $directory existe"
        else
             echo "El directorio $directory no existe"
        if
        ```
        
    2. Verificar archivo:
    Este script solicita al usuario que ingrese un nombre de archivo y luego verifica si el archivo existe utilizando la condición **`f`**. Si el archivo existe, muestra un mensaje indicando su existencia; de lo contrario, muestra un mensaje indicando que el archivo no existe.
        
        ```bash
        # Verificar archivo
        echo "Ingresa el nombre del archivo:"
        read archivo
        
        if [ -f $archivo ]; then
             echo "El archivo $archivo existe"
        else
             echo "El archivo $archivo no existe"
        if
        ```
        
    3. Verificar archivo e ingresar información:
    En este script, el usuario ingresa un nombre de archivo y se verifica si el archivo existe utilizando la condición **`f`**. Si el archivo existe, solicita al usuario que ingrese contenido para el archivo utilizando el comando **`read contenido`**. Luego, agrega el contenido ingresado al archivo utilizando **`echo $contenido >> $archivo`**. Si el archivo no existe, muestra un mensaje indicando que el archivo no existe.
        
        ```bash
        # Verificar archivo e ingresar información
        echo "Ingresa el nombre del archivo:"
        read archivo
        
        if [ -f $archivo ]; then
             echo "Ingresa el contenido para el archivo:"
             read contenido
             echo $contenido >> $archivo
        else
             echo "El archivo $archivo no existe"
        if
        ```
        
    4. Leer cada línea de un archivo:
    En este script, el usuario ingresa un nombre de archivo y se verifica si el archivo existe utilizando la condición **`f`**. Si el archivo existe, se utiliza un bucle **`while`** para leer cada línea del archivo y mostrarla utilizando **`echo $line`**. El archivo se redirige como entrada para el bucle utilizando **`done < $archivo`**. Si el archivo no existe, muestra un mensaje indicando que el archivo no existe.
        
        ```bash
        # Leer cada línea de un archivo
        echo "Ingresa el nombre del archivo:"
        read archivo
        
        if [ -f $archivo ]; then
             while IFS= read -r line; do
                  echo $line
             done < $archivo
        else
             echo "El archivo $archivo no existe"
        if
        ```
        
    - **Explicación de IFS**
        
        En Bash, el IFS (Internal Field Separator) es una variable especial que determina cómo se separan y separamos los campos (o elementos) en una cadena de texto. Por defecto, el valor de IFS está configurado en un espacio en blanco (espacio, tabulación o salto de línea), lo que significa que Bash divide una cadena en campos utilizando estos caracteres como delimitadores.
        
        El IFS se utiliza en varios contextos, como en bucles **`for`**, lectura de archivos y al trabajar con variables que contienen múltiples valores separados por el IFS.
        
        Aquí hay algunas formas comunes en las que se utiliza el IFS:
        
        1. Lectura de líneas de un archivo:
            
            ```bash
            while IFS= read -r line; do
                # Hacer algo con cada línea
                echo "$line"
            done < archivo.txt
            ```
            
            En este caso, el IFS se establece en un valor nulo (**`IFS=`**) antes de la lectura de la línea para evitar que los espacios en blanco al comienzo o al final de la línea se recorten.
            
        2. División de una cadena en campos:
            
            ```bash
            str="apple,banana,orange"
            IFS=',' read -ra fields <<< "$str"
            for field in "${fields[@]}"; do
                echo "$field"
            done
            ```
            
            Aquí, el IFS se establece en una coma (**`IFS=','`**) para dividir la cadena **`$str`** en campos utilizando la coma como delimitador. Los campos se almacenan en el array **`fields`**, y luego se pueden iterar y procesar individualmente.
            
        3. Separación de valores en variables:
            
            ```bash
            values="1 2 3 4"
            IFS=' ' read -r var1 var2 var3 var4 <<< "$values"
            echo "$var1"
            echo "$var2"
            echo "$var3"
            echo "$var4"
            ```
            
            En este caso, el IFS se establece en un espacio en blanco (**`IFS=' '`**) para separar los valores en la variable **`$values`**. Cada valor se asigna a una variable separada (**`var1`**, **`var2`**, **`var3`**, **`var4`**), y luego se pueden utilizar según sea necesario.
            
        
        El IFS proporciona flexibilidad al especificar cómo se deben separar los campos en una cadena. Puedes cambiar el valor de IFS según tus necesidades para ajustar la forma en que Bash divide y procesa los campos en diferentes contextos.
        
        Cuando se establece el IFS en un determinado valor, actúa como un delimitador para separar campos en una cadena de texto. Cada vez que el IFS encuentra el delimitador especificado en la cadena, se produce una división y se considera un nuevo campo.
        
        Por ejemplo, si estableces el IFS en una coma (","), y tienes una cadena como "apple,banana,orange", el IFS interpretará las comas como puntos de división y generará tres campos separados: "apple", "banana" y "orange".
        
        Aquí tienes un ejemplo más claro:
        
        ```bash
        str="apple,banana,orange"
        IFS=',' read -ra fields <<< "$str"
        for field in "${fields[@]}"; do
           echo "$field"
        done
        ```
        
        El resultado de este script sería:
        
        ```bash
        apple
        banana
        orange
        ```
        
        Como puedes ver, el IFS toma la coma (",") como delimitador y divide la cadena en campos separados, que luego se almacenan en el array **`fields`**. Cada campo se muestra utilizando el bucle **`for`**.
        
        Por lo tanto, el IFS es una herramienta útil para especificar el delimitador que se utiliza para separar los campos en una cadena, y puede ajustarse según tus necesidades para realizar la división de manera adecuada.
        
    - **Diferencia de IFS y FS de awk**
        
        El IFS (Internal Field Separator) en Bash y el FS (Field Separator) en Awk son variables utilizadas para separar y delimitar campos en una cadena o línea de texto, pero hay algunas diferencias importantes entre ellos:
        
        1. Ámbito de aplicación:
            - IFS en Bash: El IFS es una variable especial en Bash que afecta la forma en que se dividen los campos en Bash, no solo en Awk. Se utiliza principalmente al leer líneas de archivos, dividir cadenas en campos y al utilizar la expansión de variables.
            - FS en Awk: El FS es una variable específica de Awk y solo se aplica dentro de los scripts de Awk. Se utiliza para separar y dividir campos cuando se procesan archivos o cadenas de texto utilizando Awk.
        2. Sintaxis de asignación:
            - IFS en Bash: Para asignar un valor al IFS en Bash, se utiliza la sintaxis **`IFS=valor`**, donde **`valor`** puede ser un carácter o una cadena de caracteres.
            - FS en Awk: Para asignar un valor al FS en Awk, se utiliza la sintaxis **`F valor`** en la línea de comandos de Awk o **`FS = valor`** dentro del script Awk. Aquí, **`valor`** generalmente es un carácter o una expresión regular.
        3. Comportamiento predeterminado:
            - IFS en Bash: El valor predeterminado de IFS en Bash es un espacio en blanco (espacio, tabulación o salto de línea), lo que significa que Bash divide los campos utilizando estos caracteres como delimitadores.
            - FS en Awk: El valor predeterminado de FS en Awk es un espacio en blanco (espacio o tabulación), lo que significa que Awk divide los campos en una línea de entrada utilizando estos caracteres como delimitadores.
        4. Impacto en el procesamiento de campos:
            - IFS en Bash: El IFS en Bash afecta a todos los comandos y expresiones que implican la separación de campos, como la lectura de líneas, la expansión de variables y la división de cadenas.
            - FS en Awk: El FS en Awk afecta directamente la forma en que Awk interpreta y divide los campos en cada línea de entrada procesada por el script Awk.
        
        En resumen, mientras que el IFS en Bash es una variable especial y tiene un impacto más amplio en el procesamiento de campos en Bash, el FS en Awk es una variable específica de Awk y se utiliza exclusivamente para definir cómo se separan los campos en Awk.
        

96. **Debugging en Bash**:
    - Explicación
        
        En Bash, existen comandos y opciones de depuración que te permiten rastrear y depurar el flujo de ejecución de un script. Aquí hay una explicación de los comandos de depuración más comunes:
        
        1. **`bash -x`**,  **`set -x`** o en el shebang añadir “-x” **`#!/bin/bash -x`** :
            
            El comando **`bash -x`** o la opción **`set -x`** se utilizan para activar el modo de depuración en Bash. Cuando se ejecuta un script con **`bash -x`** o se incluye **`set -x`** en el script, cada línea de comando se muestra antes de ser ejecutada. Esto es útil para comprender cómo se ejecuta el script y verificar si hay errores o problemas en el flujo de ejecución.
            
            Ejemplo:
            
            ```bash
            # Activar el modo de depuración
            set -x
            
            # Código del script
            echo "Hello, World!"
            variable=42
            echo "The value is $variable"
            
            # Desactivar el modo de depuración
            set +x
            ```
            
            Al ejecutar este script con **`bash -x`**, se mostrará cada línea de comando antes de ejecutarla. Por ejemplo:
            
            ```bash
            + echo 'Hello, World!'
            Hello, World!
            + variable=42
            + echo 'The value is 42'
            The value is 42
            ```
            
        2. **`set +x`**:
            
            La opción **`set +x`** se utiliza para desactivar el modo de depuración (**`set -x`**). Cuando el modo de depuración está activado, se muestran todas las líneas de comando. Al utilizar **`set +x`**, se desactiva la salida de depuración y el script se ejecuta normalmente sin mostrar las líneas de comando.
            
            Ejemplo:
            
            ```bash
            # Activar el modo de depuración
            set -x
            
            # Código del script
            echo "Hello, World!"
            variable=42
            echo "The value is $variable"
            
            # Desactivar el modo de depuración
            set +x
            ```
            
            Al ejecutar este script, se mostrarán todas las líneas de comando debido a **`set -x`**. Sin embargo, después de **`set +x`**, las líneas de comando ya no se mostrarán.
            
        
        Estos comandos y opciones de depuración son útiles para identificar problemas y errores en tus scripts de Bash, ya que te permiten ver el flujo de ejecución y verificar si las variables se están asignando correctamente o si hay errores en la lógica del script.
        
    
97. **Binwalk**: es una herramienta rápida y fácil de usar para analizar, realizar ingeniería inversa y extraer imágenes de firmware.
    
    
98. **Tabulación en Bash**: “alt + {” para tabular hacia atrás y “alt + }” para tabular hacia adelante.

99. **Conectarse por ssh proporcionando la contraseña**: 
    
    ```bash
    sshpass -p ‘password1’ ssh <user>@<ip>
    ```
    

100. **Formas de hacer un ping al localhost**:
	    - ping localhost
	    - ping 127.0.0.1
	    - ping 127.x.x.x
	    - ping 127.x
	    - ping 0x7f000001
    
101. **Darle la vuelta al contenido de un archivo**: **`tac <file>`**. El comando 'tac' le da la vuelta a todo el output, de forma que lo de arriba pasa a estar abajo y lo de abajo pasa a estar arriba, manteniendo las líneas legibles.

102. **Representar output con columnas**:
    
	    ```bash
	    cat file | column
	    ```
    
103. **Mostrar progreso dinámico y barras de progreso**:
    
	    Más info: [https://stackoverflow.com/questions/238073/how-to-add-a-progress-bar-to-a-shell-script](https://stackoverflow.com/questions/238073/how-to-add-a-progress-bar-to-a-shell-script)
	    
	    Se puede crear un contador en Bash que muestre el progreso o el valor de una variable dinámicamente durante la ejecución de un script. Una forma de hacerlo es utilizando secuencias de escape ANSI para actualizar el texto en la misma línea.
	    
	    Eje:
	    
	    ```bash
	    #!/bin/bash
	    
	    # Número total de iteraciones
	    total=100
	    
	    # Bucle para simular un proceso
	    for ((i=1; i<=$total; i++)); do
	        # Borra la línea actual
	        echo -ne "\r"
	    
	        # Calcula y muestra el progreso
	        echo -n "$i/$total"
	    
	        # Espera un breve momento para simular el proceso
	        sleep 0.1
	    done
	    
	    echo  # Salto de línea al final del proceso
	    ```
	    

104. **Comando nulo (NULL)**:
    
	    No tiene ningún efecto y no hace absolutamente nada.
	    
	    ```bash
	    # Es representado por dos puntos
	    :
	    ```
