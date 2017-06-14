---
layout: post
title: Folders Test
comments: true
description: Folders Test Exercise
published: true
categories:
- csharp
tags:
- Folders
- csharp
- Exercise
---

This is a third exercise following the sequence of the other two with the current description:

* Implement a function FolderNames, which accepts a string containing an XML 
file that specifies folder structure and returns all folder names that start 
with startingLetter. The XML format is given in the example below. 

For example, for the letter 'u' and XML file:

{% highlight text %} 
<?xml version="1.0" encoding="UTF-8"?>
<folder name="c">
    <folder name="program files">
        <folder name="uninstall information" />
    </folder>
    <folder name="users" />
</folder>
{% endhighlight %}

the function should return "uninstall information" and "users" (in any order).

{% highlight csharp %} 
using System;
using System.Collections.Generic;
using System.Linq;
using System.Xml.Linq;

public class Folders
{
     public static IEnumerable<string> FolderNames(string xml, char startingLetter)
     {
         XDocument xmlDoc = XDocument.Parse(xml);

         var val = xmlDoc.Descendants("folder").ToList();

         var result = val.Where(p => p.FirstAttribute.Value.StartsWith(startingLetter.ToString())).Select(p => p.FirstAttribute.Value).ToList();

         return result;            
     }

    public static void Main(string[] args)
    {
        string xml =
            "<?xml version=\"1.0\" encoding=\"UTF-8\"?>" +
            "<folder name=\"c\">" +
                "<folder name=\"program files\">" +
                    "<folder name=\"uninstall information\" />" +
                "</folder>" +
                "<folder name=\"users\" />" +
            "</folder>";

        foreach (string name in Folders.FolderNames(xml, 'u'))
            Console.WriteLine(name);
    }
}
{% endhighlight %}

And as always, feel free to comment

Link Reference

**[https://www.testdome.com](https://www.testdome.com/questions/c-sharp/folders/7276?testId=18&testDifficulty=Hard){:target="_blank"}**.