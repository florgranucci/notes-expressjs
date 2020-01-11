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

[↑ Ir al inicio](https://github.com/undefinedschool/notes-expressjs/#contenido)

## Instalación   

_Express_ es paquete de Node, por lo que se instala con `npm` como cualquier otra dependencia de nuestro proyecto

```js
npm install express
```

[↑ Ir al inicio](https://github.com/undefinedschool/notes-expressjs/#contenido)

## ¿Qué es ExpressJS?

Es un _framework_ para desarrollar aplicaciones web (_server-side_) con _Node_, _minimalista_ y _no opinionado_. Esto último significa que no asume que vayamos a construir nuestras aplicaciones de alguna forma en particular, cómo manejar los _requests_ o generar los _responses_, etc, en ese sentido, mantenemos el control.

[↑ Ir al inicio](https://github.com/undefinedschool/notes-expressjs/#contenido)

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

[↑ Ir al inicio](https://github.com/undefinedschool/notes-expressjs/#contenido)

## Usos

- _servers_
- _APIs_
- _Microservicios_

Generalmente, vamos a recibir _requests_ con nuestra aplicación Express (_back-end_), generar la respuesta en formato JSON y enviársela al cliente (_front-end_) para que la consuma y haga con esta lo que quiera, genere las _vistas_ correspondientes, etc. También podemos generar vistas desde el servidor, utilizando HTML plano como hicimos anteriormente en Node o algún _template engine_ como [Handlebars](https://handlebarsjs.com/) o [Pug](https://pugjs.org/). 

[↑ Ir al inicio](https://github.com/undefinedschool/notes-expressjs/#contenido)

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

Al igual que en Node, utilizamos el método `listen` para dejar el servidor levantado y escuchando en el puerto que le indiquemos.

Luego, ejecutarlo con `node index.js`. ¡Ahora podemos entrar a `localhost:8080` en nuestro navegador y ver la respuesta del servidor! :rocket:

[↑ Ir al inicio](https://github.com/undefinedschool/notes-expressjs/#contenido)

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

[↑ Ir al inicio](https://github.com/undefinedschool/notes-expressjs/#contenido)

## Request/Response

### Request

Para cada _request_ tenemos que definir:

1. Verbo HTTP
2. Ruta
3. Callback

[↑ Ir al inicio](https://github.com/undefinedschool/notes-expressjs/#contenido)

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

[↑ Ir al inicio](https://github.com/undefinedschool/notes-expressjs/#contenido)

#### CRUD

Estos métodos se corresponden con los necesarios en [aplicaciones CRUD](https://rapidapi.com/blog/api-glossary/crud/).

| CRUD       | Verbo HTTP |
|------------|------------|
| **C**reate | `post`     | 
| **R**ead   | `get`      |
| **U**pdate | `put`      |
| **D**elete | `delete`   |

[↑ Ir al inicio](https://github.com/undefinedschool/notes-expressjs/#contenido)

### Response

Utilizamos el método `Response.send()` para enviar una respuesta. Este método se encarga además de cerrar la conexión automáticamente.

```js
app.get('/', (req, res) => res.send('Hello World!'));
```

El método setea el `Content-Type` según el tipo de parámetro que reciba:

- si le pasamos un _string_, `text/html`
- si recibe un objeto/array, `application/json` (y parsea el parámetro como `JSON`)

[↑ Ir al inicio](https://github.com/undefinedschool/notes-expressjs/#contenido)

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

[↑ Ir al inicio](https://github.com/undefinedschool/notes-expressjs/#contenido)

#### 404

En el caso de recibir un request a una ruta inexistente, Express va a generar una respuesta automática de 404, del estilo

```
Cannot GET /undefined/route/
```

[↑ Ir al inicio](https://github.com/undefinedschool/notes-expressjs/#contenido)

#### Servir estáticos con `express.static()`

Para servir archivos estáticos, debemos guardarlos en algún directorio (en la raíz del proyecto) y utilizar el middleware [`express.static()`](http://expressjs.com/en/starter/static-files.html#serving-static-files-in-express) (ya incluído en `Express`), referenciando este directorio

Por ejemplo, si guardamos los _assets_ en `/public`

```js
app.use(express.static('public'));
```

De esta forma, si tenemos por ejemplo un archivo `index.html` en `/public`, va a ser _servido_ automáticamente si accedemos a la _homepage_ `http://localhost:PORT`. No hay que incluir `/public` (ni ningún otro directorio que definamos como _estático_) en la URL, está implícito en esta.

También podemos setear múltiples directorios, tantos como necesitemos. `Express` va a buscar los archivos en el orden en el cual seteamos los directorios con `express.static()`.

```js
app.use(express.static('public'));
app.use(express.static('images'));
```

⚠️ **Recordar que todo el contenido de nuestros _directorios estáticos_ será público y accesible por cualquiera, mediante la URL correspondiente. Por lo tanto, no debemos guardar allí aquellos archivos e información que consideremos sensibles**. Como convención y tip para tener siempre esto en cuenta, se suele llamar `/public` al directorio desde el cual servimos los estáticos.

[↑ Ir al inicio](https://github.com/undefinedschool/notes-expressjs/#contenido)

#### JSON

Podemos responder con un `JSON` utilizando el método `Response.json()`. Este método acepta un objeto/array y se encarga de convertirlo a `JSON` antes de enviarlo

```js
app.get('/', (req, res) => res.json({salute: 'Hello World!'}));
```

**Nota:** para parsear un request que envía `JSON`, tenemos que utilizar el middleware [`body-parser`](https://stackoverflow.com/questions/47232187/express-json-vs-bodyparser-json/47232318#47232318), que nos permitirá acceder a su contenido a través de `req.body`.

Más info en [express.json()`](http://expressjs.com/en/api.html#express.json)

[↑ Ir al inicio](https://github.com/undefinedschool/notes-expressjs/#contenido)

#### `.send()` vs `.json()`

Estos métodos son muy similares: `Response.json()` invoca a `Response.send()` al final.

**La principal diferencia aparece cuando pasamos valores que no son objetos** (tipos primitivos como `null`, `undefined`, etc), `.json()` va a convertirlos a formato `JSON`, mientras que `.send()` no, por lo tanto, es preferible utilizar `.json()` cuando querramos responder con este formato.

[↑ Ir al inicio](https://github.com/undefinedschool/notes-expressjs/#contenido)

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

[↑ Ir al inicio](https://github.com/undefinedschool/notes-expressjs/#contenido)

## Routing

Dependiendo de en qué URL se realizar el _request_, debemos responder de una forma distinta, es decir, los _endpoints_ de nuestra aplicación deben reaccionar de diferentes maneras a los _requests_ del cliente.

👉 El proceso de determinar qué debe suceder cuando un _endpoint_, con determinado _verbo HTTP_ es llamado por el cliente, ó qué parte del código de nuestra aplicación debe manejar un request específico, es lo que se conoce como **routing**. 

- Ver [Basic routing](https://expressjs.com/en/starter/basic-routing.html)

[↑ Ir al inicio](https://github.com/undefinedschool/notes-expressjs/#contenido)

### Router

Para una aplicación simple, alcanza con utilizar [la funcionalidad que nos provee Express a través de `app`](https://github.com/undefinedschool/notes-expressjs/#verbos-http) para el manejo del _routing_, pero a medida que la complejidad de la aplicación crece, se vuelve más engorroso. Por esto y **para _modularizar_ más nuestra aplicación, hacerla _más mantenible_ y separar responsabilidades, la lógica de _routing_ suele implementarse por separado, en otro módulo**.

A esta parte del código, encargada del _routing_ (manejo de rutas y requests), se la conoce como **router**.

`Express` incluye como [_middleware_](https://github.com/undefinedschool/notes-expressjs/#middleware) al objeto [`router`](http://expressjs.com/en/4x/api.html#router).

**Nota:** el uso del _router_ que nos provee Express es **opcional**, podemos seguir manejando el routing con Node o usar otro _router engine_ (buscar en NPM). El router de Express es bastante minimal y **nos permite separar la configuración y la lógica de la aplicación del manejo del routing**. 

También podemos organizar las rutas de nuestra aplicación en diferentes módulos, usando el ´Router´ en cada uno de ellos y luego usándolos como _middleware_. Por ejemplo, si tuviésemos las rutas correspondientes a las diferentes secciones de un diario, organizadas en `/sports`, `/arts`, `/books`, etc, luego haríamos

```js
const sportsRoutes = require('./sports');
const artsRoutes = require('./routes');
const booksRoutes = require('./routes');

app.use(sportsRoutes);
app.use(artsRoutes);
app.use(booksRoutes);
```

**Nota:** en _todos_ los módulos donde estemos utilizando el ´Router´ tenemos que importar ´express´, ya que el ´Router´ no deja de ser un _middleware_ de este. Por ejemplo

```js
// routes/index.js
const express = require("express");

const router = express.Router();

const users = []; // this would ideally be a database, but we'll start with something simple
let id = 1; // this will help us identify unique users

// instead of `app.get`...
router.get("/users", (req, res) => {
  res.json(users);
});

router.get("/users/:id", (req, res) => {
  const user = users.find(val => val.id === Number(req.params.id));
  res.json(user);
});

// instead of `app.post`...
router.post("/users", (req, res) => {
  users.push({
    name: req.body.name,
    id: ++id
  });
  res.json({ message: "Created" });
});

// instead of `app.patch`...
router.patch("/users/:id", (req, res) => {
  const user = users.find(val => val.id === Number(req.params.id));
  user.name = req.body.name;
  res.json({ message: "Updated" });
});

// instead of `app.delete`...
router.delete("/users/:id", (req, res) => {
  const userIndex = users.findIndex(val => val.id === Number(req.params.id));
  users.splice(userIndex, 1);
  res.json({ message: "Deleted" });
});

// Now that we have built up all these routes - let's export this module for use in our app.js!
module.exports = router;
```

[↑ Ir al inicio](https://github.com/undefinedschool/notes-expressjs/#contenido)

#### Router: sintaxis más declarativa

El `Router` nos permite también utilizar una _sintaxis más declarativa y legible_ a través del método `route`.

```js
const express = require("express");
const router = express.Router();

const users = [];
let id = 1;

// declare the route first, then all the methods on it
router
  .route("/users")
  .get(() => {
    return res.json(users);
  })
  .post(() => {
    users.push({
      name: req.body.name,
      id: ++id
    });
    return res.json({ message: "Created" });
  });

router
  .route("/users/:id")
  .get((req, res) => {
    const user = users.find(val => val.id === Number(req.params.id));
    return res.json(user);
  })
  .patch((req, res) => {
    user.name = req.body.name;
    return res.json({ message: "Updated" });
  })
  .delete((req, res) => {
    users.splice(user.id, 1);
    return res.json({ message: "Deleted" });
  });

module.exports = router;
```

También podemos utilizar _prefijos_ para las rutas que definamos

```js
// prefix every single route in here with /users
app.use("/users", userRoutes);
```

...y luego definir las rutas de la forma

```js
const express = require("express");
const router = express.Router();

const users = [];
let id = 1;

// declare all the methods on the /users route (prefix specified in app.js)
router
  .route("")
  .get((req, res) => {
    return res.json(users);
  })
  .post((req, res) => {
    users.push({
      name: req.body.name,
      id: ++id
    });
    return res.json({message: "Created"});
  });

router
  .route("/:id")
  .get((req, res) => {
    const user = users.find(val => val.id === Number(req.params.id));
    return res.json(user);
  })
  .patch((req, res) => {
    const user = users.find(val => val.id === Number(req.params.id));
    user.name = req.body.name;
    return res.json({ message: "Updated" });
  })
  .delete((req, res) => {
    const userIndex = users.findIndex(val => val.id === Number(req.params.id));
    users.splice(userIndex, 1);
    return res.json({ message: "Deleted" });
  });

});

module.exports = router;
```

[↑ Ir al inicio](https://github.com/undefinedschool/notes-expressjs/#contenido)

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

[↑ Ir al inicio](https://github.com/undefinedschool/notes-expressjs/#contenido)

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

[↑ Ir al inicio](https://github.com/undefinedschool/notes-expressjs/#contenido)

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

👉 Express funciona de forma _top-down_, **se ejecuta el callback correspondiente de la primer ruta que coincida y omite el resto**, a menos que explícitamente definamos que debe continuar.

[↑ Ir al inicio](https://github.com/undefinedschool/notes-expressjs/#contenido)

## Middleware

**Una función middleware es _cualquier tipo de función_ que intercepta el proceso de _routing_ y tiene acceso al _objeto request_ (`req`), al _objeto response_ (`res`) y a la _siguiente función middleware_ (`next`) en el _ciclo request-response_ de nuestra aplicación**. Estas funciones hacen de _intermediarios_ en el ciclo request/response (de ahí el término _middleware_), con la finalidad de realizar algún tipo de operación en algún punto de esta cadena. 

Los usos más comunes incluyen 

- acceder a cierta info que nos proveen (o editar) los objetos `Request` y `Response`
- chequear si un usuario está logueado
- almacenar info en la DB
- parsear info proveniente del `body` del request
- etc

👉**Podemos decir que `Express` es un framework integrado esencialmente por 2 cosas: Router y conjunto de funciones middleware**

👉 Utilizamos `app.use(PATH)` para indicar que vamos a utilizar un _middleware_ determinado, de un `PATH` específico y agregarlo al stack de ejecución:

```js
app.use((req, res, next) => { /* */ });
```

Por ejemplo, si queremos utilizar el middleware `blackMagic` a nivel aplicación, hacemos

```js
app.use(blackMagic);
```

Mientras que si queremos utilizarlo en una ruta específica, por ejemplo `/magic`, hacemos

```js
// this will run `validateUser` on /magic, ALL HTTP methods
app.use('/magic', blackMagic);
// this will run `validateUser` on /, GET method
app.get('/magic', blackMagic);
```

### `next` y _custom middleware_

**`next` es una referencia que utilizamos para _pasar el control_ a la siguiente función middleware (siempre y cuando la ruta coincida)**. Al ser invocada, ejecuta el middleware que le sucede al actual. Vamos a llamar a `next` al final del middleware actual si queremos _pasarle el control a la siguiente función middleware_, sino, finaliza el ciclo y se envía la respuesta al cliente.

Express nos provee de algunos middlewares por default. También podemos encontrar otros como paquetes de `NPM`, o definir los nuestros propios (custom).

Ejemplo de middleware propio:

```js
function validateUser(req, res, next) {
  // some middleware code (get info from req, do smth with db, etc...)
 res.locals.validatedUser = true;
 next();
}

// this will run `validateUser` on ALL paths, ALL HTTP methods
app.use(validateUser);

app.get('/', (req, res, next) => {
  const { validatedUser } = res.locals;
  console.log(`valid user: ${validatedUser}`);
  
  res.send('Hello, user!');
  // we don't call `next` here, so the cycle ends
})
```

👉 Algo importante a tener en cuenta siempre es que **el orden en el que definimos las rutas e invocamos el middleware es importante**. Si ubicamos `app.get` por encima del `app.use` el middleware nunca se va a ejecutar, es por esto que casi siempre incluímos el middleware antes de la lógica de routing.

### Usando middleware a través NPM

En el caso de utilizar un middleware externo (a través de `NPM`), debemos seguir los siguientes pasos

1. Instalarlo (ej: `npm i body-parser`)
2. Importarlo (ej: `const bodyParser = require('body-parser')`)
3. Usarlo (ej: `app.use(bodyParser.json())`)

También podemos setear un middleware para que se ejecute sólo con algunas rutas específicas (no todas), si lo usamos como 2do. parámetro en la definición de la ruta:

```js
app.get('/', middlewareFn, (req, res) => res.send('Hello World!'));
```

Para más info, ver [Using middleware](https://expressjs.com/en/guide/using-middleware.html)

[↑ Ir al inicio](https://github.com/undefinedschool/notes-expressjs/#contenido)

### Middleware: beneficios

La principal ventaja de utilizar _middleware_ en `Express` es la de modularizar nuestro código en pequeñas funciones, que podemos utilizar a nivel de la aplicación entera, de una forma mucho más limpia y mantenible, sin necesidad de definir esta funcionalidad en cada ruta.

### Middleware: ejemplos

- [Express Static](https://expressjs.com/en/4x/api.html#express.static)
- [Express Router](http://expressjs.com/en/4x/api.html#router)
- [`body-parser`](https://github.com/expressjs/body-parser)
- [`helmet`](https://github.com/helmetjs/helmet)
- ...Y muchos [más ejemplos de Express middleware](https://expressjs.com/en/resources/middleware.html)

[↑ Ir al inicio](https://github.com/undefinedschool/notes-expressjs/#contenido)

### Tipos de _middleware_

Una aplicación Express puede utiilizar los siguientes tipos de _middleware_:

- Application-level middleware
- Router-level middleware
- Error-handling middleware
- Built-in middleware
- Third-party middleware

[↑ Ir al inicio](https://github.com/undefinedschool/notes-expressjs/#contenido)

## Templating

[EN DESARROLLO]

### Enviar HTML estático

En el caso de querer enviar un HTML estático como respuesta (sin utilizar ningún template), debemos utilizar el método `[Response.sendFile()](http://expressjs.com/en/api.html#res.sendFile)` e indicarle la _ruta absoluta_ del archivo (ya que estamos leyendo un archivo del _file system_ en lugar de generar uno nuevo a partir de un template). Para esto último, podemos utilizar el método de Node `path.join()` junto con [`__dirname`](https://alligator.io/nodejs/how-to-use__dirname/).

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

[↑ Ir al inicio](https://github.com/undefinedschool/notes-expressjs/#contenido)

### Ejercicios

1. Crear un servidor en Express, que escuche en el puerto `8080` (leerlo del archivo `.env`) y responda con un `'Hola Mundo!'` cuando reciba un request a la ruta `/`. En el caso de que no haya un puerto seteado en `.env`, la aplicación debe escuchar en el puerto `8001`. Cuando el servidor esté levantado y corriendo, la aplicación debe loguear por consola `Express app listening on port ${PORT}!`, donde `PORT` es el puerto seteado.

2. Modificar el ejercicio anterior, para que al recibir un request `GET` a la ruta `/salute/{name}`, la aplicación responda con un `'Hola {name}!'`, donde `name` es un parámetro que recibe por URL. 

3. Rehacer el [ejercicio 2](https://github.com/undefinedschool/notes-nodejs#ejercicios-1) utilizando Express. Usar [`pug` como _template engine_](http://expressjs.com/en/guide/using-template-engines.html) para retornar las diferentes _vistas HTML_ de nuestra aplicación.

4. Rehacer el [ejercicio 6](https://github.com/undefinedschool/notes-nodejs#ejercicios-1), [sirviendo los archivos estáticos (assets) con `Express`](https://expressjs.com/en/starter/static-files.html) desde la carpeta `/public`.

5. Ver [express-salad](https://github.com/undefinedschool/express-salad)

[↑ Ir al inicio](https://github.com/undefinedschool/notes-expressjs/#contenido)
