# Lesson 2 - Functions

## Recap
- On Monday we talked about arrays and objects, which hold structured data.
- What can we do with that data? This is where functions come in.
- JavaScript is a functional language, so it's all about the functions!
- Functions usually confuse people new to JavaScript, because they work differently than in other languages like Ruby.

## Function Basics
- Functions are procedures or actions that take arguments and return values.

### Define a function

- A function has a name, an argument list, and a body.
- Arguments can be named anything you want since they are placeholders for the variables, etc you will pass in when you call the function.

```javascript
function saySomething(something) {
	console.log(something);
}
```

### Running a function
Execute a function by calling it's name with () and passing in any needed arguments.

```javascript
saySomething('Hello function!'); //logs 'hello function!'
```

### Return values

Functions can 'return' a value. A function evaluates to its `return` value when run.<br>
*(note : Ruby _implicitly_ returns values. JS requires an _explicit_ return statement if you are expecting a return value)*


##### Wrong

```javascript
function add(number1, number2) {
	number1 + number2;
}

var sum = add(1,2);
console.log(sum) // undefined
```

##### Right

```javascript
function add(number1, number2) {
	return number1 + number2;
}

var sum = add(1,2);
console.log(sum); // 3
```

### Function Arguments
All functions take any number of arguments, regardless of their declared signature.

```javascript
function listNumbers(a,b) {
	console.log(a,b);
}

listNumbers(1); // '1,undefined'
listNumbers(1,2,3,4,5) // '1,2'
```

The arguments list simply creates variables that reference the arguments in order they were passed. For functions that take an unknown number of arguments, use the `arguments` object.

```javascript
function add() {
	var sum = 0;
	for(var i = 0; i < arguments.length; i++) {
		sum += arguments[i];
	}
	return sum;
}

add(1,2,3,4,5,6,7,8);
```


### Functions as Variables
Like other objects, functions can be assigned to variables.

```javascript
var add = function(a,b){
	return a + b
};
```

The difference between declaring a function that way (Function assignment) and the `function add(){}` syntax we've been using (Function Declaration) is that the latter (function declaration) hoists both the declaration and definition. For example:

##### Function Declaration
```javascript
hoisted(); // logs "foo"

function hoisted() {
  console.log("foo");
}
```

##### Function Assignment
```javascript
notHoisted(); // TypeError: notHoisted is not a function

var notHoisted = function() {
   console.log("bar");
};
```

In the above example, we get a `TypeError` because `notHoisted()` is declared, but undefined *until* the assignment expression `var notHoisted = `.

### Anonymous Functions

- It's often handy to declare a function on the fly without a name
- This is VERY common

```javascript
var calculator = {
	add: function(a,b) {
		return a + b;
	}
}

calculator.add(2,3) // 5
```

- Anonymous functions will often be passed into other functions

While, this might look strange, but you've already been doing this with Arrays and Objects. Functions are no different.

```javascript
var arrayOfMystery = [
	['anonymous','array'],
	{ name: 'anonymous object' },
	function(){ return 'Anonymous Function!'}
];

console.log(arrayOfMystery[0][1]); // array
console.log(arrayOfMystery[1].name); // anonymous object
console.log(arrayOfMystery[2]()); // anonymous function!

```
- `[ ]` creates an array in memory
- `{ }` creates an object in memory
- `function(){ }` creates an object in memory


## This Keyword
Inside a function, the keyword `this` refers to the executor of the function.

Think of a function as a piece of paper with instructions, a procedure of sorts. One of those instructions might say "touch your nose". But who is this "you" it speaks of? Obviously, the person executing the instructions. Similarly, the keyword `this` refers to the object that's executing the function.

```javascript
var teacher = {
	name: 'Mariel',
	sayName: function() {
		console.log(this.name);
	}
}
teacher.sayName(); //'Mariel'
```

Different objects can execute the same function and produce different results because `this` is different.

```javascript
function sayName() {
	console.log(this.name);
}

var teacher1 = {
	name: 'Mariel',
	speak: sayName
}

var teacher2 = {
	name: 'Lee',
	speak: sayName
}

teacher1.speak(); // 'Mariel'
teacher2.speak(); // 'Lee'
```

