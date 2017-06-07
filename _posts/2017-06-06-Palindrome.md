---
layout: post
title: Palindrome
comments: true
description: Palindrome Test
published: true
categories:
- csharp
tags:
- Palindrome
- csharp
---

It's been a long long time that I don't touch any code so I decided to make a little warm-up doing some exercises and I found this page with a couple stuff and I made the first two tests that I found and these two are supposed to be the easy ones. Also in page you can runtime the sourcecode and the web page give you a score at base of some tests evaluations that they do in the background. I will share the link at the end.

The first test it's a Palindrome, well, I didn't know what that word means, but it's a word that reads the same backward or forward, so I think that this is really cool test exercise to start.

{% highlight csharp %} 
class Palindrome
{
    public static bool IsPalindrome(string word)
    {
        string forward = "";
        string backward = "";
        bool r = false;

        foreach (char a in word.ToLower())
        {
            forward = forward + a.ToString();
            backward = a.ToString() + backward;

            if (forward == backward)
                r = true;
            else
                r = false;
        }
        return r;          
    }

    public static void Main(string[] args)
    {
        Console.WriteLine(Palindrome.IsPalindrome("Deleveled"));
    }
}
{% endhighlight %}


This is the project on visual studio, I changed the code a little just to pass my unit tests but basically it's the same code **[Source code](https://github.com/lvasquez/Palindrome){:target="_blank"}**.

And as always, feel free to comment

Web Page

**[https://www.testdome.com](https://www.testdome.com/tests/c-sharp-online-test/18){:target="_blank"}**.