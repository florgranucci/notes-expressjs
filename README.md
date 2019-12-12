> El siguiente contenido fue elaborado por [@_nhsz](https://twitter.com/_nhsz) como guía para las clases de [undefined school](https://twitter.com/undefinedSchool)

> Son bienvenidos los _issues_ y _PRs_ para mejorar el contenido, corregir errores, etc

# [WIP] Notas sobre ExpressjsJS

## Contenido

---

## Documentación

- [Tutorial oficial](https://expressjs.com/en/starter/installing.html)
- [Guía oficial y conceptos](https://expressjs.com/en/guide/routing.html)
- [Express API](https://expressjs.com/en/5x/api.html)

## Instalación

```js
npm install express
```

## ¿Qué es ExpressJS?

Es un _framework_ para desarrollar aplicaciones web (_server-side_) con _Node_, _minimalista_ y _no opinionado_. Esto último significa que no asume que vayamos a construir nuestras aplicaciones de alguna forma en particular, cómo manejar los _requests_ o generar los _responses_, etc, en ese sentido, mantenemos el control.

De todos los frameworks que existen para Node, Express es el más popular

## Ventajas sobre usar utilizar Node (`http`)

- Express está **construido sobre el módulo `http` de Node**, por lo que básicamente funciona como una _API_ que **simplifica y aumenta la funcionalidad que este módulo nos provee**.
- Nos permite construir aplicaciones web con Node de una forma mucho más **simple**
- Es **muy liviano y rápido**
- **Minimalista:** tenemos control total sobre cómo diseñar la aplicación, manejar los _requests_, _responses_
- Reduce la cantidad de código necesaria y nos abstrae de mucha de la complejidad que tenemos al utilizar el módulo `HTTP`
- Tiene una _API_ muy simple y _expresiva_

## Usos

- _servers_
- _APIs_
- _Microservicios_

Generalmente, vamos a recibir _requests_ con nuestra aplicación Express (_back-end_), generar la respuesta en formato JSON y enviársela al cliente (_front-end_) para que la consuma y haga con esta lo que quiera, genere las _vistas_ correspondientes, etc. También podemos generar vistas desde el servidor, utilizando HTML plano como hicimos anteriormente en Node o algún _template engine_ como [Handlebars](https://handlebarsjs.com/) o [Pug](https://pugjs.org/). 

## Routing

[EJERCICIO CON NODE]

El _routing_ se refiere a cómo los _endpoints_ de nuestra aplicación responden a los _requests_ del cliente. 

Ver [Basic routing](https://expressjs.com/en/starter/basic-routing.html)

Definimos _rutas_ usando los métodos que nos provee el objeto que genera Express, que se corresponden con **métodos HTTP**: por ejemplo podemos usar `app.get()` para manejar requests de tipo `GET` y `app.post()` para requests de tipo `POST`. 

Ver [todos los métodos HTTP disponibles](https://expressjs.com/en/4x/api.html#app.METHOD)

## Middleware

Las funciones middleware... **son funciones** que tienen acceso al _objeto request_ (`req`), al _objeto response_ (`res`) y a la _siguiente función middleware_ (generalmente identificada con una variable llamada _next_) en el ciclo request-response de nuestra aplicación.

Vamos a usar `app.use()` para indicar que vamos a utilizar un _middleware_ determinado como función _callback_

Ver [Using middleware](https://expressjs.com/en/guide/using-middleware.html)

### Tipos de _middleware_

Una aplicación Express puede utiilizar los siguientes tipos de _middleware_:

- Application-level middleware
- Router-level middleware
- Error-handling middleware
- Built-in middleware
- Third-party middleware

## Práctica

### Setup

1. Crear un archivo `.env`. En este archivo vamos a setear las _variables de entorno_ de nuestro proyecto. 
2. Instalar y utilizar `nodemon` para correr nuestras aplicaciones, con el script `dev` del `package.json`
3. Instalar y utilizar [`dotenv`](https://www.npmjs.com/package/dotenv) para leer _variables de entorno_ de un archivo `.env`
4. Mover toda la configuración de nuestro servidor (por ejemplo el puerto) a un módulo `config.js` e importarla en `server.js`

### Ejercicios

1. Crear un servidor en Express, que escuche en el puerto `8080` (leerlo del archivo `.env`) y responda con un `'Hola Mundo!'` cuando reciba un request a la ruta `/`. En el caso de que no haya un puerto seteado en `.env`, la aplicación debe escuchar en el puerto `8001`.
2. Rehacer el [ejercicio 3](https://github.com/undefinedschool/notes-nodejs#ejercicios-1) utilizando Express. Usar [`pug` como _template engine_](http://expressjs.com/en/guide/using-template-engines.html) para retornar las diferentes _vistas HTML_ de nuestra aplicación.
