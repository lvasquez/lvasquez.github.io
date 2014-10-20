---
layout: post
title: sb-admin bootstrap template with ASP MVC and Owin Authentication
comments: true
description: sb-admin bootstrap template with ASP MVC and Owin Authentication
published: true
categories:
- ASP MVC
- Bootstrap
tags:
- bootstrap
- template
- aspmvc
- Owin
- Authentication
- csharp
---

In my last post, I took the sb-admin bootstrap template and adapted to a ASP MVC project, now I added a Login with some Users and 
Roles to access using the new security feature design for MVC 5, **[Microsoft Owin Authentication](http://blogs.msdn.com/b/webdev/archive/2013/07/03/understanding-owin-forms-authentication-in-mvc-5.aspx){:target="_blank"}**

So when we run the project by default we get a Login view and we can access with these users:

* User: **admin**, Pass = **12345**
* User: **invite**, Pass = **12345**
* User: **lvasquez** = **lvasquez**

<div class="row previews" align="center">
	<div class="thumbnail">
		<a class="post-image-link" href="/images/btemplate-login.png">
		<p>
		<img class="img-responsive" alt="Free Bootstrap Admin Template - SB Admin" src="/images/btemplate-login.png">
		</p>
		</a>
	</div>
</div>

For example if we login as a **admin** user, we will have access to the full navbar options

<div class="row previews" align="center">
	<div class="thumbnail">
		<a class="post-image-link" href="/images/btemplate-admin.png">
		<p>
		<img class="img-responsive" alt="Free Bootstrap Admin Template - SB Admin" src="/images/btemplate-admin.png">
		</p>
		</a>
	</div>
</div>

But if we login as a **invite** user, we will have access only to two options on the navbar

<div class="row previews" align="center">
	<div class="thumbnail">
		<a class="post-image-link" href="/images/btemplate-invite.png">
		<p>
		<img class="img-responsive" alt="Free Bootstrap Admin Template - SB Admin" src="/images/btemplate-invite.png">
		</p>
		</a>
	</div>
</div>

So here is the **[source code](https://github.com/lvasquez/sb-admin-bootstrap-template-asp-mvc-authentication){:target="_blank"}**.
and any comments or suggestions feel free to do it.

