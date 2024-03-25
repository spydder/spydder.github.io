---
title: "[Lab] Explotación de SQUID Proxies"
date: 2023-12-25
categories: [OWASPTop10]
tags: [owasptop10, hacking, squid proxy]
---


# Explotación de SQUID Proxies
 

## Reconocimiento

```bash
arp-scan -I enp0s3 --localnet --ignoredups
ping -c1 192.168.0.101
nmap -p- -sS -n --open -vvv 192.168.0.101
nmap -sCV -p22,3128 -T5 192.168.0.101
```

Como se puede ver, se encontraron los puertos 22 y 3128 abiertos y este último corre el “Squid Proxy”.

![Untitled](../assets/OWASP-TOP-10/Explotación de SQUID Proxies/Untitled 1.png)

![Untitled](../assets/OWASP-TOP-10/Explotación de SQUID Proxies/Untitled 2.png)

Es posible que este escaneo no muestren todos los servicios corriendo internamente detrás del proxy, esto debido a que estos servicios no pueden ser accedidos si no es por medio del proxy, por lo que es necesario hacer peticiones por medio del Squid Proxy para conocer toda esta información, esto de primeras no es tan fácil ya que depende de la configuración de este y de cómo esté implementada la seguridad, es posible que incluso sea necesario autenticarse para utilizar este proxy.

Se puede tener una idea de si un puerto o servicio está corriendo si se hace un escaneo a ese puerto en específico y resulta en que el puerto está filtrado:

```bash
nmap -p80,81 192.168.0.101 -n -vvv
```

![Untitled](../assets/OWASP-TOP-10/Explotación de SQUID Proxies/Untitled 3.png)

## Curl - Peticiones con proxy

Se puede utilizar curl para hacer peticiones http utilizando un proxy, en este caso se hará una petición http a la dirección de la máquina víctima utilizando la dirección de su propio Squid Proxy.

```bash
# Sin autenticación
curl http://192.168.0.101 --proxy 192.168.0.101:3128

# Con autenticación
curl http://username:password@192.168.0.101 
--proxy 192.168.0.101:3128
```

![Untitled](../assets/OWASP-TOP-10/Explotación de SQUID Proxies/Untitled 4.png)

## Proxychains

Se puede utilizar proxychains para pasar transmitir peticiones por medio del proxy.

Primero se configura el archivo /etc/proxychains.conf para agregar una entrada para el proxy:

```bash
# En el archivo proxychains, ingresar:
http 192.168.0.101 3128
http 127.0.0.1 3128 # Interfaz local del proxy
```

Para comunicarse con las máquinas de la red interna del proxy, se debe ingresar una entrada para pasar por la interfaz local del proxy para después saltar a otras máquinas de la red interna con una segmentación diferente.

Ahora se puede hacer tramitar peticiones por medio del proxy, por ejemplo:

```bash
proxychains -q nmap -sT -Pn -vv -n 192.168.0.101
```

## Foxyproxy

También se podría hacer peticiones desde la aplicación web configurando, por ejemplo, el FoxyProxy para que haga peticiones a través del Squid Proxy de la máquina víctima.

![Untitled](../assets/OWASP-TOP-10/Explotación de SQUID Proxies/Untitled 5.png)

Ahora cuando se hagan peticiones http, estas pasarán por el Squid Proxy:

![Untitled](../assets/OWASP-TOP-10/Explotación de SQUID Proxies/Untitled 6.png)

Ahora que se tiene acceso a esta página web, se podría fuzzear nuevos directorios.

```bash
gobuster dir -t 5 -u http://192.168.0.101 
--proxy http://192.168.0.101:3128 -w /usr/share/wordlists/
SecLists/Discovery/Web-Content/directory-list-2.3-medium.txt
```

![Untitled](../assets/OWASP-TOP-10/Explotación de SQUID Proxies/Untitled 7.png)

Al revisar el directorio robots, se encuentra otra ruta la cual redirige a un CMS llamado Wolf:

![Untitled](../assets/OWASP-TOP-10/Explotación de SQUID Proxies/Untitled 8.png)

![Untitled](../assets/OWASP-TOP-10/Explotación de SQUID Proxies/Untitled 9.png)

## Script en Python - Descubrir puertos activos

Ya que se pueden hacer consultas a través del Squid Proxy del servidor víctima, se podría crear un script para hacer un escaneo de puertos abiertos internamente, es decir, servicios que como se mencionaba antes, no son visibles por medio de peticiones externas.

Este script hace múltiples peticiones http utilizando un conjunto de los 50 puertos más comunes, es decir, utiliza la URL “http://127.0.0.1:<puerto>” y le concatena cada puerto y, si este responde con un código de estado diferente de 503, se considera como abierto.

*Cabe recalar que a pesar de haber puertos los cuales no utilicen http, pueden ser descubiertos con peticiones http gracias a la cabecera en la respuesta.*

*Al estar tramitando peticiones a través del servidor proxy, las peticiones que se hagan a la dirección “127.0.0.1” harán referencia al servidor proxy en sí y no en la máquina local.*

```bash
#!/sr/bin/python3

import sys, signal, requests

def def_handler(sig, frame):
	print("\n[!] Exiting...\n\n")
	sys.exit(1)

# Ctrl+C
signal.signal(signal.SIGINT, def_handler)

main_url = "http://127.0.0.1"
squid_proxy = {'http': 'http://192.168.0.101:3128'}

def portDiscovery():

	common_tcp_ports = {20, 21, 22, 23, 25, 53, 67, 68, 69, 80, 88, 110, 119, 123, 135, 137, 139, 143, 161, 162, 179, 194, 389, 443, 445, 464, 465, 514, 515, 520, 521, 587, 631, 636, 993, 995, 1080, 1433, 1434, 1723, 1812, 1813, 2049, 2181, 3128, 3306, 3389, 5432, 5800, 5900, 8080}

	for tcp_port in common_tcp_ports:

		r = requests.get(main_url + ':' + str(tcp_port), proxies=squid_proxy)

		if r.status_code != 503:
			print("\n[+] Port " + str(tcp_port) + "-> OPEN")

if __name__ == '__main__':
	portDiscovery()
```

![Untitled](../assets/OWASP-TOP-10/Explotación de SQUID Proxies/Untitled 10.png)
