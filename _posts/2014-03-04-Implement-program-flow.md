---
layout: post
title: Implement program flow - HTML5 with JavaScript and CSS3 Part 2
comments: true
published: true
description: Implement program flow, Raise and handle an event, Implement exception handling, Implement a callback and Create a web worker process
---

## Implement program flow

### Collections and arrays

With javascript we can interate with arrays and collections, for example we can have a dropdown list with some elements like this:

{% highlight html %}
<select id="mySelect">
  <option>Apple</option>
  <option>Orange</option>
  <option>Pineapple</option>
  <option>Banana</option>
</select>
{% endhighlight %}

and we can interact with this elements with getting the id and assign to a variable, and the we can use a loop to read the data, like this:

{% highlight javascript %}
var x = document.getElementById("mySelect");
var txt = "";
for (var i = 0; i < x.length; i++) {
	txt = txt = x.options[i].text + "</br>";
}
document.getElementById("myparagraphId").innetHTML=txt;
{% endhighlight %}

Now let's see another example using the forEach method to enumerate collection members of this object

{% highlight javascript %}
var m = new Map();
m.set(1, "black");
m.set(2, "red");
m.set("colors", 2);
m.set({x:1}, 3);

m.forEach(function(item, index) {
    console.log(item, index);
});
//black 1
//red 2
//2 colors
//3 Object { x=1}
{% endhighlight %}

### Switch statements

Has another common language of third generation we have switch statements that we can use for different conditions, the 
w3schools give us a great example

{% highlight javascript %}
var day = new Date().getDay();
switch(day)
{
case 0:
	x="Today is Sunday";
	break;
case 1:
	x="Today is Monday";
	break;
case 2:
	x="Today is Tuesday";
	break;
case 3:
	x="Today is Wednesday";
	break;
case 4:
	x="Today is Thursday";
	break;
case 5:
	x="Today is Friday";
	break;
case 6:
	x="Today is Saturday";
	break;
}
console.log(x);
{% endhighlight %}

### Raise and handle an event

HTML DOM events allow Javascript to register different event handlers on elements in an HTML document.
Events normally works with functions, so if the function not be executed the event too.

Javascript provides a list of events that we can use in different scenarios since Mouse, Keyboard, Focus, Text, Transition, etc..
here is the complete list of events that we can use:

* <a target="_blank" href="http://msdn.microsoft.com/en-us/library/ie/ms533051(v=vs.85).aspx">http://msdn.microsoft.com/en-us/library/ie/ms533051(v=vs.85).aspx</a>

let's see the common used events:

**Onmouseover:**
The event occurs when the user moves the mouse pointer into the object, and it does not repeat unless the user moves the mouse pointer out of the object and then back into it.

**Onmouseout**
When the user moves the mouse over an object, one ***onmouseover*** event occurs, followed by one or more ***onmousemove*** events as the user moves the mouse pointer within the object. One ***onmouseout*** event occurs when the user moves the mouse pointer out of the object.

{% highlight html %}
<p onmouseover="this.style.color='red'" onmouseout="this.style.color='black'">
	Move the mouse pointer over this text to change its color. Move the pointer off the text 
    to change the color back.
</p>
{% endhighlight %}

**Onblur**
The onblur event fires on the original object before the onfocus or onclick event fires on the object that is receiving focus. Where applicable, the onblur event fires after the onchange event.

{% highlight html %}
<script>
function myFunction() {
	var x=document.getElementById("fname");
	x.value=x.value.toUpperCase();
}
</script>
Enter your name: <input type="text" id="fname" onblur="myFunction()">
<p>When you leave the input field, a function is triggered which transforms the input text to upper case.</p>
{% endhighlight %}

### Event Bubbling

Event bubbling is used from situations where a single event, such as a mouse click, may be handled by
two or more event handlers defined at different levels of the DOM hierarchy.

In bubbling the event is first captured and handled by the inner most element and then propagated to outer elements.
In capturing the event is first captured by the outer most element and propagated to the inner most element.

{% highlight html %}
<!DOCTYPE HTML>
<html>
<body>
<link type="text/css" rel="stylesheet" href="example.css">

<div class="d1" onclick="highlight(this)">1  
    <div class="d2" onclick="highlight(this)">2
        <div class="d3" onclick="highlight(this)">3 
        </div> 
    </div>
</div>

<script>
function highlight(elem) {
    elem.style.backgroundColor='yellow'
    alert(elem.className)
    elem.style.backgroundColor = ''
}
</script>

</body>
</html>
{% endhighlight %}

The Anonymous functions can be used to create temporary/private scope and it often makes sense to use anonymous functions calls in callbacks and event handlers.

## Implement exception handling

JavaScript has two basic levels of error handling: syntax errors and runtime errors. Syntax errors occur before the JavaScript code even runs, 
basically meaning that the code can't compile.

Here is a excellent reference about exception, errors, etc..

* <a target="_blank" href="http://www.htmlgoodies.com/primers/jsp/article.php/3610081/Javascript-Basics-Part-11.htm">http://www.htmlgoodies.com/primers/jsp/article.php/3610081/Javascript-Basics-Part-11.htm</a>

This is a typically example that **w3schools** give us

{% highlight html %}
<!DOCTYPE html>
<html>
<head>
<script>
var txt="";
function message()
{
try
  {
  adddlert("Welcome guest!");
  }
catch(err)
  {
  txt="There was an error on this page.\n\n";
  txt+="Error description: " + err.message + "\n\n";
  txt+="Click OK to continue.\n\n";
  alert(txt);
  }
}
</script>
</head>

<body>
<input type="button" value="View message" onclick="message()" />
</body>

</html>
{% endhighlight %}

if we can run this, the error must be said, adddlert is not define because "addlert" it doesn't mean anything,
i mean the compiler maybe said "WTF is that" lol.

Also we can marks a block of statements to try, and specifies a response, should an exception be throw.
For example:

{% highlight javascript %}
try {
   try_statements
}
[catch (exception_var_1 if condition_1) {
   catch_statements_1
}]
...
[catch (exception_var_2) {
   catch_statements_2
}]
[finally {
   finally_statements
}]
{% endhighlight %}

Also we can use Unconditional catch, for example when a single, unconditional ***catch*** is used, the ***catch***
block is entered when any exception is throw. For example:

{% highlight javascript %}
try {
   throw "myException"; // generates an exception
}
catch (e) {
   // statements to handle any exceptions
   logMyErrors(e); // pass exception object to error handler
}
{% endhighlight %}

In some cases, we can expecting in some function a variable to come with a undefined,
so we can check with a condition if the variable return undefined

{% highlight javascript %}
// To pass the undefined value
function myFunction(a,b) {
	if(typeof(a) === 'undefined') {
		// do something
	}
}

// To verify the value
function myFunction(a, b) {
	if(typeof(a)!='undefined') {
		// do something
	}
	else
		alert('Value Undefined');
}
{% endhighlight %}

In my opinion this is something that never should happen, because if this happens, maybe some of my code 
has not been tested well, or even worse, not tested anything.

try statements can also be followed by a finally keyword, which means 'no matter what happens, run this code after trying to run the code 
in the try block'. If a function has to clean something up, the cleanup code should usually be put into a finally block:

{% highlight javascript %}
function processThing(thing) {
  if(currentThing != null) 
	thrown "Oh" We are already processing something";
  
  currentThing = thing;
  try {
	// do complicated processing....
  }
  finally {
	currentThing = null;
  }
}
{% endhighlight %}

Another sample with the three blocks

{% highlight javascript %}
try {
	console.log("Outer try running...");
	try {
		console.log("Nested try running...");
		throw new Error(301, "an error");
	}
	catch(e) {
		console.log("Nested catch caught" + e.message);
		throw e;
	}
	finally {
		console.log("Nested finally is running...");
	}
}
catch(e) {
	console.log("Outer catch caught" + e.message);
}
finally {
	console.log("Outer finally running");
}
// Output:
// Outer try running...
// Nested try running...
// Nested catch caught error from nested try
// Nested finally is running...
// Outer catch caught error from nested try
// Outer finally running
{% endhighlight %}

References:

* <a target="_blank" href="http://msdn.microsoft.com/es-es/library/4yahc5d8(v=vs.94).aspx">http://msdn.microsoft.com/es-es/library/4yahc5d8(v=vs.94).aspx</a>
* <a target="_blank" href="http://www.javascriptkit.com/javatutors/trycatch.shtml">http://www.javascriptkit.com/javatutors/trycatch.shtml</a>
* <a target="_blank" href="http://eloquentjavascript.net/chapter5.html">http://eloquentjavascript.net/chapter5.html</a>
* <a target="_blank" href="http://www.htmlgoodies.com/primers/jsp/article.php/3610081/Javascript-Basics-Part-11.htm">http://www.htmlgoodies.com/primers/jsp/article.php/3610081/Javascript-Basics-Part-11.htm</a>

## Implement a callback

### HTML5 WebSocket API

To enable Web applications to maintain bidirectional communications with server-side processes, this specification introduces the WebSocket interface.

A WebSocket is a standard bidirectional TCP socket between the client and the server. The socket starts out as a HTTP connection and then "Upgrades" to a TCP socket after a HTTP handshake. After the handshake, either side can send data.
Example:

{% highlight html %}
<!DOCTYPE HTML>
<html>
<head>
<script type="text/javascript">
function WebSocketTest()
{
  if ("WebSocket" in window)
  {
     alert("WebSocket is supported by your Browser!");
     // Let us open a web socket
     var ws = new WebSocket("ws://localhost:9998/echo");
     ws.onopen = function()
     {
        // Web Socket is connected, send data using send()
        ws.send("Message to send");
        alert("Message is sent...");
     };
     ws.onmessage = function (evt) 
     { 
        var received_msg = evt.data;
        alert("Message is received...");
     };
     ws.onclose = function()
     { 
        // websocket is closed.
        alert("Connection is closed..."); 
     };
  }
  else
  {
     // The browser doesn't support WebSocket
     alert("WebSocket NOT supported by your Browser!");
  }
}
</script>
</head>
<body>
<div id="sse">
   <a href="javascript:WebSocketTest()">Run WebSocket</a>
</div>
</body>
</html>
{% endhighlight %}

References:

* <a target="_blank" href="http://www.tutorialspoint.com/html5/html5_websocket.htm">http://www.tutorialspoint.com/html5/html5_websocket.htm</a>
* <a target="_blank" href="http://www.w3.org/TR/2011/WD-websockets-20110419/">http://www.w3.org/TR/2011/WD-websockets-20110419/</a>

Now, what is a Callback? callback is a function derived from a programming paradigm called functional programming. The functional programming
is the use of function as arguments. So callback function is a function that is passed to another function as a parameter, and the callback
function, and the callback functions called(executed) inside otherFunction. For example:

{% highlight javascript %}
var names = ["Luca", "Luisa", "Luis", "Mario"];

names.forEach(function(eachName, index) {
	console.log(index + 1 ". " + eachName);
});
// 1. Luca, 2. Luis, 3. Luis, 4. Mario
{% endhighlight %}

Now, How Callback functions Work? functions are first-class objects, we can treat functions like objects, so we can pass functions like variables
and return them in functions and use then in another functions.

* <a target="_blank" href="http://cwbuecheler.com/web/tutorials/2013/javascript-callbacks/">http://cwbuecheler.com/web/tutorials/2013/javascript-callbacks/</a>
* <a target="_blank" href="http://javascriptissexy.com/understand-javascript-callback-functions-and-use-them/">http://javascriptissexy.com/understand-javascript-callback-functions-and-use-them/</a>

## Create a web worker process

One of the unfortunate things that javascript suffers, is that all its execution process remains inside a unique thread, so 
HTML5 introduces the new feature called Web Workers process, that help us to execute process independently of other scripts without affecting the performance of the page
or while other events still running, that's great because with this we can avoid that creepy window that said "A script on this page may be busy..." for some huge process
that never end and the browser be freezing.

As Web Workers will be executed on separated threads, you need to host their code into separated files from the main page.

So first we need to create new object Worker in your main page:

{% highlight javascript %}
var worker = new Worker('task.js');
{% endhighlight %}

If the file exists, the browser generate a new sub-process of worker that download in asynchronous way.
Now we can sets up a listener to handle message events from the worker, This event handler will be called when the worker calls its own ***Worker.postMessage()*** function

{% highlight javascript %}
var worker = new Worker('task.js');

worker.addEventListener('message', function(e) {
	console.log('Worker said: ', e.data);
}, false);

worker.postMessage('Hello World'); // Send data to our worker
{% endhighlight %}

and in the task.js

{% highlight javascript %}
self.addEventListener('message', function(e) {
	self.postMessage(e.data)
}, false);
{% endhighlight %}

Workers can use timeouts and intervals just like the main thread can.  This can be useful, for example, if you want to have your worker thread run code periodically instead of nonstop. 
For example we can use ***setTimeout()***, ***clearTimeout()***, ***setInterval()*** and ***clearInterval()***.

{% highlight javascript %}
function SendMessage() {
	worker.postMessage("My Message");
}
setInterval(SendMessage, "5000");
{% endhighlight %}

we can add an event listener to receive some message form the web worker

{% highlight javascript %}
worker.onmessage = function(event) {
	document.getElementById("result").innerHTML = event.data;
};
{% endhighlight %}

But there are some limitations with the Web Workers, because the Web Workers operate independently of the main browser UI thread, so they
are not able to access many of it's objects. Also we can't access the DOM, so they can't modify the HTML document. Some Objects, including
the window, document, and parent is restricted.

References:

* <a target="_blank" href="http://www.html5rocks.com/es/tutorials/workers/basics/#disqus_thread">http://www.html5rocks.com/es/tutorials/workers/basics/#disqus_thread</a>
* <a target="_blank" href="https://developer.mozilla.org/es/docs/Web/Guide/Performance/Usando_web_workers">https://developer.mozilla.org/es/docs/Web/Guide/Performance/Usando_web_workers</a>
* <a target="_blank" href="http://blogs.msdn.com/b/davrous/archive/2011/07/15/introduction-to-the-html5-web-workers-the-javascript-multithreading-approach.aspx">http://blogs.msdn.com/b/davrous/archive/2011/07/15/introduction-to-the-html5-web-workers-the-javascript-multithreading-approach.aspx</a>
* <a target="_blank" href="http://www.w3schools.com/html/html5_webworkers.asp">http://www.w3schools.com/html/html5_webworkers.asp</a>