---
title: "[Lab] Server-Side Request Forgery (SSRF)"
date: 2023-12-25
categories: [OWASPTop10]
tags: [owasptop10, hacking, php]
---




# Server-Side Request Forgery (SSRF)
    

## Introducción

Esta vulnerabilidad ocurre cuando una aplicación web permite hacer consultas HTTP del lado del servidor hacia un dominio arbitrario elegido por el atacante.

Esto le permite a un atacante hacer conexión con servicios de la infraestructura interna donde se aloja la web y exfiltrar información sensible.

# Primer escenario

![Untitled](../assets/OWASP-TOP-10/Server-Side Request Forgery (SSRF)/Untitled.png)

En este entorno habrá un entorno de producción el cual estará expuesto externamente por el puerto 80 y un entorno de pre-producción el cual no estará visible externamente, solo será accesible desde la propia máquina y usará el puerto 8089.

El objetivo de este lab es ver cómo se podría ver el contenido de entornos o puertos no disponibles externamente con SSRF.

### Direccionamiento

Máquina PRO: 172.17.0.2:80

Máquina PRE: 172.17.0.2:8089

## Instalación

```bash
# Descargar imagen
docker pull ubuntu:latest

docker run -dit --name SSRF_lab1 ubuntu
docker exec -it SSRF_lab1 bash
apt update
apt install nano apache2 php python3 lsof -y
service apache2 start
```

## Montado de la página de producción

```bash
cd /var/www/html
rm index.html
nano utility.php
```

Contenido de utility.php

```bash
<?php
	if(isset($_GET['url'])){
		$url = $_GET['url'];
		echo "\n[+] Listando el contenido de la página web" . 
		$url . "\n\n";
		include($url);
	} else {
		echo "\n[!] No se ha proporcionado ninguna URL";
	}
?>
```

Para que el include() muestre el contenido de la página web, se debe modificar el archivo “/etc/php/8.1/apache2/php.ini” para poner en On la directiva “allow_url_include”:

![Untitled](../assets/OWASP-TOP-10/Server-Side Request Forgery (SSRF)/Untitled 1.png)

Ahora al pasarle un valor al parámetro “url”, se vería de la siguiente manera:

![Untitled](../assets/OWASP-TOP-10/Server-Side Request Forgery (SSRF)/Untitled 2.png)

Ahora se creará el contenido del “pro.php”:

```bash
nano pro.php
```

Contenido de pro.php

```bash
Esto es una página en producción visible externamente
```

## Montado de la página de pre-producción

```bash
cd /tmp/
nano index.html
python3 -m http.server --bind 127.0.0.1

--bind <IP> # Sirve para ocultar la página externamente, 
# de forma que solo será visible por la máquina local
```

Contenido de la página index.html

```bash
Esto es una página de pre-producción NO visible externamente
```

Vista de la página de pre-producción sin ocultarla

![Untitled](../assets/OWASP-TOP-10/Server-Side Request Forgery (SSRF)/Untitled 3.png)

## Explotación

### SSRF - Primer escenario: Ejemplo #1

Enumeración:

Si se hace un escaneo con **nmap** a la máquina víctima a los puertos 80 y 8089, se muestra el siguiente resultado:

![Untitled](../assets/OWASP-TOP-10/Server-Side Request Forgery (SSRF)/Untitled 4.png)

Aunque el servicio está activo, el puerto 8089 se muestra cerrado debido a que se ocultó con el parámetro **`--bind`**. De forma que esta página solo será visible por la máquina local que “hostea” el servicio utilizando al interfaz de loopback (127.0.0.1), de hecho, no servirá aunque se use la IP de la interfaz de red de la propia máquina, en este caso la “172.17.0.0.2”.

En el servicio en producción, hay una utilidad que permite buscar URLs (utility.php), por lo que al ser el mismo servidor el que busca las direcciones URLs, se podría intentar buscar por puertos locales apuntando a la dirección IP loopback de la máquina local, esto con el objetivo de encontrar puertos abiertos con otros servicios que de otra manera no serían accesibles por otra máquina que no sea el propio servidor local (víctima).

Se puede utilizar wfuzz para fuzzear ese rango de puertos:

```bash
wfuzz -c -t 10 -z range,1-65535 "http://172.17.0.2/
utility.php?url=127.0.0.1:FUZZ"
```

![Untitled](../assets/OWASP-TOP-10/Server-Side Request Forgery (SSRF)/Untitled 5.png)

Debido a que muestra muchas respuestas que contiene la misma cantidad de líneas, se filtrará por resultados que contengan una cantidad de líneas diferente a eso:

```bash
wfuzz -c --hl=4 -t 10 -z range,1-65535 "http://172.17.0.2/
utility.php?url=http://127.0.0.1:FUZZ"
```

![Untitled](../assets/OWASP-TOP-10/Server-Side Request Forgery (SSRF)/Untitled 6.png)

Como se puede ver, encontró el puerto 8089 y de esta forma, al buscar esta dirección URL, se puede acceder al servicio oculto:

![Untitled](../assets/OWASP-TOP-10/Server-Side Request Forgery (SSRF)/Untitled 7.png)

# Segundo escenario

![Untitled](../assets/OWASP-TOP-10/Server-Side Request Forgery (SSRF)/Untitled 8.png)

En este caso, hay dos máquinas víctimas, una de ellas (PRO) tiene un servicio en producción por el puerto 80 y tiene dos interfaces de red, una con la dirección IP 172.17.0.2 y la otra con la 10.10.0.2 (PRE), esta última interfaz le permite comunicarse con la segundo máquina, la cual tiene un servicio en pre-producción por el puerto 8089 no visible externamente.

Por otro lado, se tiene la máquina de atacante (ATTACKER) la cual tendrá una dirección IP que le permita comunicarse con la primer máquina víctima, en este caso la 172.17.0.3, de forma que esta máquina no podrá conectarse con la segunda pero con SSRF se aprovechará del servidor web (PRO) para comunicarse con la máquina PRE.

### Direccionamiento

Máquina PRO: 172.17.0.2 10.10.0.2

Máquina PRE: 10.0.0.3

Máquina ATTACKER: 172.17.0.3

## Creación de subredes

```bash
# Primero se debe verificar que la imagen de ubuntu está 
# instalada y si no, usar el siguiente comando:
# docker pull ubuntu:latest

docker network ls
docker network prune

# Crear una interfaz de red
docker network create --drive=bridge SSRFnetwork 
--subnet=10.10.0.0/24
docker network ls
```

![Untitled](../assets/OWASP-TOP-10/Server-Side Request Forgery (SSRF)/Untitled 9.png)

Una vez creada la interfaz de red, se podrá asignar a los contendores.

## Despliegue de contenedores

Se desplegarán tres contenedores, el del atacante (ATTACKER), una máquina víctima (PRO) para simular el entorno de producción y una segunda máquina víctima que simulará el servicio de pre-producción (PRE).

### Máquina PRO

```bash
# Máquina víctima (PRO)
docker run -dit --name PRO ubuntu
```

Por defecto, al correr el contenedor, se le asigna una interfaz de red con una dirección IP pero se le puede agregar otra interfaz y en este caso será la que previamente se creó:

```bash
docker network connect SSRFnetwork PRO

docker exec -it PRO bash
apt update
apt install iproute2 nano php apache2 iputils-ping -y
service apache2 start
cd /var/www/html
rm index.html
nano utility.php
nano login.php
nano /etc/php/8.1/apache2/php.ini # allow_url_include = On
service apache2 restart
```

![Untitled](../assets/OWASP-TOP-10/Server-Side Request Forgery (SSRF)/Untitled 10.png)

Contenido de utility.php

```bash
<?php
	include($_GET['url']);
?>
```

Contenido de login.php

```bash
Esto es una página web de inicio de sesión en producción
```

### Máquina PRE

Ahora se creará la segunda máquina víctima pero esta vez se le especifica una interfaz de red para que no cree una por default.

```bash
docker run -dit --name PRE --network=SSRFnetwork ubuntu
docker exec -it PRE bash

apt update
apt install nano python3 iproute2 iputils-ping -y

ping -c1 10.10.0.2

cd /tmp/
nano index.html
python3 -m http.server 8089
```

![Untitled](../assets/OWASP-TOP-10/Server-Side Request Forgery (SSRF)/Untitled 11.png)

Contenido de index.html

```bash
Esto es una pagina de prueba que NO deberia ser visible
```

### Máquina ATTACKER

Por último se crea la máquina de atacante:

Para este lab no se puede usar la máquina principal en uso, este caso Parrot, para simularla como atacante ya que esta tiene conectividad con las dos máquinas víctimas pero se quiere solo tener conectividad con una de ellas (PRO), por lo que para hacerlo más realista, la máquina del atacante será un contenedor

```bash
docker run -dit --name ATTACKER ubuntu
docker exec -it ATTACKER bash

apt update
apt install iputils iproute2 curl -y
```

![Untitled](../assets/OWASP-TOP-10/Server-Side Request Forgery (SSRF)/Untitled 12.png)

![Interfaces de red de la máquina local Parrot](../assets/OWASP-TOP-10/Server-Side Request Forgery (SSRF)/Untitled 13.png)

Interfaces de red de la máquina local Parrot

Al hacer una prueba de conexión a las máquinas PRO y PRE, se obtiene lo siguiente:

![Untitled](../assets/OWASP-TOP-10/Server-Side Request Forgery (SSRF)/Untitled 14.png)

## Explotación

De primeras, si se intenta hacer un **curl** a la máquina PRE en la dirección “http://10.10.0.3” no habrá respuesta, sin embargo, si se aprovecha del archivo “utility.php” el cual espera un valor URL, como las peticiones las maneja el mismo servidor, se podría hacer peticiones a servicios internos los cuales no son accesibles de forma externa, por ejemplo, a la web de prueba de la máquina PRE de la siguiente manera:

```bash
curl -s http://172.17.0.2/utility.php?url=
http://10.10.0.3:8089
```
