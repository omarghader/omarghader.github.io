---
layout: post
title: Executing R From Java Using JRI Library
modified:
categories:
comments: true 
excerpt: JRI tutorial 
tags: [R,JAVA,JRI,jri tutorial,machine learning]
image:
  feature:
date: 2015-09-03T13:03:09+02:00
---

R is a very powerfull language designed for mathematics and statistics. It became one of the best data analysis tools due to its performance and its syntax simplicity. R can import dataset or generate a csv file with 1 line of code.

So, It will very useful to useful to use R with one of the most known programming languages : JAVA. After using JRI library, i will show you how to assign variables, run R scripts.... in Java.

###Initializing R engine 
{% highlight java %}
String[] arg = new String[]{"--vanilla"};
Rengine rengine= new Rengine(arg, false, null);
{% endhighlight %}

###Double variable
**eval("command")** method  will execute the command in R engine.
{% highlight java %}
rengine.eval("x=13.5");
System.out.println(rengine.eval("x").asDouble());
// output
// 13.5
{% endhighlight %}

### DataFrame
REXP is the default class for the result object. If you want to convert the result to Double, Int, Boolean.. you must use REXP methods like: .asDouble(),.asInt(),asBool() ...
{% highlight java %}
rengine.eval("myData=data.frame(x=c('v','d'),y=c('e','f'))");
REXP dataFrame=rengine.eval("myData");
System.out.println(dataFrame); //Vector containing 2 Factors
System.out.println(dataFrame.asVector().at(1));
System.out.println(dataFrame.asVector().at(1).asFactor().at(1));

// output
// [VECTOR ([FACTOR {levels=("d","v"),ids=(1,0)}], [FACTOR {levels=("e","f"),ids=(0,1)}])]
// [FACTOR {levels=("e","f"),ids=(0,1)}]
// f

{% endhighlight %}


### RBool
{% highlight java %}
RBool bool=rengine.eval("1==0").asBool();
System.out.println(bool);

// output
// FALSE
{% endhighlight %}