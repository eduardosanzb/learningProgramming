# Functional Programming
-------------------
## Que es esto?
Para poder mejorar mis habilidades como programador (especialmente JS y porque quiero aprender bien ReactJs) estudiare un tutorial de 6 partes de Functional Programming, todo esto recomendado por el libro:
* [ReactJS For Dummies: Why & How to Learn React Redux, the Right Way.](https://reactjs.co/) 

Por lo tanto estos seran mis apuntos del curso.

-----------------------
## Higher-order Functions
La primera parte del curso nos recomienda entender bien los conceptos de programacion funcional y del framework VanillaJs.

En la programacion funcional las funciones son valores.

Functions -> Values
``` javascript
var triple = function(x){
    return x * 3;
}
var waffle = triple

waffle(30) // output: 90
```

Las **Higher-order functions** (functions as paramenters)
Ahora si tenemos un array de animals (nombre,tipo) y queremos filtrar a los perros.
##### Composition
``` javascript
var animals = [
    { name: 'Fluffykins', species: 'rabbit'},
    { name: 'Caro', species: 'cat'},
    { name: 'Hammilton', species: 'dog'},
    { name: 'Harold', species: 'dog'},
    { name: 'Jimmy', species: 'fish'}
]
// La forma clasica con un for
var dogs = []
for(var i = 0; i < animals.length; i++){
    if(animals[i].species === 'dog'){
        dogs.push(animals[i])
    }
}

//De una manera funcional, utilizando la funcion filtro de //Array.prototype
var dogs = animals.filter(function(animal){
    return animal.species === 'dog'
})
```

en la funcion **filter** como parametro agregamos una funcion, la cual es llamada **callback function**, podemos ver como utilizando la funcion **filter** utiliza menos codigo, esto demuestra la composicion de las funciones y lo que nos ayuda a escribir menos codigo. Porque estamos reutilizando codigo.

Dentro de la funcion **filter** se encarga de hacer el loop en el array. Entonces al poner a trabajar una funcion con otra, eso es **composicion**.
Una forma mas clara d ever esta seria:
```javascript
var isDog = function(animal){
    return animal.species === 'dogs'
}
var dogs = animals.filter(isDog) //[Hammilton, Harold]
var otherAnimals = animals.reject(isDog) //[Fluffykins, Caro, Jimmy]
```
En este caso creamos una funcion que unicamente nos regresa animales que sean perros *isDog* la cual reutilizamos en dos funciones de Array.prototype *filter*, *reject*.

Espera y que son las funciones Array.prototype, estas son funciones que estan ligadas o son parte de los arreglos(object functions). [Array Prototype](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Array/prototype).

-------------------
## Part 2 Map Function
Esta funcion tambien es del tipo de high-order functions del Array Prottoype, pero en lugar de crear un nuevo objecto a partir de la funcion como filter, esta funcion va a transformar el mismo objeto.

Si recordamos el aray de animales de hace un rato, ahora queremos obtener un array con solo el nombre de los animales.
```javascript
//Forma convencional
var names = [];
for(var i = 0; i < animals.length; i++){
      names.push(animals[i].name)
}

//Forma funcional
var names = animals.map(function(animal){
  return animal.name
}
```

Por lo tanto podemos ver como al utilizar la funcion map, al contrario de filter, no retornamos un valor booleano para que la funcion decida si agrega o no el objeto al nuevo array, en este caso retorna un objeto que sera el que reemplaze al animal en cuestion.
Pero como map retorna un objeto nuevo, podemos retornar un objeto completamente distinto:
```javascript
//Forma funcional
var names = animals.map(function(animal){
  return animal.name + 'is a ' + animal.species
}
```
Imagina lo que tomaria si lo hicieramos con un foor convencional!!!
Tambien existen nuevas arrow functions que son parte de ES6:
```javascript
// funcional
var names = animals.map(function(animal){
  return animal.name
}
// funcional en una linea
var names = animals.map(function(animal){return animal.name})
// arrow function
var names = animals.map((animal) => {return animal.name})
// Inclusiva, si tu funcion es de una sola linea, podemos limpiar mas
var names = animals.map((animal) => animal.name)
// y aun mas corto, como una buena practiva
var names = animals.map((x) => x.name)
```
si comparamos la forma convencional junto ocn la arrow function:
```javascript
var names = animals.map((x) => x.name) // funcional super corta =)
var names = [];                        // =(
for(var i = 0; i < animals.length; i++){
      names.push(animals[i].name)
}
```
Claramente vemos como la programacion funcional hace el codigo mas leible y nos ayudara a progrmar super rapido!!! :smile:

-----------------
## Part 3 Reduce Function
Hasta el momento hemos aprendido acerca de 3 funciones high-order de array, ahora vamos a analizarlas

| Function        | Receive           | Creates/Output  |
| -------------   |:-------------:    | -------------:|
| Map      | new different {} | Array pero transformado |
| Filter      | Bool expression      |   Array mas pequeño |
| Reject | Bool expression      |    Array mas pequeño contrario a filter |
| Find |    Bool Expresion   | Un Match    |

Pero que pasas si ninguno de estos nos sirve para nuestros requisitos??? 
Pues usamos **REDUCE**,  de hecho con reduce podemos implementar Map, Filter, etc.

***Reduce es nuestra multi-herramienta***

```javascript
var orders = [
    {amout:530},
    {amout:530},
    {amout:540},
    {amout:760}
]
//Forma funcional
var totalAmount = orders.reduce(function(sum, order){
    console.log("Hello",sum, order)
    return sum + order.amount 
},0)

// arrow function
var totalAmount = orders.reduce((sum, order) => sum + order.amount, 0)

//forma convencional
var totalAmout = 0;
for(var i = 0; i < orders.length; i++){
    totalAmout += orders[i].length;
}
```
La funcion reduce recibe dos parametros:
* callback function
* objeto inicial
------------------
## Part 4 Advanced Reduce
En esta parte del tutorial, vamos a transformar un txt en un JSON, para eso utilizaremos nodeJs.
```
mark Johanson   waffle iron 80  2
mark Johanson   blende  50  1
mark Johanson   wknife  30  2
Nikita Smith   waffle iron 80  2
Nikita Smith   blende  50  1
Nikita Smith   wknife  30  2
```
Tenemos este data.txt, el cual tiene tabs para separar los elementos. Al final queremos un JSON asi:
```javascript
{
  "mark Johanson": [
    {
      "name": "waffle iron",
      "price": "80",
      "quantity": "2"
    },
    {
      "name": "blende",
      "price": "50",
      "quantity": "1"
    },
    {
      "name": "wknife",
      "price": "30",
      "quantity": "2"
    }
  ],
  "Nikita Smith": [
    {
      "name": "waffle iron",
      "price": "80",
      "quantity": "2"
    },
    {
      "name": "blende",
      "price": "50",
      "quantity": "1"
    },
    {
      "name": "wknife",
      "price": "30",
      "quantity": "2"
    }
  ]
}
```

Asi que como hacemos esto?

```javascript
var fs = require('fs');

var output = fs.readFileSync('data.txt','utf8')
        .trim()
        .split('\n')
        .map(function(line){
                return line.split('\t')
        })
        .reduce(function(customers, line){
                customers[line[0]] = customers[line[0]] || []
                customers[line[0]].push({
                        name: line[1],
                        price: line[2],
                        quantity: line[3]
                })
                return customers
        },{})

console.log(JSON.stringify(output, null, 2));
```
Descifrando el codigo:
* ```var fs = require('fs')``` obtiene la libreria de fileSystem incluida en nodeJs.
* ```var output = fs.readFileSync('data.txt','utf8')``` leemos el archivo data.txt para obtener los datos, y mandamos el encoding que deseamos **utf8**, obtenemos un string.
* ```.trim()``` Limpiamos el string del ultimo salto de linea.
* ```.split('\n')``` Creamos un nuevo Array, separando por cada salto de linea 
  ```
        [
          "mark Johanson\twaffle iron\t80\t2",
          "mark Johanson\tblende\t50\t1",
          "mark Johanson\twknife\t30\t2",
          "Nikita Smith\twaffle iron\t80\t2",
          "Nikita Smith\tblende\t50\t1",
          "Nikita Smith\twknife\t30\t2"
        ]
*  .map(function(line){ ...``` Dentro del array anterior, por cada linea creamos otravez un array pero ahora separado por \t tabs. Tendremos un Array de Arrays
    ````javascript
    [ [ 'mark Johanson', 'waffle iron', '80', '2' ],
      [ 'mark Johanson', 'blende', '50', '1' ],
      [ 'mark Johanson', 'wknife', '30', '2' ],
      [ 'Nikita Smith', 'waffle iron', '80', '2' ],
      [ 'Nikita Smith', 'blende', '50', '1' ],
      [ 'Nikita Smith', 'wknife', '30', '2' ] ]

* ```.reduce(function(customers, line){...``` Ahora utilizaremos reduce para crear el JSON que queremos, aqui utilizaremos la funcion sobre el Array que contiene Arrays, por cada elemento del array haremos un proceso unico. Para reduce enviamos 2 parametros (customers, line) donde:
 1. callback -> es la funcion que tendra el proceso de crear nuestro nuevo objeto
 2. {} -> es el objeto anterior, pero esta inicializado.

Dentro del callback la estrategia es la siguiente:
  1. customers es el objeto que inicializamos y enviaremos recursivamente a la funcion
  2. Inicializaremos el objeto, si este ya existe entonces lo dejaremos igual. El key de cada objeto sera el nombre, el cual es el primer elemento del array.
  3. Le empujaremos al nuevo array de objetos con la clave nombre, cada uno de los elementos que deseamos, obteniendo su informacion de la linea actual.

El resultado final seria:
```javascript
{ 
  'mark Johanson':[
    { name: 'waffle iron', price: '80', quantity: '2' },
    { name: 'blende', price: '50', quantity: '1' },
    { name: 'wknife', price: '30', quantity: '2' } ],
  'Nikita Smith':[ 
     { name: 'waffle iron', price: '80', quantity: '2' },
     { name: 'blende', price: '50', quantity: '1' },
     { name: 'wknife', price: '30', quantity: '2' } ] 
}
```

Entonces aqui observamos el uso funcional en el que llamamos una funcion tras de otra, dependiendo del obtejo de retorno:
* .trim() *La podemos usar porque el retorno del readFile(utf8) es string*
* .split() *La podemos usar porque el retorno de trim es un array*
* .map() *de igual manera, split retorna un array*
* .reduce() *igual aqui*
--------------------
## Part5 Closures
En javascript las funciones no son unicamente funciones, tmb son **closures** esto dice que el cuerpo de la funcion tiene acceso a variables que estan fuera de su definicion.
```javascript
var me = 'Bruce Wayne'
function greetMe(){
    console.log('Hello' + me);
}
greetMe() //Tendra un output de Hello Bruce Wayne
```
greetMe tiene acceso directo a la variable, no es que haga un snapshot o una copia.
Y cual es el punto de tener closures???
Miremos este ejemplo:
```javascript
function sendRequest(){
    var requestID = '123asdf'
    $.ajax({
        url: '/myUrl',
        success: function(response){
            alert('Request' + resqueID + 'returned')
        }
    });
}
```
En este ejemplo, la funcion ajax de jQuey tendra un acceso a la variable *requestID*, lo cual en estos casos es muy util, en lugar de utilizar un objeto de datos para pasar como parametro.
Tambien Mozilla tiene este [link](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures) para mayor informacion de closure.

-----------------------
## Part6 Currying
Es cuando una funcion no quiere obtener todos los argumentos de una llamada de una sola instancia, prefiere recibir un argumento y luego una funcion de callback, donde enviara el siguiente argumento y asi sucesivamente...

**Confuso??**, pues si, pero eso es bueno nos dice que estamos aprendiendo!!

```javascript
var dog = function(name){
    return function(size){
        return function(element){
            return name + " is a " + size + " dog that eats " + element
        }
    }
}
console.log(dog('Raksha')('tiny')('paper'))
var rakshaDog = dog('Raksha')
console.log(rakshaDog('enormous')('paper'))
```
this will output: ``` Raksha is a tiny dog that eats paper```

this will output: ``` Raksha is a enormous dog taht eats paper```

Las funciones currying  es una funcion que toma cada argumentos y o regresa como una funcion con una dependencia. Se regresa un valor final.
Aun quedan dudas de este tema, luego las resolveremos.
---------------------------------
## Part 7 Recursion
 Que es recursion?:
 ***Cuando una funcion se llama a si misma, hasta que ya no lo hace...***
 
 Ahora, jamas hay que aprender recursion con fibonacci, mejor un contador:
 
 ```javascript
 let countDownFrom = (num) => {
    if(num === 0) return;
    console.log(num)
    countDownFor(num - 1)
  }
 CountDownFrom(10)
 // should print
 //10
 //9
 //...
 //1
 ```
 Ahora veamos un ejemplo mas cool
 ```javascript
  let categories = [
    {id:'animals', parent: null},
    {id:'mammals', parent: 'animals'},
    {id:'cats', parent: 'mammals'},
    {id:'dogs', parent: 'mammals'},
    {id:'husky', parent: 'dogs'},
    {id:'labrador', parent: 'dogs'},
    {id:'persian', parent: 'cats'},
    {id:'siamese', parent: 'cats'}
  ]
  let makeTree = (array, parent) => {
    let node = {}
    array
        .filter(c => c.parent === parent)
        .forEach(c => node[c.id] = 
            makeTree(categories, c.id))
    return node
  }
  
  console.log(
    makeTree(categories, null)
  )
  */{
  "animals": {
    "mammals": {
      "cats": {
        "persian": {},
        "siamese": {}
      },
      "dogs": {
        "husky": {},
        "labrador": {}
      }
    }
  }
 }/*
 ```
 --------------------
 # Part 9. Promises
 Las promesas son como las funciones callback, pero son mas poderosas porque con compuestas. Como vimos en la primera parte de este tutorial.









































