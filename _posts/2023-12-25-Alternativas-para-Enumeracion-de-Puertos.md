---
title: "Alternativas para la Enumeración de Puertos"
date: 2023-12-25
categories: [Reconnaissance]
tags: [reconnaissance, hacking]
---



# Enumeración de puertos usando descriptores de archivo
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

# fastTCPScan - Reconocimiento silencioso

fastTCPScan es una herramienta programada en Go que sirve para descubrir puertos abiertos, esta herramienta es más silenciosa que nmap, además, utiliza el lenguaje de programación Go el cual es un lenguaje útil para sockets y conexiones.

Más info: [https://s4vitar.github.io/fasttcpscan-go/#](https://s4vitar.github.io/fasttcpscan-go/#)

## Instalación

```bash
# El script se compila de la siguiente manera
go build -ldflags "-s -w" fastTCPScan.go
upx brute fastTCPScan
```

## Uso

```go
Usage of fastTCPScan:
  -host string
        Host o dirección IP a escanear (default "127.0.0.1")
  -range string
        Rango de puertos a comprobar: 80,443,1-65535,1000-2000, ... (default "1-65535")
  -threads int
        Número de hilos a usar (default 1000)
  -timeout duration
        Segundos por puerto (default 1s)
```
