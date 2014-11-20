---
layout: post
title: RavenDB, ASP MVC and Unity
comments: true
description: How to configure RavenDB with ASP MVC using Dependency Injection
categories:
- ASP MVC
- RavenDB
tags:
- ASP MVC 5
- Visual Studio 2013
- RavenDB
- Dependency Injection
- Unity
- DI
published: true
---

In my last post about RavenDB, it shows how to configure RavenDB with ASP MVC on embedded mode, creating a RavenController and ControllerFactory.
Now I will show how to configure with client/server mode, also in this time I will use DI and Unity as IoC to properly access RavenDB in a Business layer.

So basically my structure of the solution it this:

* **MvcRavenDBSample.Web** -> My MVC project
* **MvcRavenDBSample.Data** -> My Services and Repositories
* **MvcRavenDBSample.Domain** -> My Models

So we can start with our MvcRavenDBSample.Domain class project

### MvcRavenDBSample.Domain

In this project we should only put our models, in my case I created a folder call Models, 
but you can order your domain as you like, actually Uncle Bob is writing a book call "Clean Architecture" 
for how to create an properly architecture in one solution.

So this is my dummy model

{% highlight csharp %}
public class Customer
{
	public int Id { get; set; }
	public string Name { get; set; }
	public string Address { get; set; }
}
{% endhighlight %}

now we can pass to our MvcRavenDBSample.Data class project

### MvcRavenDBSample.Data

So in this project we should have the RavenDB Client. So we can start to install the RavenDB Client by nuget package. 

{% highlight text %}
Install-Package RavenDB.Client
{% endhighlight %}

Now we can create our Repositories Services

{% highlight csharp %}
 public interface ICustomerService
{
	IEnumerable<Customer> GetCustomers();
	Customer Create(Customer customer);
	Customer Read(int id);
	Customer Update(Customer customer);
	Customer Delete(int id);
}
{% endhighlight %}

{% highlight csharp %}
 public class CustomerServices : ICustomerService
{
	DocumentStore _store = null;

	public CustomerServices(string url)
	{
		_store = new DocumentStore() { Url = url };
		_store.Initialize();
	}
	
	public IEnumerable<Customer> GetCustomers()
	{
		using (var documentSession = _store.OpenSession())
		{
			var list = documentSession.Query<Customer>().ToList();
			return list;
		}
	}

	public Customer Create(Customer customer)
	{
		using (var documentSession = _store.OpenSession())
		{
			documentSession.Store(customer);
			documentSession.SaveChanges();
			return customer;
		}
	}

	public Customer Read(int id)
	{
		using (var documentSession = _store.OpenSession())
		{
			var customer = documentSession.Load<Customer>(id);
			return customer;
		}
	}

	public Customer Update(Customer customer)
	{
		using (var documentSession = _store.OpenSession())
		{
			Customer currentCustomer = documentSession.Load<Customer>(customer.Id);
			currentCustomer.Name = customer.Name;
			currentCustomer.Address = customer.Address;

			documentSession.Store(currentCustomer);
			documentSession.SaveChanges();
			return customer;
		}
	}

	public Customer Delete(int id)
	{
		using (var documentSession = _store.OpenSession())
		{
			var customer = documentSession.Load<Customer>(id);
			documentSession.Delete<Customer>(customer);
			documentSession.SaveChanges();
			return customer;
		}
	}
}
{% endhighlight %}

so as you can see I pass in the constructor the url to connect to RavenDB in client/server mode. So now
we can pass to our MVC project.

### MvcRavenDBSample.Web

So in my case I like to use Unity for IoC, but you can use Structure Map, Ninject, etc..

{% highlight text %}
Install-Package Unity.Mvc5
{% endhighlight %}

So in our container we add the Service and pass the url that connect with our RavenDB Server

{% highlight csharp %}
container.RegisterType<ICustomerService, CustomerServices>(new InjectionConstructor("http://localhost:8080/"));
{% endhighlight %}

and now we can create our CustomerController

{% highlight csharp %}
public class CustomerController : Controller
{
	readonly ICustomerService _customerService;

	public CustomerController(ICustomerService customerService)
	{
		_customerService = customerService;
	}
	// GET: Customer
	public ActionResult Index()
	{
		var list = this._customerService.GetCustomers().ToList();
		return View(list);
	}

	public ActionResult Details(int id)
	{
		var customer = this._customerService.Read(id);
		return View(customer);
	}

	public ActionResult Create()
	{
		return View("Create", new Customer());
	}

	[HttpPost]
	[ValidateAntiForgeryToken]
	public ActionResult Create(Customer customer)
	{
		if (ModelState.IsValid)
		{
			this._customerService.Create(customer);
			return RedirectToAction("Index");
		}

		return View(customer);
	}

	public ActionResult Edit(int id)
	{
		var customer = this._customerService.Read(id);

		if (customer == null)
			return HttpNotFound();
		return View(customer);
	}

	[HttpPost]
	[ValidateAntiForgeryToken]
	public ActionResult Edit(Customer customer)
	{
		if (ModelState.IsValid)
		{
			this._customerService.Update(customer);
			return RedirectToAction("Index");
		}
		return View(customer);
	}

	public ActionResult Delete(int id = 0)
	{
		var customer = this._customerService.Read(id);

		if (customer == null)
			return HttpNotFound();
		return View(customer);
	}

	[HttpPost, ActionName("Delete")]
	[ValidateAntiForgeryToken]
	public ActionResult DeleteConfirmed(int id)
	{
		this._customerService.Delete(id);
		return RedirectToAction("Index");
	}
}
{% endhighlight %}

As you can see our actions are really clean and easy to test. So finally we just need initialize the RavenDB Server, you can download from **[here!](http://ravendb.net/download)**.

Once you downloaded, you can start the Raven Service by executing /server/raven.server.exe, and then you can then visit
http://localhost:8080 for looking at the UI. By default is using the port :8080

Once the Raven service is up, compile and done. You can download the **source code** from **[here!](https://github.com/lvasquez/RavenDBAspMvc)**.

Also we can add a User and Password to access to the RavenDB that I will write in the next post, 
so hope this helps and I'm sorry for my terrible English =)




