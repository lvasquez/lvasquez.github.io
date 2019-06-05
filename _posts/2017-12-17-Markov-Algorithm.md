---
layout: post
title: Markov Algorithm
comments: true
description: Markov Algorithm Exercise challenge
published: true
categories:
- csharp
tags:
- Markov
- csharp
- Algorithm
---

A couple months ago I received a little recruitment exercise challenge based on Markov Algorithm that I started but never finished, so today with a lot of free time and basically without any thing to do I decided to take it again and finish it, but firstable, I'll publish the base algorithm first.

So this is my solution code to solve the algorith according of the rules and requirements of the algorithm that I will put in the follow [link](https://en.wikipedia.org/wiki/Markov_algorithm){:target="_blank"}


{% highlight csharp %} 
class Program
{
    static void Main(string[] args)
    {
        List<Rules> rules = new List<Rules>()
        {
            new Rules() { source = "|0", replacement = "0||" },
            new Rules() { source = "1", replacement = "0|" },
            new Rules() { source = "0", replacement = "" }
        };

        string text = "101";
        
        for (var i = 0; i < rules.Count(); i++)
        {
            if (text.Contains(rules[i].source) == true)
            {
                int containsPosition = text.IndexOf(rules[i].source);
                int sizeSource = rules[i].source.Length;
                text = ReplaceAt(text, containsPosition, rules[i].source.Length, rules[i].replacement);
                i = -1;
                Console.WriteLine(text);
            }             
        }         
    }

    static string ReplaceAt(string str, int index, int length, string replace)
    {
        return str.Remove(index, Math.Min(length, str.Length - index))
                .Insert(index, replace);
    }
}

public class Rules
{
    public string source { get; set; }

    public string replacement { get; set; }
}
{% endhighlight %}

Output

{% highlight text %} 
0|01
00||1
00||0|
00|0|||
000|||||
00|||||
0|||||
|||||
{% endhighlight %}


If something is wrong with the code or is there are a lot better solution of this, please, feel free to comment

