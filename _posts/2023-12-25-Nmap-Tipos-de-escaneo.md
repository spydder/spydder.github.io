---
title: "Nmap: Tipos de escaneo"
date: 2023-12-25
categories: [Reconocimiento]
tags: [reconocimiento, hacking]
image:
    path: /assets/NmapTiposDeEscaneo/nmap.png
---

## Comandos de Nmap

```bash
# Especificar un rango de puertos
nmap -p1-500 12.12.12.5

# Especificar todos los puertos
nmap -p- 12.12.12.5

# Especificar puertos más comunes
nmap --top-ports 500 12.12.12.5

# Mostrar solo puertos abiertos
nmap --open 12.12.12.5

# Mostrar más información durante la ejecución del escaneo
nmap -v 12.12.5

# Omitir resolución DNS
nmap -n 12.12.12.5

# Incorporar plantilla de temporizado 
# (0:Paranoid|1:Sneaky|2:Polite|3:Normal|4:Agressive|5:Insane)
nmap -T5 12.12.12.5

# Centrar el escaneo a puertos TCP
# El usuario enviará un SYN a la máquina destino y esta 
# responderá con un RST si el puertos está cerrado 
# y con SYN/ACK si el puerto está abierto y el usuario responderá
# de vuelta con un ACK
nmap -sT 12.12.12.5

# Omitir descubrimiento por ARP
nmap -Pn 12.12.12.5

# Hacer un escaneo UDP
nmap -sU 12.12.12.5

# Hacer un escaneo SCTP
nmap -sY 12.12.12.5

# Hacer barrido de pings a todos los destinos para saber si están
# activos o no
nmap -sn 12.12.12.0/24
nmap -sn 12.12.12.0/24 | grep -oP '\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}'
nmap -sn 12.12.12.0/24 | awk '{print $5}' | grep '[09]'

# Identificar el sistema operativo de la máquina destino
nmap -O 12.12.12.5

# Identificar versión de los servicios
nmap -p22,53 -sV 12.12.12.5

# Utilizar scripts básicos de reconocimiento
nmap -p80 -sC 12.12.12.5

# Scripts NSE
	# Pequeño fuzzer de directorios
	nmap --script=http-enum 12.12.12.5

	# Analizador de vulnerabilidades
	nmap --script vulners [--script-args mincvss=<arg_val>] 12.12.12.5
	mincvss= # restringe la salida solo a aquellos CVE que tienen una 
					 # puntuación CVSS más alta que la especificada (eje: 4).

# Escanear varios hosts
nmap 12.12.12.*
nmap 12.12.12.20-30
nmap 12.12.12.20,21,25,60
nmap 12.12.10-20.*
nmap -iL hosts.txt

# Métodos de exportación
nmap 12.12.12.5 -oN # Formato normal
nmap 12.12.12.5 -oG # Formato Grepeable
nmap 12.12.12.5 -oX # Formato xml
# Con "xsltproc captura.xml > index.html" se puede visualizar la 
# captura en el navegador
```

*Que un puerto esté filtrado, no significa que no se pueda conectar a él. En caso que no haya o ya se hayan probado todos los puertos abiertos, como última instancia se puede ir probando con los filtrados.*

## Ejemplo de paquetes SYN/ACK y RST en Wireshark

Primero se verifica la interfaz de red que se usará con **iwconfig**, en este caso será la “enp0s3”**:**

![](../assets/NmapTiposDeEscaneo/Untitled.png)

Se hará uso de la herramienta **tcpdump** para capturar los paquetes de esa interfaz e ingresarlo en un archivo:

```bash
tcpdump -I enp0s3 -w capture.cap -v

# Si la tarjeta de red no soporta el modo monitor, ejecutar el
# comando sin especificarla
tcpdump -w capture.cap -v
```

Mientras este está en escucha, se ejecutará nmap para enviar paquetes TCP utilizando el parámetro -sT el cual hará el proceso de handshake con el destino, el cual mostrará SYN/ACK si el puerto está abierto y RST si el puerto está cerrado:

```bash
# Prueba con puerto abierto
nmap -p53 -sT 192.168.0.1 -v -n

# Prueba con puerto cerrado
nmap -p123 -sT 192.168.0.1 -v -n
```

Una vez terminado el escaneo de nmap, se termina el proceso de tcpdump y se verifica el archivo creado. Ahora se procede a abrir el Wireshark utilizando este nuevo archivo de captura:

```bash
wireshark capture.cap &> /dev/null & disown
```

Una vez abierto el archivo en Wireshark, se filtra por el puerto TCP usado y se verifican los paquetes enviados y recibidos:

**Si el puerto está abierto**:

![](../assets/NmapTiposDeEscaneo/Untitled1.png)

**Si el puerto está cerrado**:

![](../assets/NmapTiposDeEscaneo/Untitled2.png)

## Enumeración de puertos usando descriptores de archivo
Una alternativa a la enumeración de puertos utilizando herramientas externas es aprovechar el poder de los descriptores de archivo en sistemas Unix. Los descriptores de archivo son una forma de acceder y manipular archivos y dispositivos en sistemas Unix. En particular, la utilización de /dev/tcp permite la conexión a un host y puerto específicos como si se tratara de un archivo en el sistema.


El siguiente es un script básico en bash para descubrir puertos abiertos usando descriptores de archivos:

```bash
#!/bin/bash

function ctrl_c(){
	echo -e "\n[!] Exiting..."
	tput cnorm; exit 1
}

#Ctrl + C
trap ctrl_c SIGINT

declare -a ports=( $(seq 1 65535) )

function checkPort(){
	(exec 3<> /dev/tcp/1$/$2/) 2>/dev/null

	if [ $? -eq 0 ]; then
		echo "[+] Host $1 - Port $2 (OPEN)"
	fi

	exec 3<&-
	exec 3<&-
}

tput civis # Ocultar cursor

if [ $1 ]; then
	for port in ${ports[@]}; do
		checkPort $1 $port &
	done
else
	echo -e "\n[!] Usage: $0 <ip-address>\n"
fi

wait

tput cnorm
```


