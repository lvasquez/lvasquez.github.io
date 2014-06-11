---
layout: post
title: POCO Entities
comments: true
description: Working with POCO Entities
published: true
---


According to the Microsoft MSDN POCO is an abbreviation for ***Plain Old CLR Objects***, that  is a simple public class that not inherit 
functionality and just contains properties and methods, this classes or methods do not implement persistent logic like saving or and 
retrieving data from the database and usually called Persistence Ignorant classes, which means that they are not 
aware of where and how to save data.

But the question is?, why I need to implement this? or what is the benefit? well, basically is for when we use repositories because, for example:
we can replace the data access technology from Entity Framework, with any other data access technology, also and one of my favorites and important, we need not change or rewrite the Business Layer 
and Presentation Layer. By doing this, we reduce the dependency between layers. 

But maybe someone said, it's not the same to use **DTOs** (Data Transfer Objetcs), that we can pass data between layers? well, the difference 
between DTOs and POCOs is that DTOs do not contain any methods only public properties.

So let's see one example of a POCO class

{% highlight csharp %}
public class Categories
{
	public virtual int CategoryID { get; set; }
	public virtual string CategoryName { get; set; }
	public virtual string Description { get; set; }
	public virtual byte[] Picture { get; set; }
	public virtual IColletion<Products> Products { get; set; }
}
{% endhighlight %}

As this is a bit extensive, i will write in parts, so in the next post i will give a example using POCOs with EF.

Here is a great References, one is from <a target="_blank" href="http://thedatafarm.com/blog/">Julie Lerman</a> that explains in steps how to Model and POCO Classes 

* <a target="_blank" href="http://msdn.microsoft.com/en-us/library/vstudio/dd456853(v=vs.100).aspx">http://msdn.microsoft.com/en-us/library/vstudio/dd456853(v=vs.100).aspx</a>
* <a target="_blank" href="http://www.vkinfotek.com/poco/poco-class-entity-framework.html">http://www.vkinfotek.com/poco/poco-class-entity-framework.html</a>
* <a target="_blank" href="http://thedatafarm.com/data-access/agile-entity-framework-4-repository-part-1-model-and-poco-classes/">http://thedatafarm.com/data-access/agile-entity-framework-4-repository-part-1-model-and-poco-classes/</a>









