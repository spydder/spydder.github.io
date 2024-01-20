---
title: "[Lab] Abuso de APIs"
date: 2023-12-25
categories: [OWASPTop10]
tags: [owasptop10, hacking, api]
---

# Abuso de APIs

### Más info:

- Links
    
    techbear - (”[https://techbear.co/install-postman-debian-linux/](https://techbear.co/install-postman-debian-linux/)”)
    
    aws - (”[https://aws.amazon.com/es/what-is/api/](https://aws.amazon.com/es/what-is/api/)”)
    
    digitalocean - (”[https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-compose-on-ubuntu-20-04](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-compose-on-ubuntu-20-04)”)
    
    cloudGoogle - (”[https://cloud.google.com/apigee/docs/api-platform/security/oauth/using-jwt-oauth?hl=es-419](https://cloud.google.com/apigee/docs/api-platform/security/oauth/using-jwt-oauth?hl=es-419)”)
    

## ¿Qué es una API?

Una API es una pieza de código que permite a diferentes aplicaciones comunicarse entre sí y compartir información y funcionalidades. Una API es un intermediario entre dos sistemas, que permite que una aplicación se comunique con otra y pida datos o acciones específicas.

## ¿Qué es un EndPoint?

E**s una dirección de una API, o bien un backend que se encarga de dar respuesta a una petición**, mientras que una REST, en una API, es una interfaz que sirve para la conexión de varios sistemas. Se basa en HTTP y sirve para obtener y enviar datos e información.

## Instalación

```bash
# Antes de hacer esto, es recomendable eliminar todos los 
# contenedores, volumenes, imágenes e interfaces de docker
docker rm $(docker ps -a -q) --force
docker rmi $(docker images -q)
docker volume rm $(docker volume ls -q)
docker network prune
#~~#

# [IMPORTANTE} La versión de docker-compose debe ser mayor 
# a la 1.27.0.
# Para actualizar docker-compose, seguir las siguientes 
# instrucciones:
apt remove docker-compose
curl -L "https://github.com/docker/compose/releases/download/v2.16.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

# Una vez hecho esto, continuar con la instalación del lab:
git clone https://github.com/OWASP/crAPI
cd crAPI/deploy/docker
docker-compose pull
docker-compose -f docker-compose.yml --compatibility up -d
```

Si se generan errores en la instalación. se debe eliminar todo el proyecto e iniciar de nuevo.

## Enumeración de endpoints de API

### Endpoint - Login

Al intentar iniciar sesión, se puede ver en los headers una petición por POST a y un endpoint de la API

![Untitled](../assets/OWASP-TOP-10/Untitled.png)

En la que a nivel de datos, se está enviando una estructura json con el email y la contraseña del usuario.

![Untitled](../assets/OWASP-TOP-10/Untitled 1.png)

El servidor responde con un JWT (JSON Web Token) de sesión, esto se puede deducir por la estructura del token en el que parece tener tres partes separadas por punto:

- ¿Qué es un JWT (JSON Web Token)?
    
    Un JSON Web Token (JWT) es un estándar abierto (RFC 7519) que se utiliza para transmitir de forma segura información entre dos partes en forma de objeto JSON. Se utiliza comúnmente en aplicaciones web y servicios API para autenticar y autorizar a los usuarios.
    
    El JWT consta de tres partes separadas por puntos: el encabezado (header), la carga útil (payload) y la firma.
    
    El encabezado contiene información sobre el algoritmo de firma utilizado y el tipo de token. El payload contiene los datos que se transmiten, como el ID de usuario, roles o cualquier otra información adicional.
    
    La firma se crea a partir del encabezado, la carga útil y una clave secreta conocida solo por el servidor que emite el token. La firma garantiza que el token no haya sido manipulado durante la transmisión y que provenga de una fuente confiable.
    
    El JWT es utilizado principalmente para autenticación y autorización. Una vez que un usuario inicia sesión en una aplicación, el servidor genera un JWT que contiene información sobre el usuario y sus permisos. El JWT se envía al cliente y luego se incluye en las solicitudes posteriores como una forma de identificación. Esto permite al servidor verificar la identidad del usuario y garantizar que tenga los permisos adecuados para acceder a los recursos o realizar ciertas acciones.
    
    El uso de JWT presenta varias ventajas. Algunas de ellas son:
    
    1. Portabilidad: Los JWT se pueden enviar a través de diferentes sistemas y plataformas, lo que los hace útiles en arquitecturas distribuidas.
    2. Autenticación sin estado: Los JWT contienen toda la información necesaria para autenticar y autorizar a un usuario, lo que significa que el servidor no necesita mantener un estado de sesión. Esto simplifica el escalado y la administración del servidor.
    3. Seguridad: La firma del JWT asegura que no haya sido alterado por terceros. Además, al utilizar el protocolo HTTPS para transmitir el JWT, se garantiza la confidencialidad de los datos.
    4. Extensibilidad: El JWT permite incluir cualquier tipo de información en el payload, lo que brinda flexibilidad para personalizar los datos transmitidos.
    
    En resumen, un JSON Web Token es una forma segura y eficiente de transmitir información entre dos partes, generalmente utilizada para autenticación y autorización en aplicaciones web y servicios API.
    

![Untitled](../assets/OWASP-TOP-10/Untitled 2.png)

![Untitled](../assets/OWASP-TOP-10/Untitled 3.png)

### Endpoint - Dashboard

En el dashboard se está enviando una petición por GET a una ruta del endpoint.

![Untitled](../assets/OWASP-TOP-10/Untitled 4.png)

En la respuesta del servidor, se proporciona información del usuario autenticado esto debido a que el usuario tiene un JWT que lo identifica:

![Untitled](../assets/OWASP-TOP-10/Untitled 5.png)

Y a nivel de headers, se puede ver el campo “Authorization” el cual contiene el JWT del usuario con el tipo “Bearer”:

![Untitled](../assets/OWASP-TOP-10/Untitled 6.png)

## Postman - Instalación

- ¿Qué es Postman?
    
    Postman **es una plataforma que permite y hace más sencilla la creación y el uso de APIs**. Esta herramienta es muy útil para programar porque da la posibilidad hacer pruebas y comprobar el correcto funcionamiento de los proyectos que realizan los desarrolladores web.
    

Esta herramienta servirá para ver todos estas peticiones a los endpoints de una manera más organizada.

**Instalación**

```bash
apt install snapd
cd /opt/
wget https://dl.pstmn.io/download/latest/linux64 -O postman-linux-x64.tar.gz
tar -xzf postman-linux-x64.tar.gz -C /opt
sudo ln -s /opt/Postman/Postman /usr/bin/postman

# Correr el programa
# Es recomendable correrlo como usuario no provilegiado.
postman
```

## Postman - Enumeración de Endpoints

Lo que se hará es representar todas las rutas o endpoints de API, para esto, se debe ir verificando y añadiendo todas las peticiones que se hacen al servidor, como logins, solicitudes de cambio de contraseña, compras, etc.

Por ejemplo, se añadirá la petición que se hace al iniciar sesión:

![Untitled](../assets/OWASP-TOP-10/Untitled 7.png)

Luego, en la interfaz de postman, se debe crear una nueva colección y añadir un “HTTP Request”

![Untitled](../assets/OWASP-TOP-10/Untitled 8.png)

![Untitled](../assets/OWASP-TOP-10/Untitled 9.png)

Aquí se debe pegar la dirección del endpoint, luego en el “Body” añadir los datos en raw que se están enviando al servidor, en este caso en formato json el email y la contraseña y poner el método POST, y por último, cambiar el tipo a JSON.

![Untitled](../assets/OWASP-TOP-10/Untitled 10.png)

Al enviar la petición, se puede ver el JWT proporcionado por el servidor.

![Untitled](../assets/OWASP-TOP-10/Untitled 11.png)

Ahora, se guarda la petición y se le asigna un nombre descriptivo.

![Untitled](../assets/OWASP-TOP-10/Untitled 12.png)

## Postman - Creación de variables

Crear una variable puede es útil ya que se podrían estar manejando peticiones en las que es necesario, por ejemplo, un JWT y de esta forma no es necesario estar especificándolo en cada petición que se crea manualmente.

Para crear una variable, se debe dar click en la colección, luego ir a variables y crear una variable con un nombre descriptivo, la cual se le dará como valor el JWT proporcionado por el servidor y hecho esto, se da click en guardar.

![Untitled](../assets/OWASP-TOP-10/Untitled 13.png)

Luego en la pestaña Authorization, se debe cambiar el tipo, en este caso es Bearer y en “Token”, se debe ingresar el token pero sse puede hacer alusión a él con doble llave: {{accessToken}}

![Untitled](../assets/OWASP-TOP-10/Untitled 14.png)

Esto servirá para que a futuras peticiones que se hagan, se arrastre este JWT.

## Enumerar métodos (GET, POST, DELETE, …)

Se puede intentar enumerar los métodos que acepta una petición, por ejemplo, se tiene una URL y se quiere conocer qué métodos acepta, para esto se puede utilizar la herramienta ffuf para fuzzear el método:

```bash
ffuf -u http://localhost:8888/workshop/api/sjop/products 
-w /usr/share/SecLists/Fuzzing/http-request-methods.txt 
-X FUZZ -p 1 -mc 401,200

-p # Indica cuánto tiempo en segundos se tomará para 
# cada respuesta
```

![Untitled](../assets/OWASP-TOP-10/Untitled 15.png)

También se podría mandar una petición con el método OPTIONS el cual sirve para que muestre qué métodos son permitidos para una petición:

![Untitled](../assets/OWASP-TOP-10/Untitled 16.png)

![Untitled](../assets/OWASP-TOP-10/Untitled 17.png)

## Vectores de ataque

Una vez representadas todas las peticiones a los endpoints, se puede pensar en cómo se podría aprovechar de ellas para efectuar algún ataque, la idea es conocer cómo se tramitan las peticiones y cómo responde el servidor para enumerar potenciales ataques. A continuación se mencionan algunos ejemplos:

- Ver cómo se tramitan las peticiones de cambio o recuperación de contraseñas, correos, etc.
- Ver cómo se tramitan los pagos, devoluciones, cupones, etc.
- Cambie el método de la petición y ver qué métodos están disponibles para un endpoint dado.
- Ver cómo se tramita la información de usuarios.
