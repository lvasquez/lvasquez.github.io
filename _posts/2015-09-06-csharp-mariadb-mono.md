---
layout: post
title: C# and MySQL on Linux
comments: true
description: Example of using csharp and mariadb with mono on linux
published: true
categories:
- csharp
- archlinux
- mono
- mariadb
tags:
- csharp
- archlinux
- linux
- mariadb
- mono
- mysql
- dotnet
---

This is a quick example of using **C#** with **MariaDB** on linux. To do this, we need to have installed

* Mono
* MariaDB database
* MySql .NET Connector

So as we know, MariaDB is a fork of **MySql** by Michael Widenius and that's is other history, so if you have MySql will be the same. So first, we need the Mono provider for MySql database and that's it [MySQL Connector/Net](http://dev.mysql.com/downloads/connector/net/){:target="_blank"} for .NET & MONO platform.

 To installing MySql.Data.ll in the GAC 

{% highlight text %} 
cd path_to_your MySql.Data.dll assembly
gacutil -i MySql.Data.dll
{% endhighlight %}

My database

{% highlight sql %}
CREATE DATABASE dotnetdb;
{% endhighlight %}

My table with some records

{% highlight sql %}
DROP TABLE IF EXISTS `customers`;
CREATE TABLE `customers` (
  `Id` int(11) NOT NULL AUTO_INCREMENT,
  `name` varchar(255) DEFAULT NULL,
  `address` varchar(255) DEFAULT NULL,
  `phone` varchar(255) DEFAULT NULL,
  `email` varchar(255) DEFAULT NULL,
  `status` tinyint(4) DEFAULT NULL,
  PRIMARY KEY (`Id`)
) ENGINE=InnoDB AUTO_INCREMENT=3 DEFAULT CHARSET=utf8;

-- ----------------------------
-- Records of customer
-- ----------------------------
INSERT INTO `customers` VALUES ('1', 'Luis Vasquez', 'Guatemala City', '41508981', 'levb20@gmail.com', '1');
INSERT INTO `customers` VALUES ('2', 'Flor de Maria', 'Guatemala City', '39392202', 'fm@gmail.com', '0');
{% endhighlight %}

check the data

<div class="row previews" align="center">
        <img class="img-responsive" alt="csharp-mono2" src="/images/csharp-mysql-mono2.png">
</div>

My code

{% highlight csharp %}
using System;
using System.Linq;
using System.Data;
using System.Collections.Generic;
using MySql.Data.MySqlClient;

public class Test {
  public static void Main(string[] args) {
        
    try
    {       
        string connStr = "Server=localhost;User Id=root;Password=12345;Database=dotnetdb";
        IList<customer> list = new List<customer>();

        using (var connec = new MySqlConnection(connStr))
        {
            using (var comman = new MySqlCommand())
            {
                connec.Open();
                comman.Connection = connec;
                comman.CommandText = "select Id, name, address, phone, email, status from customers";
                comman.CommandTimeout = 0;
                MySqlDataReader reader = comman.ExecuteReader();

                while ((reader.Read()))
                {
                    customer ob = new customer();

                    ob.Id = Convert.ToInt32(reader["Id"]);
                    ob.name = Convert.ToString(reader["name"]);
                    ob.address = Convert.ToString(reader["address"]);
                    ob.phone = Convert.ToString(reader["phone"]);
                    ob.email = Convert.ToString(reader["email"]);
                    ob.status = Convert.ToBoolean(reader["status"]);
                    list.Add(ob);
                }
                connec.Close();
            }
        }

            foreach (var c in list)
            {
                Console.WriteLine(c.Id + " - " + c.name + " - " + c.address + " - " + c.phone + " - " + c.status);
            }

            Console.WriteLine("Success");
        }
        catch (Exception e)
        {
            Console.WriteLine("{0} Exception caught.", e);
        }            

    }
}

public class customer 
{
    public int Id { get; set; }
    public string name { get; set; }
    public string address { get; set; }
    public string phone { get; set; }
    public string email { get; set; }
    public bool status { get; set; }
}
{% endhighlight %}

now save the file, building the file, in my case

{% highlight text %} 
mcs test.cs -r:System.Data.dll -r:/home/luis/Documents/mysql-connector/v4.5/MySql.Data.dll 
{% endhighlight %}

and finally running:

{% highlight text %} 
mono test.exe
{% endhighlight %}


<div class="row previews" align="center">
		<img class="img-responsive" alt="csharp-mono" src="/images/csharp-mysql-mono.png">
</div>

By the way, I'm using Arch Linux =).


References:

* <a target="_blank" href="http://www.mono-project.com/docs/database-access/providers/mysql/">http://www.mono-project.com/docs/database-access/providers/mysql/</a>



