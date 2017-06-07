---
layout: post
title: UserInput Test
comments: true
description: UserInput Test
published: true
categories:
- csharp
tags:
- UserInput
- csharp
---

This is the second test that I mentioned in my previous post, basically it's a method that receives any character and then it's needs to ignore non-numeric character by overrides that method and using inherits. 

The current description of the requires is the following: (Anyway I will put the link reference of the test problem at the end)

{% highlight text %} 
User interface contains two types of user input controls: TextInput, which 
accepts all characters and NumericInput, which accepts only digits.

Implement the class TextInput that contains:

* Public method void Add(char c) - adds the given character to the current value
* Public method string GetValue() - returns the current value

Implement the class NumericInput that:

* Inherits TextInput
* Overrides the Add method so that each non-numeric character is ignored

For example, the following code should output "10":

TextInput input = new NumericInput();
input.Add('1');
input.Add('a');
input.Add('0');
Console.WriteLine(input.GetValue());
{% endhighlight %}


{% highlight csharp %} 
public class TextInput
{
    public IList<char> list = new List<char>();

    public virtual void Add(char c)
    {
        list.Add(c);
    }

    public string GetValue()
    {
        string r = "";
        foreach (char l in list)
        {
            r = r + l;
        }
        return r;
    }
}

public class NumericInput : TextInput
{
    public override void Add(char c)
    {
        if (c < '0' || c > '9') { }
        else
            list.Add(c);
    }    
}

public class UserInput
{
    public static void Main(string[] args)
    {
        TextInput input = new NumericInput();
        input.Add('1');
        input.Add('a');
        input.Add('0');
        Console.WriteLine(input.GetValue());
    }
}
{% endhighlight %}

And as always, feel free to comment

Link Reference

**[https://www.testdome.com](https://www.testdome.com/questions/c-sharp/user-input/9904?testId=18&testDifficulty=Easy){:target="_blank"}**.