---
layout: post
title: C# Exercise 3
comments: true
description: Example of validate chain character repeated based of a parameter
published: true
categories:
- csharp
tags:
- csharp
- dotnet
---

Example of validate chain character repeated based of a parameter

Recently I had a interview job and practical test with a couple exercise, unfortunately I counld't finish all the exercises in the time that I had, actually was only three exercise but I don't know if is because I don't code to much these days but I feel in due to finish this exercise for my own. I will post only the first two because the third one it was basically create a api with consuming a specifcs url that I don't remember.

The first exercise requested a function with two parameters, one is a chain o characters or string and the second parameter a int number. Based of the second parameter, you should validate how many characters of the chain should be repeat. For example:

{% highlight text %} 
input: 
param1 = "aaabbbcccddff"
param2 = 2
output:"aabbccddff"

input: 
param1 = "aaabbbcccddff"
param2 = 1
output:"abcdf"

input: 
param1 = "aaabbbcccddff"
param2 = 3
output:"aaabbbcccddff"
{% endhighlight %}

{% highlight csharp %} 
class Program
    class Program
    {
        static void Main(string[] args)
        {
            string chain = "aaabbbccccddff";
            int num = 2;

            Program ob = new Program();

            IList<char> myList = ob.myFunc(chain, num);

            foreach(var x in myList.ToList())
            {
                Console.Write(x.ToString());
            }   
        }

        public IList<char> myFunc(string chain, int num) {

            char a = ' ';
            char b = ' ';
            int tempCount = 1;

            IList<char> myList = new List<char>(); 
            
            foreach(char i in chain)
            { 
                char temp = i;           
                a = b;                 
                b = temp;       

                if (a != b)           
                    tempCount = 1;
                else
                    tempCount++;

                if (tempCount <= num) 
                    myList.Add(temp);
                        
            }
            return myList.ToList();
        }     
    }
{% endhighlight %}

result 

{% highlight text %} 
aabbccddff
{% endhighlight %}
