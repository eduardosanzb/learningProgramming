# Understanding the weird parts of JS
All this content is a resume of the __AWESOME__ udemy course of __Anthony Alicea__ [Link to course](https://www.udemy.com/understand-javascript/learn/v4/).
You should definetely __get__ this course and watch it. You can use this notes to keep track of the course.
THe first _3_ hours of the course are free in __[youtube](https://www.youtube.com/watch?v=Bv_5Zv5c-Ts)__.
## Conceptual aside #1
* Syntax parsers
    * _A program that reads your code and determines what it does and if its grammar is valid._
* Lexical enviroments
    *  _Where something sits physically in the code you write. 'Lexical' means 'having todo with words or grammar'. A lexical enviroments exists in programming languages in which where you write something is important_
* Execution contexts 
    * _A wrapper to help manage the code that is running. **It's important**_

## Conceptual aside #2
* Name/Value pairs
    *  _A name which maps to a unique value. The name may be defined more than once, but only can have one value in any given context_
    *  ``` street = 'Main Street 123' ```
* Objects
    * _The simplest definition when talking about **Javascript**_
    * ``` javascript
        {
            street: 'Main ,
            Number: 123,
            Apartment:
             {
                Floor:3,
                Number: 301
             }
        }
    * We see the nested objects inside another object, **its just that simple**

--------------
## The gloabla enviremoent and the global object

The code run in a *Global execution context*, that runs when we start a program.
Creates:
* Global Object. (window)
* the variable __this__. 
At the global context this are the same.

Global: _Not inside a Function_
So if you create a variable or function, will be attached to the window
``` javascript
    a = 'Hello'
    function b(){}
````
* ``` window.a == a ```
* ``` window.b == b ```
The execution Context create:
1. Global Object
2. _this_
3. Outer enviroment
4. Your code

## The execution context: Creation and 'Hoisting'
We have 2 phases
1. Creation phase
    * Create global object
    * Create _this_
    * Setup the outer enviroment
    * Setup memory space for variables and functions: **_'Hoisting'_**
2. Execution phase
    * Runs your code line by line
## Conceptual aside #3
**Undefined**
this is the value set to all the variables in your code in the creation phase, so JS reserve the memory for the vars and then set this placeholder.

## Conceptual aside #4
* Single threaded
    *  _One command at a time. Under the hood of the browser, maybe not._
* Synchronous execution
    * ___One at a time.__  And in order..._
__________________

## Function invocation and the execution stack
* Invocation:
    * _Running a function. In js we use parenthesis ()_
```` javascript
    function b(){
    }
    function a() {
        b()    
    }
    a()
````
WTF happen:
1. Global Execution Context (created and code is executed)
2. When it hits ``````a()``````
    3. New execution context is created with the 2 phases.
    4. Then when hits ``````b()`````` a new execution context is created.
    5. Now we end to fill the stack and we start poping.

Here we have to keep in mind the __Execution stack__
```` javascript
    function a(){
      b()
      console.log('from a');
    }
    
    function b(){
      var a = 'from b'
      console.log(a);
    }
    
    a()
    console.log('test');
    var sd= 'sd'
    console.log(sd);
````
Will logout: 
``` 
    - from b
    - from a
    - test
    - sd
```
____________
## Functions, Context and Variable Enviroments
* Variable enviroments
    * Where the bariables live. And how they relate to each other in memory.
___________
## The scope chain
For example:
```` javascript
    function a(){
      myVar = 2
      b()
    }
    
    function b(){
      //var myVar
      console.log(myVar);
    }
    
    var myVar = 1;
    a()
    //console.log(myVar);
````
In a() nad b() if we dont found a local  ```` myVar ````  Js will chain the scope and look up in the parent or under stack context enviroment. b() will not look for ```myVar``` in the context of a()

* Scope
    * Where a variable is available in your code. And if its truly the same variable, or a new copy.
* Asynchronous
    * More than one at a time.
### The Javascript engine
Lives inside a 'browser', but besides this  engine we have another engines/modules. f/e:
* Rendering Engine
* Http request

This will event the ```event loop```
## Conceptual Aside #5
### Types and Javascript
* Dynamic Typing
    * You dont tell the engine what type of data a variable holds, it figures it out when while your code is running

##### Primitive types (A type of data that represents a single value || something that is not an object):
1. __undefined__: represesnts a lack of existence.(You shouldnt set a variable to this).
2. __null__: represents lack of existence. (You can set this one).
3. __boolean__: true || false.
4. __number__: Floating point number. Ir can make math _weird_.
5. __string__: A asequene of characters. (both '' and "" can be used).
6. __symbol__: Is ES6... not yet

## Conceptual Aside #6
* Operators
    * A special function that is syntatically (lexically) differently.
* Operator precedence
    * Which operator function gets called first. Functions are called in order of precedence.
* Associativity
    * What order operator functions get called in: left-to-right or right-to-lleft. When functions have the same precedence.
```` javascript
    var a = 3 + 4 * 5;
    console.log(a) // => 23
`````
[Link to a table of precedence operators](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Operator_Precedence)

## Conceptual Aside #7
* Coercion
    * COnverting a value from one type to another. That happens quite often in JS cuz the dynamic typing.

All this lead us to __comparison operators__, that if you dont understand what we talk before you can find this _weird_!!
```javascript
    1 < 2 < 3  => true
    3 < 2 < 1 => true //why? It should be false!
```
But if we remember the _operator precedence_ associativite left-to-right

```javascript
    3 < 2 < 1 separating with the precedence =>
        3 < 2 => false
        then
        false < 1 => 0 < 1 => true
```
That happens because the __coercion__ of `false` to number will be `0`.

So this will lead us to the equality that is something ___"weird"___ of JS.

Why the _HECK_ we have `==`and `===`, because:
* `==` : Checks equality with coercion.
* `===`: Checks equiality without coercion.

There is this [link](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Equality_comparisons_and_sameness) with a table of the comparisons.
### Existance and Booleans
Now what happen if we coerce to Booelan:
```javascript
    Boolean(undefined) => false
    Boolean(null) => false
    Boolean("") => false
    Boolean(0) => false // Alert!!
```
But sometimes the coercion is helpful for example:
```javascript
    var a;
    // we fetch come data form internet to the var a
    if(a){
        console.log('we have data')
    }
```
So in this example the if statement will fail if we couldn't fetch any data from the internet. But if we fetch a value `0` it could fail also the if. Thats something not that common. _We could fix this putting this instead `ìf(a || 0)`._
__________________

## Default Values
Let see the next function
```javascript
   function greet(name){
    console.log('Hello' + name)
   }
   greet('lalo')
```
The result will be `hello lalo`, but what would happen if we called like `greet()`, JS dont care if you dont send a paramenter, because in the __creation phase__ will allocate the space in memory and will set the default value of name to `undefined`.

But the result of the function would print `Hello undefined`, that will coerce the undefined to string. To avoid this we could set a default value to `name`. In ES6 we will have a nice way to do it, but in a lot of legazy code we coud see this approach.
```javascript
    function greet(name){
    name = name || '<your name here>' // Pay attention to 0
    console.log('Hello' + name)
   }
```
This is funny because we take advantage of the `||` operator in JS that have a neat way of work. Because operators are functions that returns value. `||` will return the first value that coerce to true.
### _Framework Aside #1_
Some libraries use default values to check if the name of the variable has not been taken.
_____
## Objects and function
### Objects and the __dot__
Objects can be callenction of name/value pairs.
But how that represents in memory?
First and object can have:
* __property__
    * Primitives
    * Objects
* __methods__
    * Functions
    
What happen in memory level is that JS gives an address to the root object _e.g:_ `0x001` and then have references to the other components of the object.
```javascript
    object = {            //0x001
        primitive: true,           //0x002
        object2: {...},            //0x003
        function: function(){...}  //0x004
    }
```
But how can we access to those spaces of memory??

There are two ways of do it.

The first one is using the _operator_ ___Computed Member Access___ : `[]`.
```javascript
    var person = new Object();              //We create a new object.
    person["firstname"] = "Eduardo"; //We access to the memomry space
    person["lastname"] = "Sánchez";
    
    var firstNamePerson = "firstname";
    
    console.log(person);                    // {...}
    console.log(person[firstNamePerson]);   // Eduardo, 
                 //we access to the memory space                                                      //but with a variable. Pretty neat.
```
And the second way to access is with the _operator_ ___Member Access___:`.` _the dot_.
```javascript
    ...continuation of the above code
    console.log(person.lastname)    //Sánchez
        //Now we access with the _dot_ operator.
    person.address = new OBject();
    person.address.street = "123 Main St.";
```
Using the first _operator_ is used more in _libraries/frameworks_ to access to the property dynamically. And the second one is the most common when using a _library/framework_.
___________
## Objects and Object Literals
To create an object we have also another way called _object literal_:`{}`.
This is not an operator, the JS engine when detect those characters assume that we are creating an object. But also we can initialize an object using this _object literal_
```javascript
    var person = {
        firstname: "Lalo",
        lastname: "Sánchez",
        address: {
            street:"123 Main St.",
            city: "Huaquechula"
        }
    }
```
### _Framework Aside #2_
* Namespace
    * A container for variables and functions. Typically to keep variables and functions with the same name separate.

So we are going to fake namespaces. Because in some frameworks may use the same variable name. To avoid collitions with other variables we can fake the namespaces.
```javascript
    var english = {};
    var spanish = {};
    english.greet = "Hello!";
    spanish.greet = "Hola!";
    //Or even we could do this:
    english = {
        greetings:{
            basic: "Hello!"
        }
    }
```
The point here is to see the limit of the space of the variables inside a same ___execution context___.
___________________
## JSON and Object Literals
A common mistake is to think that these two are the same, but nope, JSON are inspired by the _object literal notation_.
But in JSONs:
* JSON are __strings__.
* Properties have to be wrap in _quotes_: `""`.

So All JSONs are valid in JS notation but not ALL JS notation is valid to JSON.
JS have some functionalities to parse the JSON using the JSON object.
* `JSON.stringify({})`
* `JSON.parse(<string>)`
_____________
## Functions are objects
Now we are going to deep dive in one ore the more fundamentals topics in JS, that divide the good JS developers.
The idea is called __First class functions__.
* First class functions
    * Everything you can do qith other types you can do with functions. _Assign then to variables, pass them around, creat them on the fly_.

This change the way you program.
The functions reside in memory, its a special typo of object tho, because it has all the features of a normal object plus other special properties.
To a function we could attach:
* Primitives.
* Objects.
* Functions.
* ___A Name___ _It could be anonymous_.
* ___Code___ _The code that you write_. 
    * So the code that you write is not the function is another propertie of your function. BUt this porperty is __Invocable `()`__.
``` javascript
    function greet() {
        console.log('hi);
    }
    greet.language = 'english';
    console.log(greet);
```
The greet function will have:
* NAME: __greet__.
* CODE: __console.log('hi');__  That is _invocable `()`_.
* language: `'english'`.

This will lead to some interesting code, that we will see next on.
________
## Function statements and function expressions
* Expression
* A unit of code that results in a value. _It doesn't have to save to a variable_
``` javascript
    var a;
    // scripting in the chrome console...
    > a = 3;    // = is a function that returns a value. The second parameter
    < 3
    > 1 + 2;    //same here
    < 3 
```
But ___statements___ dont return values, inside statement you have an __expression__, so if _expressions_ return values, __statements__ evaluate thems.

In JS because _functions are objects_ we have objects: __statements__ and __expressions__.
```javascript
    //we only put this function in memory, that is an object and have a CODE property. But will transform in an expression until become executable.
    greet();    //This will be succesfull. BEcause function is hoisted.
    function greet() {
        console.log('hi');
    }
    //cuz fn are objects we can attach them to a variable that is a space in memory.
    anonymousGreet();   //Will not be executed, because the object of the var is not defined. Only hoist the variable.
    //*The undefined primitive is not a function*
    var anonymousGreet = function(){
        console.log('hi');
    }
    anonymousGreet(); //This will work, because now we already have set the object to the variable.
```
## Conceptual aside #8
___By value vs By reference___

In both cases we are talking about variables, 
If we set a `var a` will have an address _i.e._`0x001` and then I set `b=a` or `someFunc(a) -> (x)=>{}`,  `b`will be a copy of the memory address of `a` _i.e_`0x002`. this will pass a __by value__ because is a copy of _primitive value_. We will have 2 separate spits in memory.

Now if we have an object (__all objects__, including functions) so `a={}` and have an address _i.e_ `0x001` and then `b=a` so then `b` will point to the same place in memory `0x001`. This is called __by reference__ because the object just have kinda 2 names / alias (in this case).
```javascript
    // by value (primitives)
    var a = 3;
    var b;
    b = a;
    a = 2;
    console.log(a); //2
    console.log(b); //3
    
    //by reference (all objects (including functions))
    var c = { greetin: 'hi' };
    var d;
    d = c;
    c.greeting = 'hello';   // we 'mutate' the object with c reference
    console.log(c); //Object{greeting: "hello"}
    console.log(d); //Object{greeting: "hello"}
    
    //by reference (even as parameters)
    function changeGreeting(obj) {
        obj.greeting = 'Hola'; //mutate
    }
    changeGreeting(d);
    console.log(c); //Object{greeting: "Hola"}
    console.log(d); //Object{greeting: "Hola"}
    
    //equials operator sets up new memory space (new address)
    c = { greeting: 'howdy' };
     console.log(c); //Object{greeting: "Howdy"}
    console.log(d); //Object{greeting: "Hola"}
```
* Mutate
    * To change something. "Immutable" means it __can't be changed__.
________________
## Objects, Functions and 'this'
The keyword `this` will reference to the global enviroment, however inside an object, this will reference to the object context. But if you go deeper inside the object (like 2 levels inside this) will reference to `this` of the __global context__.
```javascript
    function a() {
      console.log(this.b);
      var m = function(){
        console.log(this); // the global this
      }
      m();
    }
    
    var b = function(){
      console.log(a);
      a();
    }
    
    b();
    
    c = {
      myName:'c object',
      func: function(){
        console.log('---------');
        console.log(a); //The a of the global enviroment
        console.log(this.a);  //undefined because is the a of the object 'c'
        var self = this; 
        console.log(this.myName); //the var of this object
        var setName = function(newName){
          this.myName = newName //This will be the global this
          self.myName = newName // but with self we reference to the this of the object.
        }
        setName('newName')
        console.log(self); 
      }
    }
    c.func();
```
## Conceptual aside #9
##### Arrays, collections of anything
```javascript
    var arr = [1, 2, 3];
    //but also
    var arr = [
    1,
    false,
    {  
        name: 'Eduardo',
        city: 'Mx'
    },
    function(name){
        var greeting = 'Hello ';
        console.log(greeting + name);
    },
    'String'
    ];
    
    console.log(arr) // [1, false, Object, function, "String"]
    arr[3](arr[2].name); // Hello Eduardo
```
As you can see, array can hold anything and thats somethign unique of JS.
______________
## 'arguments' and Spread
_'arguments'_ will not be use too much with ES6 thats why we will see _Spread_.
_arguments_ contains all values that are passed to functions.
* arguments
    *  The paramenters you pass to a function. JS gives you a keyword of the dame name which contains them all.
```javascript
    function greet(firstname, lastname, language){
        language = language || 'es';  //neat trick =)
        if(arguments.length === 0){
            console.log('Missing paramenters');
            console.log('----------------');
        } else {
            console.log(firstname);
            console.log(lastname);
            console.log(language);
            console.log(arguments);
            console.log('----------------');
        }
    }
    greet(); //hoisting will take care of the parameters, even if they are not send.
    greet('Lalo');
    greet('Lalo', 'Doe');
    greet('Lalo', 'Doe', 'DE');
    //In ES6 we will be able to set default values. But until this will be default we can use the neat trick of using ||
```
_arguments_ actually are _like arrays_ _`[]`_. Still _arguments_ we will be deprecated. in ES6 we will use _spread_ `...``.
```javascript
    function greet(firstname, ...other){
        
    }
    greet('Lalo');
    greet('Lalo', 'Sanchez');
```
### _Framework aside #3_
___Function overloading___ is not available in JS, but thats not a problem.
BEcause having _first class functions_ give us more possibilities.
Usually what happen is to spread functionality in subfunctions inside an object, because __functions are objects__.
## Conceptual Aside #10
__Syntax parsers__, remember that the code tahat you write pass through a program that parse your code and analize it.
This parser read char by char, making assumptions, executing rules and sometimes __making some changes__.
## xx DANGEROUS ASIDE: Automatic semicolon insertion xx
This is a feature of te _syntax parser_, it works searching where should be a semicolon and then insert it.
But in the use of `return` it could get _nasty_.
```javascript
    function getPerson() {
        return //;
        {
            firstname: 'Tony'
        }
    }
    console.log(getPerson()) // will not be executed
```
Still from my view (lalo) I think that js will not stop to add semicolons, so i will not use `;` but I will be taking care with using `{` properly =) .
### _Framework aside #4_
* Whitespace:
    *   Invisible characters that create literal 'space' in your writter code. Carriage returns, tabs, spaces. 
```javascript
    var 
        // First name of the person
        firstname,
        // last name of the person
        lastname
        // the language
        // can be 'en' or 'es'
        language;
        
    var person = {
        // the first name
        firstname: 'John,
        // (always required)
        lastnmae: 'Doe
    }
    console.log(person);
```
So as you can see, JS is very flexible with the whitespaces, but this help a lot to comment the code properly.
______________________
## Inmediately invoked function expressions (IIFE)$
 ```javascript
    // using a function expression
    function greet(name) {
        console.log('Hello ' + name);
    }
    greet('Lalo');
    //using an Inmediately invoked function expression (IIFE)
    var greeting = function(name) {
        console.log('Hello ' + name);
    }('Lalo');
    console.log(greeting); // Hello Lalo
 ```
 As you can see in the example above, when we use a IIFE the `var greeting` after the inline execution of the function will transform into a _string_ because thats the `return` value of the function.
But this __IIFE__ are really used in this way: Where we run a function on the flight (an anonymous one!).
```javascript
    var lastname = 'Sanchez';
    //(function{}());
    (function(name){
        var greeting = 'Inside IIFE: Hello ' + name;
        console.log(greeting);
    }(lastname));
    //or also (function(){})();
    (function(name){
        var greeting = 'Inside IIFE: Hello ' + name;
        console.log(greeting);
    })(lastname);
```
### _Framework aside #5_
How are used the IIFE in popular frameworks and libraries?
So we have this example:
```javascript
    (function(name){
        var greeting = 'Hello' 
        console.log(greeting + name );
    })('Lalo');
    //This will execute inmediately:
    // Hello Lalo
```
Let see whats happening behind the scene.
If we think about the _execution context_ in this case will not do any kind of _hoisting_ and will inmediately create in memory our function, but then when the parser see the `('Lalo')` will run the function in ___another separately context___ where the var `greeting` will exists.

![diagramn](https://github.com/eduardosanzb/learningProgramming/blob/master/JS/img/IIFE_context.png?raw=true)
_(Image is a screenshot from the __AWESOME__ tutorial of this resume in UDEMY. [link to course](https://www.udemy.com/understand-javascript/learn/v4/))_

This is really powerful cuz, we can sagely now create variables. Remeber when we talk that including 2 JS files in the html will only put one above the other or concatenate them!. Well now we dont have to worry of variable names collisions. Because using IIFE will create a _unique_ context for the file.
_________________
## Understanding Closures







