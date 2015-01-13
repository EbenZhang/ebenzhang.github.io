The 'interesting' JavaScript Types
---------------------------------------------

## `typeof`  primitive types ##
Here lists the `typeof` results of primitive types:
 
```javascript
var undefinedVariable;
var nullValue = null;
var anObject = new Object();
var func = function () { };

typeof ("string"); //result in "string"
typeof (1024); // number
typeof (true); // boolean
typeof (undefinedVariable); // undefined
typeof (anObject ); // object
typeof (undeclaredValue); // undefined
typeof (nullValue); // object
typeof (null) // object
typeof (func) // function
typeof (Object) // function
```
Things need to be highlighted are:

 - Both `undefined` and `undeclared` variable are of type `undefined`
 - Variable is `null` when assigned with null otherwise it will be `undefined`
 - `Object` is a `function`
 - `null` is an `object`, I guess it means a null object.

## `Object` is a `function` rather than an `object` ##

`Object` is actually a constructor function to create objects (that why `typeof(Object)` result in `function`), just like the `Person` function shown below.

```javascript
var alice= new Object();
alice.name = "Alice";
//or alice = { name : "Alice" };, the result will be the same.
console.log(alice.constructor.name);// Object;
console.log(typeof alice);// object
console.log(typeof Object);// fucntion

function Person(name){
    this.name = name;
}

var bob = new Person("Bob");
console.log(typeof bob); // object
console.log(typeof Person); // function
console.log(bob.constructor.name); // Person
```

Similarly, the upper case `String`, `Number`, `Boolean` are `functions` as well as `Object`:

```javascript
typeof(String);  // function
typeof(Number);  // function
typeof(Boolean); // function

typeof(string);  //string is not defined
typeof(boolean); // boolean is not defined

var s = new String("test");
typeof(s); // object
s.constructor.name;; // String
"test".constructor.name; // String
```

##Other functions##

Array, Date, RegExp are functions as well.
```javascript
typeof(Array);// function
typeof(new Array());// object
typeof(Date); // function
typeof(new Date()); // object
typeof(RegExp); // function
typeof(new RegExp());// object
```

Now, how can we determine my object is of a specific function? The answer is `instanceof`

    new Array() instanceof Array;// true
    [] instanceof Array; // true
    [].constructor.name; // Array

##function##
functions are first-class objects, they are objects and can be manipulated and passed around just like any other object. Specifically, they are Function objects, just like the relationship between `Object` and `object`.

```javascript
typeof(Function); // function
typeof(function(){}); // function
var x = function(){};
x.constructor.name; // Function
x instanceof Function; // true
function y(){}
typeof(y); //function
y.constructor.name; // Function
y instanceof Function; // true
```
