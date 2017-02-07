# Learn and Understand NodeJs

This an [Udemy]() course to learn and become an amazing NodeJs Developer.

We go deep dive under the hood of NodeJs understanding whats happening from the Js perspective.

__Don't Imitate, Understand__

## Introduction

Basically we will be using the command line

* Command Line Interface
  * A utility to type commands to your computer rather than clicking
    * Bash on linux
    * Terminal on Mac
    * Cmd on Windows (not anymore)

[Command Line resources]([http://cli.learncodethehardway.org/book/](http://cli.learncodethehardway.org/book/) )

_____________________________________

## V8: The Javascript Engine

This is the _core_ of NodeJs

#### Conceptual Aside: Processors, Machine Code and C++

* Processors: Little machines that executes instructions in several types of languages.
* Machine Code (Language)
  * Programming languages spoken by computer processors. __Every__ program you run on yout computer has been converted (compiles) into machine code.
* C++ is the language in what NodeJs is __written__. Because the _V8_ is written in C++.

#### Javascript Aside: Javascript engines and the Ecmascript specification

* Ecmascript
  * The standard javascript is based on. Needed a standard since there are many engines.
* A javascript engine
  * A program that converts Js code into something the computer processor can understand.
  * And it should follow the ECMAScript standard on how the language should work and what features it should have.

#### V8 Under the hood

[Chrome V8 Google search ](code.google.com/p/v8)

* V8 is Google's open source Js engine
* V8 is written in C++.
* V8 implements ECMAScript
* V8 Supports differents processors architectures

#### Adding features to Javascript

![My c++](https://github.com/eduardosanzb/learningProgramming/blob/master/JS/img/nodejs1.png?raw=true)

Because of this, we can add features to JS if we create our own C++ program.

_____________________________________

## The Node Core

### Conceptual Aside: Servers and Clients

There is this Architecture way of creating applications, where you have a __Server__ that is a _computer_ that executes code/logic in a petition to a __client__ application.

![client-server-model](https://github.com/eduardosanzb/learningProgramming/blob/master/JS/img/client-server.png?raw=true)

This kind of _architecture/model_  is the one that is use in the _internet_. Having a __web server__ and the __browser__ as a _client_.

![client-webserver-model](https://github.com/eduardosanzb/learningProgramming/blob/master/JS/img/webserver-client.png?raw=true)

Where we will be using __javascript__ as the language both in the __client/browser__ as in the __webserver__.

### What does javascript need to manage a server

What are the features we need in JS, to be able to manage webservices.

* Better ways to organize our code into reusable pieces.
* Ways to Deal with files.
* Ways to deal with databases.
* The ability to communicate over the internet.
* The ability to accept requests and sernd responses (in the standard format).
* A way to deal with work that takes a long time.

Some of this thngs that JS can't handle. Thats where __nodeJs__  solve for us.

### The C++ core

_In this episode we deep into the nodeJs source code._

This C++ core are features created in C++ for the _V8_.  That exists to solve the problems taht javascript cant handle for ur.

### The Javascript core

This is a large amount of Js code to perform tasks , that normally are wrappers for the C++ code.

### Let's run some JS in Node

* Install NodeJs
* run `node` in the terminal

 Whats worth of this lecture (to add to this resume) is the use of [Visual Studio Code](https://code.visualstudio.com/) an __amazing__ IDE for nodeJs.

* Breakpoint:
  * A spot in our code where we tel a debugging tool to spause the execution of our code. So we can figure out whats going on.

## Modules, exports and require

### Conceptual Aside Modules

* Module
  * A reusable block of code whose existence does not accedentally impact other code. JS didnt have this before.
* CommonJs Modules
  * An agreed upon standard for how code moudles should be structured.

#### Javascript-Aside: First-class funcions and Function expressions

* First-class functions
  * Eveything you cand do with other types you can do with functions
    * You can use functions loie strings, numners, etc.
* An expression
  * A block of code taht results ain a value
    * Funcion expresions are poebille in JS because FCF
* Invoke
  * run the function

```javascript
// function statement
function greet() {
    console.log('HI')
}
greet()

// function are firs-class
function logGreeting(fn) {
    fn()
}
logGreeting(greet)

// function expression
var greetMe = function() {
    console.log('Hi lalo');
}
greetMe()
// it's first class'
logGreeting(greetMe)

//function exression on the fly
logGreeting(() => {console.log('es6!!')})
```

Basic Js on how to use functions.

### Let's build a module

We coded a bit, I setup VS code for babel es6

### How do Modules works

We debig the require function from nodejs.

![require](https://github.com/eduardosanzb/learningProgramming/blob/master/JS/img/requireFn.png?raw=true)

But because we are going to use ES6 we will use:

+ `import` _instead_ `require`
+ `export` _instead_  `module.exports`

### More on _REQUIRE_

Basically when we `require` a folder, will __only__ require `index.js`  then we can add more files to that folder and _import_ those to the `index.js`.

### Module Patterns

* 1

```javascript
/// greet1.js
module.exports = function() {
  console.log('Hello World')
}
```

* 2

```javascript
// greet2.js
module.exports.greet = funciton() {
  console.log('Hello world')
}
```

* 3

```javascript
// greet3.js
function Greetr() {
  this.greeting = 'Helo world!!!'
  this.greet = function() {
    console.log(this.greeting)
  }
}
module.exports = new Greetr()
```

Using the files:

```javascript
var greet1 = require('./greet1')
greet1()
var greet2 = require('./greet2').greet
greet2()
var greet3 = require('./greet3')
greet3.greet()
```

But we have to be cautious with the last pattern.

```javascript
var greet3 = require('./greet3')
greet3.greet() //Helo World
greet3.greeting = 'Changed hello world'
var greet3b = require('./greet3')
greet3.greet() // Changed hello world
```

When we use the _pattern3_ NodeJs, will _cache_ the creation of the object and will pass by _reference_  the same object in multiple _requires_. This is a good approach for some cases. We will se in the future.

The __rule__ is: _When we use `new` object inside the module to export, NodeJs will cache the object._ 

In case we don't want thise, there is another _pattern_

* 4

```javascript
//  greet4.js
function Greetr() {
  this.greeting = 'Helo world!!!'
  this.greet = function() {
    console.log(this.greeting)
  }
}
module.exports = Greetr
```

and when we use the files:

```javascript
var greet4 = require('./greet4')
var grtr = new greet4()
grtr.greet()
```

* 5

```javascript
// greet5.js
var greeting = 'Hello world'
function greet() {
  console.log(greeting)
}
module.exports = {
  greet: greet
}
```

In this approach called the:

* Revealing module pattern
  * exposing only the properties and methids you want via an returned object
    * A very common and clean way to structure and protec code within modules.

Using it:

```javascript
var greet5 = require('./greet5').greet;
greet5()
```

### exports vs modle.exports

 Just to be aware that to be able to use the `exports` we have to _mutate_ the object.

```javascript
exports.greet ) function() {
  console.log('Hello')
}
console.log(exports === module.exports) // true
```

But maybe is bettter to use only `module.exports`

### Requiring native (core) modules

First we can go to the [Nodejs documentation](https://nodejs.org/api) to see which modules we have.

To grab them we do this:

```javascript
var fs = require('fs')
var util = require('util')
```

How easy!!!

### Modules and ES6

Just to remember that now we can use this for manage modules usin the ES6.

______________-

## Events and the Event Emitter

### Conceptual Aside: Events

* Event
  * Something that has happened in our app that we can respond to.
    * In Node we actually talk about two different kinds of events.

Basically in NodeJs we have __2__ different kinds of _events_.

* __System Events__. From the C++ Core _libuv_.  Where get wrapped by JS functions.
* __Custom Events__. From the JS core _Event Emitter_. Where actually we are faking events.

![events_basic](https://github.com/eduardosanzb/learningProgramming/blob/master/JS/img/events_basic.png?raw=true)

### The Node Event Emitter part 1

* Event Listener
  * The code that responds to an event. In JS case, the listener will be a function.

In this case, we are going to create or _own_ emitter.

```javascript
// emitter.js
export default class Emitter {
    constructor() {
        this.events = {}
    }

    on(type, listener) {
        this.events[type] = this.events[type] || [];
        this.events[type].push(listener)
    }
    emit(type) {
        if (this.events[type]){
            this.events[type].forEach( listener => listener() );
        }
    }
}
```

```javascript
// app.js
import Emitter from './emitter'

let emtr = new Emitter()

// setting the listeners
emtr.on('greet', function(){
    console.log('Somewhere, someone said hello,');
})
emtr.on('greet', function(){
    console.log('A greeting ocurred!');
})
// emitting the functions
console.log('Hello');
emtr.emit('greet')
```

```javascript
// output / console
Hello
Somewhere, someone said hello,
A greeting ocurred!

```

### The Node Event Emitter part 2

+ Magic String
  + A string that has some special meaning in our code. This is bad because it makes it easy for a typo to cause a bug and hard for tools to help us find it.

In this part we use the _native_ nodeJs module `events`, basically the same code we just change our import: `import EventEmitter from 'events'`  but also we set a config to avoid the _magic strings_.

We define a config file

```javascript
// config.js
module.exports =  {
    events: {
        GREET: 'greet',
        FILESAVED: 'filesaved',
        FILEOPENED: 'fileopened'
    }
}
```

and we use it like this:

```javascript
import EventEmitter from 'events'
import config from './config'

let emtr = new EventEmitter()

// setting the listeners
emtr.on(config.events.GREET, function(){
    console.log('Somewhere, someone said hello,');
})
// emitting the functions
emtr.emit(config.events.GREET)
```

#### Inheriting from the event emitter

Here we inherit properties form the _noejs_ event emitter.

Using explicit prototypal inheritence, without classes:

```javascript
var EventEmitter = require('events');
var util = require('util');

function Greetr() {
	EventEmitter.call(this);
	this.greeting = 'Hello world!';
}

util.inherits(Greetr, EventEmitter);

Greetr.prototype.greet = function(data) {
	console.log(this.greeting + ': ' + data);
	this.emit('greet', data);
}

var greeter1 = new Greetr();

greeter1.on('greet', function(data) {
	console.log('Someone greeted!: ' + data);
});

greeter1.greet('Tony');
```

Using ES6 classes:

```javascript
import EventEmitter from 'events'
import util from 'util'

class myEmitter extends EventEmitter{
    constructor() {
        super()
        this.greeting = 'hello'
    }
    greet(data) {
        console.log(`${this.greeting} : ${data}`);
        this.emit('greet', data)
    }
}

  let myemtr = new myEmitter()
  myemtr.on('greet',(data) => {
      console.log('Someone greeted: ' + data);
  })

  myemtr.greet('lalo')
  // console
  hello : lalo
  Someone greeted: lalo
```

____________--

## Asynchronous Code, libuv, The Event Loop, Streams, Files, and more…

### Javascript Aside: JS is Synchrnous

* Asynchronous:
  * More than one process running simultaneously. Node does things asynchronous. V8 not.
* Synchronous
  * One process executing at a time. Js is synchronoues. Think of it as only one line of code executing at a time. NodeJs is asynchronous.

### Conceptual Aside: Callbacks

+ Callback
  + A function passed to some other function, which we assume will be invoked at some point.
    + The function 'calls back' invoking the function you give it when it is done doing its work.

### libuv, The Event loop, and non-blocking asynchronues code.

+ Non-blocking
  + Doing other things without stopping your programming from running. This is made possible by Node's doing things asynchronously.

We dive into the libuv source code.

![libuv](https://github.com/eduardosanzb/learningProgramming/blob/master/JS/img/libuv-eventloop.png?raw=true)

[video on the event loop](https://www.youtube.com/watch?v=8aGhZQkoFbQ)

### Conceptual aside: Streams and buffers

* Buffer
  * A tempporary holding spot for data being moved from one place to another. Intentianally limited in size.
* Stream
  * A sequence of data made avaialbae over time. Pirces of data that eventually combine into a whole.

__Streams:__![stream](https://github.com/eduardosanzb/learningProgramming/blob/master/JS/img/stream.gif?raw=true)



__Buffers:__![stream&&buffer](https://github.com/eduardosanzb/learningProgramming/blob/master/JS/img/buffer.gif?raw=true)



### Conceptual Aside: Binary data, character sets and encoding

* Binary data
  * Data stored in binary (sets of 1a and 0s)
    * The core of the math that computers are based on. Each one or zero is called a __bit__ or __binary digit__.
* Character set
  * A representation of characters as numbers. Each character gets a number. UNICODE and ASCII are character sets.
* Character encoding
  * How characters are stored in binary. The numbers (or __code points__) are converted and stored in binary.

### Buffers

[nodejs documentation](https://nodejs.org/api/buffer.html)

### Js Aside: ES6 typed Arrays

The new Js now can handle arrys of types.

```javascript
var buffer = new ArrayBuffer(8)
var view = new Int32Array(buffer)

console.log(view);
view[0] = 5
view[1] = 15
console.log(view);
// output
Int32Array [ 0, 0 ]
Int32Array [ 5, 15 ]
```

We will not use it inside Nodejs for the time being, but is good to know that they exists!

### Files and fs

* Error-first callback
  * Callbacks take error object as their __first__ parameter.
    * `null`  if no error, otherwise will contain an object defininf the error. This is a _standard_ so we know in what order to place our parameters for our callbacks.

```javascript
import fs from 'fs'

fs.readFile(__dirname + '/../src/greet2.txt', 'utf8', (err, data)=>{
    if(err) console.error(err)
    console.log(data); // Nooo wey
})

let greet = fs.readFileSync(__dirname + '/../src/greet.txt', 'utf8')
console.log(greet); // Hello world
console.log('done');
// output
Hello World
done
Nooo wey
```

We can understand now the differences between a _synchrnous_ and _asynchronous_ callbacks.

__But__ We can have a problem here. Imagine we are _buffering_ large size files, and we have 1 million of concurrent callbacks. We can get the application to overflow our server. To solve this we will use __streams__.

### Streams

* Chunks
  * A piece of data being sent through a stream. Data is split in 'chunks'.
* Abstract class
  * A type of constructuro you never work directly with, but inherit from.
    * We create new custom objects which inherot from the abstract base class.

So the `Stream` object is an abstract class. That we use to create other _custom_ stream objects.

![streams](https://github.com/eduardosanzb/learningProgramming/blob/master/JS/img/streams.png?raw=true)

The _client-server_ architecture in NodeJs use this approach with _Streams_.

![streams_webserver](https://github.com/eduardosanzb/learningProgramming/blob/master/JS/img/stremwebserver.png?raw=true)



Now we are going to read a file and copy to another one.

```javascript
import fs from 'fs'

let dir = `${__dirname}/../src`
let readable = fs.createReadStream(dir+'/greet.txt',{
    encoding:'utf8',
    highWaterMark: 16*1024
});
let writable = fs.createWriteStream(dir+'/greet2.txt');

readable.on('data', chunk => {
    console.log(chunk.length);
    writable.write(chunk)
});
```

There is another best way to do this in NodeJs.

### Conceptual Aside: Pipes

* Pipes
  * Connecting two streams by writing to one stream by writing to one stream what is being read from another.
    * In node you pipe from a _Readable stream_ to a _Writable stream_.

![pipies](https://github.com/eduardosanzb/learningProgramming/blob/master/JS/img/pipes.png?raw=true)

```javascript
import fs from 'fs'

let dir = `${__dirname}/../src`
let readable = fs.createReadStream(dir+'/greet.txt');
let writable = fs.createWriteStream(dir+'/greet2.txt');

readable.pipe(writable)
```

More easy and clean code. But we can _chain_ methods necause the `pipe` returns another _stream_.

```javascript
import fs from 'fs'
import zlib from 'zlib'
const dir = `${__dirname}/../src`

let readable = fs.createReadStream(dir+'/greet.txt');
let writable = fs.createWriteStream(dir+'/greet2.txt');
let compressed = fs.createWriteStream(dir+'/greet.txt.gz');
let gzip = zlib.createGzip();

readable.pipe(writable)
readable.pipe(gzip)
        .pipe(compressed)
```

In this case, we use the `gzip` module to compress our code. Then we _chain_ out _chunk_ and the that same _Chunk_ we write it on the new file. This is possible because `gzip` use a _duplexStream_ that can __read__ adn __write__.

_____________

## HTTP and being a Web Server

### Conceptual Aside: TCP/IP

* Protocol
  * A set of rules wtwo sides agree on to use when communicating.
  * Both the client and server are pregrammed to understand and use that particular set of rules.

### Conceptual Aside: Addresses and Ports

* Port
  * Once a computer receives a packet, how it knows what program to send it to.

### Conceptual Aside; HTTP

* Http
  * A set of rules (and a format) for data being transferred on the web.
  * Stands for 'HyperText Transfer Protocol'. It's a format ( of carious) defining data being transferred via TCP/IP.
* MIME type
  * A Standard for specifying the type of data being.sent-
  * Stands for 'Multipurpose INternet Mail Extensions'. _Examples_: _application/json, text/html, image/jpeg_.

### http_parser

This is a embebed program in NodejS. That is a C program. We dive into the code

### Let's build our webserver

Finally we are creating a webserver, but this is a different first time, because we really understand whats going on.

```javascript
import http from 'http'

http.createServer((req, res) => {

   res.writeHead(200, {'Content-type':'text/plain'})
   res.end('Hello World\n')

}).listen(1337, '127.0.0.1');
```

We are importing the core library of `http`, that returns an object, that have the `createServer` function that is a _EventEmitter_  inheritance object.

We create the object and we send a _callback_ function that recceives two __Streams__ that we will use for send/receive data from our connections.

### Outputing HTML and Templates

Now we will not be sending just plan text, we need to send some real content.

```javascript
import http from 'http'
import fs from 'fs'

http.createServer((req, res) => {

   res.writeHead(200, {'Content-type':'text/html'})
   let html = fs.readFileSync(`${__dirname}/../src/index.html`, 'utf8')
   let message = 'My msg'
   html = html.replace('{Message}', message)
   res.end(html)

}).listen(1337, '127.0.0.1');
```

In this case we introduce the concept of __templates__  where we have an _HTML_ that will have some variables.

### Streams and Performance

ONe problem with the previous example is the use of `fs.readFileSync` because our app needs to wait to read the file to then send the content. If the file is two big and we have a lot of clients, this iwll be slow.

Instead of using an async call, we can use __streams__.

```javascript
import http from 'http'
import fs from 'fs'

http.createServer((req, res) => {

   res.writeHead(200, {'Content-type':'text/html'})
   fs.createReadStream(`${__dirname}/../src/index.html`, 'utf8')
        .pipe(res)

}).listen(1337, '127.0.0.1');
```

### Conceptual Aside: APIs and Endpoints

* API
  * A set of tools for building a software application.
  * Stands for 'Application Program Interface'. oN the web the tools are usuarlly made available via a set of URLs which accept and send only data via HTTP and TCP/IP.
* Endpoint
  * One url in a web API
  * Sometims that endpoint does multiple thinks by making choices based on the HTTP request headers.

### Outputing JSON

To be able to send JSON through an HTTP connection, we need to _serialize_ the object.

* Serialize
  * Translating an object into a format that can be stored or trasferred.
  * JSON, CSV, XML and others are popular.
  * 'Desiaralize' is the opposite

```javascript
import http from 'http'
import fs from 'fs'

http.createServer((req, res) => {
    res.writeHead(200, {'Content-type':'application/json'})
    let obj = {
        firstname: 'John',
        lastname: 'Doe'
    }
   res.end(JSON.stringify(obj))

}).listen(1337, '127.0.0.1');
```

### Routing

* Routing
  * Mapping HTTP requesto to content. Wheter actual files that exist on the server, or not.

```javascript
import http from 'http'
import fs from 'fs'

http.createServer((req, res) => {
    if(req.url === '/') {
        fs.createReadStream(`${__dirname}/../src/index.html`, 'utf8')
        .pipe(res)
    }
    else if(req.url === '/api'){
        res.writeHead(200, {'Content-type':'application/json'})
        let obj = {
            firstname: 'John',
            lastname: 'Doe'
        }
        res.end(JSON.stringify(obj))
    }
    else{
        res.writeHead(404)
        res.end()
    }

}).listen(1337, '127.0.0.1');
```



___________--

## NPM: The Node package manager

### Conceptual Aside: Packages and Packages manager

* Package
  * Code...
  * … manages and maintainded with a package management system.
* Package management system
  * Software that automates installing and updating packages.
  * Deals with what versio you have or need, and manages __dependencies__.
* Dependencies
  * Code taht another set of code depneds on to function.
  * If you use that code in your app, it is a dependency. Your app depends on it.

### Conceptual Aside: Semantic Versioninig (SEMVER)

* Versioning
  * Specifying what version of a set of code this is..
  * …so others can track if a new version has come out. THis allows to watch for new features, ot ro watch for 'breaking changes'.
  * The word _semantic_ implies that somethind conveys meaning.
  * MAJOR.MINOR.PATCH
  * 1.7.3
  * [semver](semver.org)

### NPM and npm registry

[npm](npmjs.com)

We talked about the NPM and all that!

_______

## Express

We will be using express.

We will install it to our app

`npm i express -save`

* Enviroments variables
  * Global variables specific to the server enviroment out code lives in.
  * Different server can have different variables, and we can access to them by code.
* HTTP Method
  * pecifies the type of action the request wish to made. Also called __Verbs__.

```javascript
import express from 'express'
let app = express()
let port = process.env.PORT || 3000

app.get('/', (req, res) => {
    res.send('<html><head></head><body>hols</body></html>')
})

app.get('/api', (req, res) => {
    res.json({color:"red", number:5})
})
app.listen(port)
```

### Routing

Another thing we can get via the _HTTP requests_ are variables, this is when _routing_ comes handy

```javascript
app.get('/person/:id', (req, res) => {
    res.send(`<html><head></head><body>person: ${req.params.id}</body></html>`)
})
```

With that function, we get the value `:id`  form the _http request_.

### Static files and Middleware

* Middleware
  * Code that sits between two layers of software. In this case Express sitting between the reques and the response.

```javascript
import express from 'express'
let app = express()
let port = process.env.PORT || 3000

app.use('/assets', express.static(`${__dirname}/../public`))

app.get('/', (req, res) => {
    res.send('<html><head><link href=assets/style.css type=text/css rel=stylesheet /></head><body><h1>Hello world!</h1></body></html>');
})

app.get('/api', (req, res) => {
    res.json({color:"red", number:5})
})

app.get('/person/:id', (req, res) => {
    res.send(`<html><head></head><body>person: ${req.params.id}</body></html>`)
})


app.use('/', (req, res, next) => {
    console.log(`Requested URl: ${req.url}`);
    console.log(`${__dirname}/public`);
    next()
})

app.listen(port)
```

__Important__ don't forget the use of the `next()` function in your middleware!!!!

### Templates and templates engines

We simply add the instruction 

`app.use('view engine', 'ejs')` Because thats the one we choose to use. And then in each request:

```javascript
app.get('/person/:id', (req, res) => {
    res.render('person', {ID: req.params.id})
})
```

### Querystring and post parameters

* __Query__ are used in the GET method, _express_ already parse those for us.

```javascript
// http://localhost:3000/person/34?qstr=123
app.get('/person/:id', (req, res) => {
    res.render('person', {ID: req.params.id, Qstr: req.query.qstr})
})
```

Where we use the `req.query.qstr`

* URLEnconded used in _POST_ method.

```javascript
let urlencodedParser = bodyParser.urlencoded({extended:false})
app.post('/person', urlencodedParser, (req, res) => {
    res.send('Thank you')
    console.log(JSON.stringify(req.body));
})
```

* JSON data

```javascript
let jsonParser = bodyParser.json ()
app.post('jsonperson', jsonParser, (req, res) => {
    console.log('Thanks for the json data');
    console.log(JSON.stringify(req.body));
})
```

I search in the web which one use in the _POST_  they recommend use JSON data.

### RESTful API's and JSON

+ REST
  + Architectural _style_  for building API's. Stands for 'Representational State Transfer'. We decide that HTTP's verbs and URLs mean something.

Basically this means, combine the _HTTP Verbs_ with the _URL_ to define a structure.

```javascript
app.get('/api/person/:id', function(req, res) {
	// get that data from database
	res.json({ firstname: 'John', lastname: 'Doe' });
});

app.post('/api/person', jsonParser, function(req, res) {
	// save to the database
});

app.delete('/api/person/:id', function(req, res) {
	// delete from the database
});
```

## Strcturing and APP

This is about organize your application code.

There are several options, some of them:

* Express-generator
* MVC

### Conceptual Aside: Relational Databases and SQL

We got the explanation of what is a relational database.

### NODE and MYSQL

We connect to a mysql database, the important thing ot remember is that we will get a JS object (array, json) from our _query_ result.

### Node and nosql

* NOSQL
  * A variaety of technologies that are alternative to tables and SQL. One of those types are __documents__ and _MongoDB_ is one of those.

### MongoDB and Mongoose

The most popoular way to use MongoDb with Nodejs is to use Mongoose.

Here we connect to a cloud mongo test database. We need to learn/practice mongo queries.

_____________

## The MEAN Stack

### MongoDB, Express, Angular, NodeJS

* Stack
  * The combination of all technologies used to build a piece of software.

## Now we will be talking about AngularJS, so we will skip it. The intro nope



* DOM
  * The structure browsers use to store and manage web pages. A set of C++ objects
  * Stands for 'Document Object Model'. Browser gives the JS engine the ability to manipulate the DOM

_____-

## Let's Build an App

We will create a __NodeTodo__ app, with the next requirements:

* An user can add, edit and delete 'todos'.
* Each todo can be marked as complete.
* Each todo cn have one optional file attachment.
* One person cannot access another person todos.