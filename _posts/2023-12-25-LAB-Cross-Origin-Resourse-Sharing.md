---
title: "[Lab] Cross-origin resource sharing (CORS)"
date: 2023-12-25
categories: [OWASPTop10]
tags: [owasptop10, hacking, cors]
---


# Cross-origin resource sharing (CORS)
    

### Más info:

- Links
    
    PortSwigger - (”[https://portswigger.net/web-security/cors](https://portswigger.net/web-security/cors)”)
    
    developer - (”[https://developer.mozilla.org/es/docs/Glossary/Preflight_request](https://developer.mozilla.org/es/docs/Glossary/Preflight_request)”)
    

## Same-Origin Policy (SOP)

Por defecto, el navegador impone una política del "mismo origen" (same-origin policy), que restringe las solicitudes de recursos a un dominio específico. Esto significa que las páginas web solo pueden realizar solicitudes de recursos (como imágenes, scripts, estilos, etc.) al mismo dominio desde el cual se cargó la página pero no se pueden realizar solicitudes de recursos del dominio desde otro dominio diferente.

Por ejemplo, una web que es vulnerable a XSS, el atacante ingresa unas etiquetas <script> que contienen un atributo “src” que hace que el usuario que ejecute el código, cargue un dominio diferente (del atacante) en el que se definió un recurso JavaScript que hace que tramite una solicitud al sitio web vulnerable para que, por ejemplo, copie todo el código fuente de ese sitio y lo envié al atacante y en esa situación, es en esa situación en la que no será posible la acción debido a que son dominios diferentes, es decir, el dominio del atacante no puede traerse recursos de otro dominio por la política del “same-origin policy” o SOP.

## ¿Qué es CORS?

![Untitled](../assets/OWASP-TOP-10/Insecure Direct Object Reference (IDORs)/Untitled.png)

Sin embargo, en algunos casos, es necesario permitir que las páginas web realicen solicitudes a recursos en otros dominios. Aquí es donde CORS entra en juego. Permite que un servidor especifique qué dominios (o orígenes) tienen permiso para acceder a los recursos que ofrece. El servidor incluye encabezados de respuesta especiales en sus respuestas HTTP para indicar si los recursos pueden ser compartidos con el origen que realizó la solicitud.

Cuando una página web realiza una solicitud a otro dominio, el navegador realiza una "solicitud previa" (preflight request) enviando una solicitud HTTP OPTIONS al servidor para verificar si la solicitud está permitida. El servidor responde con encabezados que especifican los orígenes permitidos, los métodos HTTP permitidos, los encabezados permitidos, etc. Si el servidor aprueba la solicitud previa, el navegador realizará la solicitud real al recurso deseado.

Si el servidor no permite el acceso desde el origen que realiza la solicitud, el navegador bloqueará la respuesta y la página web no podrá acceder al recurso. Esto ayuda a prevenir ataques de tipo Cross-Site Scripting (XSS) y Cross-Site Request Forgery (CSRF).

## Instalación de lab

```bash
docker pull blabla1337/owasp-skf-lab:cors
docker run -ti -p 127.0.0.1:5000:5000 blabla1337/owasp-skf-lab:cors
```

La página web se ve de esta forma y las credenciales del usuario administrador son: “usuario: admin” y “contraseña: admin”.

![Untitled](../assets/OWASP-TOP-10/Insecure Direct Object Reference (IDORs)/Untitled 1.png)

El objetivo para este lab será crear un sitio web malicioso en el que habrá un recurso javaScript que cuando el usuario admin lo cargue, se copiará todo el código HTML del sitio web vulnerable en el que está logueado este usuario para mostrarlo en el sitio web del atacante. Esto será posible debido a la mala configuración del CORS en el sitio web ya que se está permitiendo cualquier origen, es decir, permite que cualquier dominio externo cargue recursos del propio sitio web.

Para demostrar la mala configuración de CORS, se interceptará una solicitud al sitio web con Burpsuite.

Al enviar la solicitud, se puede ver en los encabezados de la respuesta el “**`Access-Control-Allow-Credentials: true`**” y “**`Access-Control-Allow-Origin: *`**”, el primero permite el envío de credenciales como cookies en solicitudes HTTP y en el segundo se indican los dominios externos que se les permite hacer solicitudes a recursos del propio sitio web, en este caso se permite a cualquier dominio y esto es un problema ya que la idea es que se especifiquen solo los dominios que requieren de este permiso, no cualquiera.

![Untitled](../assets/OWASP-TOP-10/Insecure Direct Object Reference (IDORs)/Untitled 2.png)

El atacante ahora podría añadir un campo “Origin” en la cabecera de la solicitud para que el dominio que ingrese tenga permitido cargar recursos del sitio web vulnerable. Por ejemplo, se agrega un sitio web que el atacante controlará.

También es importante que el envío de credenciales esté en true ya que permite que la víctima arrastre su cookie de sesión en el caso que se quiera extraer información de un recurso del sitio web el cual se requiera una previa autenticación.

![Untitled](../assets/OWASP-TOP-10/Insecure Direct Object Reference (IDORs)/Untitled 3.png)

Ahora se creará el archivo malicioso que el usuario víctima cargará, en este caso se llamará “malicious.html” y servirá para que cuando el usuario víctima lo cargue, haga una petición al sitio web vulnerable y copie todo el código HTML para que lo replique en el sitio web del atacante:

- Código
    
    ```bash
    <script>
    	var req = new XMLHttpRequest();
    	req.onload = reqListener;
    	req.open('GET', 'http://localhost:5000/confidential', true);
    	req.withCredentials = true;
    	req.send();
    
    	function reqListener() {
    		document.getElementById("stolenInfo").innerHTML = req.responseText;
    	}
    </script>
    
    <br>
    <center><h1>Has sido hackeado y esta es la informaci&oacute;n que te he robado</h1></center>
    
    <p id="stolenInfo"></p>
    ```
    

![Untitled](../assets/OWASP-TOP-10/Insecure Direct Object Reference (IDORs)/Untitled 4.png)

- Explicación de código
    - **`req.onload = reqListener;`** → Esto indica qué pasará cuando el usuario cargue la página web y “reqListener” es la función en donde se orquestará esta función.
    - **`document.getElementById("info1")` →**  Es una llamada a la función **`getElementById`** del objeto **`document`**. Busca un elemento HTML en el documento con el atributo **`id`** igual a "info1". Esto devuelve una referencia al elemento encontrado.
    - **`.innerHTML`** → Es una propiedad del objeto del elemento HTML. Permite obtener o establecer el contenido HTML dentro del elemento.
    - **`req.responseText`** → Es una propiedad que contiene la respuesta de la solicitud XHR. **`req`** se asume como el objeto XHR previamente configurado y utilizado para hacer la solicitud. La propiedad **`responseText`** contiene los datos de respuesta devueltos por el servidor.

Ahora se crea un servidor HTTP en el directorio de este recurso y se simulará que el usuario admin, estando autenticado en el sitio web vulnerable, carga este recurso malicioso:

![Untitled](../assets/OWASP-TOP-10/Insecure Direct Object Reference (IDORs)/Untitled 5.png)

Como se puede ver, se hizo una replica del contenido HTML del sitio web en el sitio del atacante aunque solo se ve la estructura HTML pero se podrían descargar los recursos extra del sitio web para que se vea igual al original.

El atacante al compartir su sitio web con PHP, se pueden ver los logs generados por la víctima, entre ellos hay solicitudes a recursos no existentes en la máquina del atacante, estos recursos corresponden a los estilos de la página web por lo que se pueden crear las mismas carpetas en la máquina local y descargar los recursos de la página original:

```bash
php -S 0.0.0.0:80
```

![Untitled](../assets/OWASP-TOP-10/Insecure Direct Object Reference (IDORs)/Untitled 6.png)

```bash
# Creación de directorios
mkdir -p static/css/
mkdir -p static/img/

# Descarga de recuros de página web original
cd static/css/
wget http://localhost:5000/static/css/Normalize.css
wget http://localhost:5000/static/css/datepicker3.css
wget http://localhost:5000/static/css/bootstrap-table.css
wget http://localhost:5000/static/css/styles.css
cd static/img
wget http://localhost:5000/static/img/badge.svg
```

Con esto descargado ahora la página web debería verse de esta manera:

![Untitled](../assets/OWASP-TOP-10/Insecure Direct Object Reference (IDORs)/Untitled 7.png)

## ¿Cómo se vería esta petición por Burpsuite?

Al interceptar esta petición de la víctima al servidor web del atacante, se ven las siguientes solicitudes:

Primero hace una petición por GET al archivo “malicious.html“:

![Untitled](../assets/OWASP-TOP-10/Insecure Direct Object Reference (IDORs)/Untitled 8.png)

Luego este archivo malicioso hará que la víctima tramite una petición al recurso “confidential” del sitio web vulnerable para traerse todo el código HTML de esa web a la web del atacante, en esta petición se puede ver que automáticamente se establece el campo “Origin” con el dominio de la web atacante y como la web es vulnerable, en la respuesta se ve que establece este dominio en el campo “Access-Control-Allow-Origin” permitiendo a este dominio cargar recursos del sitio web.

![Untitled](../assets/OWASP-TOP-10/Insecure Direct Object Reference (IDORs)/Untitled 9.png)

## ¿Cómo mitigar esta vulnerabilidad?

En este caso, se cambiará la configuración del CORS del sitio web de este lab, para esto, hay que entrar a la consola del contenedor:

```bash
docker exec -it <IDContenedor> bash
```

Y en el directorio actual “home/apps/CORS” habrá un archivo “CORS.py” el cual contiene la configuración del CORS por lo que este será el archivo a editar.

En este archivo, al final del todo, se cambiarán los siguientes valores para especificar los dominios externos autorizados, por ejemplo, se ingresará el dominio “http://example.com” y se cambiará “Access-Control-Allow-Credentials” a “Access-Control-Allow-Origin”.

![Archivo original y vulnerable](../assets/OWASP-TOP-10/Insecure Direct Object Reference (IDORs)/Untitled 10.png)

Archivo original y vulnerable

![Archivo modificado](../assets/OWASP-TOP-10/Insecure Direct Object Reference (IDORs)/Untitled 11.png)

Archivo modificado

Ahora si la víctima vuelve a cargar la página del atacante, el campo “Access-Control-Allow-Origin” no será modificado y devolverá como respuesta los domonios autorizados de CORS, en este caso “http://example.com” ya que es el que está contemplado en la configuración del CORS.

![Untitled](../assets/OWASP-TOP-10/Insecure Direct Object Reference (IDORs)/Untitled 12.png)

La página web ya no se replica y en la consola se muestra un mensaje de error indicando el bloqueo por parte del CORS.

![Untitled](../assets/OWASP-TOP-10/Insecure Direct Object Reference (IDORs)/Untitled 13.png)

![Untitled](../assets/OWASP-TOP-10/Insecure Direct Object Reference (IDORs)/Untitled 14.png)

## Otras fallas en la configuración CORS

### Regex mal implementada:

Es posible que los administradores configuren el CORS de forma que en los dominios autorizados ingresen algo como esto:

```bash
*.example.com
```

Esto quizá haciendo alusión a subdominios pero esto es un problema ya que es una regex mal implementada porque el punto contempla un caracter de forma que un atacante podría utilizar un nombre de dominio con una sola letra al inicio como “Gexample.com” y ser válido y autorizado para hacer solicitudes.

### Descubrir subdominios con el campo “Origin”

Hay veces que al hacer una solicitud al servidor con un campo “Origin” con un dominio establecido, el campo “Access-Control-Allow-Origin” no se muestra en la respuesta, esto puede deberse a que el dominio del campo “Origin” proporcionado no es válido de forma que solo se mostrará solo cuando el dominio es válido y esto puede ser una forma potencial para descubrir subdominios.
