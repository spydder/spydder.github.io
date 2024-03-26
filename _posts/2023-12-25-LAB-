---
title: "[Lab] CSS Injection (CSSI)"
date: 2023-12-25
categories: [OWASPTop10]
tags: [owasptop10, hacking, css]
---



# CSS Injection (CSSI)


## Instalación

```bash
git clone https://github.com/blabla1337/skf-labs
cd skf-labs/nodeJs/CSSI
npm install
npm start
```

## Explotación

![Untitled](../assets/OWASP-TOP-10/Inyecciones CSS (CSSI)/Untitled.png)

La página consiste en que el usuario puede elegir un color y este se mostrará en el texto “COLOR”, al ver el código fuente, se puede ver cómo se representa esto:

![Untitled](../assets/OWASP-TOP-10/Inyecciones CSS (CSSI)/Untitled 1.png)

![Untitled](../assets/OWASP-TOP-10/Inyecciones CSS (CSSI)/Untitled 2.png)

![Untitled](../assets/OWASP-TOP-10/Inyecciones CSS (CSSI)/Untitled 3.png)

Como se puede ver, se están empleado etiquetas “<style>” para establecer un color a la clase “colorful” y viendo que esto lo puede controlar el usuario, se podría intentar inyectar código CSS para cerrar la etiqueta “<style>” y abrir etiquetas <script> para inyectar código JavaScript y derivar el ataque a un XSS:

```bash
{% raw %}
red } </style> <script>alert("HOLA")</script>
{% endraw %}

# Primero se cierra la llave del estilo colorful 
# y se cierra la etiqueta <style> para instroducir 
# etiquetas <script></script>
```

![Untitled](../assets/OWASP-TOP-10/Inyecciones CSS (CSSI)/Untitled 4.png)

![Untitled](../assets/OWASP-TOP-10/Inyecciones CSS (CSSI)/Untitled 5.png)

![Untitled](../assets/OWASP-TOP-10/Inyecciones CSS (CSSI)/Untitled 6.png)
