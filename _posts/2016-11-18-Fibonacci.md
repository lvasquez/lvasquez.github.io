---
layout: post
title: Fibonacci
comments: true
description: Understand Fibonacci 
published: true
categories:
- csharp
tags:
- csharp
- dotnet
- fibonacci
---

I found this really cool exercise that maybe all of us, in one time of our lives, we saw the Fibonacci sequence in mathematics and this will 
be a small explanation of how it works in code with a couple lines.

Well, basically the sequence consist that every number after the first two is the sum of the two preceding ones.

{% highlight text %} 
n = 0   1   2   3   4   5   6   7    8    9    10   11   12   13   14...
xn= 0   1   1   2   3   5   8   13   21   34   55   89   144  233  377...
{% endhighlight %}

So the xn are the sequence and we start with 0, so 0 + 1 = 1, then 1 + 1 = 2, then 2 + 1 = 3, then 3 + 2 = 5, etc.. etc.. etc... 
And how this works in code, well this is the souce code:

{% highlight csharp %} 
class Program
{
    static void Main(string[] args)
    {
        int a = 1;
        int b = 0;
        for (var i = 0; i <= 14; i++)
        {
            int temp = a;
            a = b;
            b = temp + b;

            Console.WriteLine(i + " - " + a);
        }
    }
}
{% endhighlight %}

As you can see it looks really simple and easy, but how it works?, well the key is on these 3 lines:

{% highlight csharp %}
int temp = a;
a = b;
b = temp + b; 
{% endhighlight %}

so if we can replace each value on each loop we can see how the sequence starts to begin, so let's start to replace values with the first loop
at the beginning we have "a" and "b" with a default value, in these case 1 and 0, so if we replace on the first loop

{% highlight text %}
int temp = 1;
a = 0;
b = 1 + 0; // (1) 
{% endhighlight %}

our result of the sequence should be "a", now if we replace the numbers of the second loop, we should have this

{% highlight text %}
int temp = 0 // because "a" takes the value of 0 in the last loop;
a = 1; // because b takes the last value of the last loop that now is 1
b = 0 + 1; // (1)  
{% endhighlight %}

the third loop

{% highlight text %}
int temp = 1 // because "a" takes the value of 1 in the last loop;
a = 1; // because b takes the last value of the last loop that now is 1
b = 1 + 1; // (2)  
{% endhighlight %}

the fourth loop

{% highlight text %}
int temp = 1 // last value loop
a = 2;
b = 1 + 2; // (3)  
{% endhighlight %}

fifth loop

{% highlight text %}
int temp = 2 // last value loop
a = 3;
b = 2 + 3; // (5)  
{% endhighlight %}

sixth loop

{% highlight text %}
int temp = 3 // last value loop
a = 5;
b = 3 + 5; // (8)  
{% endhighlight %}

etc.. etc.. etc..

output

{% highlight text %} 
0 - 0
1 - 1
2 - 1
3 - 2
4 - 3
5 - 5
6 - 8
7 - 13
8 - 21
9 - 34
10 - 55
11 - 89
12 - 144
13 - 233
14 - 377
{% endhighlight %}

So maybe it's a simple thing but I really like to see how some simple things work

