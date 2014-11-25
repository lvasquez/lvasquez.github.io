---
layout: post
title: Entity Framework 6 with PostgreSQL
comments: true
description: Entity Framework 6 and PostgreSql example without generate the Edmx file
published: true
categories:
- .net
tags:
- EntityFramework
- Postgresql
- csharp
---


Pretty much like the last post with mysql I will use the concept of DataBase First but without using the Visual Studio 
wizard to create the Edmx file, so instead of that, I will do it manually again. I will create a Console Application again, 
so first install the next packages

{% highlight text %}
Install-Package EntityFramework
{% endhighlight %}

{% highlight text %}
Install-Package Npgsql
{% endhighlight %}

{% highlight text %}
Install-Package Npgsql.EntityFramework
{% endhighlight %}

Be sure that you have already installed the npgsql provider or check this **[link](https://github.com/npgsql/Npgsql/wiki/Visual-Studio-Design-Time-Support---DDEX-Provider){:target="_blank"}**

Add the connection string in the app.confing

{% highlight xml %}
 <connectionStrings>
    <add name="MonkeyFist" connectionString="server=localhost;user id=myuser;password=mypass;database=mydatabase" providerName="Npgsql" />
  </connectionStrings>
{% endhighlight %}

the app.confing should be like this

{% highlight xml %}
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <configSections>
    <!-- For more information on Entity Framework configuration, visit http://go.microsoft.com/fwlink/?LinkID=237468 -->
    <section name="entityFramework" type="System.Data.Entity.Internal.ConfigFile.EntityFrameworkSection, EntityFramework, Version=6.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" requirePermission="false" />
  </configSections>
  <connectionStrings>
    <add name="MonkeyFist" connectionString="server=localhost;user id=myuser;password=mypass;database=mydatabase" providerName="Npgsql" />
  </connectionStrings>
  <entityFramework>
    <defaultConnectionFactory type="System.Data.Entity.Infrastructure.SqlConnectionFactory, EntityFramework" />
    <providers>
      <provider invariantName="Npgsql" type="Npgsql.NpgsqlServices, Npgsql.EntityFramework" />
    </providers>
  </entityFramework>
</configuration>
{% endhighlight %}

the Entity

{% highlight csharp %}
 [Table("customer", Schema = "public")]
public class Customer
{
   [Key]
   [Column("id_customer")]
   public int id { get; set; }

   public string customer { get; set; }

   public string nit { get; set; }

   public string address { get; set; }
}
{% endhighlight %}

the Context

{% highlight csharp %}
public partial class db_Entities : DbContext
{
   public db_Entities() : base(nameOrConnectionString: "MonkeyFist") { }

   public DbSet<Customer> Customer { get; set; }
}
{% endhighlight %}


{% highlight csharp %}
class Program
{
  static void Main(string[] args)
  {
    using (var context = new db_Entities())
    {
      var customers = context.Customer.ToList();
      foreach (var cust in customers)
      {
        Console.WriteLine(cust.id + " " + cust.customer + " " + cust.address);
      }

    }
  }
}
{% endhighlight %}

the result

<center>
<img alt="EntityFrameworkPostgresql" src="/images/efpostgres.png">
</center>

