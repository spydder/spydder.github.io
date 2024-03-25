---
title: "[Lab] Explotación de Json Web Tokens (JWT)"
date: 2023-12-25
categories: [OWASPTop10]
tags: [owasptop10, hacking, jwt]
---

# Enumeración y explotación de Json Web Tokens (JWT)

### Más info:

- Links
    
    patzi(sergiodxa) - (”[https://platzi.com/blog/introduccion-json-web-tokens/#:~:text=Tiempo de expiración ( exp ) -,cuando fue creado el JWT](https://platzi.com/blog/introduccion-json-web-tokens/#:~:text=Tiempo%20de%20expiraci%C3%B3n%20(%20exp%20)%20%2D,cuando%20fue%20creado%20el%20JWT).”)
    
    pentesteracademy - (”[https://blog.pentesteracademy.com/hacking-jwt-tokens-bruteforcing-weak-signing-key-jwt-cracker-5d49d34c44](https://blog.pentesteracademy.com/hacking-jwt-tokens-bruteforcing-weak-signing-key-jwt-cracker-5d49d34c44)”)
    
    github(KINGSABRI) - (”https://github.com/KINGSABRI/jwtear”)
    

## Instalación

```bash
git clone https://github.com/blabla1337/skf-labs

# JWT-null
cd skf-labs/nodeJs/JWT-null
npm install
npm start

# JWT-secret
cd skf-labs/nodeJs/JWT-secret
npm install
npm start
```

![Untitled](../assets/OWASP-TOP-10/Explotación de Json Web Tokens (JWT)/Untitled.png)

Se tienen dos usuarios: “user” y “user2”, al darle a “Authenticate” se proporcionará un JWT y el botón “Admin” sirve para loguearse y mostrar el usuario actualmente en uso.

![Untitled](../assets/OWASP-TOP-10/Explotación de Json Web Tokens (JWT)/Untitled 1.png)

![Untitled](../assets/OWASP-TOP-10/Explotación de Json Web Tokens (JWT)/Untitled 2.png)

## JWT - Null Chiper

La idea para este lab es que siendo el usuario “guest”, se debe tratar de manipular el JWT para lograr loguearse como el usuario “mortal”.

El JWT asignado se puede encontrar en el almacenamiento:

![Untitled](../assets/OWASP-TOP-10/Explotación de Json Web Tokens (JWT)/Untitled 3.png)

Se sabe que esto es un JWT por su estructura, la primer parte es la cabecera, luego el payload y por último la firma:

![Untitled](../assets/OWASP-TOP-10/Explotación de Json Web Tokens (JWT)/Untitled 4.png)

En el header se puede ver el campo “alg” el cual corresponde al algoritmo utilizado para crear la firma, en este caso, el HS256 y luego está el campo “typ” o tipo que corresponde a un JWT

En el payload se ven las propiedades que involucran al usuario, el campo a destacar en este caso es el id.

La firma sirve para verificar la integridad del JTW, esto evita que los datos sean alterados, esta firma se crea a partir un algoritmo de hash y una clave secreta. Si se quiere modificar este JWT, se debe utilizar la clave secreta y esta ingresarla en el campo “your-256-bit-secret”, si se quiere generar un JWT válido para le web en el que se está utilizando, se debe usar la clave secreta configurada en el servidor y esta por seguridad no está visible y solo se puede ver en los archivos de configuración del servidor web

El problema es que hay veces que los JWT aceptan como algoritmo “NONE”, es decir, ninguno, esto hace que la firma no sea necesaria para generar un JWT válido permitiendo así, modificar los datos del JWT.

Para aprovecharse de esto, se podría crear otro JTW pero esta vez añadiendo “NONE” como valor en el campo “alg” y en este caso, se quiere modificar el payload para que en lugar de usar el id en 1, cambiarlo al 2 ya que corresponde al id del usuario “mortal:

```bash
# HEADER #
{
  "alg": "NONE",
  "typ": "JWT"
}

# PAYLOAD #
{
  "id": 2,
  "iat": 1689424603,
  "exp": 1689428203
}
```

Ahora para crear el JWT hay que tomar en cuenta que este esta codificado en base64 por lo que hay que codificar sus partes en base64:

```bash
# HEADER #
echo -n '{"alg":"NONE","typ":"JWT"}' | base64

# PAYLOAD #
echo -n '{"id":2,"iat":1689424603,"exp":1689428203}' | base64

# RESULTADO #
# [NOTA] Se deben eliminar los signos "=" del resultado
# y no se debe usar comillas ("") para números al codificar 
# la cadena
eyJhbGciOiJOT05FIiwidHlwIjoiSldUIn0.eyJpZCI6MiwiaWF0IjoxNjg5NDI0NjAzLCJleHAiOjE2ODk0MjgyMDN9.
```

Una vez generado este nuevo JTW, se puede utilizar para loguearse como el usuario “mortal” sin necesidad de utilizar la firma ya que por la mala configuración del JTW, es posible utilizar como valor el “NONE”.

![Untitled](../assets/OWASP-TOP-10/Explotación de Json Web Tokens (JWT)/Untitled 5.png)

## JWT - Secret

![Untitled](../assets/OWASP-TOP-10/Explotación de Json Web Tokens (JWT)/Untitled 6.png)

La idea para este lab es parecida al anterior en el sentido que hay que conseguir generar un nuevo JWT válido que permita loguear al usuario como “mortal” solo que ahora modificar el campo “alg” con el valor en “NONE” no servirá y esta vez será necesaria una clave secreta.

![Intento de JTW con valor “NONE” fallido](../assets/OWASP-TOP-10/Explotación de Json Web Tokens (JWT)/Untitled 7.png)

Intento de JTW con valor “NONE” fallido

La idea es que la clave secreta sea robusta y difícil de adivinar pero hay veces en que esto no es así y se configura una clave secreta muy débil. Un atacante podría intentar, con fuerza bruta, intentar adivinar esta clave secreta generando muchos JTW hasta dar con el válido.

Por ejemplo, en este caso la clave secreta es “secret” por lo que se podría utilizar esta clave para generar un JWT válido. En este caso se modificará el campo del “id” para cambiar el valor a 2, el cual corresponde al id del usuario número 2:

![Untitled](../assets/OWASP-TOP-10/Explotación de Json Web Tokens (JWT)/Untitled 8.png)

![Untitled](../assets/OWASP-TOP-10/Explotación de Json Web Tokens (JWT)/Untitled 9.png)
