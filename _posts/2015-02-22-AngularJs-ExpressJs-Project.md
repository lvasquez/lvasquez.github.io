---
layout: post
title: AngularJS and ExpressJS Project
comments: true
description: AngularJS and ExpressJS Project with Mysql
published: true
categories:
- AngularJS
- ExpressJS
- NodeJS
tags:
- AngularJS
- ExpressJS
- NodeJS
- Mysql
- Karma
- Jasmine
- KendoUI

---

Hey there, in my free times, I recently created a little webapp using **[AngularJS](https://angularjs.org/){:target="_blank"}** with **[ExpressJS](http://expressjs.com/){:target="_blank"}**, my idea is create a little
template to start with AngularJS and ExpressJS, right now the site contains a login authentication, a dynamic navbar and a CRUD using
**[KendoUI](http://demos.telerik.com/kendo-ui/){:target="_blank"}** Grid component.

So for the login, I used a little database on mysql to persist the user using the npm-mysql of node, I used ExpressJS to used 
the HTTP methods and basically this is the complete list of things that I used:

### Front-End

- AngularJS
- Bootstrap
- KendoUI

### Back-End

- NodeJS
- ExpressJS
- Mysql

Some screenshots:

<center>
<img alt="angular1" src="/images/angular1.png">
<img alt="angular2" src="/images/angular2.png">
<img alt="angular3" src="/images/angular3.png">
<img alt="angular4" src="/images/angular4.png">
</center>

<br>

I based the login authentication and the dynamic navbar at the link references at the end of the post.
So I would like to receive some feedback or comments or even a pull request to improve the code, in my last commit I added 
**[Karma](http://karma-runner.github.io/0.12/index.html){:target="_blank"}** to start the unit tests. If I have some thing of free time
I will finish the user account administrator this week =)

The repo of the project it's here **[https://github.com/lvasquez/AngularDemo ](https://github.com/lvasquez/AngularDemo ){:target="_blank"}**

References:

* <a target="_blank" href="http://jasonwatmore.com/post/2014/05/26/AngularJS-Basic-HTTP-Authentication-Example.aspx">http://jasonwatmore.com/post/2014/05/26/AngularJS-Basic-HTTP-Authentication-Example.aspx</a>
* <a target="_blank" href="https://github.com/sirajc/dynamic-menu">https://github.com/sirajc/dynamic-menu</a>


