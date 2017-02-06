---
title: C#要点
categories:
  - Unity
tags:
  - Unity
  - C#
date: 2017-02-06 14:03:36
updated: 2017-02-06 14:03:36
---

我真正使用C#是在Unity中的。

# 阐明

Unity可以使用多种编程语言，例如C#，JavaScript。大部分情况下我们会选择C#，我们自然就会面对一个问题，Unity使用了那个C#版本？这涉及到我们是否可以使用C#的一些特性以及界定Unity编程环境中一些与众不同的地方。

这其实需要了解一下C#的生态了。

首先需要明确的是，Unity使用了Mono

## C#都有哪些版本

**[What are the correct version numbers for C#?](http://stackoverflow.com/questions/247621/what-are-the-correct-version-numbers-for-c)**

C# 只有`1.0`，`2.0`，`3.0`，`4.0`，`5.0`，`6.0`

## CLR


## Mono

## .Net Framework

## .Net Core

关于Unity到底使用了什么版本的.Net 或者 Mono之谜，答案就如下的链接里……


* [Is C# different in Unity?](http://gamedev.stackexchange.com/questions/80295/is-c-different-in-unity)
* [Why doesn't my C#6 code compile in Unity?](http://gamedev.stackexchange.com/questions/114193/why-doesnt-my-c6-code-compile-in-unity)
* [What're exact version of c# and .net unity use?](http://answers.unity3d.com/questions/263771/whatre-exact-version-of-c-and-net-unity-use.html)
* [Unity C#'s version?](https://forum.unity3d.com/threads/unity-c-s-version.355757/)
* [What is the version of .net in Unity 5?](http://answers.unity3d.com/questions/924021/what-is-the-version-of-net-in-unity-5.html)
* [.Net Core vs Mono](http://stackoverflow.com/questions/37738106/net-core-vs-mono)
* [Differences in development between .NET and Mono](http://stackoverflow.com/questions/2783268/differences-in-development-between-net-and-mono)
* **[UNDERSTANDING THE NEW CROSS-PLATFORM .NET](https://steveperkins.com/understanding-the-new-cross-platform-net/)**
* [.Net vs Mono](https://www.reddit.com/r/learnprogramming/comments/4kary0/net_vs_mono/)
* [UNITY 5.5 IS READY FOR YOU](https://blogs.unity3d.com/2016/11/29/unity-5-5-is-ready-for-you/)
* [Mono Compatibility](https://docs.unity3d.com/401/Documentation/ScriptReference/MonoCompatibility.html)
* [UNITY JOINS THE .NET FOUNDATION](https://blogs.unity3d.com/2016/04/01/unity-joins-the-net-foundation/)
* [What is Mono? Is it a compiler? A language? Or what?](http://answers.unity3d.com/questions/8555/what-is-mono-is-it-a-compiler-a-language-or-what.html)
* [About Mono](http://www.mono-project.com/docs/about-mono/)
* [A Primer For Unity Developers: What The Heck Is Mono?](http://www.lifehacker.com.au/2016/05/a-primer-for-unity-developers-what-the-heck-is-mono/)
* [Why does Unity use .NET 2.0 when Mono supports .NET 3.5?](http://stackoverflow.com/questions/34480657/why-does-unity-use-net-2-0-when-mono-supports-net-3-5)
* [Why is .NET 2.0 subset the default option ?](https://forum.unity3d.com/threads/why-is-net-2-0-subset-the-default-option.332885/)
* [What version of C# is used by Mono 2.6.4?](http://stackoverflow.com/questions/12956576/what-version-of-c-sharp-is-used-by-mono-2-6-4)
* [Release Notes Mono 2.0](http://www.mono-project.com/docs/about-mono/releases/2.0.0/)
* [What are the correct version numbers for C#?](http://stackoverflow.com/questions/247621/what-are-the-correct-version-numbers-for-c)
* [C# in Depth](http://csharpindepth.com/Articles/Chapter1/Versions.aspx)


# C#要点

## 数据类型

## 常量(const)

The const keyword indicates that a value will never change and needn't be variable. Its value will be computed during compilation and is directly inserted wherever it's referenced.

By the way, the compiler will pre-compute any constant expression, so writing (1 + 1) and simply writing 2 will both result in the same program.

## 变量(variable)

> A variable is value that can be changed. This can be either a reference to an object or something simple like an integer. A variable must be of a specific type, which is written before its name when defined.
> 
> Variables exist only inside the scope that their are defined in. By default, if a variable is defined inside a class, every object instance of that class has its own version of that variable. However, it can be marked as static, in which case it exists once for that class, independent of any objects. If a variable is defined inside a method, you can think of it only existing while that method is being invoked.

### 类型转换(cast)

Casting changes the type of a value. You indicate this by writing the type within parentheses before the value. Casting simple values results in a conversion, like converting a floating-point value to an integer by discarding the fractional part.

Casting an object reference to another type doesn't convert the object, it only changes how that reference is treated. You'd only do this when you're sure about the object's type.

## 函数/方法(method)

> A method is a chunk of behavior, which is defined in a class. It can accept input and produce an output. Input is defined and provided after the method name, in parentheses, even if there is none. The method's type is that of its output. If there's no output this is indicated with void.
>
> By default, methods define behavior of objects, but it's possible to define behavior that doesn't need an object to function. Such methods are marked as static.

## 什么是属性(property)

> A property is a method that pretends to be a variable. It might be read-only or write-only.
>
> Note that variables exposed by the Unity editor are also commonly called properties, but it's not the same thing.

## if条件语句

## 循环语句

## switch语句

## Scope and Access Modifiers

## Conventions and Syntax

## 枚举

## 集合

http://www.cnblogs.com/kissdodog/archive/2013/01/29/2882195.html
https://msdn.microsoft.com/zh-cn/library/9ct4ey7x(v=vs.90).aspx


### 数组

### 列表

### 字典

## 什么是结构(struct)

A struct is a blueprint, just like a class. The difference is that whatever it creates is treated as a simple value, like an integer, instead of an object. It doesn't have the same sense of identity as an object.

## 类(class)
A class is a blueprint that can be used to create objects that reside in your computer's memory. The blueprint defines what data these objects contain and how they behave.

## 继承

## 多态


##  命名空间(namespace)
>
> It's like a website domain, but for code stuff. For example, MonoBehaviour is defined inside the UnityEngine namespace, so it can be addressed with UnityEngine.MonoBehaviour.
>
> Just like domains, namespaces can be nested. The big difference is that it's written the other way around. So instead of forum.unity3d.com it would be com.unity3d.forum. For example, the ArrayList type exists inside the Collections namespace, which in turn exists inside the System namespace. So you need to write System.Collections.ArrayList to get hold of it.
>
> By declaring that we're using a namespace, we don't need to write it again each time we want to use something from it. So, when using UnityEngine, instead of having to write UnityEngine.MonoBehaviour we can suffice with MonoBehaviour.
