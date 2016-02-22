# Lesson 1 - Intro & Syntax

## History

### Invention
* 1995 Netscape wanted to add scripting to web pages
* Brandon Eich wrote the first version in 10 days

### JavaScript is NOT JAVA
* Originally called Mocha, then LiveScript, then JavaScript
* Netscape marketing department named it JavaScript to capitalize on popularity of Java
* Eich was a fan of functional language Scheme which heavily influenced JavaScript

### JavaScript was designed for hobbyists
* Easy to learn
* Forgiving
* loose typing
* JavaScript has a bad name because much bad code was written by amateurs
* But its open nature made it extremely popular

### Where does JS run?
* Browser engines
* Web server (NodeJS)
* Databases (mongo, rethink, couchdb)
* Commandline tooling
* Native mobile (JavaScriptCore)
* Desktop OSX (JavaScript for Automation in Yosemite), Windows

### Open platform
* JavaScript's official name is EcmaScript!
* ECMA maintains and updates Standards (TC39 committee)
* Open platform -> fragmentation (features and performance)
		* Features - Often implementation happens before specification
		* Performance - different performance characteristics
* Most JS is ES5, but ES6 was ratified summer of 2015



### Variable declaration
Variables are declared with var. All statements in JS end with a semicolon.

```javascript
var x = 1;
var y = "2";
var z = true;
```

### Loose typing
variables are declared with var, but have a type that can be changed

```javascript
var z = 1;
z = 'abc'; //would throw an error in strong typed languages
typeof z //string
z = 1
typeof z //number
```

### Coersion
variable type is coerced to a type that makes sense when reached

**IMPLICIT** coercion

```javascript
7 + 7 + 7; // = 21

// First two 7's are calculated... then concatened into a string. 
// returning a string value
7 + 7 + "7"; // = "147"

// The entire expression is implicitly converted into a string and concatenated 
"7" + 7 + 7; // = "777"
```
Above, when we add Integers (numbers) together, we get a sum of 21. 
However, in our second example, the first two 7s are added together (14) and then converted into a string and concatenated with the string value of 7.
In the final example, the entire expression is almost immediately converted into a string.

**EXPLICIT** coercion:

As you can see, JavaScript is trying to be helpful, and sometimes this is the desired behavior. 
However, what happens when you retrieve data from a user and it needs to be forcibly coerced? This is where Explicit coercion comes in.

Let's say you are helping a local non-profit raise funds and the pledges are coming in through the website. Obviously, the data is going to come in a string. It then needs to be coerced into a number (so that it can be added).

Here's an example:

```javascript
// perform in browser console
var amountRaisedSoFar = 1000;

var newDonation = prompt("How much would you like to donate?"); 

amountRaisedSoFar = Number(newDonation) + amountRaisedSoFar;

console.log("We have now raised: " + amountRaisedSoFar + "!");
```

Here we are taking the user input in as a string, then converting it into a number when adding it to the amountRaisedSoFar variable. Without the Number(), it would concatenate the two numbers as strings.


### Type Casting

You can convert from one type to another using these other built-in conversion functions as well.
* `ParseInt`
* `parseFloat()`
* `toString()`


## Flow control
* if
* Operators (<>, = vs == vs ===)
* truthy values
* for
* while


### `If` statement
```javascript
//Simple if statement
var x = true;
if(x) {
	console.log(true);
} else {
	console.log(false);
}
```

**CHALLENGE 1** Do you need more coffee?<br>

Write a script that:
- stores the number of cups (that a person has consumed) in a variable
- if the person has had *less than* 3 cups 
	- log a message to the console saying: ("Yes I'll take another cup of coffee")
- if not
	- log a message to console saying ("I think I'm okay for now")   


**CHALLENGE 1 ANSWER:**

```javascript
var cups = 1

if(cups < 3){
    console.log("Yes. I'll take another cup of coffee!");
} else {
    console.log("I think I'm okay for now.");
}
```

### Loose Equality
The double equals `==` tries to ignore the type when comparing. The triple equals `===` takes type into account. These are called loose and strict equality checks. You pretty much always want to use strict.

```javascript
var x = 10;
//Type coersion is happening here!
if(x == '10') {
	console.log(true); //true
}

if(x === '10') {
	console.log(true); //false
}
```

### Truthy and Falsey Values
Pretty much all objects and variables are coerced to `true`.

```javascript
if('abc') {
	//true
}

if(10 && 'abc' && true) {
	//true
}
```

Only falsey values: `false`, `0`, `null` or `undefined` evaluate to false

```javascript
var whatever;
if(false || 0 || null || whatever) {
	console.log(true)
} else {
	console.log(false);
}
```

### Comparison Operators
* `<` 
* `>`
* `<=`
* `>=`
* `==`
* `===`
* `!=`
* `!==`


### Short-circuit evaluation
As soon as the interpreter knows that a statement will evaluate to `true` or `false`, the rest of the statement is not executed.

```javascript
true || somethingTotallyUndefined;  //true, no error
false && somethingTotallyUndefined; //true, no error
```

**CHALLENGE 2**

1. Create a variable for the temperature and set it to 80
2. Write a statement that outputs the temperature as "The temperature is 80 degrees";
3. If the temp is greater than 80, output "time to swim" (set temp to 60, 90) and test;
4. Create a precipitation variable and set it to false
5. Only output "time to swim" if temp is greater than 80 and its not raining
6. Set the precipitation variable to 'raining' or 'snowing' and only output 'time to swim' if there is no precipitation
7. Create an 'indoors' variable and set it to true
8. If indoors, then output 'time to swim' regardless of the temp and precip.


**CHALLENGE 2 ANSWER:**

```javascript
var temp = 85;
var precipitation = false;
var indoors = true;

console.log("The temperature is " + temp + " degrees");

if (indoors || (temp > 80 && !precipitation)) {
  console.log("time to swim!");
} else {
  console.log("lets's not swim today");
}
```

## Iteration

### For loop

```javascript
for (var i = 0; i <= 9; i++) {
    console.log( i );
}
```

Let's work through each part of the loop. 
Within the parentheses, there are 3 key things happening: 
1. We are **setting** a variable's value (in this case, to 0)
2. We are **comparing** the variable's value to the desired break-point
3. we are **incrementing** the *value* of the variable on each *iteration*


**CHALLENGE 3** - 99 Bottles<br>
(5 minutes)

- Using a `for` loop.
- Write a simple version of "99 bottles of beer on the wall"
  -(note: make sure you're logging the result to the console)
- Once you get the program running, log "Hey! We need more beer!" to the console when your counter hits 0


**CHALLENGE 3 ANSWER:**

```javascript
var bottle = 99;

for (bottle; bottle >= 0; bottle --){

  if (bottle === 0) {
    console.log("Hey! Go buy more beer!");
  } else {
      console.log(bottle + " bottles of beer on the wall");
  }
}
```

### While Loop
```javascript
var x = 0;
while(x < 10) {
	x = x + 1;
	// or x++;
}
```


## Arrays
* Arrays are ordered lists with methods to traverse and edit.
* Zero based index
* Dynamic length and types

### Creating an Array
```javascript
var teachers = ['Mariel', 'Lee'];
```

### Addressing an Array
```javascript
console.log(teachers[0]) //'Mariel'
```

### Get Array Length
```javascript
teachers.length;  //3
```

### Push and Pop 
```javascript
var teachers = ['Mariel', 'Lee'];
teachers.push('Zack'); //['Mariel', 'Lee', 'Zack']
var teacher1 = teachers.pop(); //teacher1 == 'Zack', teachers == ['Mariel', 'Lee']
```

### Shift and Unshift (from the front)
```javascript
var teachers = ['Mariel', 'Lee'];
teachers.unshift('Zack'); // ['Zack', Mariel', 'Lee']
var teacher = teachers.shift(); //teacher == 'Zack', teachers = ['Mariel', Lee']
```

### Arbitrary Adding
```javascript
teachers[4] = 'Shane'; // ['Mariel', 'Lee', 'Zack', undefined, 'Shane'];
```

### Finding an item
```javascript
teachers.indexOf('Mariel'); //0
teachers.indexOf('Bryan'); //-1 (not in the array)
```

### Iterating Over an Array
Iterating over Arrays using for loop and forEach

```javascript
var teachers = ['Assaf', 'Shane', 'Zack']
for(var i = 0; i < teachers.length; i++) {
	console.log(teachers[i]);
}

//Uses a function, more on that next class
teachers.forEach(function(item) {
	console.log(item);
})
```

### Converting Arrays to Strings
```javascript
teachers = ['Mariel', 'Lee'];
teachers.toString(); 'Mariel,Lee'; //notice there is no space
teachers.join(' & '); 'Mariel & Lee';
```

### Ordering
```javascript
var a = [2, 1, 3]
a.sort(); //[1,2,3]
a.reverse(); //[3,2,1]
```


## Objects

An object is an unordered set of keys and values, like a hash in Ruby.

```javascript
var course = {
	name: 'Code Immersion',
	awesome: true
}
```

Values can be anything--strings, numbers, booleans, arrays, functions, or even other objects

```javascript
var course = {
	name: 'Code Immersion',
	awesome: true,
	students: ['Emily', 'Yassi'],
	instructor: {
		name: 'Mariel',
		company: 'Tech Talent South'
	}
}
```

### Addressing an Object
Object properties can be referenced in two ways. The more common *dot* notation, or the *bracket* notation, which is useful if you have a property name saved in a string.

```javascript
course.name
course['name']
```

You can combine dot and bracket notation to address infinitely deeply nested values inside objects.

```javascript
var course = {
	name: 'Code Immersion',
	awesome: true,
	teachers: ['Mariel', 'Lee']
}

console.log(course.teachers[0]); //Mariel
```

A more complex example:

```javascript
var course = {
	name: 'Code Immersion',
	awesome: true,
	teachers: ['Mariel', 'Lee'],
	students: [
		{
			name: 'Bryan',
			computer: {
				OS: 'OSX',
				type: 'macbook'
			}
		}
	]
};

console.log(course.students[0].computer.OS); //OSX
```

### Update an Object
Properties of objects can be updated after an object is created.

```javascript
course.name = "super duper class";
```

### Mutate an Object

You can also assign entirely new keys, delete existing ones.

```javascript
course.fun = true; //add a property
delete course.name; //remove one
```

**CHALLENGE 4: Addressing Objects**
Given the following object:

```javascript
var course = {
    name: 'Code Immersion',
    awesome: true,
    teachers: ['Mariel', 'Lee'],
    students: [
        {
            name: 'Tina',
            computer: {
                OS: 'OSX',
                type: 'macbook'
            }
        },
        {
            name: 'Marcus',
            computer: {
                OS: 'Windows',
                type: 'laptop'
            }
        },
        {
            name: 'Bryan',
            computer: {
                OS: 'OSX',
                type: 'macbook'
            }
        }
    ],
    preReqs : {
        equipment: {
            laptop: true,
            OSOptions: ['linux', 'osx', 'windows']
        }
    }
};
```

1. Name of the course ('Code Immersion')
2. Name of the second teacher ('Lee')
3. Name of the first student ('Tina')
4. Bryan's computer type ('macbook')
5. The preReq equipment object
6. The second OSOption from equipment prereqs ('osx')
7. String listing the OSOptions separated by 'or' ('linux or osx or windows')
8. An array of all the students that are using OSX.


## Homework
1. Finish the Challenge #4

2. Create a rock paper scissors game that runs until one player has three wins

	- Store the player names and number of wins for each player in variables
	- Use a while loop to run the game until one player has 3 wins
	- Use `parseInt(Math.random()*10)%3` to generate a random number from 0 to 2 (0 == rock, 1 == paper, 2 == scissors)
	- Output each player's hand to the console
	- Use if statements to determine a winner of the round
	- Output the round winner's name to the console
	- Keep track of how many rounds each player has won
	- When one player wins 3 rounds, announce that player's name as the game winner

3. Complete the Nodeschool javascripting tutorial and post your completed screenshot to Slack by Wednesday morning [https://github.com/sethvincent/javascripting](https://github.com/sethvincent/javascripting) 
