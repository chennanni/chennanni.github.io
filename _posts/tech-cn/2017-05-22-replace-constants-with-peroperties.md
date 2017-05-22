---
layout: post
title:  "工作笔记：将Constants Labels替换成Properties Labels"
date:   2017-05-22
categories: [tech-cn]
comments: true
---

## 问题阐述

程序中有如下这样的一个接口：

``` java
public interface SomeFooExceptions {
     public static final String A_EXCEPTION = "This is A exception";
     public static final String B_EXCEPTION = "This is B exception";
     ...
```

在其他类需要引用这个接口中的变量时，是这样用的：`SomeFooExceptions.A_EXCEPTION `。

**现在的要求是：不要在程序中hard code各种exception的值，而是把这些值记录在Message Center，从而方便查询管理。**

Message Center可以分为两个部分，一个是UI，一个是DB。两个部分都可以对记录的值进行修改。示意图如下：

![message-center-structure](/source/img/message-center-structure.PNG)

其中分为两种类型：

- 一种是Label，对应一个标签
- 一种是Message，对应一个消息对话框

## 问题分析

一般来说Label的值在JSP被引用到的例子较多，都是这样子用的：

``` jsp
<bean:message bundle="UIFramework" key="TXT_SOME_LABEL" />
```

在Java中有引用Message的例子，是这样的：

``` java
UIFrameworkErrorMessages.UM_SOMETHING_IS_WRONG
```

但是没有找到在Java中引用Label的例子。

经过分析我发现，这个问题的解决关键在于理解`common-message`这个项目是如何打包各种不同类型的message的。

进入`common-message` project，分析了下它的file structure发现大概分为三个类别

- `messages`：js 格式的message constant
- `resource_bundles`：message property
- `com.XXX.XXX.messages`：message的class file
- `com.XXX.XXX.properties`：label property

经过一些测试分析，确定最后一个类别里面是打包的labels。
那么接下来的问题就变成了：如何在程序中读取`common-message`中的位于`com.XXX.XXX.properties`的property file，
并用它来代替原来的Constant Labels。

## 问题解决

参考了stack overflow的这个[回答](https://stackoverflow.com/questions/333363/loading-a-properties-file-from-java-package/333385#333385)，
我的采取了如下的解决方案：在static block里完成读取properties files并赋值的工作。这样子并不需要改动使用到这个label的地方。

``` java
public static String A_EXCEPTION;
private static Properties properties;
static {
  properties = new Properties();
  InputStream in = myClass.class.getResourceAsStream( "/com/XXX/XXX/SomeException.properties" );
  try {
    properties .load(in );
    loadExceptions();
  } catch (IOException e ) {
    e.printStackTrace();
  } finally {
    try {
      in.close();
    } catch (IOException e ) {
      e.printStackTrace();
    }
  }
}
private static void loadExceptions() {
  A_EXCEPTION = properties .getProperty("A_EXCEPTION");
}
```
