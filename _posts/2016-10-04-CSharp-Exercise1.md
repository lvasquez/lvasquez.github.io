---
layout: post
title: C# Exercise 1
comments: true
description: Example of the total sum of pair and impair numbers in an array
published: true
categories:
- csharp
tags:
- csharp
- dotnet
---

Example of the total sum of pair and impair numbers in an array.

This is one of a couple examples that one mate that start to programing ask me to help him to resolve. I'm not sure if he really wanted my help because he never said something like "when we can gather them", instead he send me by email the exercises hoping that I can resolve the exercises and send him the code without any explanation, I mean, for me that is not really helpful, by anyway I found this exercise fun and decide to resolve.

The first exercise basically requires to get the total sum of pair and impair numbers in an array.


{% highlight csharp %} 
class Program
{
    static void Main(string[] args)
    {                   
        int[] array = { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 };
        List<int> pairs = new List<int>();
        List<int> impairs = new List<int>();

        foreach (int i in array)
        {
            int r = i % 2;

            if (r == 0)
                pairs.Add(i);
            else
                impairs.Add(i);
        }

        System.Console.WriteLine("Total Sum Pairs: {0} ", pairs.Sum());
        System.Console.WriteLine("Total Sum Impairs: {0} ", impairs.Sum());
        System.Console.ReadLine();
    }
}
{% endhighlight %}

result 

{% highlight text %} 
Total Sum Pairs: 30
Total Sum Impairs: 25
{% endhighlight %}

If anyone have another and more clean solution, are welcome =)