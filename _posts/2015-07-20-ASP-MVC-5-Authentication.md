---
layout: post
title: Just simple Authentication with ASP MVC 5 and Owin Security
comments: true
description: Just simple Authentication with ASP MVC 5 using the new security features
published: true
categories:
- ASP MVC
- Owin
tags:
- Owin
- Authentication
- aspmvc
- Security
- csharp
---

Since **ASP MVC 5** came out, came with a new security features based on 
[OWIN](http://blogs.msdn.com/b/webdev/archive/2013/07/03/understanding-owin-forms-authentication-in-mvc-5.aspx){:target="_blank"} authentication middleware. So when you created a new project 
with ASP MVC from Visual Studio, you can choose what kind of authentication you want, 

* No Authentication
* Individual User Accounts
* Organizational Accounts
* Windows Authentication

that it's fine, but the problem is when we want to create a new project using the new Authentication of OWIN. Usually we need to create
a Empty or Basic project and choosing No Authentication, and from there, go installing from scratch each of the libraries for authentication
and creating and set our classes, because if we choose Individual User Account, Visual Studio will generate the
project template with all code source example that we don't need, you know, roles, manage users, entity framework, 
the all scheme based authentication that microsoft give us and it's fine to learning and not more.

So it will be nice if we would have one type of authentication more with the basic assemblies and maybe the set classes but unfortunately we don't.
So tired to create always a empty project and install the assemblies and set the classes every time that I needed to create a new project.
I decided to create a base and simple template with just the assemblies of owin and the classes already configured to start to work.
No database, Not Entity Framework, not all scheme based security of visual studio. Just only a AccountController with a action post like this:

{% highlight csharp %}
public ActionResult Login(LoginViewModel model)
{
    if (!ModelState.IsValid)
        return View();

    if (this._accountServices.getUsers().Any(a => a.userName == model.UserName && a.password == model.Password && a.status == true))
    {
        var identity = new ClaimsIdentity(new[] { new Claim(ClaimTypes.Name, model.UserName), }, DefaultAuthenticationTypes.ApplicationCookie);

        this._auth.SignIn(new AuthenticationProperties
        {
            IsPersistent = model.RememberMe
        }, identity);
		
        return RedirectToAction("Index", "Home");
    }
    else
    {
        ModelState.AddModelError("", "Invalid login attempt.");
        return View(model);
    }
}
{% endhighlight %}

a Login View and the initial classes of Owin and that's all. As I said before, No database, not scheme based roles, only the assemblies of
OWIN 

* Owin
* Microsoft.AspNet.Identity.Core
* Microsoft.AspNet.Identity.Owin
* Microsoft.Owin.Host.SystemWeb
* Microsoft.Owin.Security
* Microsoft.Owin.Security.Cookies
* Microsoft.Owin.Security.OAuth
* Microsoft.Owin.Host.SystemWeb

with the classes already configured.

You can download the source code from **[github](https://github.com/lvasquez/Simple-Authentication-ASP-MVC-5){:target="_blank"}** that 
included the unit tests or you can installed as visual studio extension.

<div class="row previews" align="center">
		<img class="img-responsive" alt="Free Bootstrap Admin Template - SB Admin" src="/images/authExtension.png">
</div>

To download the extension from the visual studio gallery click
**[here](https://visualstudiogallery.msdn.microsoft.com/1a637f69-b7a4-40d6-806f-7a6fba318653){:target="_blank"}** 
and download the VSIX file install extension

some screenshots

<div class="row previews" align="center">
		<img class="img-responsive" alt="Free Bootstrap Admin Template - SB Admin" src="/images/AuthImage.png">
</div>

<br>

<div class="row previews" align="center">
		<img class="img-responsive" alt="Free Bootstrap Admin Template - SB Admin" src="/images/AuthImage2.png">
</div>




