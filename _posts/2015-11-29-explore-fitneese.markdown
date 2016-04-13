---
layout: post
title:  "A Little Explore Of Fitnesse"
date:   2015-11-29
categories: fitnesse
comments: true
---

## What is Fitnesse

Fitnesse is an integration testing tool to facilitate the testing activities between developers and customers/QAs.

For customers/QAs, they create acceptance test cases(tables). The tables call “fixtures”(which is built by developers) to test against the real code.

You can find the official documents
[One Minute Description](http://fitnesse.org/FitNesse.UserGuide.OneMinuteDescription) and
[Two Minute Example](http://fitnesse.org/FitNesse.UserGuide.TwoMinuteExample) very helpful.

Fitnesse comes from [Fit](http://fit.c2.com/wiki.cgi?IntroductionToFit) and then adapt to [Slim](http://butunclebob.com/FitNesse.UserGuide.SliM) as its underlying test system.

## How to use Fitnesse

### 1. Preparation
- Download `fitnesse-standalone.jar` form the website and unzip to a folder.
- Start the application by typing `java -jar fitnesse-standalone.jar -p 8080`.
- Start up a browser and go to http://localhost/8080.

See detaild instruction [here](http://fitnesse.org/FitNesseDownload).

### 2. Write Testing Tables
At the 'Front Page', 'Add' a 'Test Page'. Input the following text in the content.

~~~
<test page>

!define TEST_SYSTEM {slim}
!path /Users/Max/Documents/j2ee_workspace/Fitnesse/build/

Test 1: Division by Fitnesse
| eg.Division |
| numerator | denominator | quotient? |
| 10 | 2 | 5.0 |
| 12.6 | 3 | 4.2 |
| 22 | 7 | ~=3.14 |
| 9 | 3 | <5 |
| 11 | 2 | 4<_<6 |
| 100 | 4 | 33 |
~~~

Let's see what these means line by line.

`!define TEST_SYSTEM {slim}` defines the test system as "slim". If you don't specify this, it uses "fit" as default.

`!path` defines the classpath of the test case. In other words, you can put the code to test (which is a class file/files) there.

`| eg.Division |` tells the system which class (in the backend) this table is going to test against. `eg` is the package name, `Division` is the class name.

`| numerator | denominator | quotient? |` is a [decision table](http://fitnesse.org/FitNesse.UserGuide.WritingAcceptanceTests.SliM.DecisionTable). It is very eazy to understand: if numerator is x and denominator is y, is quotient going to be z? If true, then the test passes, otherwise fails.

### 3. Write Fixtures

After you finish writing the test table, you can directly click the "Test" button on top of the panel to see the test result.

But what is the magic behind this? Actually the "fixtures" is already there in the `fitnesse-standalone.jar`. If you open the jar file, you can see a `Division.class` file under the `eg` folder. The `division.java` should be something like this:

~~~ java
package eg;
import java.lang.String;

public class Division {
  private int numerator;
  private int denominator;

  public void setNumerator(int numerator) {
    this.numerator = numerator;
  }
  public void setDenominator(int denominator) {
    this.denominator = denominator;
  }
  public String quotient() {
    return String.valueOf(numerator/denominator);
  }
}
~~~

### 4. Fit V.S. Slim
I'm not going to discuss which one is better here. What I want to do is giving another simple example using both. You can figure out more depth difference later.

Test page for Fit

~~~
<test page>

!path /Users/Max/Documents/j2ee_workspace/Fitnesse/build/

| myexample.SayHelloWithFit |
| name | sayHello()? |
| Alice | Hello Alice |
| Bob | Hello Bob |
~~~

Test Page for Slim

~~~
<test page>

!define TEST_SYSTEM {slim}
!path /Users/Max/Documents/j2ee_workspace/Fitnesse/build/

| myexample.SayHelloWithSlim |
| name | sayHello()? |
| Alice | Hello Alice |
| Bob | Hello Bob |
~~~

Fixtures for Fit

~~~ java
package myexample;
import fit.ColumnFixture;
public class SayHelloWithFit extends ColumnFixture {
  public String name;
  public String sayHello() {
    return "Hello " + name;
  }
}
~~~

Fixtures for Slim

~~~ java
package myexample;
public class SayHelloWithSlim {
  private String name;
  public void setName(String name) {
    this.name = name;    
  }
  public String sayHello() {
    return "Hello " + name;
  }
}
~~~

Read more about the two test system here:

- [Fit Test System](http://fitnesse.org/FitNesse.UserGuide.WritingAcceptanceTests.FitFramework)
- [Slim Test System](http://fitnesse.org/FitNesse.UserGuide.WritingAcceptanceTests.SliM)
