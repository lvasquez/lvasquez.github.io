---
layout: post
title: WCF Serialize and DeSerialize XML Sample
comments: true
description: WCF Serialize and DeSerialize XML with generic function Sample
categories:
- WCF
- c#
tags:
- WCF
- Visual Studio 2013
- XML
- Serialize
- Deserialize
published: true
---

This is a little example how to Serialize Object to XML and Deserialize XML to Object using a simple generic function with c# on WCF Application. 

This code take it and change it to one of the references listed at the end and you can download the project from **[here!](https://github.com/lvasquez/WcfXmlSample)**.

So first we can create our Class

{% highlight csharp %}
[DataContract]
    public class Person : IExtensibleDataObject
    {
        [DataMember()]
        public string FirstName;
        [DataMember]
        public string LastName;
        [DataMember()]
        public int ID;
        public Person(string newfName, string newLName, int newID)
        {
            FirstName = newfName;
            LastName = newLName;
            ID = newID;
        }
        public Person() { }
        private ExtensionDataObject extensionData_Value;
        // this illustrates using the versioning ExtensionData property, which is not used
        public ExtensionDataObject ExtensionData
        {
            get
            {
                return extensionData_Value;
            }
            set
            {
                extensionData_Value = value;
            }
        }
    }
{% endhighlight %}

then our Generic Serialize and Deserialize functions

{% highlight csharp %}
 public class GenericDataContractSerializer<T>
    {
        public static string SerializeObject(T obj)
        {
            try
            {
                var xmlSerializer = new XmlSerializer(typeof(T));
                var stringBuilder = new StringBuilder();
                var stringWriter = new StringWriter(stringBuilder);
                xmlSerializer.Serialize(stringWriter, obj);
                return stringBuilder.ToString();
            }
            catch (Exception exception)
            {
                throw new Exception("Failed to serialize data contract object to xml string:", exception);
            }
        }
        /// <summary>
        /// DeserializeXml
        /// </summary>
        /// <param name="xml"></param>
        /// <returns></returns>
        public static T DeserializeXml(string xml)
        {
            try
            {
                var xmlSerializer = new XmlSerializer(typeof(T));
                return (T)xmlSerializer.Deserialize(new StringReader(xml));
            }
            catch (Exception exception)
            {
                throw new Exception("Failed to deserialize xml string to data contract object:", exception);
            }
        }
    }
{% endhighlight %}

Our interface Service

{% highlight csharp %}
  [ServiceContract]
    public interface IService1
    {
        [OperationContract]
        string Serialize();

        [OperationContract]
        Person DesSerialize(string xmlString);
    }
{% endhighlight %}

Our functions inheritance from Interface

{% highlight csharp %}
 public class Service1 : IService1
    {
        public string Serialize()
        {
            Person person = new Person("Santa", "Clause", 0929);
            string xmlString = GenericDataContractSerializer<Person>.SerializeObject(person);
            return xmlString;
        }

        public Person DesSerialize(string xmlString)
        {
            Person dPerson = GenericDataContractSerializer<Person>.DeserializeXml(xmlString);
            return dPerson;
        }
    }
{% endhighlight %}

So if you are working with EF you can use things like Automapper to mapping the new object to the Entity object and Insert or Update the data.

and here is some great references:

* <a target="_blank" href="http://www.nullskull.com/a/1580/wcf-generic-datacontract-object-serializer.aspx">http://www.nullskull.com/a/1580/wcf-generic-datacontract-object-serializer.aspx</a>

