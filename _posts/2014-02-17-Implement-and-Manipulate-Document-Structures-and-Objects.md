---
layout: post
title: Implement and Manipulate Document Structures and Objects - HTML5 with JavaScript and CSS3 Part 1
comments: true
published: true
description: Create the document structure, Write code that interacts with UI controls, Apply styling to HTML elements programmatically, Implement HTML5 APIs, Establish the scope of objects and variables, Create and implement objects and methods 
---

## Create the document structure

This is a example of the HTML document structure

{% highlight html %}

<!DOCTYPE HTML>
<html>
<head> 
  <meta charset="utf-8" />
  <title>Title</title>
<!-- css references -->
</head>
<body> 
  <header><h1>Header of the page</h1></header>
  <nav>for some menu with items
	<div>
	  <a>Item 1</a>
      <a>Item 2</a>
	</div>
  </nav>
  <aside>tangentially content</aside>
  <article>
	<section><p>content section</p></section>
  </article>
  <footer></footer>
</body>
</html>

{% endhighlight %}

Here is the New Semantic Elements in HTML5 

* header
* nav
* section
* article
* aside
* figure
* figcaption
* footer
* details
* summary
* mark
* time

to replace things like this: 
{% highlight html %}
<div id="nav">, <div class="header">, <div id="footer">, 
{% endhighlight %}

also i found a great post about Semantic Markup and Page Layout

* <a target="_blank" href="http://blogs.msdn.com/b/jennifer/archive/2011/08/01/html5-part-1-semantic-markup-and-page-layout.aspx">http://blogs.msdn.com/b/jennifer/archive/2011/08/01/html5-part-1-semantic-markup-and-page-layout.aspx</a>

## Write code that interacts with UI controls

To Programmatically add and modify HTML elements right now we have a lot of libraries and frameworks like <a target="_blank" href="http://jquery.com/">jquery</a>, <a target="_blank" href="http://knockoutjs.com/">knockoutjs</a>, <a target="_blank" href="http://dojotoolkit.org/">dojo</a>, etc..
but here is a basic example how to do this using javascript at the DOM (Document Object Model), but wait, what's is the **DOM**? the W3C defines like

> The Document Object Model is a platform- and language-neutral interface that will allow programs and scripts to dynamically access and update the content, structure and style of documents.

We can said that, is a API that helps to give form to your HTML and you can access to manipulate to change your content. So here is the example:

{% highlight html %}
<p>Click the button to add some HTML element</p>
<div id="list"></div>
<button onclick="myFunction()">Add Element</button>
<script>
	function myFunction() {		
		var text = "<li>Item</li>";
		var list = document.getElementById("list");
		var item = document.createElement("div");
		item.innerHTML = text;
		var elements = item.childNodes;
		list.appendChild(item);
	}
</script>
{% endhighlight %}

and here is some link for more information:

* <a target="_blank" href="http://www.elated.com/articles/changing-page-elements-with-the-dom/">http://www.elated.com/articles/changing-page-elements-with-the-dom/</a>
* <a target="_blank" href="http://krasimirtsonev.com/blog/article/Convert-HTML-string-to-DOM-element">http://krasimirtsonev.com/blog/article/Convert-HTML-string-to-DOM-element</a>

About the intermedia controls the idea is to use new features to HTML5 to embed video and audio content with this elements:

* video
* audio

{% highlight html %}

<video src="video/What-is-HTML5.mp4" controls preload="none" width="640" height="360"></video>

{% endhighlight %}

<center>
<video src="{{ site.url }}/media/What-is-HTML5.mp4" controls preload="none" width="640" height="360" poster="{{ site.url }}/images/html5-blog.png"></video>
</center>

and with the audio

{% highlight html %}

<audio controls>
  <source src="audio/11_showcase_~_jaguar_xj220.mp3" type="audio/mp3">
</audio>

{% endhighlight %}

<center>
<audio controls>
  <source src="{{ site.url }}/media/11_showcase_~_jaguar_xj220.mp3" type="audio/mp3">
</audio>
</center>

and of course here is more references for more information:

* <a target="_blank" href="http://www.w3schools.com/tags/ref_av_dom.asp">http://www.w3schools.com/tags/ref_av_dom.asp</a>
* <a target="_blank" href="http://www.emmanuelndayiragije.com/comment-building-html5-media-video-audio-controls-with-javascript">http://www.emmanuelndayiragije.com/comment-building-html5-media-video-audio-controls-with-javascript</a>
* <a target="_blank" href="http://www.adobe.com/devnet/html5/articles/html5-multimedia-pt1.html">http://www.adobe.com/devnet/html5/articles/html5-multimedia-pt1.html</a>

Implement HTML5 canvas can be a little difficult, with canvas you can draw 3D and 2D graphics with coordinate system, animations, using JavaScript, also you can use equations, matrix multiplication, sin, cos, etc.. at this point is when will turn a little complex, but in this case i only show a easy example how to use this element.

{% highlight html %}
<table border="1">
 <tr>
   <td>Original Image</td>
   <td>With Canvas</td>
 </tr>
 <tr>
   <td><img id="scream" src="your_image.jpg"></td>
   <td><canvas id="myCanvas" width="300" height="300"></td>
 </tr>
</table> 
<script>
	var c = document.getElementById("myCanvas");
	var ctx = c.getContext("2d");
	var img = document.getElementById("scream");
	ctx.drawImage(img,0,0);
</script>
{% endhighlight %}

you get something like this
<center>
<img src="{{ site.url }}/images/html-canvas.png"/>
</center>

more information here:

* <a target="_blank" href="http://msdn.microsoft.com/en-us/library/gg589510%28v=vs.85%29.aspx">http://msdn.microsoft.com/en-us/library/gg589510%28v=vs.85%29.aspx</a>
* <a target="_blank" href="http://www.w3schools.com/tags/ref_canvas.asp">http://www.w3schools.com/tags/ref_canvas.asp</a>

Also we have SVG (Scalable Vector Graphics) to create 2D-graphics, SVG is a language  type diagrams like Pie charts, Two-dimensional graphs 
in an X,Y coordinate system, graphic application in XML, etc...
This is a simple example that i stole how to use SVG:

{% highlight html %}
<h2>HTML5 SVG Circle</h2>
<svg id="svgelem" height="200" xmlns="http://www.w3.org/2000/svg">
 <circle id="redcircle" cx="50" cy="50" r="50" fill="red" />
</svg>
{% endhighlight %}

and of course, I found another great link about SVG and Canvas in the next link: 

* <a target="_blanck" href="http://www.tutorialspoint.com/html5/html5_svg.htm">http://www.tutorialspoint.com/html5/html5_svg.htm</a>

## Apply styling to HTML elements programmatically

Here is a simple example how to change some element:

{% highlight html %}
<h1 id="header">Old Header</h1>
<script>
function change() {
   var element=document.getElementById("header");
   element.innerHTML="New Header";
}
</script>
<p>"Old Header" was changed to "New Header"</p>
<button onclick="change()">Change</button>
{% endhighlight %}

also we can change the position adding this two lines

{% highlight html %}
element.style.position="absolute";
element.style.top="100px";
{% endhighlight %}

so this should be looks like this

{% highlight html %}
<h1 id="header">Old Header</h1>
<script>
function change() {
  var element=document.getElementById("header");
  element.style.position="absolute";
  element.style.top="100px";
  element.innerHTML="New Header";
}
</script>

<p>"Old Header" was changed to "New Header"</p>
<button onclick="change()">Change</button>
{% endhighlight %}


## Implement HTML5 APIs

### Storage APIs

Two new features:

* local Storage
* session Storage

Here's is a example for local Storage:

{% highlight html %}
<script type="text/javascript">
  function clicked() {
	if( localStorage.hits ){
	   localStorage.hits = Number(localStorage.hits) + 1;
	}else{
	   localStorage.hits = 1;
	}
	document.getElementById("result").innerHTML="Total Clicked " + localStorage.hits + " time(s).";
  }
</script>
  <p>Click the button or refresh the page.</p>
  <p><button onclick="clicked()">Click me!</button>
  <div id="result"></div>
{% endhighlight %}

and you can delete the local Storage with this

{% highlight html %}
localStorage.clear();
{% endhighlight %}

and for the Session Storage 

{% highlight html %}
<script type="text/javascript">
  function clicked() {
    if( sessionStorage.hits ){
       sessionStorage.hits = Number(sessionStorage.hits) + 1;
    }else{
       sessionStorage.hits = 1;
    }
    document.getElementById("result").innerHTML="Total Clicked " + sessionStorage.hits + " time(s).";
  }
</script>
<p>Click the button or refresh the page.</p>
<p><button onclick="clicked()">Click me!</button>
<div id="result"></div>
{% endhighlight %}

but what's the difference? the sessionStore deleted when you close the browser because is designed for scenarios where the user is carrying out a single transaction

### AppCache API

So what is the AppCahe API? is that awesome application that helps when you don't have a internet connection and how it works? well, like this

First, you need to call an attribute called **manifest** in each page that we want to put in cache.
 
**manifest**: that is text file that contain the resources to cache own page

{% highlight html %}
<html manifest="demo.appcache"> 
  ...
</html>
{% endhighlight %}

also you cant put url's like this

{% highlight html %}
<html manifest="http://www.example.com/example.mf">
  ...
</html>
{% endhighlight %}

for more information:
 
* <a target="_blanck" href="http://www.html5rocks.com/es/tutorials/appcache/beginner/">http://www.html5rocks.com/es/tutorials/appcache/beginner/</a>

### Geolocation API

The geolocation is basically to get the geographical position of a user or ours, share the location with other web sites, etc.. Also you can integrate with Google and Bing Maps.

{% highlight html %}
<p id="demo">Click the button to get your coordinates:</p>
<button onclick="getLocation()">Try It</button>
<script>
	var x=document.getElementById("demo");
	function getLocation() {
		var geolocation = navigator.geolocation;
		geolocation.getCurrentPosition(showPosition);
	}
	function showPosition(position) {
		x.innerHTML="Latitude: " + position.coords.latitude + 
		"<br>Longitude: " + position.coords.longitude;	
	}
</script>
{% endhighlight %}

## Establish the scope of objects and variables

With javascript variables that you declare can be global or local. 

If you declare some variable outside of any function is global and variables create inside of any function are local variables.

{% highlight js %}
str = "This is a global string";
function MyFun()
{
   var str = "This is a local string";
   document.write(str);
}
MyFun();
document.write(str);
{% endhighlight %}

One of the important things and good practices is that you should avoid creating global objects unless they are absolutely necessary because
sometimes when we work with a third parties if we don't named that variables we may have risk of naming collision and there a different ways
to do this, so I stole this little example of a common way to do this

{% highlight js %}
var myApp = {}
 
myApp.id = 0;
 
myApp.next = function() {
    return myApp.id++;  
}
 
myApp.reset = function() {
    myApp.id = 0;   
}
 
window.console && console.log(
    myApp.next(),
    myApp.next(),
    myApp.reset(),
    myApp.next()
); //0, 1, undefined, 0 
{% endhighlight %}

the problem with this, is not maintainable because if we change the name of the variable we will have a bit of work with the rest but it works

I found this great posts for more depth ways to do this

* <a target="_blanck" href="http://www.zendesk.com/blog/keep-javascript-libraries-from-colliding">http://www.zendesk.com/blog/keep-javascript-libraries-from-colliding</a>
* <a target="_blanck" href="http://javascriptweblog.wordpress.com/2010/12/07/namespacing-in-javascript/">http://javascriptweblog.wordpress.com/2010/12/07/namespacing-in-javascript/</a>
* <a target="_blanck" href="http://addyosmani.com/blog/essential-js-namespacing/">http://addyosmani.com/blog/essential-js-namespacing/</a>

Now let's talk about scope variables, we can have the same name as a global variable and local variable, but it is entirely separate because with
the local variables ocreated and destroyed every time the function is executed, this is a little example of the msdn references:

{% highlight js %}
// Global definition of aCentaur.
var aCentaur = "</br>Global Variable: a horse with rider,";

// A local aCentaur variable is declared in this function.
function antiquities(){
  var aCentaur = "Local Variable: A centaur is probably a mounted Scythian warrior  ";
  return aCentaur;
}

antiquities();
aCentaur += " as seen from a distance by a naive innocent.";
document.write(antiquities());
document.write(aCentaur);
// Output: "Local Variable: A centaur is probably a mounted Scythian warrior"  
// Output: "Global Variable: a horse with rider, as seen from a distance by a naive innocent."
{% endhighlight %}

One of the important things that if we don't know maybe will cause a headache, is that JavaScript processes all variable declarations before executing any code,
so we need to be careful where and when declare variables

Now we have **this** that in short words is a key that has reference to the own function  where is invoked, so we can use
this on global context, function context, since a object, constructor, DOM events, etc... let's see one example in global context

{% highlight js %}
this.a = "Hellos World"
console.log(window.a); // Hello World
{% endhighlight %}

now let's see in an object 

{% highlight js %}
var myData = {
	name: 'Luis',
	lastName: 'Vasquez',
	completeName: function(){
		return this.name + ' ' + this.lastName;
	 }
}
console.log(myData.completeName()); // Luis Vasquez
{% endhighlight %}

note that we need to create a function in an object property because if we don't do it this, the key this, is reference to the global context

In some cases a method can be not used in functions that help them do their job because they do not have access to their properties, what i mean with this? let's see an example,

{% highlight js %}
var myApp = function(){
	var name = "Luis"
	var lastName = function(){
		console.log( this.name );
  };
  lastName(); // Invoke the function
};
myApp(); // (an empty string)
{% endhighlight %}

As you can see the function is not the property of an object, **this** point to the global context, maybe we might think that **this** should be works because should be point the container function, but not, for do it this we need to create a variable that we can use as cache, for example:

{% highlight js %}
var myData = function() {
	var that = this;
	var name = "Luis"
	var lastName = function() {
		console.log(this.name);
	};
	lastName();
};
myData(); // Luis
{% endhighlight %}

## Create and implement objects and methods

Javascript has three objects categories: Native Objects, Host Objects, and User-Defined Objects.

**Native objects** are those objects supplied by JavaScript. Examples of these are String, Number, Array, Image, Date, Math, etc.

**Host objects** are objects that are supplied to JavaScript by the browser environment. Examples of these are window, document, forms, etc.

And, **user-defined** objects are those that are defined by you, the programmer.

Examples:

{% highlight js %}
var myobj = new Object();
	myobj.name = "Luis Vasquez";
	myobj.age = 26;
	
var Image1 = new Image(50,100);    
	Image1.src = "myDog.gif";

var myNumber = new Number(2);    
	myNumber.doubleIt = new Function("return this*2");    
	

console.log(myobj); // Object { name="Luis Vasquez", age=26 }
console.log(Image1); // <img width="50" height="100" src="myDog.gif">
console.log(myNumber.doubleIt()); // displays 4    
{% endhighlight %}

References:

* <a target="_blanck" href="http://www.sitepoint.com/oriented-programming-1-4/">http://www.sitepoint.com/oriented-programming-1-4/</a>
* <a target="_blanck" href="http://www.tutorialspoint.com/javascript/javascript_objects.htm">http://www.tutorialspoint.com/javascript/javascript_objects.htm</a>

now let's see prototypes, prototypes are properties of the function objects. Here is a example:

{% highlight js %}
function foo(a, b){return a * b;}

var exec = foo(2,2);

console.log(foo.length);
console.log(foo.constructor);
console.log(typeof foo.prototype);
console.log(exec);

function Gadget(name, color) { 
  this.name = name; 
  this.color = color; 
  this.whatAreYou = function(){ 
   return 'I am a ' + this.color + ' ' + this.name; 
  }
}

Gadget.prototype = { 
  price: 100, 
  rating: 3, 
  getInfo: function() { 
   return 'Rating: ' + this.rating + ', price: ' + this.price; 
  }
};

var newtoy = new Gadget('webcam', 'black');
console.log(newtoy.name + " " + newtoy.color);
console.log(newtoy.whatAreYou());
console.log(newtoy.price + " " + newtoy.rating);
console.log(newtoy.getInfo());
{% endhighlight %}

References:

* <a target="_blanck" href="http://javascriptweblog.wordpress.com/2010/06/07/understanding-javascript-prototypes/">http://javascriptweblog.wordpress.com/2010/06/07/understanding-javascript-prototypes/</a>
* <a target="_blanck" href="http://www.packtpub.com/article/using-prototype-property-in-javascript">http://www.packtpub.com/article/using-prototype-property-in-javascript</a>

A method is a function too, although attached to an object, a method is just an object property that happens to be a function. like this:

{% highlight js %}
var foo = {
	bar = funtion() {
		console.log(this.number);
	}
	number = 7;
}
foo.bar();
{% endhighlight %}