---
layout: post
title: Recursion or while loops (Test fire to know if you are a programmer or not)
comments: true
description: Recursion or while loops (Test fire to know if you are a programmer or not)
published: true
categories:
- csharp
tags:
- recursion
- loops
- csharp
- dotnet
---

A couple days ago I had an interesting test to apply a job which unfortunately I couldn't completely resolve, but for me it was fun because 
I realized that this was supposed to be a test to know if you are unfit to be a programmer or yes and apparently I'm not. The test had easy requirements so I decide to did it
again for myself and posted as a exercise and understand my logic.

So the first two requirements are these:

1) display the numbers between 1 to 100 that are divisible by 7
<br>
2) display the numbers that finish with the number 7

Well, for the first one using the operator mod it's easy to solve, actually I dind't had problems with this, so the code basically was a loop for 
between 1 to 100 and to get what numbers are divisible by 7, the operator mod gaves me the residue of the numbers divided by 7 and I only needed
to validate what numbers are, if the residue was 0 should be a number:

{% highlight csharp %} 
class Program
{
    static void Main(string[] args)
    {                   
        for (int i = 0; i <= 100; i++)
        {
            int t = i % 7; // mod to get the residue
            if (t == 0) // validate if the residue is equal to 0, should be a number
            {
                 Console.WriteLine(i);
            }
        }
    }
}
{% endhighlight %}

result 

{% highlight text %} 
0
7
14
21
28
35
42
49
56
63
70
77
84
91
98
{% endhighlight %}

not much to say right?, well for the second requirement the first thing that was coming to my mind it was validate the second number of the serie by 7,
but one of the problems with this that I started to think and the evaluator told me as well, is that the numbers with one digit would not display unless I did some stupid like this:

{% highlight csharp %} 
public static void Main()
{
    for (int i = 0; i <= 100; i++)
    {
        List<char[]> serial = new List<char[]>();

        var chars = i.ToString().ToCharArray();
        int t = i % 7;

        if (t == 0)
        {
           serial.Add(chars);
        }
   
        if (i >= 10)
        {          
            if (chars[1] == '7')
            {
               serial.Add(chars);
            }
        }
        else
        {
            if (chars[0] == '7')
            {
               serial.Add(chars);
            }
        }

        foreach (var n in serial.Distinct())
        {
            Console.WriteLine(n);
        }              
    }
}
{% endhighlight %}

so with this I got the result right? but the code is a fucking mess, actually I should be fired if I implement something like that, so instead of this mess 
I can just use the same mod operator and find the number that give me 7 as a residue to find those numbers, that number is 10, usually I use a pen and a paper 
to make a quick division and find the number, but I was on camera so didn't know if I can used this material so the number 10 was little tip that the evaluator told me.

So let's do it in correct way just only adding the mod operator with the conditional "or" in the if statement

{% highlight csharp %} 
public static void Main()
{
    for (int i = 0; i <= 100; i++)
    {
        int t = i % 7;
        int p = i % 10;

        if (t == 0 || p == 7)
        {
            Console.WriteLine(i);
        }                         
    }
}
{% endhighlight %}

result

{% highlight text %} 
0
7
14
17
21
27
28
35
37
42
47
49
56
57
63
67
70
77
84
87
91
97
98
{% endhighlight %}

so we got the same result with a clean code and of course the right way. So for the last requirement it was

3) Use a recursive method to remove the for loop

So this requirement basically it was the fire test to know my understanding of the vital concepts of programming and if I am a programmer or not. 
I usually don't use recursion unless my logic compels me to do it that in occasions is very very rare, so with this I realized that I'm not a programmer 
or the programmer that I should be.

So this is the code using recursion and displayed the same result as before. Now I don't remeber if this was the same code that I got in the evalution because 
I rewrote this code and took me more time that it's supposed.

{% highlight csharp %} 
public static void Main()
{
    serial(0);
}

public static void serial(int i)
{
    if (i <= 100) { 

      int t = i % 7;
      int j = i % 10;

      if (j == 7 || t == 0)
      {
        Console.WriteLine(i);               
      }
      i++;
      serial(i);
    }
}
{% endhighlight %}

Now, what problems I had with this code, basically the first if statement, it took me too much time to implement it and this entire exercise it's supposed to
be resolved in less than 10 minutes.

So I find this a good exercise and a good filter that prospective employer can use to looking for people who can program.

Please feel free to comment and give me your opinion.