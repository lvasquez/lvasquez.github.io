---
layout: post
title: ASP Web Api and RavenDB
comments: true
description: A little dummy example with ASP WebApi, RavenDB, Autofac, KendoUI and Moq
published: true
categories:
- dotnet
- ASP MVC
tags:
- csharp
- RavenDB
- WebApi
- ASP MVC
- Kendo UI
- Autofac
---

I worked in this little and maybe dummy example, because involve to many things for do it a simple thing, a simple CRUD but the idea
is to show how can we combine all this stuff, because when I started this example, my idea it was to create a simple CRUD using **[ASP WebApi](http://msdn.microsoft.com/es-es/library/hh833994(v=vs.108).aspx){:target="_blank"}** 
with **[Kendo UI](http://demos.telerik.com/kendo-ui/){:target="_blank"}** component, but then I asked me, why not use **[RavenDB](http://ravendb.net/){:target="_blank"}** for my store because I already had configured the server side, and then, why not applied something of dependency injection but
with this I needed to use a IoC, so use **[Autofac](http://autofac.org/){:target="_blank"}** but also I will needed something to testing my repositories, so I decided to use **[Moq](https://github.com/Moq/moq4/wiki/Quickstart){:target="_blank"}**. 

So at the end I finished with to many things for a dummy CRUD, but the cool thing is how I scaled little by little with TDD used since the
beginning. So here is the list of things that I used:

* **ASP Web Api**
* **RavenDB**
* **Autofac**
* **Moq**
* **KendoUI**

And this is my structure solution

* **WebAppNoSql.Domain** -> Models
* **WebAppNoSql.Repo** -> Repositories and RavenDB Client
* **WebAppNoSql.Web** -> ASP MVC and Web Api
* **WebAppNoSql.Tests** -> Unit Tests

I created a ASP MVC Empty project and added ASP Web Api, RavenDB Client and Moq via nuget packaged. So this is my Customer Model

{% highlight csharp %}
public class Customer
{
    public int Id { get; set; }
    public string name { get; set; }
    public string address { get; set; }
    public int phone { get; set; }
    public string email { get; set; }
    public bool status { get; set; }
}
{% endhighlight %}

my unit tests with Moq 
{% highlight csharp %}
[TestClass]
public class CustomerControllerTest
{
    private Mock<ICustomerRepository> _mockService; 
    private CustomerController _controller;

    [TestInitialize]
    public void TestInitialize()
    {
        _mockService = new Mock<ICustomerRepository>();
        _controller = new CustomerController(_mockService.Object);
    }

    [TestMethod]
    public void GetCustomers_Return_NotNull()
    {
        List<Customer> list = new List<Customer>
        {
            new Customer(){ Id = 1, name="Luis", address = "Content 1", phone = 57678979, email = "levb20@gmail.com", status = true },
            new Customer(){ Id = 2, name="Maria", address = "Content 2", phone = 2221123, email = "morx@gmail.com",  status = true }
        };
		
        _mockService.Setup(f => f.GetCustomers()).Returns(list.AsQueryable());

        var result = _controller.GetCustomers();

        // Assert
        Assert.IsNotNull(result);
        Assert.AreEqual(2, result.Count());
    }
  

    [TestMethod]
    public void GetCustomer_Returns_Customer()
    {
        Customer customer = new Customer() { Id = 1, name = "Luis Vasquez", address = "GT", phone = 2223143, email = "levb20@gmail.com", status = true };

        _mockService.Setup(m => m.Read(customer.Id)).Returns(customer);
        // Arrange
        _controller.Request = new HttpRequestMessage();
        _controller.Configuration = new HttpConfiguration();

        // Act
        var response = _controller.GetCustomer(customer.Id);

        // Assert
        Assert.IsTrue(response.TryGetContentValue<Customer>(out customer));
        Assert.AreEqual(1, customer.Id);
    }

    [TestMethod]
    public void GetCustomer_Returns_Customer_Null()
    {
        Customer customer = new Customer() { Id = 1, name = "Luis Vasquez", address = "GT", phone = 2223143, email = "levb20@gmail.com", status = true };
	 
        // Arrange
        _mockService.Setup(m => m.Read(customer.Id)).Returns<Customer>(null);
        _controller.Request = new HttpRequestMessage();
        _controller.Configuration = new HttpConfiguration();

        // Act
        var response = _controller.GetCustomer(customer.Id) as HttpResponseMessage;

        // Assert
        Assert.IsInstanceOfType(response, typeof(HttpResponseMessage));
        Assert.AreEqual(HttpStatusCode.NotFound, response.StatusCode);
    }

    [TestMethod]
    public void Post_Return_NotNull()
    {
        Customer customer = new Customer() { Id = 1, name = "Luis Vasquez", address = "GT", phone = 2223143, email = "levb20@gmail.com", status = true };

        _controller.Request = new HttpRequestMessage();
        _controller.Configuration = new HttpConfiguration();

        string locationUrl = "http://localhost:1466/api/Customer";

        var mockUrlHelper = new Mock<UrlHelper>();
        mockUrlHelper.Setup(x => x.Link(It.IsAny<string>(), It.IsAny<object>())).Returns(locationUrl);
        _controller.Url = mockUrlHelper.Object;

        // Act
        var response = _controller.Post(customer);

        // Assert
        Assert.AreEqual(locationUrl, response.Headers.Location.AbsoluteUri);
        _mockService.Verify(foo => foo.Create(customer), Times.Once);
        _mockService.VerifyAll();
    }

    [TestMethod]
    public void Post_Return_BadRequest()
    {
        Customer customer = new Customer() { Id = 1, name = "Luis Vasquez", address = "GT", phone = 2223143, email = "levb20@gmail.com", status = true };

        _controller.ModelState.AddModelError("Id", "error message");

        _controller.Request = new HttpRequestMessage();
        _controller.Configuration = new HttpConfiguration();

        // Act
        var response = _controller.Post(customer) as HttpResponseMessage;

        // Assert
        Assert.IsInstanceOfType(response, typeof(HttpResponseMessage));
        Assert.AreEqual(HttpStatusCode.BadRequest, response.StatusCode);
    }

    [TestMethod]
    public void Put_Return_Ok()
    {
        Customer customer = new Customer() { Id = 1, name = "Luis Vasquez", address = "GT", phone = 2223143, email = "levb20@gmail.com", status = true };

        _controller.Request = new HttpRequestMessage();
        _controller.Configuration = new HttpConfiguration();

        // Act
        var response = _controller.PutCustomer(customer) as HttpResponseMessage;

        // Assert
        Assert.IsInstanceOfType(response, typeof(HttpResponseMessage));
        Assert.AreEqual(HttpStatusCode.OK, response.StatusCode);
    }

    [TestMethod]
    public void Put_Return_BadRequest()
    {
        Customer customer = new Customer() { Id = 1, name = "Luis Vasquez", address = "GT", phone = 2223143, email = "levb20@gmail.com", status = true };     

        _controller.ModelState.AddModelError("Id", "error message");
        _controller.Request = new HttpRequestMessage();
        _controller.Configuration = new HttpConfiguration();

        // Act
        var response = _controller.PutCustomer(customer) as HttpResponseMessage;

        // Assert
        Assert.IsInstanceOfType(response, typeof(HttpResponseMessage));
        Assert.AreEqual(HttpStatusCode.BadRequest, response.StatusCode);
    }

    [TestMethod]
    public void Put_Return_NotFound()
    {    
        Customer customer = new Customer() { Id = 1, name = "Luis Vasquez", address = "GT", phone = 2223143, email = "levb20@gmail.com", status = true };

        _mockService.Setup(m => m.Update(customer)).Throws<Exception>();

        _controller.Request = new HttpRequestMessage();
        _controller.Configuration = new HttpConfiguration();

        // Act
        var response = _controller.PutCustomer(customer) as HttpResponseMessage;

        // Assert
        Assert.IsInstanceOfType(response, typeof(HttpResponseMessage));
        Assert.AreEqual(HttpStatusCode.NotFound, response.StatusCode);
    }

    [TestMethod]
    public void DeleteCustomer_Returns_Ok()
    {
        Customer customer = new Customer() { Id = 1, name = "Luis Vasquez", address = "GT", phone = 2223143, email = "levb20@gmail.com", status = true };
        // Arrange
        _mockService.Setup(m => m.Delete(customer)).Returns(customer);
        
        _controller.Request = new HttpRequestMessage();
        _controller.Configuration = new HttpConfiguration();

        // Act
        var response = _controller.DeleteCustomer(customer) as HttpResponseMessage;

        // Assert
        Assert.IsTrue(response.TryGetContentValue<Customer>(out customer));
        Assert.AreEqual(1, customer.Id);
        Assert.AreEqual(HttpStatusCode.OK, response.StatusCode);
    }

    [TestMethod]
    public void DeleteCustomer_Returns_NotFound()
    {
        // Arrange
        Customer customer = new Customer() { Id = 1, name = "Luis Vasquez", address = "GT", phone = 2223143, email = "levb20@gmail.com", status = true };

        _mockService.Setup(m => m.Delete(customer)).Throws<Exception>();
        
        _controller.Request = new HttpRequestMessage();
        _controller.Configuration = new HttpConfiguration();

        // Act
        var response = _controller.DeleteCustomer(customer) as HttpResponseMessage;

        // Assert
        Assert.AreEqual(HttpStatusCode.NotFound, response.StatusCode);
    }
}
{% endhighlight %}

my ICustomerRepository
{% highlight csharp %}
public interface ICustomerRepository
{
    IQueryable<Customer> GetCustomers();
    Customer Create(Customer customer);
    Customer Read(int id);
    Customer Update(Customer customer);
    Customer Delete(Customer customer);
}
{% endhighlight %}

my CustomerRepository with RavenDB store Initialize
{% highlight csharp %}
public class CustomerRepository : ICustomerRepository
{
    DocumentStore _store = null;

    public CustomerRepository(string url)
    {
        _store = new DocumentStore() { Url = url };
        _store.Initialize();
    }

    public IQueryable<Customer> GetCustomers()
    {
        using (var documentSession = _store.OpenSession())
        {
            var list = documentSession.Query<Customer>().AsQueryable();
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
            currentCustomer.name = customer.name;
            currentCustomer.address = customer.address;
            currentCustomer.phone = customer.phone;
            currentCustomer.email = customer.email;
            currentCustomer.status = customer.status;

            documentSession.Store(currentCustomer);
            documentSession.SaveChanges();
            return customer;
        }
    }

    public Customer Delete(Customer customer)
    {
        using (var documentSession = _store.OpenSession())
        {
            Customer currentCustomer = documentSession.Load<Customer>(customer.Id);
            documentSession.Delete<Customer>(currentCustomer);
            documentSession.SaveChanges();
            return customer;
        }
    }
}
{% endhighlight %}

my Autofac config
{% highlight csharp %}
public static void RegisterContainer()
{
    var builder = new ContainerBuilder();

    var config = GlobalConfiguration.Configuration;
	
    builder.RegisterApiControllers(Assembly.GetExecutingAssembly());
    builder.RegisterWebApiFilterProvider(config);
    builder.RegisterType<CustomerRepository>().As<ICustomerRepository>().WithParameter(new TypedParameter(typeof(string), "http://localhost:8081/ravendbserver/databases/northwind"));
    var container = builder.Build();
    config.DependencyResolver = new AutofacWebApiDependencyResolver(container);
}
{% endhighlight %}

my WebApi Controller
{% highlight csharp %}
public class CustomerController : ApiController
{
    readonly ICustomerRepository _customerRepository;

    public CustomerController(ICustomerRepository customerRepository)
    {
        _customerRepository = customerRepository;
    }
    // GET: api/Customer
    public IQueryable<Customer> GetCustomers()
    {
        return _customerRepository.GetCustomers();
    }

    // GET: api/Customer/5
    public HttpResponseMessage GetCustomer(int id)
    {
        Customer customer = this._customerRepository.Read(id);
        if (customer == null)
        {
            return Request.CreateResponse(HttpStatusCode.NotFound);
        }
        return Request.CreateResponse(customer);
    }

    // POST: api/Customer
    public HttpResponseMessage Post(Customer customer)
    {
        if (ModelState.IsValid)
        {
            this._customerRepository.Create(customer);

            HttpResponseMessage response = Request.CreateResponse(HttpStatusCode.Created, customer);
            response.Headers.Location = new Uri(Url.Link("DefaultApi", new { id = customer.Id }));
            return response;
        }
        else
        {
            return Request.CreateResponse(HttpStatusCode.BadRequest);
        }
    }

    // PUT: api/Customer/5
    public HttpResponseMessage PutCustomer(Customer customer)
    {
        return PutCustomer(customer.Id, customer);
    }

    public HttpResponseMessage PutCustomer(int id, Customer customer)
    {
        if (ModelState.IsValid && id == customer.Id)
        {
            try
            {
                this._customerRepository.Update(customer);
                return Request.CreateResponse(HttpStatusCode.OK, customer);
            }
            catch (Exception)
            {
                return Request.CreateResponse(HttpStatusCode.NotFound);
            }             
        }
        else
        {
            return Request.CreateResponse(HttpStatusCode.BadRequest);
        }
    }

    // DELETE: api/Customer/5
    public HttpResponseMessage DeleteCustomer(Customer customer)
    {
        try
        {
            this._customerRepository.Delete(customer);
            return Request.CreateResponse(HttpStatusCode.OK, customer);
        }
        catch (Exception)
        {
            return Request.CreateResponse(HttpStatusCode.NotFound);
        }        
    }
}
{% endhighlight %}

my view with KendoUi grid component
{% highlight html %}
<link rel="stylesheet" href="http://cdn.kendostatic.com/2014.1.318/styles/kendo.common.min.css" />
<link rel="stylesheet" href="http://cdn.kendostatic.com/2014.1.318/styles/kendo.bootstrap.min.css" />

<script src="http://cdn.kendostatic.com/2014.1.318/js/jquery.min.js"></script>
<script src="http://cdn.kendostatic.com/2014.1.318/js/kendo.all.min.js"></script>

<br />
<div id="grid"></div>

<script>
    var remoteDataSource = new kendo.data.DataSource({
        pageSize: 20,
         transport: {
             read: {
                 url: "http://localhost:1466/api/Customer/",
                 dataType: "json"
             },
             create: {
                 url: "http://localhost:1466/api/Customer/",
                 dataType: "json",
                 type: "POST"
             },
             update: {
                 url: "http://localhost:1466/api/Customer/",
                 dataType: "json",
                 type: "PUT"
             },
             destroy: {
                 url: "http://localhost:1466/api/Customer/",
                 dataType: "json",
                 type: "DELETE"
             }
         },
         schema: {
             model: {
                 id: "Id",
                 fields: {
                     Id: { editable: false, type: "number" },
                     name: { validation: { required: true} },
                     address: { validation: { required: true} },
                     phone: { validation: { required: true} },
                     email: { validation: { required: true} },
                     status: { type: "boolean" }
                 }
             }
         }
     });

    $('#grid').kendoGrid({
        dataSource: remoteDataSource,
        toolbar: [{name:"create", text: "Create Customer"}],
        editable: "popup",
        scrollable: true,
        sortable: true,
        filterable: true,
        pageable: {
            refresh: true,
            pageSizes: true,
            buttonCount: 5
        },
        columns: [
                {
                    field: "Id",
                    title: "Id"
                },
                {
                    field: "name",
                    title: "Name"
                },
                {
                    field: "address",
                    title: "Address"
                },
                {
                    field: "phone",
                    title: "Phone"
                },
                {
                    field: "email",
                    title: "E-mail"
                },
                {
                    field: "status",
                    title: "Status"
                },
                {
                    command: ["edit", "destroy"],
                    width: "200px"
                }
        ]
    });
</script>
{% endhighlight %}

Kendo UI Grid
<br />
<center>
<img alt="kendogridcomponent" src="/images/webapi-ravendb1.png">
</center>

<br />
<center>
<img alt="kendogrideditcomponent" src="/images/webapi-ravendb2.png">
</center>

RavenDB server web interface
<br />
<center>
<img alt="ravendbsite" src="/images/webapi-ravendb3.png">
</center>

<br />
Source code **[here](https://github.com/lvasquez/webapi-kendoui-ravendb){:target="_blank"}**.
