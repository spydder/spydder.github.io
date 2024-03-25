---
title: "[Lab] Insecure Direct Object Reference (IDORs)"
date: 2023-12-25
categories: [OWASPTop10]
tags: [owasptop10, hacking, idor]
---

# Insecure Direct Object Reference (IDORs)


## Lab - XVWA
Link de descarga: [https://www.vulnhub.com/entry/xtreme-vulnerable-web-application-xvwa-1,209/](https://www.vulnhub.com/entry/xtreme-vulnerable-web-application-xvwa-1,209/)

El objetivo de este lab es mostrar cómo se puede acceder a recursos “ocultos” o no destinados para ser mostrados al usuario cambiando el id del producto:

En este caso se tiene una página web en la que se puede mostrar diferentes tipos de café por medio de su código, es decir, al poner el código número 1, se mostrará información relacionada al café con ese código y así con todos los demás y así hasta el 5. Además, se puede ver un parámetro que toma este código en la URL:

![Untitled](../assets/OWASP-TOP-10/Insecure Direct Object Reference (IDORs)/Untitled.png)

![Untitled](../assets/OWASP-TOP-10/Insecure Direct Object Reference (IDORs)/Untitled 1.png)

Al ver esto, el atacante puede intentar cambiar el valor que se ingresa en este parámetro con otros números aunque no estén contemplados en la misma página web, por ejemplo el 7:

![Untitled](../assets/OWASP-TOP-10/Insecure Direct Object Reference (IDORs)/Untitled 2.png)

Esta es una muestra de las vulnerabilidades IDORs en las cuales se puede listar más información de la debida cambiando este identificador de producto para intentar listar otros no contemplados por la web.

## Lab - SKF

```bash
git clone https://github.com/blabla1337/skf-labs
cd skf-labs/nodeJs/IDOR
npm install
nom start
```

Una vez instalado, la web debería estar corriendo en la dirección del localhost por el puerto 5000

El objetivo de este lab es el mismo que el anterior en el sentido de ver información no destinada para que cualquier usuario la vea. En esta página web, se permite ingresar un mensaje y con base en ello, se creará un archivo PDF con un identificador único el cual posteriormente se puede usar para descargar el archivo PDF

![Untitled](../assets/OWASP-TOP-10/Insecure Direct Object Reference (IDORs)/Untitled 3.png)

![Untitled](../assets/OWASP-TOP-10/Insecure Direct Object Reference (IDORs)/Untitled 4.png)

En este lab, se aplicará la misma idea de buscar por otros identificadores para descubrir nuevos archivos a los cuales no se debería tener acceso pero esta vez con un fuzzer como wfuzz.

En este caso en concreto cuando se ingresa un identificador el cual no contiene ningún archivo PFD, se muestra un mensaje que indica un rango de IDs el cual servirá al crear el payload para el fuzzing:

![Untitled](../assets/OWASP-TOP-10/Insecure Direct Object Reference (IDORs)/Untitled 5.png)

Ahora se utilizará la herrmienta wfuzz para fuzzear por IDs válidos para lograr ver el contenido de estos.

```bash
wfuzz
```

Se descubrieron dos IDs de documentos, el “839” y el “1243”, el cual es el que se creó previamente pero al ver el ID del documento “839”, se puede ver el siguiente mensaje:

```bash
wfuzz -c --hh=10703,1233 -t 5 -z range,1-1500 
-d 'pdf_id=FUZZ' -X POST http://localhost:5000/download
```

![Untitled](../assets/OWASP-TOP-10/Insecure Direct Object Reference (IDORs)/Untitled 6.png)

![Untitled](../assets/OWASP-TOP-10/Insecure Direct Object Reference (IDORs)/Untitled 7.png)
