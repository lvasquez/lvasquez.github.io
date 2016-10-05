---
layout: post
title: C# Exercise 2
comments: true
description: Example of sum two Multidimensional Arrays 3x3
published: true
categories:
- csharp
tags:
- csharp
- dotnet
---

Example of sum two Multidimensional Arrays 3x3.

This example basically requires to sum two Multidimensional Arrays 3x3 using two methods, one for sum and the other for print. In this case I only use one method because I see no need to use the second method.

{% highlight csharp %} 
class Program
{
    static void Main(string[] args)
    {
        int[,] matriz1 = { { 0, 1, 1 }, { 3, 1, 5 }, { 1, 7, 1 } };
        int[,] matriz2 = { { 1, 1, 2 }, { 1, 4, 1 }, { 6, 1, 8 } };

        sum(matriz1, matriz2);   
    }

    static void sum(int[,] m1, int[,] m2)
    {
        for (int f = 0; f < 3; f++)
        {
            for (int c = 0; c < 3; c++)
            {
                int r1 = m1[f, c];
                int r2 = m2[f, c];

                int t = r1 + r2;

                Console.Write(t + " ");
            }
            Console.WriteLine();
        }
        Console.ReadKey();           
    }
}
{% endhighlight %}

result 

{% highlight text %} 
1 2 3
4 5 6
7 8 9
{% endhighlight %}

As always, comments and better solutions are always welcome =)