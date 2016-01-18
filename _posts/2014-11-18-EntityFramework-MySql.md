---
layout: post
title: Entity Framework 6 with MySql
comments: true
description: A little sample to configurate Entity Framework 6 with MySql without Edmx file
published: true
categories:
- .net
tags:
- EntityFramework
- MySql
- csharp
---

For the Entity Framework 6 support we need to have this requirements

* MySQL Connector/Net 6.8.x
* MySQL Server 5.1 or above
* Entity Framework 6 assemblies
* .NET Framework 4.0 or above

I will use the concept of DataBase First but without using the Visual Studio wizard to create the Edmx file, so instead of that,
I will do it manually. I will create a Console Application, so first install the Entity Framework via Nuget Package

{% highlight text %}
Install-Package EntityFramework
{% endhighlight %}

{% highlight text %}
Install-Package MySql.Data.Entity.EF6
{% endhighlight %}

Add the connection string in the app.confing

{% highlight xml %}
<connectionStrings>
    <add name="MonkeyFist" connectionString="server=localhost;user id=user;password=mypass;database=mydb" providerName="MySql.Data.MySqlClient" />
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
    <add name="MonkeyFist" connectionString="server=localhost;user id=root;password=mypass;database=mydb" providerName="MySql.Data.MySqlClient" />
  </connectionStrings>
  <startup>
    <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5" />
  </startup>
  <entityFramework>
    <defaultConnectionFactory type="System.Data.Entity.Infrastructure.SqlConnectionFactory, EntityFramework" />
    <providers>
      <provider invariantName="MySql.Data.MySqlClient" type="MySql.Data.MySqlClient.MySqlProviderServices, MySql.Data.Entity.EF6" />
      <provider invariantName="System.Data.SqlClient" type="System.Data.Entity.SqlServer.SqlProviderServices, EntityFramework.SqlServer" />
    </providers>
  </entityFramework>
<system.data>
    <DbProviderFactories>
      <remove invariant="MySql.Data.MySqlClient" />
      <add name="MySQL Data Provider" invariant="MySql.Data.MySqlClient" description=".Net Framework Data Provider for MySQL" type="MySql.Data.MySqlClient.MySqlClientFactory, MySql.Data, Version=6.8.3.0, Culture=neutral, PublicKeyToken=c5687fc88969c44d"/>
    </DbProviderFactories>
  </system.data>
  <runtime>
    <assemblyBinding xmlns="urn:schemas-microsoft-com:asm.v1">
      <dependentAssembly>
        <assemblyIdentity name="EntityFramework" publicKeyToken="b77a5c561934e089" culture="neutral" />
        <bindingRedirect oldVersion="0.0.0.0-6.0.0.0" newVersion="6.0.0.0" />
      </dependentAssembly>
    </assemblyBinding>
  </runtime>
</configuration>
{% endhighlight %}

the Entity

{% highlight csharp %}
[Table("customer")]
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
<img alt="EntityFrameworkMysql" src="/images/efmysql.png">
</center>

References:

* <a target="_blank" href="http://dev.mysql.com/doc/connector-net/en/connector-net-entityframework60.html">http://dev.mysql.com/doc/connector-net/en/connector-net-entityframework60.html</a>

