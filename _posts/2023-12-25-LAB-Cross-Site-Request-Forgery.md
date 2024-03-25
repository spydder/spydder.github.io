---
title: "[Lab] Cross-Site Request Forgery"
date: 2023-12-25
categories: [OWASPTop10]
tags: [owasptop10, hacking]
---

# Cross-Site Request Forgery (CSRF)


## Instalación

```bash
wget https://seedsecuritylabs.org/Labs_20.04/Files/Web_CSRF_Elgg/Labsetup.zip
unzip Labsetup.zip
rm Labsetup.zip
cd Labsetup/
docker-compose up -d
```

Luego se debe modificar el archivo /etc/hosts para añadir las siguientes direcciones:

```bash
10.9.0.5 www.seed-server.com
10.9.0.5 www.example32.com
10.9.0.105 www.attacker32.com
```

Las credenciales de los usuarios son las siguientes

alice: seedalice

samy: seedsamy

## Explotación

El objetivo será aprovecharse de los campos de envío de mensajes y utilizar etiquetas HTML para que los usuarios víctima carguen imágenes que por detrás hará que hagan peticiones HTTP y modificar su información.

En este caso, el rol de atacante lo tendrá Samy y Alice será la víctima.

Como se puede ver, el usuario actualmente en uso es Samy, esta enviará un mensaje a Alice utilizando etiquetas las <h1></h1> gracias a que la página lo permite.

![Untitled](../assets/OWASP-TOP-10/Cross-Site Request Forgery (CSRF)/Untitled.png)

![Mensaje de Samy recibido por Alice](../assets/OWASP-TOP-10/Cross-Site Request Forgery (CSRF)/Untitled 1.png)

Mensaje de Samy recibido por Alice

Hay que tener en cuenta es que cada usuario tiene un identificador, si se hace hovering sobre “Add friend”, se puede ver el identificador del usuario:

![Untitled](../assets/OWASP-TOP-10/Cross-Site Request Forgery (CSRF)/Untitled 2.png)

También se puede cambiar el nombre del usuario, esto de cara a un CSRF podría ser interesante.

![Untitled](../assets/OWASP-TOP-10/Cross-Site Request Forgery (CSRF)/Untitled 3.png)

Para ver cómo se tramita esta petición, se utilizará Burpsuite

![Untitled](../assets/OWASP-TOP-10/Cross-Site Request Forgery (CSRF)/Untitled 4.png)

![Untitled](../assets/OWASP-TOP-10/Cross-Site Request Forgery (CSRF)/Untitled 5.png)

Se puede ver que la petición está viajando por POST, también hay otros campos en los cuales se tramitan los datos y dos de ellos parecen campos para identificadores de cada usuario.

Para testear el CSRF, primero se cambiará el método de envío por GET y debido a que hay campos para identificadores, se probará si estos campos son necesarios o no para el envío:

![Petición original](../assets/OWASP-TOP-10/Cross-Site Request Forgery (CSRF)/Untitled 6.png)

Petición original

![Petición modificada](../assets/OWASP-TOP-10/Cross-Site Request Forgery (CSRF)/Untitled 7.png)

Petición modificada

Se envió la petición y la respuesta fue exitosa, eso significa que los campos de identificación únicos no son necesarios para enviar la petición:

![Untitled](../assets/OWASP-TOP-10/Cross-Site Request Forgery (CSRF)/Untitled 8.png)

![Untitled](../assets/OWASP-TOP-10/Cross-Site Request Forgery (CSRF)/Untitled 9.png)

Otra cosa a tener en cuenta es que en la petición se puede especificar un ID, el cual corresponde al ID del usuario al cual se le cambiará la petición.

Sabiendo esto, qué pasaría si se hace que otro usuario tramite una petición por GET como esta, eso causaría que el usuario cambie el nombre de su cuenta. Esto se podría hacer aprovechándose del envío de mensajes con código HTML, haciendo que el usuario que reciba el mensaje cargue una imagen:

Primero se captura la petición que sirve para actualizar información del perfil de la cuenta:

![Untitled](../assets/OWASP-TOP-10/Cross-Site Request Forgery (CSRF)/Untitled 10.png)

Luego se cambia el método de envío de POST a GET y se elimina los campos identificadores innecesarios y por último, se debe cambiar el ID por el ID del usuario víctima ya que es el que cargará la petición. La petición debe verse como esta:

![Untitled](../assets/OWASP-TOP-10/Cross-Site Request Forgery (CSRF)/Untitled 11.png)

Ahora se procede a estructurar el mensaje que se le enviará a Alice, en este mensaje se utilizará la etiqueta <img /> para cargar una imagen la cual contendrá como “source” una petición por GET la cual se tramitará cuando el usuario víctima cargue la imagen. Cuando el usuario víctima cargue la imagen, como la imagen no existe, se verá un ícono de una imagen rota, para evitar esto y que no sea visible, se puede ajustar con width y height y establecerlos en 1.

![Untitled](../assets/OWASP-TOP-10/Cross-Site Request Forgery (CSRF)/Untitled 12.png)

![Mensaje recibido de Alice](../assets/OWASP-TOP-10/Cross-Site Request Forgery (CSRF)/Untitled 13.png)

Mensaje recibido de Alice

![Vista del perfil de Alice](../assets/OWASP-TOP-10/Cross-Site Request Forgery (CSRF)/Untitled 14.png)

Vista del perfil de Alice

Esto sucedió debido a que no hay ningún tipo de validación por parte del servidor y confía en el input del usuario.
