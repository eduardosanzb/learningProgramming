# Understanding the weird parts of JS
All this content is a resume of the __AWESOME__ udemy course of __Anthony Alicea__ [Link to course](https://www.udemy.com/understand-javascript/learn/v4/).
You should definetely __get__ this course and watch it. You can use this notes to keep track of the course.
Thenvironmentse first _3_ hours of the course are free in __[youtube](https://www.youtube.com/watch?v=Bv_5Zv5c-Ts)__.
## Conceptual aside #1
* Syntax parsers
    * _A program that reads your code and determines what it does and if its grammar is valid._
* Lexical enviroments
    *  _Where something sits physically in the code you write. 'Lexical' means 'having todo with words or grammar'. A lexical enviroments exists in programming languages in which where you write something is important_
* Execution contexts 
    * _A wrapper to help manage the code that is running. **It's important**_

## Conceptual aside #2
*   Name/Value pairs
    *  _A name which maps to a unique value. The name may be defined more than once, but only can have one value in any given context_
    *  ``` street = 'Main Street 123' ```
*   Objects
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
      ```
    * We see the nested objects inside another object, **its just that simple**

--------------
## The global environment enviremoent and the global object

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
​````
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
​```` javascript
    function b(){
    }
    function a() {
        b()    
    }
    a()
​````
WTF happen:
1. Global Execution Context (created and code is executed)
2. When it hits ``````a()``````
    3. New execution context is created with the 2 phases.
    4. Then when hits ``````b()`````` a new execution context is created.
    5. Now we end to fill the stack and we start poping.

Here we have to keep in mind the __Execution stack__
​```` javascript
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
​````
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
​```` javascript
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
​````
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
​```` javascript
    var a = 3 + 4 * 5;
    console.log(a) // => 23
​`````
[Link to a table of precedence operators](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Operator_Precedence)

## Conceptual Aside #7
* Coercion
    * COnverting a value from one type to another. That happens quite often in JS cuz the dynamic typing.

All this lead us to __comparison operators__, that if you dont understand what we talk before you can find this _weird_!!
​```javascript
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
This is a really important feature of JS but also kinda difficult to understand.
Well actually after learning everything we have learned will pay off.
We will start with some code:
```javascript
    function greet(whattosay){
        return function(name){
            console.log(whattosay + ' ' + name);
        }
    }
    greet('Hi')('Lalo') // Hi Lalo
    var sayHi = greet('Hi');
    sayHi('Lalo');  // Hi Lalo
```
We have a function that returns another function, but we can see something really weird.
In the first call of greet is kinda normal that we get `Hi Lalo`. 
But in the second example how does the sayHi function know the value of `greet()`, well this is possible thanks to __closures__.

What happens under the hood:
1. The ___Global__ execution context_ is generated
2. Then the _execution context_ of the `greet` function
3. When we call the `sayHi` function, will create his own execution context.

But cuz the `sayHi` execution context cant find the variable will go up in the __scope chain__, looking for the variable, but the JS engine will keep the memory direction of the `greet()` function.

![closures](https://github.com/eduardosanzb/learningProgramming/blob/master/JS/img/closures.png?raw=true)

This phenomenon is called a __closure__. This __IS NOT__ something that you type or create, is something that __happen__ within the JS engine.

### The classic example:
``` javascript
    function buildFunctions() {
        var arr = [];
        for(int i = 0; i < 3; i++){
            arr.push(
                function(){
                    console.log(i);
                }
            );
        }
        return arr;
    }
    
    var fs = buildFunctions();
    fs[0]();    // 3
    fs[1]();    // 3
    fs[2]();    // 3
```
Why did we get the value: `3`?
Because the _anonymous_ function will lookup in the __scope chain__ and because of __closures__ will find the variable _memory address__ and the last value of that variable was: `3`.
We could fix this using _ES6_ -  ___let___ or with and _array_ or an __IIFE__.
```javascript
function buildFunctions() {
        var arr = [];
        for(int i = 0; i < 3; i++){
        //or using let j = i without the IIFE
            (function(j){arr.push(
                function(){
                    console.log(j);
                }
            )})(i);
        }
        return arr;
    }
    
    var fs = buildFunctions();
    fs[0]();    // 3
    fs[1]();    // 3
    fs[2]();    // 3
```
### _Framework aside #6_
__Function Factories__
``` javascript
    function makeGreeting(language) {
        return function(firstname, lastname){
            if(language === 'es'){
                console.log('Hola ' + firstname + ' ' + lastname)
            } else if(language === 'en'){
                console.log('Hello ' + firstname + ' ' + lastname)
            }
        }
    }
    
    var greetingEnglish = makeGreeting('en');
    var greetingSpanish = makeGreeting('es');
    greetingEnglish('John', 'Doe')  // Hello John Doe
    greetingSpanish('Juan', 'Perez') // Hola Juan Perez
```
In this case each time the function `makeGreeting()` is called, JS create a complete new _context_ where a new memory space and __closures__ will scope chain the variable `language`.
____________
## Closure and Callbacks
* __Callback Function__
    * A function you give to another function, to be run when the other function is finished.
__________
## call(), apply(), bind()

This is related with the __this__ on each function, so we can control the value of __this__.

```javascript
    var person = {
        firstname: 'Lalo',
        lastname: 'Sanchez',
        getFullName: function() {
            var fullname = this.firstname + ' ' + this.lastname;
            return fullname;
        }
    }
    
    var logName = function(lang1, lang2){
        console.log('Logged: ' + this.getFullName()); //This will fail, because this is pointing to the global object.
    }
```
So how could we fix this:
There are 3 ways to do it using the special functions, this 3 functions are in all the _function objects_ as another inner property of the objects.
#### Using _bind()_
```javascript
    var logPersonName = logName.bind(person);
    logPersonName('en');   
```
_bind()_ creates a copy of the function with a new __this__ property.
#### Using _call()_ and _apply()_
```javascript
    logName.call(person, 'en', 'es');
    logName.apply(person, ['en', 'es']);
```
Here we use the original function, without creating a new copy, but using the other 2 special functions, as the first _argument_ we send the object we want to be __this__.
The only differences beetween _call()_ and _apply()_ is the way in how we send the arguments, _call()_ is like a normal function and _apply()_ with an array.

We could also use an IIFE with this functions:
```javascript
    
    (function(lang1, lang2) {
        console.log('Logged: ' + this.getFullName());
        console.log('Arguments: ' + lang1 + ' ' + lang2);
        console.log('-----------');
        
    }).apply(person, ['es', 'en']);
```

### Examples of using _bind()_, _call()_, and _apply()_
#### Function Borrowing
```javascript
    // function borrowing
    var person2 = {
        firstname: 'Jane',
        lastname: 'Doe'
    }
    console.log(person.getFullName.apply(person2));
```
Here we borrow the function `getFullName()` from the `person`object to the `person2` object.
#### Function currying
* function currying
    * Creating a copy of a function but with some preset parameters. _Very useful in mathematical situations._
```javascript
    // function currying
    function multiply(a, b) {
        return a*b;   
    }
    var multipleByTwo = multiply.bind(this, 2);
    console.log(multipleByTwo(4));
    
    var multipleByThree = multiply.bind(this, 3);
    console.log(multipleByThree(4));
```
As you can see, the functions `multiplyByTwo()` and `multiplyByThree()` are the same `multiply() ` function but with a preset variable. Also because we dont care about the __this__ in this functions we set the _global_ __this__.
_______
## Functional Programming
_Also one of my favourite stuffs of JS_

Bsically we could say that FP is about to create _functions_ that return _functions_, eventually split functionality in _functions_. All this looking for making our code __reusable__.

Your _functions_ should not __mutate__ data, or at least in the more specific _functions_.

So lets see some examples:

So we started with a simple array `arr1` that will contain numbers, and lets imagine we want to _loop_ the whole array and duplicate it. 

```javascript
var arr1 = [1,2,3];
console.log(arr1)
var arrBy2 = []
for (var i=0; i < arr.length; i++) {
        newArr.push(arr1[i]*2)
    };
console.log(arrBy2)
```

So this code is kinda _not sweet_ cuz we have quite a lot of lines and the code is not reusable.

But in JS we can pass functions as parameters, lets take advatange of that. So we can make the code __reusable__.

```javascript
function mapForEach(arr, fn) {
    var newArr = [];
    for (var i=0; i < arr.length; i++) {
        newArr.push(
            fn(arr[i])   
        )
    };
    return newArr;
}

var arr1 = [1,2,3];
console.log(arr1);

var arr2 = mapForEach(arr1, function(item) {
   return item * 2; 
});
console.log(arr2);
```

We extract the functionality of the for loop and we segmented that in a new _function_ `mapForEach()`.

That let us reuse the code for different approachs.

```javascript
var arr3 = mapForEach(arr1, function(item) {
   return item > 2; 
});
console.log(arr3);
```

Here we check if the item that we are looping is greater than _2_, we used the same `mapForEach()` function, using __first class functions__ for our advantage, and we create a _clean_ code.

But now, let's mix all of our knowledge, let's say we are going to use our `mapForEach` to check if the content of the _Array_ is greater than a limit. But we want to make this code reusable. 

Not like in the example of the `arr3`.

```javascript
var checkPastLimit = function(limiter, item) {
    return item > limiter;   
}
var arr4 = mapForEach(arr1, checkPastLimit.bind(this, 1));
console.log(arr4);
```

Here we created the _function_ `checkPastLimit` that receives two parameters, the _limit_ and the _item_ we want to _compare_.

But our `mapForEach()` only receives one parameter, so to be able to use our function we need to _default_ the value of `limiter`. 

So we use `bind()` to create a copy of the function with a default value of `1`. 

But this code is not THAT clean, let's create a function that only receives the limiter.

```javascript
var checkPastLimitSimplified = function(limiter) {
    return function(limiter, item) {
        return item > limiter;   
    }.bind(this, limiter); 
};

var arr5 = mapForEach(arr1, checkPastLimitSimplified(1));
console.log(arr5);
```

Here we created another _function_ that returns a _function_ (I understand it like a _wrapper_).

Now we had a really clean code for check the limit.

### Underscore.js

Its a funcitonal programming library. That implements cool functions.

It's recommended to chekc out their source code, they eve have a cool feature in their website.

```javascript
_.map([1,2,3], function(item){return item * 3})
// [3, 6, 9]
_.filter([1,2,3,4,5,6,7], function(item){return item > 2})
//[2,4,6]
```

________________________



## Object-Oriented JS and Prototypal inheritance

### Conceptual aside _Classic inheritance vs prototypal_

* Inheritance:
  * One object gets access to the properties and methods of another object

The classical is the one we use in JAVA, the one we learn in school. But this inheritance can be very _verbose_, you have to learn a lot of keywords.

Prototypal is more simple, __flexible__, very __extensible__ and very __easy to understand__.

First we need to understand the __prototype__

Until now we know that objects can have properties that we access with `.`but also we know taht the objects have hidden properties.

__All__ objects also have another propertie(an object) call `proto`.  But this property can be shared with other objects.

![prototype](https://github.com/eduardosanzb/learningProgramming/blob/master/JS/img/prototype.png?raw=true)



```javascript
var person = {
  firstname: 'Default',
  lastname: 'Degault',
  getFullName: function(){
    return this.firstname + ' ' + this.lastname
  }
}

var john = {
  firstname: 'John',
  lastname: 'Doe'
}
//dont do this EVER for demo purposes only!!!
john.__proto__ = person;
console.log(john.getFullName()); // John Doe
console.log(john.firstname); // John => because of the propotype chain. First look the firstname in john

var jane = {
  firstname: 'Jane'
}
jane.__proto__ = person;
console.log(jane.getFullName()) // Jane Default
```

### Everything is an Object (or a primitive)

Everything in JS have a prototype, except the original object.

```javascript
var a = {}
var b = function() { };
var c = [];
```

```
In our chrome console:
a.__proto__
will return an object
b.__proto__ also will have an object
c.__proto__ ALSOOO!!! (like array)
```

### Reflection and Extend

This another feature of the OO in JS.

* Reflection
  * An object can look at itself, listing and changing its properties and methods.

```javascript
var person = {
  firstname: 'Default',
  lastname: 'Degault',
  getFullName: function(){
    return this.firstname + ' ' + this.lastname
  }
}

var john = {
  firstname: 'John',
  lastname: 'Doe'
}
//dont do this EVER for demo purposes only!!!
john.__proto__ = person;

for(var prop in john){
  console.log(prop + ': ' + john[prop]) //This will console all the properties, even the one in the prototypes.
}

for(var prop in john){
  if(john.hasOwnProperty(prop))
	  console.log(prop + ': ' + john[prop])
} // Now we only console the props of JOHN (firstname and lastname)
```

So the function `hasOwnProperty()`is the reflection idea of JS.

```javascript
var jane = {
  address: '111 Main St.'
  getFormalFullName: function(){
    return this.lastname + ',' + this.firstname
  }
}

var jim = {
  getFirstName: function(){
    return firstname;
  }
}
_.extend(john, jane, jim);
```

this underscore funciton is quite differente from the prototypal inheritance. Because will add up the properties that don't have to the origianl main object properties.

Now go to check the source code of undersocre. 

____________

# Building Objects 

## Function constructors, 'new', and the history of JS

Was build by Brenden mike, when was the programming languages war. Js was created for the web specifically. It's called like Java, kinda looks like it, but its nothing like it.

But actually JS doestn have a class, even taht in ES6 we have a new `class` word.

```javascript
function Person(){
  this.firstname = 'John';
  this.lastname = 'Doe';
}

var john = new Person();
console.log(john) // Person {firstname: "John" lastname:"Doe"}
```

We called the function `Person`with the `new`operator, and we set it to the `john`var.

This is a way to create objects. 

__All__ this _section_ of code is about _how to create objects in JS_

So the `new`keyword, is an operator, that creates inmediately an empty object is created and then invoques the function. We know that when the function is called, JS creates a context for the function, so when the function is called with the `new`keyword, JS set the `this` to the `new`object created.

Let's verify that the object is invoqued. 

```javascript
function Person(){
  console.log(this) // Person {}
  this.firstname = 'John';
  this.lastname = 'Doe';
  console.log('his function is invoked') // The same
}

var john = new Person();
console.log(john) // Person {firstname: "John" lastname:"Doe"}
```

So now we understand, when the function is called with the `new`the engine will set the `this`context to a new object. And then will return wathever is in the function append to `this. We can return other things tho.

```javascript
function Person(){
  console.log(this) // Person {}
  this.firstname = 'John';
  this.lastname = 'Doe';
  console.log('his function is invoked') // The same
  return { greeting: 'i got in the way'};
}

var john = new Person();
console.log(john) // Object { greeting: 'i got in the way' }
```

In this example, we forced to return an object of the `Object`type. So we interrupted the JS engine process.

So what happen if we want to make generic the `Constructor`? The `Constructor`is still a function, so lets treat it like that!!!

```javascript
function Person(firstname, lastname){
  this.firstname = firstname
  this.lastname = lastname
}
var jane = new Person("Jane", "Doe");
console.log(jane) // Person {firstname: "Jane" lastname: "Doe"}
```

Just like you would do it in Java.

* Function Constructors
  * A normal function that is used to construct objects. __The `this` variable points a new empty object, and that object is returned from the function automatically__

But what about _How to set the prototype_?

## Function Constructors and .Prototype

The `new` keyword will set the prototype automatically, because a Function is an object with different properties ( ex: Code, Name, prototype ). The property `prototype`is only used when the function is created with the `new`.

__But__ no confused the property `prototype`with the `prototype`of the function.

`Person.prototype !== Person.__proto__`

So just to not get confused. The __function__ `Person()` have a `__proto__`that is the `function`prototype. And when we create a `new`object with the constructor, that object will have a `prototype` __property__.

Even that `jane.__proto__ === Person.prototype` is not the same as `jane.__proto__ !== Person.__proto__`.

![prototype_new_object](https://github.com/eduardosanzb/learningProgramming/blob/master/JS/img/object_creation_new_proto.png?raw=true)

```javascript
function Person(firstname, lastname){
  this.firstname = firstname
  this.lastname = lastname
}
Person.prototype.getFullName = function() {
  returh this.firstname + ' ' + this.lastname
}
var jane = new Person("Jane", "Doe");
console.log(jane) // Person {firstname: "Jane" lastname: "Doe"}
console.log(jane.getFullName()) // Jane Doe

Person.prototype.getFormaFullName = function() {
  return this.lastname + ', ' + this.firstname
}

console.log(jane.getFormalFullName()) // Doe, Jane
```

So we can add functions to the prototype, even later of the creation because the object will look for the proto chain.

But why not add the functions to the Person `Constructor`?

Because lets remember that __functions are objects__ so if we set the function in the `constructor`when we create the object, we will create an object ofr the function and if we create 1Million of Persons, we will create 1 million of function objects.

The solution is to add the methods to the protype, so we will have only onje object.

### xx DANGEROUS ASIDE: 'New' and Functions xx

What could happen if we forget the `new`keyword?  _Will set the variables to `undefined`._

So to avoid this, lets have a convention, that if your function is a constructor, let the first letter as Capital letter.

### _ Conceptual Aside Built-in Function Constructors _

Js have some functions that exists like:

* Number
  * `var a = new Number(3)`
* String
  * `var a = String('John')`

This are not `primitives`they are objects, but still in some cases the JS engine will wrap the primitives into the objects. LIke String.

So we can add features to the __Primitves__ objects.

```javascript
String.prototype.isLengthGreaterThan = function(limit) {
  return this.length > limit
}
console.log("John".isLenghtGreateThan(3)) // True
```

In this case, the string `"John"`was wrapped into the String object and inherit the function that we set in the prototype.

```javascript
Number.prototype.isPositive = function() {
  return this > 0;
}
console.log(3.isPositive()) // Error
```

Because JS don't convert/wrap numbers in objects.

### xx DANGEROUS ASIDE: Built-in Constructors xx

```javascript
var a = 3
var b = new Number(3)

a == b // True cuz cohersion
a === b // false cuz without cohersion
```

So is not recommended to not use the Built-in constructors, only if you really need them.

### xx DANGEROUS ASIDE: Arrays and for..in xx

```javascript
Array.prototype.myCustomFeature = 'cool!'
var arr = ['John', 'Jane', 'Jim']
for(var prop in arr) {
  console.log(prop + ': ' + arr[prop+])
}
//This will also print the new feature
                                
```

So avoid use the `for..in` in arrays and use the normal `for`or a `map()`or some function lat that one.

## Object.create and pure prototypal inheritance

This is a way to create objects, in the JS way.

```javascript
var person = {
  firstname: 'Default',
  lastname: 'Degaul',
  grett: frunciton () {
    return 'Hi ' + this.firstname;
  }
}
var john = Object.create(person)
console.log(john) // Object{}
```

Will create an empty object, __but__ with the `__ proto __ `  will be the `person`object.

```javascript
var person = {
  firstname: 'Default',
  lastname: 'Degaul',
  greet: funciton () {
    return 'Hi ' + this.firstname;
  }
}
var john = Object.create(person)
john.firstname = 'John'

console.log(john.greet) // Hi, John
```

This is pure prototypal inheritance.

* Polyfill
  * Code that adds a featura which the engine may lack

### ES6 AND CLASSES

This another way to create objects and setup prototypes.

Is a way to keep the familiarity with other languages. But maybe is better to appreciate the JS way.

```javascript
class Person {
  constructor(firstname, lastname){
    this.firstname = firstname
    this.lastname = lastname
  }
  greet(){
    return 'Hi ' + firstname
  }
}
var john = new Person('Jon', 'Doe')

//And the inheritance?

class InformalPerson extends Person {
  constructor(firstname, lastname) {
    super(firstname, lastname)
  }
  greet() {
    return 'Yo ' + firstname
  }
}
```

This is what comming ot the new JS, but is just another syntatical way to create objects.

* Syntactic Sugar
  * A different way to type something that doestn change how it works under the hood

____________________

# ODDS AND ENDS

Concepts interesting of JS.

## 1 Initialization

```javascript
	var people = [
      {
        // the 'john' object
        firstname: 'John',
        lastname: 'Doe',
        addresses: [
          '111 Main St.',
          '222 Third St.'
        ]
      },
      {
        // the 'john' object
        firstname: 'Jane',
        lastname: 'Doe',
        addresses: [
          '111 Main St.',
          '222 Third St.'
        ],
        greet: function() {
          return 'Hello'
        }
      }
	]
    console.log(people) // [Object, Object]
```

Maybe this could look difficult, but is not. Just keep an eye on the `,` This could be useful to setup some data.

## 2 typeof, instances, and Figuring Out What Something is

```javascript
var a = 3;
console.log(typeof a); // number

var b = "Hello";
console.log(typeof b); // string

var c = {};
console.log(typeof c); // object

var d = [];
console.log(typeof d); // object => weird!
console.log(Object.prototype.toString.call(d)); // [object Array] => better!

function Person(name) {
    this.name = name;
}

var e = new Person('Jane');
console.log(typeof e); // object
console.log(e instanceof Person); // true

console.log(typeof undefined); // undefined => makes sense
console.log(typeof null); // object => a bug since, like, forever...

var z = function() { };
console.log(typeof z); // function
```

## 3 Strict Mode

Js can be liberal in some ways.

You can make the JS engine to be more strict or picky.

```javascript
var person;
persom = {}
console.log(persom) // Object {}
```

But we can tell to the engine to implement some extra rules when parse the code

```javascript
"use strict";

var person;
persom = {}
console.log(persom) // Error persom is not defined
```

But you can also set the strict mode to an specific context

```javascript
function logNewPerson() {
  "use strict";
  
  var person2;
  persom2 = {}
  console.log(persom2) // Error persom2 is not defined.
}
var person;
persom = {}
console.log(persom) // Object {}
```

[Strict Mode Reference]([https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Strict_mode](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Strict_mode))

___________

# Learning from Other's Good Code

### An opensource education

Basically we can learn a lot from diving into the source code of some libraries, like angularJs or jquery.

* Method Chaining
  * Calling one method after another and each method affects the parent object
    * So obj.method1().method2() where both methods end up with a thisvariable pointing at "obj"

You can achieve this by returning `this` at the end of the object.

# Let's build a framework / Library

[Repo with code](https://github.com/eduardosanzb/learningProgramming/tree/master/JS/LetsBuildOurFramework)

# Bonus Lectures

## Typescript, ES6 and Transpiled languages

* Transpile
  * Convert the syntax of one programming language to another.
    * In this case languages that don't really ever run anywhere, but instead are processed by 'transpilers' that generate JS.

### Typescript

Provided by Microsoft,  The main feature of this transpiler, is taht JS now have Types.

### Traceur

This will transpile ES6 and 	ES7 to traditional JS.

_____________

For more on Typescript head to: [http://www.typescriptlang.org](http://www.typescriptlang.org/)

and try out writing Typescript code in your browser here: [http://www.typescriptlang.org/Playground](http://www.typescriptlang.org/Playground)

For more on Traceur head to: [https://github.com/google/traceur-compiler](https://github.com/google/traceur-compiler)

and try out writing ES6 code in Traceur in your browser here: [https://google.github.io/traceur-compiler/demo/repl.html#](https://google.github.io/traceur-compiler/demo/repl.html#)

______________

# BONUS: Getting Ready for ECMAScript 6

## Existing and upcoming features

[Link to the repo](https://github.com/lukehoban/es6features)



# Conclusion

Js is a weird but __beautiful__ language. It's worth to keep learning it!!!

:D

