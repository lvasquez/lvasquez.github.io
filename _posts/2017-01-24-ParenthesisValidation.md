---
layout: post
title: Parenthesis Math Validation
comments: true
description: Parenthesis Math Expression Validation
published: true
categories:
- java
tags:
- java
- math
- expression
- parenthesis
---

This is a little math expression validation with java, basically evaluated if a math expression is correct according of the parenthesis or not, I tried to used basic stuff validations to do this.

{% highlight java %} 
package parenthesisvalidation;

import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

/**
 *
 * @author Luis
 */
public class ParenthesisValidation {

    /**
     * @param args the command line arguments
     */
    public static void main(String[] args) {
        
        String expression = "(A * (C + D))";
        ExpressionValidation(expression); 
    }
    
    public static List<String> marklist = new ArrayList<>();
    
    public static boolean ExpressionValidation(String expression)
    {
        boolean flag = false;       
        char charArray[] = expression.toCharArray();
        
        for (char c : charArray)
        {
            if (c != ' ')
            {
                String ex = new String(new char[]{c});                    
                marklist.add(ex);

                if (")".equals(marklist.get(0)))
                    flag = false;

                int pOpen = Collections.frequency(marklist, "(");
                int pClose = Collections.frequency(marklist, ")");

                flag = pOpen >= 1 && pOpen == pClose;
            }
        }
        System.out.println(flag);       
        return flag;
    }
    
}
{% endhighlight %}

result 

{% highlight text %} 
true
{% endhighlight %}

Now if you change your math expression for example something like this

{% highlight java %} 
String expression = "A * (C + D))";
{% endhighlight %}

this will be return false because at the end are a parenthesis that never opened

{% highlight text %} 
false
{% endhighlight %}

hope it helps for something and feel free to comment