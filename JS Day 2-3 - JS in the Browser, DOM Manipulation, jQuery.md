# Lesson 2/3 - JS in the Browser, DOM Manipulation, and jQuery

## Loading a Script
Load a script into an HTML page by adding a script tag

In the head tag

```html
<html>
  <head>
    <meta charset="utf-8">
    <title>A Page with JS</title>
    <script src="javascript/your_file.js"></script>
  </head>
  <body>
    
  </body>
</html>
```

In the body

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>A Page with JS</title>
  </head>
  <body>
    <h1> Hello World</h1>

    <script src="javascript/your_file.js"></script>
  </body>
</html>
```

##### Blocking and Performance considerations

When loading a script in the head, you'll want to consider the concept of blocking.

When a page loads: 

- Resources in the page are blocked from downloading if they are below the script.
- Elements are blocked from rendering if they are below the script.

This can seriously affect a user's experience, and slow down your app's performance. So, be mindful of where you are referencing your JS. 


####Basic example script running in a page

*index.html*

```html
<body>
  <h1 id="greeting"> Hi! </h1>

  <script type="text/javascript" src="javascript/main.js"></script>
</body>
```

*javascript/main.js*

```javascript
function greetOnLoad () {
    var name = prompt("Hi! What's your name?");
    var myelement = document.getElementById("greeting");
    greeting.innerHTML= "Hello " + name + ", it's nice to meet you!";
}

greetOnLoad();
```

### Inline vs External

Above you have seen how to load a script *externally*. However, you can also embed your js right in your html page. This is called *inline* scripting. 

##### Inline Examples

```html
  <body>
    <p id="example">Lorem ipsum dolor sit amet <br> Yes, this is a random paragraph </p>
    <button type="button" onclick="modifyColor()">Click Me</button>
    <script>
    function modifyColor() {
        document.getElementById("example").style.color = "aqua";
        document.getElementById("example").style.backgroundColor = "gray";
    }
    </script>
  </body>

```

```html
<body>
  <button type="button" onclick="alert(Math.floor(Math.random() * (7 - 1)) + 1)">Click Me</button>

  <script type="text/javascript" src="javascript/main.js"></script>
</body>
```

While inline scripting may seem convenient, it is not recommended. As you can imagine, it can get very messy.

##### Comparison

**Inline scripts**

- Difficult to maintiain
- Larger HTML file size
- Does not cache

**External scripts**

- Maintainable
- Once an external script is downloaded, the browser stores it in the cache so if another page reference it no additional download is required.
- Can be used to load client code on demand and reduce overall download time and size.


## Chrome Dev Tools / JS console
Chrome includes developer tools that let you: 

- inspect the state of JS on your page 
- send commands
- debug 

In addition to that

- variables (from your `.js` files) are visible and editable

to access your console you can:

- right click
- choose: inspect element
- click on the console tab

Alternatively, you can use the keyboard shortcut `command` + `option` + `j`

By now, you are well familiar with console.log, However, your console has a few other handy actions 

- console.warn
- console.error

## Global Objects

The global object is created as soon as the Javascript interpreter starts. For Javascript running inside a browser, this means that as soon as a new page is loaded by the browser, the global object will be created.

**Meaning: When scripts are loaded into an html document, the browser inserts some globals**

In browsers, the `window` object doubles as the `global` object

Essentially, when your working with JS in the browser, `this` initially refers the `window`

some examples: 

```javascript
window.alert();
window.prompt();
window.confirm();
```
is the same as:

```javascript
this.alert();
this.prompt();
this.confirm();
```

Other kinds of data types that loaded into global include:

- functions such as: `parseFloat()`
- properties such as `NaN` 
- objects such as `Math`

In summary, the `Window` object is basically a representation of an individual browser window.

## Document Object Model	
The most important global object is `window.document`. It represents the HTML document loaded in the window.

- DOM is a tree of nodes

- DOM provides methods for finding and editing all nodes from JS

[The HTML DOM Document Object](http://www.w3schools.com/jsref/dom_obj_document.asp)

[The HTML DOM Element Object](http://www.w3schools.com/jsref/dom_obj_all.asp)


The DOM and the nodes have an API that lets you find nodes and edit them directly. This isn't terribly useful by itself, but it lays foundational concepts.

running `window.document` in your console will give you access to the entire Document Object

## Find Nodes
There are a few different ways to get a node or array of nodes from the document.

### getElementById()
Gets a single node based on id attribute

**HTML**<br>
```html
<input id="username"></input>
```

**JS**<br>
```javascript
//Get a single node
var el = document.getElementById('username');
```

### getElementsByTagName() & getElementsByClassName()
Gets an array of nodes based on a tag name or className

**HTML**<br>
```html
<input type="text" class="error"></input>
<input type="password"></input>
<button>submit</button>
```

**JS**<br>
```javascript
//Get all inputs
var inputs = document.getElementsByTagName('input');
var inError = document.getElementsByClassName('error');

inputs.length; //2
inError.length; //1
```

### querySelector & querySelectorAll
In practice, these are the only DOM selectors you will ever need. They take a CSS selector as an argument, which means you can easily duplicate the functionality from the other DOM selection functions.

**HTML**<br>
```html
<input type="text" class="error"></input>
<input type="password" class="password"></input>
<button>submit</button>
```

**JS**<br>
```javascript
//Get all inputs
var firstButton = document.querySelector('button');
var inError = document.querySelectorAll('input.error');

firstButton //single button node
inError //Node list of inputs with class 'error'

```

## NodeList vs Array
It seems like querySelectorAll should return an Array of elements. In fact, it returns a nodeList, which offers a similar, but not identical API to Array.

```javascript
var links = document.querySelectorAll('a');

//Works!
var linkCount = links.length;
var firstLink = links[0];

//Doesn't work!
links.forEach(function(link){
	//do something with link
});
```

Array methods like `forEach`, `map`, `reduce`, and so on, don't work. Luckily, its easy enough to convert a nodeList into an Array;

```javascript
var links = document.querySelectorAll('a');
var arrayOfLinks = Array.from(links);
```

## Traversing the DOM
You can use the children, parent, nextElmentSibling, and previousElementSibling attributes to find nodes relative to a node you have. This is called "traversing the DOM".

### Children
Use the children property to gets a **nodeList** of all the nodes contained in the node.

**HTML**<br>
```html
<ul>
  <li>Item 1</li>
	<li>Item 2</li>
</ul>
```

**JS**<br>
```javascript
var listItems = document.querySelector('ul').children;
listItems.length; //2
```

### Siblings and Parents
Use parent, nextElementSibling, and previousElementSibling to find nodes up the tree and across it.

**HTML**<br>
```html
<header>
	<ul>
		<li class="first">Item 1</li>
		<li class="selected">Item 2</li>
		<li class="last">Item 3</li>
	</ul>
</header>
<section>
	Hello!
</section>
```

**JS**<br>
```javascript

var selectedItem = document.querySelector('li.selected')
var first = selectedItem.previousElementSibling;
var last = selectedItem.nextElementSibling;
var list = selectedItem.parent;
var header = selectedItem.parent.parent;
var section = selectedItem.parent.parent.nextElementSibling;
```

## Exercise 1: Selecting Nodes
```html
<html>
	<body>
		<header>
			<ul>
				<li class="first">Item 1</li>
				<li class="selected">Item 2</li>
				<li class="last">Item 3</li>
			</ul>
		</header>
		<div class="col">
			<section>
				<h2>Section 1</h2>
			</section>
			<section class="current">
				<h2 class="highlight">Section 2</h2>
			</section>
			<section>
				<h1>Section 3</h1>
			</section>
		</div>
	</body>
</html>
```

1. Get the header element
2. Get all the section elements
3. Get the section element with class="current"
4. Get the section that comes after the current section
5. Get the h2 node from the section before the 'current' section
6. Get the div that contains the section that has an h2 with a class of 'highlight'
8. Get all the sections that contain an H2 (using a single statement);


## Exercise 1 Answer

```javascript
//Laziness is your friend...
var q = document.querySelector.bind(document),
	qa = document.querySelectorAll.bind(document);

q('header');
qa('section');
q('section.current');
q('section.current').nextElementSibling;
q('section.current').previousElementSibling.children[0];
q('h2.highlight').parent.parent;

Array.from(qa('section h2'))
	.map(function(headline){
		return headline.parent;
	})
```

## Editing a node
A **Node** object has some useful properties and methods to let you access its contents and edit its appearance and content.

### innerHTML
The sledgehammer approach. Get or set the html text inside a node. This is really simple and sufficient in most cases.

**HTML**<br>
```html
<div>
  <a href="#">Click me</a>
</div>
```

**JS**<br>
```javascript
//Get all inputs
var div = document.querySelector('div');
var a = document.querySelector('a');

a.innerHTML; //"click me"
div.innerHTML; //'<a href="#">Click me</a>'

a.innerHTML = "Updated link text";

```

### Attributes
Get and set attributes like object properties

```html
<a href="http://google.com" name="googleLink">Click me</a>
```

```javascript
var a = document.querySelector('a');

//Get an attribute
a.href; //"http://google.com"

//Set an attribute
a.name = 'new link name';

//Add a new attribute
a.target = "_blank";

```

### Removing nodes
Use `remove` to remove a node from a document.

**HTML**<br>
```html
<header>
	<ul>
		<li class="first">Item 1</li>
		<li class="selected">Item 2</li>
		<li class="last">Item 3</li>
	</ul>
</header>
<section>
	Hello!
</section>
```

**JS**<br>
```javascript
//Remove the first list item
document.querySelector('.first').remove();
```

### Adding nodes
Create a node using `document.createElement('tagname')` and `node.appendChild(el)`

**HTML**<br>
```html
<header>
	<ul>
		<li>Item 1</li>
	</ul>
</header>
```

**JS**<br>
```javascript
var newLI = document.createElement('li');
newLI.innerHTML = "Item 2";

var list = document.querySelector('ul');
list.appendChild(newLI); //Insert after item 1
```

## DOM Events
Elements emit events based on user input. You can run code in response to them. Events include

- **Mouse events** - click, mouseover, mouseout
- **Keyboard events** - keydown, keyup, etc
- **Form events** - submit, blur, focus, change,
- **window events** - load, hashchange, etc.
- **touch events** - touchstart, touchend, etc.

```javascript
var myElement = document.getElementById('myEl');
el.addEventListener('click', function(event){
	alert('clicked!');
})

//Combine with DOM editing
el.addEventListener('mouseover', function(event){
	el.innerHTML('over');
})
```

[http://www.w3schools.com/jsref/dom_obj_event.asp](http://www.w3schools.com/jsref/dom_obj_event.asp)

### Event object
You may be wondering what that event parameter is. An event object is passed to the event handler that describes what happened. The event object is different depending on the type of event.

- target - element where event occurred
- Mouse: clientX, clientY
- Keyboard: keyCode, shiftKey

### Event Bubbling
When an event is triggered on an element, it then gets fired on that element's parents, all the way to the top.

- event.target is the element where the event originally occurred
- event.currentTarget is the element running the event handler (this!).

```html
<div class="outer">
	<div class="inner">click me</div>
</div>
```

```javascript
document.querySelector('.outer').addEventListener('click', function(e){
	console.log(e.target, e.currentTarget);
	//inner, outer
})
```

##jQuery

- jQuery is a LIBRARY not a language
- jQuery is the #1 most used library on the web
- Same pattern as DOM API - select something, watch for an event, do something when it happens
- jQuery "wraps" DOM elements and makes them more convenient to work with.

### Selecting Elements
Just like querySelectorAll()

```javascript
var elements = $('#my .css .query');
```

You can further refine using a filter function.

```javascript
var elements = $('#my .css .query').filter(function(){
	return this.innerHTML === 'myElement';
});
```

### Attaching Event Handlers
Just like element.on('event', handler), except `this` refers to the element.

```javascript
$('#myButton').on('click', function(event){
	//Envoked when #myButton gets a click event
	this.innerHTML = "button clicked";
})
```

Alternately, you might see this syntax:

```javascript
$('#myButton').click(function(event){
	//Envoked when #myButton gets a click event
	this.innerHTML = "button clicked";
})
```

### Manipulating DOM
Just like innerHTML, appendChild etc, but more powerful.

```javascript
$('#myButton').on('click', function(event){
	//Update button html
	$(this).html("button clicked");

	//Create a new element
	var newEl = '<a>look at me!</a>';

	//Append new element
	$('#someOtherElement').append(newEl);

	//Add an attribute
	newEl.attr({'href' : 'http://techtalentsouth.com'})

	//Remove it
	newEl.remove();

})
```

### Document.ready
Most of your jQuery scripts should be loaded on the DOM's document.ready event. Pass a function into jQuery object to run it on DOM ready.

```javascript
	$(document).ready(function() {
		console.log('this runs second');
	});

	console.log('this runs first');
```

This makes sure the document exists before your code tries to manipulate it.


## jQuery Next Steps

### Updating Styles and Classes

```javascript
//Edit an inline style
$('#myButton').css({color: 'red'});

//Better...

//Add a CSS Class
$('#myButton').addClass('newClass');

//Remove a CSS Class
$('#myButton').removeClass('newClass');

```

### Chaining
All jQuery commands return a jQuery object, which means you can call multiple functions in a row. This is called "chaining"

```javascript
$( "#content" )
    .find( "h3" )
    .eq( 2 )
        .html( "new text for the third h3!" )
        .end() // Restores the selection to all h3s in #content
    .eq( 0 )
        .html( "new text for the first h3!" );
```


### DOM Traversal
You can search for elements up and down the DOM tree. There are many methods to search for parents, siblings, and children that you can read about. A few examples:

```javascript
<div class="grandparent">
    <div class="parent">
        <div class="child">
            <span class="subchild"></span>
        </div>
    </div>
    <div class="surrogateParent1"></div>
    <div class="surrogateParent2"></div>
</div>


// Parents - Selecting all the parents of an element that match a given selector:
// returns [ div.parent ]
$( "span.subchild" ).parents( "div.parent" );

// Siblings - Find the next or previous sibling
// returns [ div.surrogateParent1 ]
$( "div.parent" ).next();

// Children, Grandchildren - Search down the tree
// returns [ div.child, div.parent, div.surrogateParent1, div.surrogateParent2 ]
$( "div.grandparent" ).find( "div" );
```

### Animations
jQuery has convenience methods for basic animation. Most of the time, you should do animations using CSS, but jQuery animations are used quite often.

html

```html
<html>
  <head>
    <link id="" rel="stylesheet" href="main.css" media="screen" title="no title" charset="utf-8">
  </head>
  <body>
      <div id="box"></div>
      <script src="https://ajax.googleapis.com/ajax/libs/jquery/2.2.0/jquery.min.js"></script>
      <script src="main.js" charset="utf-8"></script>
  </body>
</html>
```

css

```
#box {
  background-color: aqua;
  width: 300px;
  height: 200px;
  margin: 200px auto;
}
```


```javascript
//Animate fade out
$('#box').fadeOut();

//Animate fadeIn
$('#box').fadeIn();

//Slide and remove element
$('#box').slideUp('slow');

// Custom effects with .animate()
$( "#box" ).animate(

	//properties to animate
	{opacity: 0.25},

    // Duration
    300,

    // Callback to invoke when the animation is finished
    function() {
        console.log( "done!" );
    }
);
```
