# TWEB — Résumé

## URL parts

* Scheme: `https://`
* Credentials: `user:pwd@`
* Domain: `example.com`
* Port: `443`
* Path: `/index.html`
* Query: `?param=value`
* Fragment: `#fragment`

## Javascript

* `var`: scope is global if declared outside of a function.
* `let`: scope is always local.
* `const`: scope is always local, but used to declare constants.
* `Number`, `Boolean`, `String`, `Undefined`, `Null`, `BigInt`, `Symbol`: 7 primitive types which are immutable.
* `Object`: a special mutable type.

```
for (let prop in obj) // iterates over enumerable properties of an object
for (let prop of arr) // iterates over all elements of array or iterable
```

## Prototypes

* **Prototype chaining**: if we modify a method of a prototype (e.g. `Array.prototype.toString`), the modification will be reflected through all existing objects.
* In a method (function stored in an object's property), `this` is bound to the object, but not when using the arrow syntax. In a function not bound to a property, this correspond to the global object.

## Classes

* Javascript is class-free but we can create a function, define methods in its prototype and then use the `new` keyword when invoking the function to create a new object that will inherit the function's prototype.
* The class syntax has been added recently but just uses the existing prototype-based inheritance. Private properties can be declared by prefixing them with a #. We can also define getters and setters with the get and set keywords. The static keyword can also be used.

## Modules

```
// fruit.js
class Fruit {}
export Fruit;

// apple.js
import Fruit from 'fruit.js'
class Apple extends Fruit {}
```

## Canvas

```
document.getElementById('canvas')
const ctx = canvas.getContext('2d')
ctx.beginPath()
ctx.lineTo(20, 20)
ctx.lineTo(50, 50)
ctx.stroke()
```

## Functional programming

```
arr.reduce((accumulator, currentValue, index, array) => {
    // perform operations
}, initialValue)

arr.flatMap((currentValue, index, array) => {
    // ...
}, thisArg)
```

## Iterable

https://stackoverflow.com/questions/37124006/iterator-and-a-generator-in-javascript

An iterator is an object with a next method (which return the next value) and a done property.

```
var myIterable = {
    *[Symbol.iterator]() {
        yield 1;
        yield 2;
        yield 3;
    }
}

for (let value of myIterable) {
    console.log(value);
}
```

* Use yield to return a value, yield* to return a generator or iterable object.
* function* is used to defined a generator or iterable.

## Regexp

```
var re = /ab+c/g;
re.test("ac"); // false
re.exec("abbc"); // ["abbc"]
"abc abbc".matchAll(re); // [["abc"], ["abbc"]]
```

In addition to matchAll, a string comes with the match, replace, search and split methods.

## Express

```
const express = require('express')
const app = express()
const port = 3000

// Serve static files
app.use(express.static("public"));

// Define a Middleware
app.use('/form/', (req, res, next) => {
    console.log(req);
    next();
});

// Handle get requests
app.get('/form/', (req, res) => {
    res.send('Hello World!');
});

// Handle post requests
app.post('/form/', (req, res) => {
    res.send('Post saved!')
});

app.listen(port, () => console.log(`Example app listening on port ${port}!`))
```

##  Event Loop

The Javascript runtime uses a message queue which is a list of messages to be processed by the event loop. Each message has an associated function (the callback).

## Promise

A Promise is used for asynchronous operations. When created its state is *pending*. If the associated operation succeeds, the state will change to *resolved*, or *rejected* if the operation fails. If the Promise is either *resolved* or *rejected*, then it is said to be *settled*.

A Promise's operation is executed only once.

```js
let iPromiseIWillGiveYouMoney = new Promise((resolve, reject) => {
    // I promise I'll give you money… but we can never be sure…
    if (Math.random() >= 0.5) {
        // `resolve' is the function that will be passed to the
        // `then' function.
        resolve(Math.random() * 1000)
    }
    else {
        // `reject' is the function that will be passed to the
        // `catch' function.
        reject("No money for a script kiddie like you.")
    }
})

iPromiseIWillGiveYouMoney
    .then(money => {
        console.log(`I have received $${money}`)
    })
    .catch(reason => {
        console.log(`No money received: ${reason}`)
    })
```

### API

```js
Promise.all // until all are resolved
Promise.allSettled // until all are settled
Promise.race // until the first is settled
Promise.resolve // return a resolved promise
Promise.reject // return a rejected promise
```

## `async` & `await`

The `await` operator is used to wait for a Promise. It can only be used inside an async function. `async` means that the function returns a Promise.

## Fetch API

```js
const promise = fetch("https://api.github.com/users/tweb-classroom", {
    method: 'post',
    headers: {'Content-Type': 'application/json'},
    body: JSON.stringify({'value': 'Hello, World!' })
})
```

## HTTP

Version 1.1 introduced:

* persistent connections (reuse of underlying TCP connection for multiple HTTP requests),
* pipelined connections (multiple requests can be sent without having to wait for the response),
* chunked transfers (data streams can be divided into a series of chunks that can be received independently of each other),
* protocol upgrades (the client can ask the server another application protocol).

### example-chat

* **Polling**: using `setInterval()` on the client side, we poll the server for new messages every 10 seconds.
* **Long polling**: using `EventEmitter` on the server side, we emit an event each time we receive a message from the client. We listen for such event when the client ask for new messages, which will cause the function to return only when a event is emitted.
* **Server-side events**: using `EventSource` on the client side, we can open a persistent connection to the server. The server will then send messages using the `text/event-stream` content-type. Messages can only be sent from the server to the client, unlike web sockets.
* **Web socket**: the client opens a web socket connection with the server. Messages can be sent and received from both sides.

## Cookies

Cookies are data initially sent from the server to the client (using the `Set-Cookie` header). The client will then send the cookies back to the server at every requests (using the `Cookie` header). Purposes: session management, personalization, tracking.

* A cookie can be set to expire at a given time using the `Expires` directive. It is said to be permanent.
* Otherwise it's called a session cookie. It will be deleted when the client shuts down (but most web browser will do *session restoring*).
* It can be made inaccessible to Javascript using the `HttpOnly` directive.
* It can require the use of HTTPS with the use of the `Secure` directive.

```
Set-Cookie: id=a3fWa; Expires=Wed, 21 Oct 2015 07:28:00 GMT; Secure; HttpOnly
```

### CSRF

When on website A, any request made to website B (through an img tag, form, etc.) will be made by the browser using the cookies currently existing for website B. If these cookies are authentication cookies, it means that website A can make a request to website B on the behalf the user.

## Authentication mechanisms

* Cookies and sessions
* `Authorization` header
  * HTTP Basic
  * HMAC token
  * Bearer token
  * JWT token
* `X-API-Key` header
* OAuth2
* WebAuthN

https://developer.mozilla.org/en-US/docs/Web/HTTP/Authentication

## JSON Web Tokens (JWT)

* Can be used for authentication and authorization.

## Cross-Origin Resource Sharing (CORS)

* CORS is a security policy that is enforced by the browser.
* An origin is defined by the domain name, the protocol and the port number of the URL.
* A cross-origin request is made when a resource from a different origin than the original one is requested. For example, when Javascript code downloaded by the client from origin A wants to make a fetch request to origin B.
* Not all cross-origin requests are controlled by  the browser. Examples of a few that are:
  * `fetch` and `XMLHTTPRequest`,
  * Web Fonts,
  * WebGL textures
  * `drawImage` of the Canvas API,
  * CSS Shapes from images.
* HTTP Headers are use both by the browser (`Origin`) making the cross-origin requests and by the server (`Access-Control-Allow-Origin`) responding to that request.
* Some types of cross-origin requests requires the browser to make a preflight request (HTTP `OPTIONS`) to the server, to check if it is aware of the methods and headers used by the client. The browser will use additional HTTP headers (e.g. `Access-Control-Request-Method`, `Access-Control-Request-Headers`) and so will the server (e.g. `Access-Control-Allow-Methods`, `Access-Control-Allow-Headers`).
* When making a cross-origin request to a domain, a flag can be set to include any existing cookies for that domain. This requires the server response to include both of these headers: `Access-Control-Allow-Credentials` and `Access-Control-Allow-Origin`. For the response to be accepted by the browser, the first one must be set to `true` and the second one must specify the domain name (i.e. not use `*`). This only concerns `fetch` and `XMLHTTPRequest` API.

## XSS

* Vulnerability that allows an attacker to inject code into a website that will be executed by the client.

* `Content-Security-Policy`, either as an HTTP header or in a `meta` element, can be used to whitelist the provenance of content that can loaded through a web page (e.g. sources, images, styles). It tells the browser what it can or cannot load.

  ```
  Content-Security-Policy: default-src 'self'; img-src instagram.com
  Content-Security-Policy: default-src 'self' *.mailsite.com; img-src *
  ```

## Certificates

* Bound to a hostname.
* Contain a public key.
* Contain a proof that the owner has the private key.
* Validity period.
* Certificate authority.

### Let's Encrypt

* Non-profit certificate authority.
* Provides X.509 certificates for TLS for free.
* Run by the Internet Security Research Group.

### Automatic Certificate Management Environment (ACME)

* Communication protocol for automating interactions between CAs and their user's webservers.
* Allows for automated certificate renewals, etc.

## REST

Defines a set of constraints:

* Client-server
* Uniform interface
* Stateless
* Cache
* Layered System
* Code-on-demand (optional)

Fits well with HTTP but can work with different protocols.

### Uniform Interface

Architectural constraints are:

* Identification of resources (URI)
* Manipulation of resources (HTTP methods)
* Self-descriptive messages (Content-type header)
* Hypermedia as the engine of application state (HATEOAS)

### Stateless constraint

REST is stateless. Any state information is contained within the client's request. This allows for:

* better visibility: only the request's data needs to be considered,
* better reliability: ease of recovering from partial failures,
* better scalability: servers don't have to manage user's resources.

### Cache constraint

Better efficiency, eases scalability, improves user-perceived performance. However, can expose users to stale data (impacting the system's consistency). A CDN relies on this constraint.

### Layered System constraint

An example is GraphQL which is an (opinionated) way of building an API and of layering multiple data sources and services (MongoDB, Postgres, 3rd-party API, legacy API, etc.)

Adds overhead latency due to the processing of data.

## Declarative Programming

Paradigm that expresses the logic of a computation without describing its control flow (e.g. SQL where the control flow is decided by the query optimizer).

Vue.js, unlike jQuery, promotes a declarative style. The template engine will update the DOM when necessary.

```js
<div id="content">{{ message }}</div>

new Vue({
  el: '#content',
  data: {
    message: 'Hello Vue!'
  }
})
```

## UI Frameworks and Progressive Web Apps

* reduction of UI code with declarative rendering
* modular applications with components

### Vue.js

* Connects the view and the model via two-way bindings.
* DOM and State manipulations are abstracted by the framework.
* Reusable components (see also Web Components Specification)

#### Proxy example

```js
<div id="message">message</div>
<script>
    // Original object
    var data = { message: "Hello, World!" };
    
	// Hypothetical proxy created by the framework
    var proxy = new Proxy(data, {
        get: function(obj, prop, value) {
            return obj[prop];
        },
        set: function(obj, prop) {
            // here
            document.getElementById(prop).innerHTML = value;
            obj[prop] = value;
        }
    })
    
    // Logic of the application
    proxy.message = "Hello, Binding!";
</script>
```

### Progressive Web Applications

* Native application-like UX

* Works offline

* Push notifications

* Device hardware access

* Includes a Web App Manifest

  ```html
  <link rel="manifest" href="/manifest.json">
  
  {
      "name": "Simple PWA",
      "short_name": "Simple PWA",
      "icons": [
          {
              "src": "icons/icon-32.png",
              "sizes": "32x32",
              "type": "image/png"
          }
      ],
      "start_url": "/index.html",
      "display": "fullscreen",
      "theme_color": "#B12A34",
      "background_color": "#B12A34"
  }
  ```

* New alternatives: ReactNative, Flutter.