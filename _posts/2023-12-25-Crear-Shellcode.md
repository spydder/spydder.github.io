---
title: "Creación manual de Shellcodes"
date: 2023-12-25
categories: [Assembly]
tags: [shellcode, assembly, reverse shell]
image:
    path: /assets/thumbnail/assembly.png
---

# Introducción

    
Los **shellcodes** son programas pequeños y altamente optimizados que se utilizan para explotar vulnerabilidades de seguridad y ejecutar código malicioso en una máquina objetivo. Los shellcodes suelen ser escritos en lenguaje **ensamblador** para garantizar una ejecución rápida y eficiente.
    
En este ejercicio exploraremos cómo funcionan los shellcodes por detrás mediante la creación de algunos shellcodes manualmente. Por ejemplo, intentaremos crear un shellcode que muestre por consola el mensaje “**Hola mundo**” utilizando interrupciones del sistema. Asimismo, intentaremos aplicar una llamada a nivel de sistema para lograr ejecutar un comando deseado.
    
Una vez que se ha generado el compilado resultante, se puede utilizar el comando **objdump** para convertir el archivo binario generado en un shellcode que pueda ser utilizado en un Buffer Overflow.

De primeras, el shellcode cambiará dependiendo del sistema operativo.

Para crear un shellcode para una arquitectura de 32 bits para Linux, se podría utilizar msfvenom:

```bash
msfvenom -p linux/x86/exec CMD="echo 'Hola Mundo'" 
-f elf -o binary
```

Esto creará un archivo ejecutable que al ejecutarse, hará un echo. Al ver esto en formato raw, se puede ver cómo se estructura el binario, en este caso al ser para un sistema de 32 bits, las cadenas se ven al revés, es decir, se usa el formato “little endian”.

```bash
msfvenom -p linux/x86/exec CMD="echo 'Hola Mundo'" 
-f raw | xxd

# or

msfvenom -p linux/x86/exec CMD="echo 'Hola Mundo'" -f raw
```

![Untitled](/assets/Assembly/Create-Shellcode/Untitled.png)

Para ver esto desensamblado o en lenguaje ensamblador, se puede usar el siguiente comando:

```bash
msfvenom -p linux/x86/exec CMD="echo 'Hola Mundo'" 
-f raw | ndisasm -b12 -

# Para verlo con colores (son necesarias las librerías
# pwntools):
msfvenom -p linux/x86/exec CMD="echo 'Hola Mundo'" 
-f raw | disasm
```

![Untitled](/assets/Assembly/Create-Shellcode/Untitled 1.png)

## ¿Cómo crear un shellcode en ensamblador?

Primero se creará un binario en código ensamblador que ejecute un “echo ‘Hola Mundo’”, luego este binario se desensamblará para obtener el shellcode.

¿Para qué sirve el “int 0x80” o “int 80h”?

Es una instrucción que se usa en ensamblador para provocar una interrupción e invocar funciones, estas funciones sirven para acceder al hardware y se llaman servicios a programas o APIs.

En EAX se pone el número de servicio y luego se ponen los demás parámetros en los restantes registros: EBX, ECX, EDX, ESI, EDI y EBP.

Por ejemplo, en el binario anterior, se puede ver que está haciendo llamada al servicio “write” al cual se le están pasando ciertos parámetros:

```bash
strace binary
```

![Untitled](/assets/Assembly/Create-Shellcode/Untitled 2.png)

Se puede ver la estructura de write() en el manual:

```bash
man 2 write
```

![Untitled](/assets/Assembly/Create-Shellcode/Untitled 3.png)

La estructura se puede interpretar de la siguiente manera:

1. Indica la unidad de salida estándar. 1 indica que se utilizará la salida estándar normal, stdout. Esto se especifica en EBX.

2. Contiene la dirección en donde está contenida la cadena a mostrar. Esto se especifica en ECX.

3. Número máximo de caracteres a mostrar. Esto se especifica en EDC.

Además, en EAX se debe especificar el número del servicio al que se está llamando, en este caso el número del servicio es 4.

A continuación se pueden ver los diferentes servicios y su número de servicio:

```bash
cat /usr/include/asm/unistd_32.h

# Si no, se pude utilizar find para encontrar el archivo:
# find / -name unistd_32.h 2>/dev/null
```

![Untitled](/assets/Assembly/Create-Shellcode/Untitled 4.png)

## Creación del código en ensamblador

Se creará un archivo llamado “code.asm” con el siguiente código.

```nasm
section .text
  global _start

_start:
  mov eax, 4 ; sys_write
  mov ebx, 1 ; stdout
  push 0x0a6f64 ; do
  push 0x6e754d20 ; Mun
  push 0x616c6f48 ; Hola
  mov ecx, esp ; puntero a la cadena
  mov edx, 11 ; total de caracteres
  int 80h ; Interrupción
```

Explicación del código:

- **section .data**: En esta sección se encontrarán las sentencias que le indicarán al ensamblador qué hacer. Debe de contener la directiva **global _start** para indicarle al **kernel** en dónde iniciar la ejecución del programa.
- **Comentarios**: Los comentarios se ingresan utilizando “;”.
- Para hacer un llamado a la función “write” se debe hacer una interrupción utilizando “int 0x80” o “int 80h”.

Ahora la idea es indicar los valores para cada registro (EAX, EBX, ECX y EDX):

- Como se mencionó anteriormente, en el EAX se indicará el número del servicio al cual se llamará, en este caso “write” corresponde a 4 entonces “mov eax, 4”.
- En el EBX se indica la salida estándar, en este caso se usa 1 que corresponde a stdout, por lo tanto, “mov ebx, 1”.
- En el ECX se indica el puntero a donde se encuentra la cadena. Hay que recordar que el registro ESP apunta a la pila o “stack” y es aquí en donde se almacenará la cadena “Hola Mundo”. Para insertar la cadena en la pila, se puede utilizar “push”, esto insertará la cadena al tope de la pila.
    
    Para hacer esto, hay que dividir e insertar la cadena “Hola Mundo” de 4 en 4 bytes y cada grupo en hexadecimal en formato “little endian”, es decir, al revés y por último representar el final de la cadena con un null byte “push 0x0” o un salto de línea “push 0x0a”. El proceso es el siguiente:
    
    1. Dividir la cadena a insertar de 4 en 4 bytes:
    
    ```bash
    Hola Mundo → “Hola”, “ mun” y “do”
    ```
    
    2. Convertir cada cadena en hexadecimal:
    
    ```bash
    # Conversión de "Hola" en hexadecimal
    echo -n "Hola" | xxd -ps
    # Resultado:
    486f6c61
    
    # Representación de " Mun" en hexadecimal
    echo -n " Mun" | xxd -ps
    # Resultado:
    204d756e
    
    # Representación de "do" en hexadecimal
    echo -n "do" | xxd -ps
    # Resultado:
    646f
    ```
    
    3. Hacer un push de cada cadena en formato “little endian”, es decir, al revés, esto insertará la cadena al tope de la pila.
    
    Y al ingresar el ECX, se debe indicar la dirección en donde se encuentra la cadena y como esta está en la pila y el ESP apunta a la pila entonces se hace alusión al ESP:
    
    ```bash
    # Representación en ensamblador con formato 
    # little endian:
    push 0x0a6f64 # do con un salto de línea
    push 0x6e754d20 # Mun
    push 0x616c6f48 # Hola
    mov ecx, esp
    ```
    
- En EDX se indica la cantidad máxima de caracteres de la cadena incluyendo espacios y/o saltos de línea, entonces “mov edx, 11”.

Ahora hay que convertir el archivo en un archivo de objeto “.o” y a partir de él, crear un archivo “.elf”.

```bash
# Convertir a un archvio ".o"
nasm -f elf code.asm

# Crear el binario final
ld -m elf_i386 -o <nuevoNombrefinal> code.o
```

- Explicación de estos comandos
    
    El comando **`nasm -f elf code.asm`**, es un comando utilizado para ensamblar un archivo fuente escrito en lenguaje ensamblador utilizando la herramienta NASM (Netwide Assembler). Aquí está la explicación de cada parte del comando:
    
    1. **`nasm`**: Esto es simplemente el nombre del programa NASM, que es un ensamblador ampliamente utilizado para la arquitectura x86 y x86-64. NASM se utiliza para convertir código fuente en lenguaje ensamblador en código binario ejecutable.
    2. **`f elf`**: Esta parte del comando le indica a NASM qué formato de archivo de salida debe generar. En este caso, se está especificando el formato ELF (Executable and Linkable Format), que es un formato de archivo comúnmente utilizado en sistemas Unix y Linux para archivos ejecutables y objetos compartidos.
    3. **`code.asm`**: Aquí se proporciona el nombre del archivo fuente en lenguaje ensamblador que deseas ensamblar. En este caso, se asume que el archivo se llama "code.asm". Debes asegurarte de que este archivo exista en el directorio actual desde donde estás ejecutando el comando.
    
    Cuando se ejecuta el comando **`nasm -f elf code.asm`**, NASM genera un archivo de objeto con extensión **`.o`** (en este caso, "code.o"). Un archivo de objeto es un archivo intermedio que contiene el código ensamblador traducido a código de máquina y la información necesaria para que el enlazador (linker) pueda combinar varios archivos de objeto y bibliotecas para crear un programa ejecutable.
    
    El archivo de objeto (.o) contiene símbolos, direcciones de memoria y otras referencias que son necesarios para la fase de enlace. Sin embargo, el archivo de objeto por sí mismo no es un programa ejecutable; debe pasar por la fase de enlace para crear un archivo ejecutable.
    
    Para crear un programa ejecutable a partir del archivo de objeto, necesitas usar un comando de enlace, como el comando **`ld`** (Linker) en sistemas Unix y Linux. Por ejemplo:
    
    ```bash
    ld -m elf_i386 -s -o executable_name code.o
    ```
    
    En este comando, **`executable_name`** sería el nombre que deseas darle al archivo ejecutable final. La opción **`-m elf_i386`** indica la arquitectura objetivo, y la opción **`-s`** genera un archivo ejecutable más pequeño eliminando símbolos de depuración y otras informaciones innecesarias.
    
    En resumen, el archivo de objeto ("code.o") generado por NASM es una pieza intermedia en el proceso de compilación que contiene el código ensamblador traducido y la información necesaria para la creación de un programa ejecutable a través de la fase de enlace.
    

![Untitled](/assets/Assembly/Create-Shellcode/Untitled 5.png)

Al ejecutar el nuevo archivo ejecutable, se muestra el siguiente error:

![Untitled](/assets/Assembly/Create-Shellcode/Untitled 6.png)

Esto es porque cuando el EIP apunta a la pila, después ya no sabe a dónde apuntar por lo que se podría agregar una nueva interrupción en la cual se haga un llamado a la función “exit” para salir del proceso actual.

![Untitled](/assets/Assembly/Create-Shellcode/Untitled 7.png)

Esta función toma como parámetro un código de estado, en este caso se usará 0 ya que corresponde a un código de estado exitoso.

![Untitled](/assets/Assembly/Create-Shellcode/Untitled 8.png)

```nasm
section .text
  global _start

_start:
  mov eax, 4 ; sys_write
  mov ebx, 1 ; stdout
  push 0x0a6f64 ; do
  push 0x6e754d20 ; Mun
  push 0x616c6f48 ; Hola
  mov ecx, esp ; puntero a la cadena
  mov edx, 11 ; total de caracteres
  int 80h ; interruption

	; Salida del proceso actual
	mov eax, 1 ; sys_exit
	mov ebx, 0 ; or -> xor ebx, ebx
	int 80h
```

Ahora EAX debe tomar el valor de la función a la cual se llamará y 1 corresponde a la función “exit” y esta tomará como argumento un código de estado para concluir con el proceso actual, en este caso, 0 entonces “mov ebx, 0”, también se podría especificar utilizando un XOR, entonces “xor ebx, ebx” esto da como resultado 0 ya que todo número al que se le aplique un XOR a sí mismo, da como resultado 0.

![Untitled](/assets/Assembly/Create-Shellcode/Untitled 9.png)

## Obtención del shellcode

Una vez hecho el código en ensamblador ahora se obtendrá el shellcode.

Primero hay que desensamblar el código y quedarse solo con las cadenas en hexadecimal, es decir, la segunda columna:

```bash
objdump -d finalcode
```

![Untitled](/assets/Assembly/Create-Shellcode/Untitled 10.png)

```bash
printf '\\x' && objdump -d finalcode | grep "^ " | cut -f2 | tr -d '\n' | xargs | sed 's/ /\\x/g'

# forma alternativa:
printf '\\x' && objdump -d finalcode | grep "^ " | cut -f2 | tr -d ' ' | tr -d '\n' | sed 's/.\{2\}/&\\x /g' | head -c-3 | tr -d ' '; echo

# Resultado final:
\xb8\x04\x00\x00\x00\xbb\x01\x00\x00\x00\x68\x64\x6f\x0a\x00\x68\x20\x4d\x75\x6e\x68\x48\x6f\x6c\x61\x89\xe1\xba\x0b\x00\x00\x00\xcd\x80\xb8\x01\x00\x00\x00\x31\xdb\xcd\x80
```

![Untitled](/assets/Assembly/Create-Shellcode/Untitled 11.png)

## Extra: Código en ensamblador para obtener una “/bin/sh”

Para esto se llamará a la función “execve”

![Untitled](/assets/Assembly/Create-Shellcode/Untitled 12.png)

Esta función toma tres argumentos:

![Untitled](/assets/Assembly/Create-Shellcode/Untitled 13.png)

- pathname: Será la dirección del ESP porque es donde se pusheará la cadena de “/bin/sh”.
- Los demás argumentos en este caso no se usarán entonces tendrán como valor 0.

Para este binario, la idea será la misma al ejercicio anterior, se deben establecer los valores para los registros EAX, EBX, ECX y EDX.

```nasm
section .text
	global _start

_start:
	mov eax, 11 ; sys_execve
	push 0x0 ; null byte
	push 0x68732f2f ; //sh
	push 0x6e69622f ; /bin
	mov ebx, esp ; puntero a cadena "/bin//sh"
	mov ecx, 0 ; or -> xor ecx, ecx
	mov edx, 0 ; or -> xor edx, edx
	int 80h ; Interrupción
```

- Explicación de cada argumento de execve
    
    Claro, la función **`execve`** es una función de la biblioteca estándar en sistemas Unix y Linux que se utiliza para ejecutar un nuevo programa en lugar del programa actual. Esta función reemplaza el proceso actual con el nuevo programa especificado. A continuación, te explico los tres argumentos que toma la función **`execve`**:
    
    1. **pathname**: Este es un puntero a una cadena de caracteres que representa la ruta del archivo ejecutable del programa que deseas ejecutar. Debe ser la ruta completa o relativa del archivo ejecutable que quieres lanzar.
    2. **argv[]**: Este es un array (vector) de punteros a cadenas de caracteres, que representa los argumentos pasados al programa que será ejecutado. El último elemento de este array debe ser un puntero nulo (**`NULL`**), lo que indica el final de la lista de argumentos. Cada cadena en este array representa un argumento separado. Por ejemplo, si tu nuevo programa espera recibir argumentos como **`./mi_programa argumento1 argumento2`**, entonces **`argv`** debería contener punteros a las cadenas **`"./mi_programa"`**, **`"argumento1"`** y **`"argumento2"`**, seguidos de un puntero nulo.
    3. **envp[]**: Este es otro array de punteros a cadenas de caracteres que representa el entorno en el que se ejecutará el nuevo programa. El entorno es un conjunto de variables de entorno que afectan el comportamiento del programa. Al igual que con **`argv`**, el último elemento debe ser un puntero nulo (**`NULL`**) para indicar el final de la lista.
    
    La función **`execve`** no devuelve un valor en caso de éxito. Si la llamada a **`execve`** tiene éxito, el nuevo programa se ejecutará en lugar del programa actual. Si hay un error, la función **`execve`** puede devolver -1 y establecer la variable global **`errno`** para indicar el tipo de error que ocurrió.
    
    En resumen, **`execve`** se utiliza para reemplazar el proceso actual con un nuevo programa, y los argumentos **`pathname`**, **`argv`** y **`envp`** se utilizan para especificar la ruta del archivo ejecutable, los argumentos pasados al programa y el entorno en el que se ejecutará el programa, respectivamente.
    

Explicación del código:

- EAX tomará el número de la función que será llamada, en este caso el valor 11 corresponde a la función “execve”.
- EBX tomará el primer argumento de la función, este debe ser una dirección que apunte al binario que se ejecutará, en este caso será a la cadena “/bin/sh” y estará en la pila por lo que EBX deberá tomar la dirección del ESP.
    
    Esta cadena “/bin/sh”, será almacenada al tope de la pila por lo que será necesario hacer el mismo proceso que en el binario anterior:
    
    1. Dividir la cadena de 4 en 4 bytes. En este caso, la cadena “/sh” tiene tres caracteres pero se podría rellenar con una barra inclinada y su efecto será el mismo.
    
    ```bash
    # Cadena: /bin/sh
    # Cadena dividida: "/bin" "//sh"
    ```
    
    2. Ahora se debe pasar cada cadena a hexadecimal
    
    ```bash
    echo "/bin" | xxd -ps
    # Resultado:
    2f62696e
    
    echo "//sh" | xxd -ps
    # Resultado:
    2f2f7368
    ```
    
    3. Estas cadenas se deben pushear al tope de la pila, para hacer esto se deben ingresar sus caracteres en reversa y las cadenas en sí deben ir al revés, es decir, primero “//sh” y luego “/bin”. Además se debe agregar al final un null byte “0x0” para indicar el final de la cadena:
    
    ```bash
    push 0x0 # null byte
    push 0x68732f2f # //sh
    push 0x6e69622f # /bin
    mov ebx, esp # puntero hacia la cadena
    ```
    
- ECX y EDX no serán necesarios en este ejemplo por lo que se establece su valor en 0.
    
    Esto se puede expresar pasando el número 0 a cada registro o aplicando un XOR así mismo:
    
    ```bash
    mov ecx, 0
    mov edx, 0
    
    # or
    
    xor ecx, ecx
    xor edx, edx
    ```
    
    ## Obtención del shellcode
    
    Primero hay que desensamblar el código y quedarse solo con las cadenas en hexadecimal, es decir, la segunda columna:
    
    ```bash
    objdump -d shell
    ```
    
    ![Untitled](/assets/Assembly/Create-Shellcode/Untitled 14.png)
    
    ```bash
    printf '\\x' && objdump -d shell | grep "^ " | cut -f2 | xargs | sed 's/ /\\x/g'
    
    # Resultado:
    \xb8\x0b\x00\x00\x00\x6a\x00\x68\x2f\x2f\x73\x68\x68\x2f\x62\x69\x6e\x89\xe3\x31\xc9\x31\xd2\xcd\x80
    ```
    
    ![Untitled](/assets/Assembly/Create-Shellcode/Untitled 15.png)


### Más info:

- Links
    
    wikipedia - (”[https://es.wikipedia.org/wiki/Int_80h](https://es.wikipedia.org/wiki/Int_80h)”)
    
    codingornot - (”[https://codingornot.com/02-nasmx86-partes-del-codigo-fuente](https://codingornot.com/02-nasmx86-partes-del-codigo-fuente)”)
    
    codingornot - (”[https://codingornot.com/expresiones-regulares-i](https://codingornot.com/expresiones-regulares-i)”)
    
    nasm - (”[https://www.nasm.us/doc/nasmdocb.html](https://www.nasm.us/doc/nasmdocb.html)”)
    
    openxmldeveloper - (”[https://openxmldeveloper.org/understanding-the-difference-between-elf-and-exe-files/](https://openxmldeveloper.org/understanding-the-difference-between-elf-and-exe-files/)”)
