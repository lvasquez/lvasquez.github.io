---
layout: post
title: Configure RavenDB with ASP MVC
comments: true
description: How to configure RavenDB with ASP MVC step by step using Visual Studio 2013 and ASP MVC 5
categories:
- ASP MVC
- RavenDB
tags:
- ASP MVC 5
- Visual Studio 2013
- RavenDB
published: true
---

This is a little example how to configure step by step RavenDB with ASP MVC in base of the first reference at the end of the post.

So how we know RavenDB is a NoSQL database base on Document database and RavenDB can run in one of two modes to connecting to data store:

* a client/server mode, where communication is made via HTTP;
* an embedded mode, in which the client API makes direct calls against the Database API.

In this case I will use the embedded mode, also I'm using Visual Studio 2013 and ASP MVC 5, **You need to Run Visual Studio as Administrator**.

So the first thing that we have to do is create a Empty ASP MVC project without Authentication

Then we need to install the RavenDB package using

{% highlight text %}
Install-Package RavenDB.Embedded -DependencyVersion Highest
{% endhighlight %}

This will be installed all that we need to start to work, so now we need to create a ControllerFactory,
by default ASP MVC uses DefaultControllerFactory class for creating controller after receiving request from Route Handler.
This diagrams shows the request processing:

![alt tag](http://lvasquez.github.io/images/requestprocessing.png)

So our ControllerFactory should be like this:

{% highlight csharp %}
 public class ControllerFactory : DefaultControllerFactory
    {
        private readonly Dictionary<string, Func<RequestContext, IController>> _controllerMap;

        public ControllerFactory(IDocumentStore documentStore)
        {
            if (documentStore == null)
            {
                throw new ArgumentNullException("documentStore");
            }
            this._controllerMap = new Dictionary<string, Func<RequestContext, IController>>();
            this._controllerMap["Home"] = context => new HomeController(documentStore);
        }

        public override IController CreateController(System.Web.Routing.RequestContext requestContext, string controllerName)
        {
            if (this._controllerMap.ContainsKey(controllerName))
            {
                return this._controllerMap[controllerName](requestContext);
            }
            else
            {
                return null;
            }
        }

        public override void ReleaseController(IController controller)
        {
            base.ReleaseController(controller);
        }
    }
{% endhighlight %}

so every new controller that we will create we need to set here, but why we need the ControllerFactory? In this case we need to pass the 
EmbeddableDocumentStore that's a class to inherits from DocumentStore that contains all methods to initialize the same.

So now we need to create a class to initialize our Storage File and pass this Storage File to our Controller Factory

{% highlight csharp %}
  private readonly IControllerFactory controllerFactory;

        public CompositionRoot()
        {
            this.controllerFactory = CompositionRoot.CreateControllerFactory();
        }

        public IControllerFactory ControllerFactory
        {
            get
            {
                return controllerFactory;
            }
        }

        private static IControllerFactory CreateControllerFactory()
        {
            var cacheRepository = new EmbeddableDocumentStore();
            cacheRepository.ConnectionStringName = "RavenDB";
#if DEBUG
            cacheRepository.UseEmbeddedHttpServer = true;
#endif
            cacheRepository.Initialize();
            var controllerFactory = new ControllerFactory(cacheRepository);
            return controllerFactory;
        }
{% endhighlight %}

now we need to initialize this class on our Project in the void Application_Start() in Global.asax like this

{% highlight csharp %}
   var root = new CompositionRoot();
   ControllerBuilder.Current.SetControllerFactory(root.ControllerFactory);
{% endhighlight %}

now we need to add the connection string into our web.config, so below of <configuration> add this

{% highlight text %}
  <connectionStrings>
    <add name="RavenDB" connectionString="DataDir=~\App_Data\RavenDB" />
  </connectionStrings>
{% endhighlight %}

with all this, a file called RavenDB will be generated automatically and this basically will be our storage, we can create our
RavenController like this:

{% highlight csharp %}
 public abstract class RavenController : Controller
    {
        public new IDocumentSession Session { get; set; }

        protected IDocumentStore _documentStore;

        public RavenController(IDocumentStore documentStore)
        {
            _documentStore = documentStore;
        }

        protected override void OnActionExecuting(ActionExecutingContext filterContext)
        {
            Session = _documentStore.OpenSession();
            base.OnActionExecuting(filterContext);
        }

        protected override void OnActionExecuted(ActionExecutedContext filterContext)
        {
            using (Session)
            {
                if (Session != null && filterContext.Exception == null)
                {
                    Session.SaveChanges();
                }
            }

            base.OnActionExecuted(filterContext);
        }
        protected HttpStatusCodeResult HttpNotModified()
        {
            return new HttpStatusCodeResult(304);
        }
{% endhighlight %}

so now we are ready to create our controller actions, but first, we can create our Model, for example:

{% highlight csharp %}
    public class Customer
    {
        public int Id { get; set; }
        public string Name { get; set; }
        public string Address { get; set; }
    }
{% endhighlight %}

now our controller need to inherit from RavenController to pass the IDocument Store, and our controller should be like this

{% highlight csharp %}
 public class HomeController : RavenController
    {
        public HomeController(IDocumentStore store)
            : base(store)
        { 
        }

        public ActionResult Index()
        {
		   return View(Session.Query<Customer>().ToList());
        }


        public ActionResult Create()
        {
            return View(new Customer());
        }


        [HttpPost]
        [ValidateAntiForgeryToken]
        public ActionResult Create(Customer customer)
        {
            if (ModelState.IsValid)
            {
                Session.Store(customer);
                return RedirectToAction("Index");
            }
            return View(customer);
        }

        public ActionResult Edit(int id)
        {
            var data = Session.Load<Customer>(id);
            return View(data);
        }

        [HttpPost]
        [ValidateAntiForgeryToken]
        public ActionResult Edit(Customer customer)
        {
            if (ModelState.IsValid)
            {
                Customer currentCustomer = Session.Load<Customer>(customer.Id);
                currentCustomer.Name = customer.Name;
                currentCustomer.Address = customer.Address;
                Session.Store(currentCustomer);
                return RedirectToAction("Index");
            }
            return View(customer);
        }

        public ActionResult Details(int id)
        {
            Customer customer = Session.Load<Customer>(id);
            return View(customer);
        }

        public ActionResult Delete(int id)
        {
            Customer register = Session.Load<Customer>(id);
            return View(register);
        }


        [ValidateAntiForgeryToken]
        [HttpPost, ActionName("Delete")]
        public ActionResult DeleteConfirmed(int id)
        {
            Session.Delete<Customer>(Session.Load<Customer>(id));
            return RedirectToAction("Index");
        }

        protected override void Dispose(bool disposing)
        {
            base.Dispose(disposing);
        }
{% endhighlight %}

so finally we just need to create our views in base to our Customer Model, also you can download the sourcecode **[here!](https://onedrive.live.com/?cid=ab0e3e430bc079dc&id=AB0E3E430BC079DC%2113333)**.

![alt tag](http://lvasquez.github.io/images/RavenIndex.png)

and here is some great references:

* <a target="_blank" href="http://www.dotnetcurry.com/showarticle.aspx?ID=787">http://www.dotnetcurry.com/showarticle.aspx?ID=787</a>
* <a target="_blank" href="http://www.youtube.com/watch?v=qI_g07C_Q5I">http://www.youtube.com/watch?v=qI_g07C_Q5I</a>
* <a target="_blank" href="http://ravendb.net/docs/2.5/intro/what-is-nosql">http://ravendb.net/docs/2.5/intro/what-is-nosql</a>
* <a target="_blank" href="http://www.codeproject.com/Tips/732449/Understanding-and-Extending-Controller-Factory-i">http://www.codeproject.com/Tips/732449/Understanding-and-Extending-Controller-Factory-i</a>
