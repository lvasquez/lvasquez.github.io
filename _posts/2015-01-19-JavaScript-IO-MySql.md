---
layout: post
title: JavaScript I/O, Express, MySql and KendoUI
comments: true
description: Example with JavaScript I/O, Express, MySql and KendoUI
published: true
categories:
- JavaScript I/O
tags:
- JavaScript I/O
- Express
- MySql
- KendoUI
---

Playing a little with **[Express](http://expressjs.com/){:target="_blank"}** and without nothing to do at my work, I created a little example of a simple **CRUD** using Express 
and Mysql, I'm using right now the new release of **[JavaScript I/O](https://iojs.org/){:target="_blank"}**, but if you are using nodeJs doesn't matter because the npm packages are the same.

If you saw my older posts maybe you noticed that I like to use **[KendoUI](http://demos.telerik.com/kendo-ui/){:target="_blank"}** components, and in this case it will be not the exception. I like 
the Kendo UI components because are really easy to use and I always like show that we can use this components with different technologies with the same functionality. 
If you saw one of my last posts, I used ASP WebApi with RavenDB in the server side but this time I will use Express and Mysql.

So basically these are the technologies used

Server Side:

* **JavaScript I/O**
* **Express**
* **MySql**

Client Side:

* **KendoUI**

the structure project

* **public**
    * **index.html**
* **routes**
    * **customers.js** 
* **server.js** 

server.js file

{% highlight javascript %}
var express = require('express'),
    path = require('path'),
    http = require('http');
    customer = require('./routes/customers');

var app = express();
var bodyParser = require('body-parser');
var multer = require('multer'); 

app.set('port', process.env.PORT || 3000);
app.use(express.static(path.join(__dirname, 'public')));

app.use(bodyParser.json());  // parse application/json
app.use(bodyParser.urlencoded({ extended: true })); // for parsing application/x-www-form-urlencoded
app.use(multer()); // for parsing multipart/form-data

// Http Methods
app.get('/customers', customer.findAll);
app.get('/customers/:id', customer.findById);
app.post('/customers', customer.addCustomer);
app.put('/customers/:id', customer.updateCustomer);
app.delete('/customers/:id', customer.deleteCustomer);

app.listen(app.get('port'), function() {
	console.log("Express server listening on port " + app.get('port'));
});
{% endhighlight %}

customers.js file

{% highlight javascript %}
var mysql      = require('mysql');
var connection = mysql.createConnection({
  host     : 'localhost',
  user     : 'myuser',
  password : 'mypass',
  database: 'mydb'
});

// GET
exports.findAll = function(req, res) {	
    connection.query('select Id, name, address, phone, email, status from customer', function(err, results) {
        if (err) throw err;
	  
        res.send(results);
    });
};

// GET/Id
exports.findById = function(req, res) {
    var id = req.params.id;
	var sql    = 'SELECT Id, name, address, phone, email, status FROM customer WHERE Id = ' + connection.escape(id);
	connection.query(sql, function(err, results) {
	  if (err) throw err;		  
		res.send(results);
	});
};

// POST
exports.addCustomer = function(req, res) {
    var customer = req.body;
    if (customer.status == 'true')
		customer.status = 1;
	else
		customer.status = 0;
		
    console.log('Adding customer: ' + JSON.stringify(customer)); 	
	connection.query('INSERT INTO customer SET ?', customer, function(err, result) {
	  if (err) throw err;

		console.log('Success: ' + JSON.stringify(result));
		res.send(customer);
	});
};

// PUT
exports.updateCustomer = function(req, res) {
    var id = req.params.id;
    var customer = req.body;
    delete customer._id;
    	
    if (customer.status == 'true')
		customer.status = 1;
	else
		customer.status = 0;

	console.log('Updating customer: ' + id);
	connection.query('UPDATE customer SET name = ?, address = ?, phone = ?, email = ?, status = ? WHERE Id = ?', [customer.name, customer.address, customer.phone, customer.email, customer.status, id],  function (err, result) {
		if (err) throw err;

		console.log('Updated: ' + JSON.stringify(result));
		res.send(customer);
	})
};

// DELETE
exports.deleteCustomer = function(req, res) {
    var id = req.params.id;
    console.log('Deleting customer: ' + id);
	connection.query('DELETE FROM customer WHERE Id = ' + connection.escape(id), function (err, result) {
		if (err) throw err;

		console.log('deleted ' + result.affectedRows + ' rows');
		res.send(req.body);
	})	
};
{% endhighlight %}

index.html file

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
                 url: "http://localhost:3000/customers/",
                 dataType: "json"
             },
             create: {
                 url: "http://localhost:3000/customers/",
                 dataType: "json",
                 type: "POST"
             },
             update: {
			    url : function (item) {
					return 'http://localhost:3000/customers/' + item.Id;
				 },
                 dataType: "json",
                 type: "PUT"
             },
             destroy: {
                 url : function (item) {
					return 'http://localhost:3000/customers/' + item.Id;
				 },
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
                    title: "Status",
					template: '<input type="checkbox" #=status ? "checked=checked" : "" # disabled="disabled" ></input>'
                },
                {
                    command: ["edit", "destroy"],
                    width: "200px"
                }
        ]
    });
</script>
{% endhighlight %}

<center>
<img alt="nlogConsole" src="/images/nodejs-mysql.png">
</center>

<center>
<img alt="nlogConsole" src="/images/nodejs-mysql2.png">
</center>