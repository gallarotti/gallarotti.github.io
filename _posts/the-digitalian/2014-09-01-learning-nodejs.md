---
layout: post
categories:
- digitalian
- node.js
comments: true
title: Learning Node.js
image:
  feature: fog.jpg
  credit: Francesco Gallarotti
  creditlink: http://www.gallarotti.net
---
### From the Kingdom on Nouns to the Kingdom of Verbs

Having learned Modula3 and Java in college, and having worked mostly on ASP.NET development at work, my mind is **hard-wired to think in terms of objects**. This is probably the main reason I have been finding hard to relate to a language like Javascript, which is much more functional-oriented than object-oriented in its nature. Some people try to shoe-horn the concept of function into the concept of class, but this way of thinking hasn't really helped me understand the power of this language at all, so far.

Recently, though, I have been working on some ideas for a side-project. While I already know that I want to use **Neo4J** for data storage, I have been trying to decide whether I wanted to proceed implementing the web application using an ASP.NET MVC 5 solution, or whether it made more sense to build a Single Page Application (SPA) using primarily Javascript, both on the front-end and on the server side, using **node.js** - a Javascript asynchronous event driven framework designed to build scalable network applications. Although I am not too familiar with MVC 5, certainly I am much less familiar with Javascript. Thus, at least looking at things from that perspective, I was leaning towards the first solution. On the other hand, though, while sketching the user interface that I envision for this application, I came to realize that a single page application would be a much more appropriate solution. And so I started reading more about node.js, Javascript and the gazillion libraries that are commonly employed to build applications in this wide ecosystem.

Today, I stumbled into [nodebeginner.org](http://www.nodebeginner.org/), a very nice book, written by [@manuelkiessling](https://twitter.com/manuelkiessling), whose first few pages are available online for free. While most node-related blog posts, tutorials and articles online are either too simple or too sophisticated to really learn anything from reading them, this is something that I **highly recommend reading**, if you are interested in *really understanding* what node.js is and where does the power of Javascript really come from, especially if, like me, you have your mind hard-wired into thinking in terms of objects.

His tutorial on node.js starts, like most other tutorials, from the exercise of building a minimalistic HTTP server. I am pretty sure that I have seen the following lines of code at least a hundred times by now:

```
var http = require("http");

http.createServer(function(request, response) {
  response.writeHead(200, {"Content-Type": "text/plain"});
  response.write("Hello World");
  response.end();
}).listen(8888);
```

If you save the above code in a file named `index.js` and you execute this code using **node** with the command `node index.js` you have the simplest form of HTTP server, which listens on a given port (8888) for incoming requests and, for every request, always responds with *"Hello World"*.

From this point on, though, I really feel that Manuel, unlike many others, pushes the envelope. On one hand he introduces one new concept after the other, both from a Javascript and from a node.js perspectives - such as *anonymous functions*, *node.js modules* and *event-driven asynchronous callbacks*, just to name a few. On the other hand, he keeps reorganizing and refactoring his code with the overall aim to keep the architecture of the application as clean and modular as possible.

Little by little, he takes the above code, and extracts it into its own `server.js` external module:

```
var http = require("http");
var url = require("url");

function start() {
  function onRequest(request, response) {
    var pathname = url.parse(request.url).pathname;
    console.log("Request for " + pathname + " received.");
    response.writeHead(200, {"Content-Type": "text/plain"});
    response.write("Hello World");
    response.end();
  }
  http.createServer(onRequest).listen(8888);
  console.log("Server has started.");
}

exports.start = start;
```

reducing the main `index.js` node application to:

```
var server = require("./server");
server.start();
```

This post is really not the place to discuss the steps Manual followed to get to this point - if you want you can read all about it [on his website](http://www.nodebeginner.org/#finding-a-place-for-our-server-module). The only reason I mentioned the code above, is because at this point, in his writing, Manuel introduces the concept of *routing* the incoming HTTP requests. And, while doing so, he was able to **really make me understand the difference between my object oriented way of thinking and the Javascript approach to doing things**.

Manuel introduces the routing mechanism building a very simple module `router.js`:

```
function route(pathname) {
  console.log("About to route a request for " + pathname);
}

exports.route = route;
```

Obviously, this module does essentially nothing. What is important, though, is how this gets wired into the main application. The HTTP server needs to know about this new module and how to use it.
First, we extend the server's `start()` function to enable us to pass the routing mechanism into the server:

```
var http = require("http");
var url = require("url");

function start(route) {
  function onRequest(request, response) {
    var pathname = url.parse(request.url).pathname;
    console.log("Request for " + pathname + " received.");

    route(pathname);

    response.writeHead(200, {"Content-Type": "text/plain"});
    response.write("Hello World");
    response.end();
  }

  http.createServer(onRequest).listen(8888);
  console.log("Server has started.");
}

exports.start = start;
```

Next, from the `index.js` we inject the `route` function into the server:

```
var server = require("./server");
var router = require("./router");

server.start(router.route);
```

As soon as I read this code, my immediate and instinctive reaction was that we could have simply passed the `router` "object" into the server, and the server could have called `router.route()` to route the request. And this is where Manuel opened my eyes.

> May I once again stray away for a while and talk about functional programming again?

> Passing functions is not only a technical consideration. With regard to software design, it's almost philosophical. Just think about it: **in our index file, we could have passed the router object into the server, and the server could have called this object's route function**.

> This way, we would have passed a **thing**, and the server would have used this thing to do something. **Hey, router thing, could you please route this for me?**

> But the server doesn't need the thing. **It only needs to get something done, and to get something done, you don't need things at all, you need actions. You don't need nouns, you need verbs.**

> **Understanding the fundamental mind-shift that's at the core of this idea is what made me really understand functional programming.**

> And I did understand it when reading Steve Yegge's masterpiece [Execution in the Kingdom of Nouns](http://steve-yegge.blogspot.it/2006/03/execution-in-kingdom-of-nouns.html). Go read it now, really. It's one of the best writings related to software I ever had the pleasure to encounter.* 




