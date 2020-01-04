> El siguiente contenido fue elaborado por [@_nhsz](https://twitter.com/_nhsz) como guía para las clases de [undefined school](https://twitter.com/undefinedSchool)

> Son bienvenidos los _issues_ y _PRs_ para mejorar el contenido, corregir errores, etc

# [WIP] Notas sobre ExpressjsJS

## Contenido

- [Documentación](#documentación)
- [Instalación](#instalación)
- [¿Qué es ExpressJS?](#qué-es-expressjs)
- [Ventajas sobre usar utilizar Node (`http`)](#ventajas-sobre-usar-utilizar-node-http)
- [Usos](#usos)
- [Routing](#routing)
  - [Router](#router)
  - [Rutas](#rutas)
  - [URL Parameters](#url-parameters)
- [Middleware](#middleware)
  - [Tipos de middleware](#tipos-de-middleware)
- [Práctica](#práctica)
  - [Setup](#setup)
  - [Ejercicios](#ejercicios)

---

## Documentación

- [Tutorial oficial](https://expressjs.com/en/starter/installing.html)
- [Guía oficial y conceptos](https://expressjs.com/en/guide/routing.html)
- [Express API](https://expressjs.com/en/5x/api.html)

## Instalación   

_Express_ es paquete de Node, por lo que se instala con `npm` como cualquier otra dependencia de nuestro proyecto

```js
npm install express
```

## ¿Qué es ExpressJS?

Es un _framework_ para desarrollar aplicaciones web (_server-side_) con _Node_, _minimalista_ y _no opinionado_. Esto último significa que no asume que vayamos a construir nuestras aplicaciones de alguna forma en particular, cómo manejar los _requests_ o generar los _responses_, etc, en ese sentido, mantenemos el control.

## Ventajas sobre usar utilizar Node (`http`)

- Express está **construido sobre el módulo `http` de Node**, por lo que básicamente funciona como una _API_ que **simplifica y aumenta la funcionalidad que este módulo nos provee**.
- Nos permite construir aplicaciones web con Node de una forma mucho más **simple**
- Es **muy liviano y rápido**
- **Minimalista:** hace muy pocas cosas, se enfoca más bien en reducir el _boilerplate_ y las partes más repetitivas y tediosas del código en Node (setear _headers_, MIME types, _cerrar conexiones_, servir archivos estáticos, simplifica mucho el _routing_, etc)
- **No opinionado:** tenemos control total sobre cómo diseñar la aplicación, qué librerías y _middlewares_ utilizar, cómo manejar los _requests_, _responses_, etc
- Reduce la cantidad de código necesaria y nos abstrae de mucha de la complejidad que tenemos al utilizar el módulo `HTTP`
- Tiene una _API_ muy simple y _expresiva_
- **Middleware:** funciones que nos van a permitir interceptar y manipular _requests_ antes de hacer cualquier otra cosa
- Express **es el framework más popular** para desarrollar aplicaciones web con Node

## Usos

- _servers_
- _APIs_
- _Microservicios_

Generalmente, vamos a recibir _requests_ con nuestra aplicación Express (_back-end_), generar la respuesta en formato JSON y enviársela al cliente (_front-end_) para que la consuma y haga con esta lo que quiera, genere las _vistas_ correspondientes, etc. También podemos generar vistas desde el servidor, utilizando HTML plano como hicimos anteriormente en Node o algún _template engine_ como [Handlebars](https://handlebarsjs.com/) o [Pug](https://pugjs.org/). 

## Hola Mundo

Escribir el siguiente código en un archivo `index.js`

```js
const express = require('express');
const app = express();
const HOSTNAME = '127.0.0.1';
const PORT = 8080;

app.get('/', (req, res) => res.send('Hello World!'));
app.listen(PORT, () => console.log('Server listening on http://${HOSTNAME}:${PORT}...'));
```

Luego, ejecutarlo con `node index.js`. ¡Ahora podemos entrar a `localhost:8080` en nuestro navegador y ver la respuesta del servidor! :rocket:

## Inicialización: ¿qué es `app`?

En cualquier tutorial, guía o documentación que busquemos sobre Express, nos vamos a encontrar con la variable `app` al inicio del código del programa. Si analizamos el código fuente del framework, veremos que lo que Express exporta (y estamos importando al hacer el _require_) es una _función_, `createApplication()`. Esta función retorna una _instancia de una aplicación Express_, que estamos almacenando en `app`. 

Por lo tanto, `app` es un objeto, el cual nos permite acceder mediante diferentes propiedades y métodos a toda la funcionalidad que nos provee el framework.

```js
const express = require('express');
const app = express();
```

Luego, para iniciar el servidor, debemos simplemente definir una _ruta_, su correspondiente _callback_ y dejarlo escuchando en algún puerto. Al igual que en Node, el callback va a ejecutarse en cada request recibido en la ruta definida, creando una instancia de los objeto `Request` y `Response`, que se corresponden con los parámetros `req` y `res`, respectivamente.

```js
const PORT = 8080;

app.get('/', (req, res) => res.send('Hello World!'));

app.listen(PORT);
```

## Request/Response

### Request

Para cada _request_ tenemos que definir:

1. Verbo HTTP
2. Ruta
3. Callback

### Verbos HTTP

Podemos recibir [diferentes tipos de _requests_](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods), según el tipo de acción que se quiera realizar con un determinado _recurso_. _Express_ nos proporciona diferentes métodos para cada uno de estos. Entre los más usuales, tenemos 

- `GET`: pedir un recurso
- `POST`: enviar info
- `PUT`: actualizar info
- `DELETE`: eliminar info
- etc

```js
app.get('/', (req, res) => console.log('Give me more chocolate!'));
app.post('/', (req, res) => console.log('I'm posting something! :)'));
app.put('/', (req, res) => console.log('Update that!'));
app.delete('/', (req, res) => console.log('DELETE THIS #RIP'));
```

Cada uno de estos métodos recibe como parámetros 

1. una ruta específica (en el ejemplo `'/'`)
2. un _callback_, que será ejecutado cuando recibamos un request del tipo definido por el verbo HTTP (`get`, `post`, `put`, `delete`, etc), en esa ruta.

Express también posee un método [`app.all()`](http://expressjs.com/en/5x/api.html#app.all), que _matchea_ con cualquier tipode verbo HTTP.

#### CRUD

Estos métodos se corresponden con los necesarios en [aplicaciones CRUD](https://rapidapi.com/blog/api-glossary/crud/).

| CRUD       | Verbo HTTP |
|------------|------------|
| **C**reate | `post`     | 
| **R**ead   | `get`      |
| **U**pdate | `put`      |
| **D**elete | `delete`   |

### Response

Utilizamos el método `Response.send()` para enviar una respuesta. Este método se encarga además de cerrar la conexión automáticamente.

```js
app.get('/', (req, res) => res.send('Hello World!'));
```

El método setea el `Content-Type` según el tipo de parámetro que reciba:

- si le pasamos un _string_, `text/html`
- si recibe un objeto/array, `application/json` (y parsea el parámetro como `JSON`)

#### Redirect

Podemos redireccionar utilizando el método `Response.redirect()` (con status `302` por default):

```js
res.redirect('/another-url');
```

Un redirect `301` sería de la forma

```js
res.redirect(301, '/go-there');
```

El _path_ al que redireccionamos puede ser absoluto, relativo ó una URL.

#### 404

En el caso de recibir un request a una ruta inexistente, Express va a generar una respuesta automática de 404, del estilo

```
Cannot GET /undefined/route/
```

#### Static

Para servir archivos estáticos, debemos guardarlos en algún directorio (en la raíz del proyecto) y utilizar el middleware [`express.static()`](http://expressjs.com/en/starter/static-files.html#serving-static-files-in-express) (ya incluído en `Express`), referenciando este directorio

Por ejemplo, si guardamos los _assets_ en `/public`

```js
app.use(express.static('public'));
```

De esta forma, si tenemos por ejemplo un archivo `index.html` en `/public`, va a ser _servido_ automáticamente si accedemos a la _homepage_ `http://localhost:PORT`. No hay que incluir `/public` en la URL.

También podemos setear múltiples directorios (Express va a buscar los archivos en el orden en el cual seteamos los directorios con `express.static()`)

```js
app.use(express.static('public'));
app.use(express.static('images'));
```

#### JSON

Podemos responder con un `JSON` utilizando el método `Response.json()`. Este método acepta un objeto/array y se encarga de convertirlo a `JSON` antes de enviarlo

```js
app.get('/', (req, res) => res.json({salute: 'Hello World!'}));
```

**Nota:** para parsear un request que envía `JSON`, tenemos que utilizar el middleware [`body-parser`](https://stackoverflow.com/questions/47232187/express-json-vs-bodyparser-json/47232318#47232318), que nos permitirá acceder a su contenido a través de `req.body`.

#### `.send()` vs `.json()`

Estos métodos son muy similares: `Response.json()` invoca a `Response.send()` al final.

**La principal diferencia aparece cuando pasamos valores que no son objetos** (tipos primitivos como `null`, `undefined`, etc), `.json()` va a convertirlos a formato `JSON`, mientras que `.send()` no, por lo tanto, es preferible utilizar `.json()` cuando querramos responder con este formato.

#### HTTP Status

Cuando enviamos una respuesta, `Express` siempre setea automáticamente en los _headers_ un _status code_ por defecto (generalmente, `200`). En el caso de que querramos utilizar uno en particular, podemos utilizar el el método `Response.status()` para setear el _status code_ correspondiente

```js
res.status(404).end();
```

**Nota:** el método `Response.end()` envía una respuesta vacía, sin contenido en el `body`.

```js
res.status(404).send('File not found');
```

- [Referencia de `HTTP Status Codes` disponibles](https://httpstatuses.com/)

## Routing

Dependiendo de en qué URL se realizar el _request_, debemos responder de una forma distinta, es decir, los _endpoints_ de nuestra aplicación deben reaccionar de diferentes maneras a los _requests_ del cliente.

👉 El proceso de determinar qué debe suceder cuando un _endpoint_, con determinado _verbo HTTP_ es llamado por el cliente, ó qué parte del código de nuestra aplicación debe manejar un request específico, es lo que se conoce como **routing**. 

- Ver [Basic routing](https://expressjs.com/en/starter/basic-routing.html)

### Router

Para una aplicación simple, alcanza con utilizar [la funcionalidad que nos provee Express a través de `app`](https://github.com/undefinedschool/notes-expressjs/#verbos-http) para el manejo de requets y routing, pero a medida que la complejidad de la aplicación crece, se vuelve más engorroso. Por esto y para _modularizar_ más nuestra aplicación, hacerla _más mantenible_ y separar responsabilidades, la lógica de _routing_ suele implementarse por separado, en otro módulo.

A esta parte del código, encargada del _routing_ (manejo de rutas y requests), se la conoce como **router**.

Express incluye [_middleware_](https://github.com/undefinedschool/notes-expressjs/#middleware) objeto [`router`](http://expressjs.com/en/4x/api.html#router).

**Nota:** el uso del _router_ que nos provee Express es **opcional**, podemos seguir manejando el routing con Node o usar otro _router engine_ (buscar en NPM). El router de Express es bastante minimal y **nos permite separar la configuración y la lógica de la aplicación del manejo del routing**. 

### Rutas

Definimos _rutas_ usando los métodos que nos provee el objeto que genera Express, que se corresponden con **métodos HTTP**: por ejemplo podemos usar `app.get()` para manejar requests de tipo `GET` y `app.post()` para requests de tipo `POST`. 

**Nota:** los métodos de `app` pueden encadenarse

```js
const express = require('express');
const app = express();
const PORT = 8080;

app
  .get('/', (req, res) => res.send('Hola Mundo!'))
  .listen(PORT, () => console.log(`Express app listening on port ${PORT}!`));
```

También existe el método `.all`, que va a aceptar cualquier tipo de _request_

```js
// '*' es una ruta comodín, cualquier request a cualquier ruta que hagamos va a caer acá
app.all('*', (req, res) => res.send('Hello'));
```

Ver [todos los métodos HTTP disponibles](https://expressjs.com/en/4x/api.html#app.METHOD)

### URL Parameters

Podemos proveer diferentes respuestas a través de una misma ruta si utilizamos parámetros. Vamos a obtener estos parámetros a través de la URL. Esto se conoce como **_dynamic routing_** (ruteo dinámico). Por el contrario, las rutas que no tienen parámetros son _rutas estáticas_.

#### `req.params`

Estos parámetros se almacenan en el objeto `params`, el cual existe dentro del objeto `request`.  Por ejemplo, si queremos acceder al parámetro `color` y utilizarlo en la respuesta, podemos hacer lo siguiente:

```js
app.get('/favorites/:color', (req, res) => {
  const { color } = req.params;
  res.send(`${color} es tu color favorito!`);
});
```

Para especificar qué parte de una URL será un parámetro, agregamos el caracter `:` y luego le damos un nombre. Este valor se va a mapear a una propiedad con ese nombre dentro del objeto `params`. 

**Podemos tener múltiples parámetros en una misma URL**. Por ejemplo:

```js
// Route path
/users/:userId/books/:bookId
// Request URL
http://localhost:3000/users/34/books/8989

req.params: { "userId": "34", "bookId": "8989" }
```

Para más info, ver [Route parameters](http://expressjs.com/en/guide/routing.html#route-parameters)

#### `req.query`

[EN DESARROLLO]

**Es importante notar que los parámetros recibidos por URL son siempre `strings`**.

### ⚠️ El orden en el que declaramos las rutas importa

[EN DESARROLLO]

Si tenemos, por ejemplo, definidas las siguientes rutas:

```js
app.get('/', (req, res) => res.send('Hello World!'));
```

```js
app.get('/', (req, res) => res.send('Bye World!'));
```

la segunda nunca será alcanzada. 

👉 Express funciona de forma _top-down_, **se ejecuta el callback correspondiente de la primer ruta que coincida**, a menos que explícitamente definamos que debe continuar.

## Middleware

Las funciones middleware... **son funciones** que interceptan el proceso de _routing_ y tienen acceso al _objeto request_ (`req`), al _objeto response_ (`res`) y a la _siguiente función middleware_ (`next`) en el _ciclo request-response_ de nuestra aplicación. Estas funciones hacen de _intermediarios_ en el ciclo request/response (de ahí el término _middleware_), con la finalidad de realizar algún tipo de operación en algún punto de esta cadena. 

Los usos más comunes incluyen acceder a cierta info que nos proveen (o editar) los objetos `Request` y `Response`, chequear si un usuario está logueado, etc.

Vamos a usar `app.use()` para indicar que vamos a utilizar un _middleware_ determinado y agregarlo al stack de ejecución:

```js
app.use((req, res, next) => { /* */ });
```

`next` es una referencia a la siguiente función middleware. Al ser invocada, ejecuta el middleware que le sucede al actual. Siempre vamos a llamar a `next` al final del middleware actual, a menos que querramos finalizar la respuesta y enviársela al cliente.

Express nos provee de algunos middlewares por default. También podemos encontrar otros como paquetes de `NPM`, o definir los nuestros propios.

En el caso de utilizar un middleware externo (a través de `NPM`), debemos seguir los siguientes pasos

1. Instalarlo (ej: `npm i body-parser`)
2. Importarlo (ej: `const bodyParser = require('body-parser')`)
3. Usarlo (ej: `app.use(bodyParser.json())`)

También podemos setear un middleware para que se ejecute sólo con algunas rutas específicas (no todas), si lo usamos como 2do. parámetro en la definición de la ruta:

```js
app.get('/', middlewareFn, (req, res) => res.send('Hello World!'));
```

Para más info, ver [Using middleware](https://expressjs.com/en/guide/using-middleware.html)

### Ejemplos

- [Express Router](http://expressjs.com/en/4x/api.html#router)
- [`body-parser`](https://github.com/expressjs/body-parser)
- [`helmet`](https://github.com/helmetjs/helmet)
- ...Y muchos [más ejemplos de Express middleware](https://expressjs.com/en/resources/middleware.html)

### Tipos de _middleware_

Una aplicación Express puede utiilizar los siguientes tipos de _middleware_:

- Application-level middleware
- Router-level middleware
- Error-handling middleware
- Built-in middleware
- Third-party middleware

## Templating

[EN DESARROLLO]

## Práctica

### Setup

1. Crear un archivo `.env`. En este archivo vamos a setear las _variables de entorno_ de nuestro proyecto. 
2. Instalar y utilizar `nodemon` para correr nuestras aplicaciones, con el script `dev` del `package.json`
3. Instalar y utilizar [`dotenv`](https://www.npmjs.com/package/dotenv) para leer _variables de entorno_ de un archivo `.env`

```js
const dotenv = require('dotenv');
dotenv.config();
```

Luego accederemos a las variables definidas en el archivo `.env` a través de `process.env`. Por ejemplo

```js
// .env
PORT=8080
```

```js
// server.js
const { PORT } = process.env;
```

:warning: **Nota**: el archivo `.env` no debe comitearse, agregarlo al `.gitignore`

### Ejercicios

1. Crear un servidor en Express, que escuche en el puerto `8080` (leerlo del archivo `.env`) y responda con un `'Hola Mundo!'` cuando reciba un request a la ruta `/`. En el caso de que no haya un puerto seteado en `.env`, la aplicación debe escuchar en el puerto `8001`. Cuando el servidor esté levantado y corriendo, la aplicación debe loguear por consola `Express app listening on port ${PORT}!`, donde `PORT` es el puerto seteado.

2. Modificar el ejercicio anterior, para que al recibir un request `GET` a la ruta `/salute/{name}`, la aplicación responda con un `'Hola {name}!'`, donde `name` es un parámetro que recibe por URL. 

3. Rehacer el [ejercicio 2](https://github.com/undefinedschool/notes-nodejs#ejercicios-1) utilizando Express. Usar [`pug` como _template engine_](http://expressjs.com/en/guide/using-template-engines.html) para retornar las diferentes _vistas HTML_ de nuestra aplicación.

4. Rehacer el [ejercicio 6](https://github.com/undefinedschool/notes-nodejs#ejercicios-1), [sirviendo los archivos estáticos (assets) con `Express`](https://expressjs.com/en/starter/static-files.html) desde la carpeta `/public`.

5. Crear una aplicación con `Express`, que tenga al archivo `app.js` como _entrypoint_ y escuche en el puerto `9001` (leerlo del archivo `.env`). Los _assets_ (archivos estáticos) deben estar ubicados en la carpeta `/assets`. Utilizar el middleware `static` para servirlos. En el caso de que no haya un puerto seteado en `.env`, la aplicación debe escuchar en el puerto `8001`. Cuando el servidor esté levantado, loguear el mensaje `Server listening on http://${HOSTNAME}:${PORT}`, donde `HOSTNAME` es otra variable de entorno con el valor `localhost`. Probar los endpoints con _Postman_. Utilizar el middleware [`body-parser`](https://www.npmjs.com/package/body-parser) para parsear los requests que envíen `JSON`
`app.js` debe contar con las siguientes rutas (**la lógica de _routing_ debe estar separada, utilizando el `Router` de Express**), definidas en `/routes/index.js` e importadas y utilizadas desde `app.js`:

- `GET /`: debe retornar un `JSON` listando todos los endpoints disponibles, agrupados por tipo de verbo HTTP
- `GET /over/:ki`: si el parámetro recibido es mayor a 9000, debemos retornar [esta imagen](https://scontent.faep8-2.fna.fbcdn.net/v/t1.0-9/13557935_10154299834588430_6953082742839667877_n.jpg?_nc_cat=108&_nc_ohc=NaJZnDsaLH4AQmlOMjlSHEY-Ie0cKmNMiM6JfFXvi5XqS7Vy7dFIqjyWg&_nc_ht=scontent.faep8-2.fna&oh=e8224bf46f2ff62926ce3edb89229f17&oe=5EA8758F). Si el parámetro está recibido entre 8000 y 9000, debe responder con [esta imagen](https://i.pinimg.com/originals/c4/5a/2b/c45a2b80dfe53775508dad0335eb117f.jpg)
- `GET /download`: debe retornar el mensaje (string) `No te entiendo.`
- `GET /download/internet`: debe generar una descarga de [esta imagen](https://www.mememaker.net/api/bucket?path=static/img/memes/full/2017/Jan/18/16/download-all-the-internet.jpg), con el nombre `download-all-the-internet.jpg`. Investigar qué método de `Express` provee esta funcionalidad.
- `GET /area51`: responder con el `JSON` `{ message: "RESTRICTED AREA. NO TRESPASSING." }` y status code `401`
- `POST /area51`: enviar el `JSON` `{ "secret": {SECRET_VALUE}}`. Si `SECRET_VALUE` es `aliens`, responder con el `JSON` `{ message: "ACCESS GRANTED." }` y status code `200`, sino responder con el `JSON` `{ message: "RESTRICTED AREA. NO TRESPASSING." }` y status code `401`
- `GET /undefined`: responder con el `JSON` `{ message: "404 - Ni idea, no lo encuentro" }` y status code `404`
- `GET /shout/:word`: responder con el `JSON` `Quisiste decir {WORD}???` donde `WORD` es el parámetro recibido, convertido a mayúsculas
- `POST /users`: recibe un request de la forma `{ name: {NAME}, email: {EMAIL}, age: {AGE} }`. Usar [`express-validator`](https://express-validator.github.io/docs/) para validar que `name` tiene al menos 3 letras (y sólo contiene letras), `email` es un mail válido y `age` es un entero entre 0 y 120. Si pasa la validación y no existe otro usuario con ese email, agregarlo al array `registeredUsers`, dentro del módulo `users.js`. Si algún campo no pasa la validación, responder con un status [422](https://httpstatuses.com/422) y retornar los errores en formato `JSON`. Usar además, las funcionalidades que nos provee [`express-validator`](https://express-validator.github.io/docs/) para _sanitizar el input_, de forma tal que `name` no contenga espacios al inicio ni al final ni otros caracteres especiales, `email` esté _normalizado_ y `age` no contenga espacios al inicio ni al final ni otros caracteres especiales.
- `GET /users`: retorna un HTML generado con`pug`, el cual contiene una tabla con las columnas `Nombre`, `Email` y `Edad`, generada a partir del contenido del array `registeredUsers`.
- `GET /html/:name/:color`: `name` y `color` son parámetros que representan un nombre y un color, respectivamente. Responder con el [siguiente HTML](https://gist.githubusercontent.com/nhsz/5d4d9c339e99ad565116ddc8de0bb199/raw/25277d382208e3aa335d24b3b1888364084b015a/index.html), en un archivo `index.html`, generado con [`Pug`](https://pugjs.org/). Si el color recibido es `red` ó `blue`, el html debe tener un `background-color: red` ó `background-color: blue`, respectivamente. Este CSS debe estar definido en un archivo `styles.css`. Tanto el `index.html` como el `styles.css` deberán ser servidos estáticamente desde la carpeta `/public`.
- `GET /google`: redirigir a `https://google.com`, con un status code `301`.
- `POST /series`: el `body` del request será el siguiente [`JSON`](http://api.tvmaze.com/singlesearch/shows?q=mr-robot&embed=episodes). Retornar un `JSON` con la siguiente data, procesada y extraída a partir del request: 

```
{ 
  "series name": {nombre de la serie},
  "number of episodes": {cantidad total de episodios},
  "total runtime": {cantidad total de minutos que dura la serie},
  "first episode": {
    "name": {nombre},
    "date": {fecha de estreno}
  },
  "last episode": {
    "name": {nombre},
    "date": {fecha de estreno}
  },
}
```

Utilizar el método [`route()`](http://expressjs.com/en/4x/api.html#router.route) del `Router`, para definir las rutas de una forma más declarativa. En caso de necesitar _debuggear_ la aplicación, utilizar [esta guía](https://itnext.io/the-absolute-easiest-way-to-debug-node-js-with-vscode-2e02ef5b1bad).
