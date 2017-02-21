---
title: C#要点
categories:
  - Unity Scripting
tags:
  - Unity
  - Scripting
  - C#
date: 2017-02-06 14:03:36
updated: 2017-02-06 14:03:36
---

**概述:** 我真正使用C#是在Unity中的。Unity可以使用多种编程语言，例如C#，JavaScript。大部分情况下我们会选择C#，我们自然就会面对一个问题，Unity使用了那个C#版本？这涉及到我们是否可以使用C#的一些特性以及界定Unity编程环境中一些与众不同的地方。

这其实需要了解一下C#的生态了。

首先需要明确的是，Unity使用了Mono

<!--more-->

<!-- TOC -->

- [C#语言的生态环境](#c语言的生态环境)
- [C#都有哪些版本](#c都有哪些版本)
- [CLR Mono .Net Framework .Net Core](#clr-mono-net-framework-net-core)
- [脚本即行为组件](#脚本即行为组件)
- [常量(const)](#常量const)
- [变量与函数](#变量与函数)
    - [类型转换(cast)](#类型转换cast)
- [Conventions and Syntax 语法](#conventions-and-syntax-语法)
    - [点](#点)
    - [分号](#分号)
    - [缩进](#缩进)
- [if条件语句](#if条件语句)
- [循环](#循环)
    - [while](#while)
    - [do...while](#dowhile)
    - [for](#for)
    - [foreach](#foreach)
- [访问域与访问修饰符](#访问域与访问修饰符)
    - [ScopeAndAccessModifiers](#scopeandaccessmodifiers)
    - [AnotherClass](#anotherclass)
- [Data types 数据类型](#data-types-数据类型)
- [什么是结构(struct)](#什么是结构struct)
- [classes 类](#classes-类)
    - [new](#new)
    - [this](#this)
- [Instantiate 实例化](#instantiate-实例化)
- [数组](#数组)
- [enumerations 枚举](#enumerations-枚举)
- [switch语句](#switch语句)
- [Properties属性](#properties属性)
    - [自动实现](#自动实现)
- [三元运算符](#三元运算符)
- [Statics静态](#statics静态)
    - [静态方法](#静态方法)
    - [静态类](#静态类)
- [methods-overloading方法重载](#methods-overloading方法重载)
- [Generics泛型](#generics泛型)
- [inheritance继承](#inheritance继承)
- [polymorphism多态](#polymorphism多态)
- [Member Hiding](#member-hiding)
- [Overriding重写](#overriding重写)
- [接口](#接口)
- [Extension Methods 扩展方法](#extension-methods-扩展方法)
- [namespaces命名空间](#namespaces命名空间)
- [列表和字典](#列表和字典)
- [相关文档](#相关文档)
- [社区资源](#社区资源)

<!-- /TOC -->

## C#语言的生态环境

我反正一开始是对什么.NET，.NET Framework，.NET Core，ASP.NET，CLI，CIL，CLR这些词语混淆了，而且看来还不止我一个，StackOverflow上有个答案清晰的解释了它们都表示什么。

[.NET vs ASP.NET vs CLR vs ASP](http://stackoverflow.com/questions/3103360/net-vs-asp-net-vs-clr-vs-asp)

* [ASP](http://en.wikipedia.org/wiki/Active_Server_Pages), Active Server Pages (now referred to as ASP Classic) is a server-side scripting environment that predates .Net and has nothing to do with it
ASP pages are usually written in VBScript, but can be written in any language supported by the Windows Scripting Host - JScript and VBScript are supported natively, with third-party libraries offering support for PerlScript and other dynamic languages.
* [.Net](http://en.wikipedia.org/wiki/.NET_Framework) is a framework for managed code and assemblies
.Net code can be written in any language that has an CIL compiler.
* [CLR](http://en.wikipedia.org/wiki/Common_Language_Runtime), Common Language Runtime, is the core runtime used by the .Net framework
The CLR transforms CIL code (formerly MSIL) into machine code (this is done by the JITter or by ngen) and executes it.
* [ASP.Net](http://en.wikipedia.org/wiki/ASP.NET) is a replacement for ASP built on .Net
ASP.Net pages can be written in any .Net language, but are usually written in C#.
Other terms that you didn't ask about:

* [CIL](http://en.wikipedia.org/wiki/Common_Intermediate_Language), Common Intermediate Language, is an intermediate language that all .Net code is compiled to.
The CLR executes CIL code.
* [CLI](http://en.wikipedia.org/wiki/Common_Language_Infrastructure), Common Language Infrastructure, is the open specification for the runtime and behavior of the .Net Framework
* [Mono](http://en.wikipedia.org/wiki/Mono_%28software%29) is an open-source implementation of the CLI that can run .Net programs
* [ASP.Net](http://en.wikipedia.org/wiki/ASP.NET_MVC_Framework) MVC is an MVC framework built on ASP.Net


关于从符合CLI语言标准到CIL，然后在CLR上运行过程，这里有张图片很好地说明了：

![](../_images/unity/520px-Overview_of_the_Common_Language_Infrastructure.svg)

这就解释了为什么说Unity使用了Mono，但是有人说它用的是C#编程语言的问题，也解释了为什么一开始Unity支持`Boo(Python)`

首先`C#`和`Boo`都是符合CLI的标准的，而这些语言最终都由其编译器编译为CIL，而CIL可以运行在任何CLI的实现环境上，例如.NET中的CLR，Mono Runtime等。

The Common Language Infrastructure (CLI) is an open specification (technical standard) developed by Microsoft and standardized by ISO[1] and ECMA[2] that describes executable code and a runtime environment that allows multiple high-level languages to be used on different computer platforms without being rewritten for specific architectures. This implies it is platform agnostic. The .NET Framework and the free and open source Mono and Portable.NET are implementations of the CLI.



## C#都有哪些版本

**[What are the correct version numbers for C#?](http://stackoverflow.com/questions/247621/what-are-the-correct-version-numbers-for-c)**

C# 只有`1.0`，`2.0`，`3.0`，`4.0`，`5.0`，`6.0`

## CLR Mono .Net Framework .Net Core

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


## 脚本即行为组件

https://unity3d.com/learn/tutorials/modules/beginner/scripting/scripts-as-behaviour-components?playlist=17117

https://youtu.be/YvA1O7MYs_w

Scripts should be considered as behaviour components in Unity. As with other components in Unity they can be applied to objects and are seen in the inspector. With this particular example, this cube has a rigidbody component which gives it a physical mass.

And when you press play the cube falls to the ground as it uses gravity. We also have added an examples script. This behaviour script has code in it which changes the colour of the cube by effecting the colour value of the default material attached to that object. When we press the R key on the keyboard the colour gets changed to red. When we press G the colour gets changed to green. And when we press B it gets changed to blue. By attaching this script to the object when we refer to game object we're referring to this particular item. We then drill down to the value that we want and effect it. Here we're addressing the game object this script is attached to, we're then addressing the renderer, which is the component seen here, mesh renderer. We're then addressing the material attached to that renderer and finally the colour of that material. And we're setting it to a shortcut called red which is part of the colour class. Let's see this in action. If I press play, then use the R, G or B keys on the keyboard I can change the colour. And you can see that the material is being effected. So this material is applied to the renderer, default diffuse, you can see that listed there, and we're then effecting the main colour value and setting it to a certain value in here. The same as it would if I was actually doing it by hand in the editor.

Scripts can be created in the project panel by choosing Create and then choosing a language of your choice. For example, they can then be attached to objects either by dragging and dropping or by choosing the Add Component button at the bottom of the Component menu and then choosing from the list of scripts in your current project. Scripts can also be created using the Add Component button by choosing New Script from the bottom and naming the script and selecting a language type from the drop-down menu. This can then be created and added in one step. The final way to add a script to your object is to select the object in the hierarchy and then choose Components - Scripts and then choose from the list of scripts in your current project. Of course you can apply scripts to do all manner of other behaviours of objects. Try to think of scripts as components that you create yourself, allowing you to create behaviour for different game objects in your game, this could be characters, it could be environments or it could be scripts that manage the functionality of the game. This example script that we've looked at is written in C# but in Unity you can write in Javascript, C# and Boo. Boo is a derivative of Python, though it's not as commonly used as the other two. So you'll likely see Javascript or C# examples when you see scripting from Unity around the web. The videos that you see in this learning area will be written in C# but we'll also provide the Javascript equivalent beneath the video.

```cs
using UnityEngine;
using System.Collections;

public class ExampleBehaviourScript : MonoBehaviour
{
    void Update()
    {
        if (Input.GetKeyDown(KeyCode.R))
        {
            GetComponent<Renderer>().material.color = Color.red;
        }
        if (Input.GetKeyDown(KeyCode.G))
        {
            GetComponent<Renderer>().material.color = Color.green;
        }
        if (Input.GetKeyDown(KeyCode.B))
        {
            GetComponent<Renderer>().material.color = Color.blue;
        }
    }
}
```

## 常量(const)

The const keyword indicates that a value will never change and needn't be variable. Its value will be computed during compilation and is directly inserted wherever it's referenced.

By the way, the compiler will pre-compute any constant expression, so writing (1 + 1) and simply writing 2 will both result in the same program.

## 变量与函数

What are Variables and Functions, and how do they store and process information for us?

https://unity3d.com/learn/tutorials/topics/scripting/variables-and-functions?playlist=17117
https://youtu.be/WQnpeR7u7HM

A variable is value that can be changed. This can be either a reference to an object or something simple like an integer. A variable must be of a specific type, which is written before its name when defined.
 
Variables exist only inside the scope that their are defined in. By default, if a variable is defined inside a class, every object instance of that class has its own version of that variable. However, it can be marked as static, in which case it exists once for that class, independent of any objects. If a variable is defined inside a method, you can think of it only existing while that method is being invoked.


A method is a chunk of behavior, which is defined in a class. It can accept input and produce an output. Input is defined and provided after the method name, in parentheses, even if there is none. The method's type is that of its output. If there's no output this is indicated with void.

By default, methods define behavior of objects, but it's possible to define behavior that doesn't need an object to function. Such methods are marked as static.

### 类型转换(cast)

Casting changes the type of a value. You indicate this by writing the type within parentheses before the value. Casting simple values results in a conversion, like converting a floating-point value to an integer by discarding the fractional part.

Casting an object reference to another type doesn't convert the object, it only changes how that reference is treated. You'd only do this when you're sure about the object's type.

```cs
using UnityEngine;
using System.Collections;

public class VariablesAndFunctions : MonoBehaviour
{
    int myInt = 5;

    void Start ()
    {
        myInt = MultiplyByTwo(myInt);
        Debug.Log (myInt);
    }


    int MultiplyByTwo (int number)
    {
        int ret;
        ret = number * 2;
        return ret;
    }
}
```

## Conventions and Syntax 语法

https://unity3d.com/learn/tutorials/topics/scripting/conventions-and-syntax?playlist=17117

https://youtu.be/xtvPpR0YdQs

### 点

就像一个地址。

### 分号

### 缩进

```cs

using UnityEngine;
using System.Collections;

public class BasicSyntax : MonoBehaviour
{
    void Start ()
    {
        Debug.Log(transform.position.x);
        
        if(transform.position.y <= 5f)
        {
            Debug.Log ("I'm about to hit the ground!");
        }
    }
}
```

## if条件语句

https://unity3d.com/learn/tutorials/topics/scripting/if-statements?playlist=17117

https://youtu.be/ZKVAkAoGNwQ


```cs
using UnityEngine;
using System.Collections;

public class IfStatements : MonoBehaviour
{
    float coffeeTemperature = 85.0f;
    float hotLimitTemperature = 70.0f;
    float coldLimitTemperature = 40.0f;

    void Update ()
    {
        if(Input.GetKeyDown(KeyCode.Space))
            TemperatureTest();

        coffeeTemperature -= Time.deltaTime * 5f;
    }


    void TemperatureTest ()
    {
        // If the coffee's temperature is greater than the hottest drinking temperature...
        if(coffeeTemperature > hotLimitTemperature)
        {
            // ... do this.
            print("Coffee is too hot.");
        }
        // If it isn't, but the coffee temperature is less than the coldest drinking temperature...
        else if(coffeeTemperature < coldLimitTemperature)
        {
            // ... do this.
            print("Coffee is too cold.");
        }
        // If it is neither of those then...
        else
        {
            // ... do this.
            print("Coffee is just right.");
        }
    }
}
```

## 循环

How to use the For, While, Do-While, and For Each Loops to repeat actions in code.

https://unity3d.com/learn/tutorials/topics/scripting/loops?playlist=17117
https://youtu.be/NiLqfxSw5WA

### while

```cs
using UnityEngine;
using System.Collections;

public class WhileLoop : MonoBehaviour
{
    int cupsInTheSink = 4;
    
    
    void Start ()
    {
        while(cupsInTheSink > 0)
        {
            Debug.Log ("I've washed a cup!");
            cupsInTheSink--;
        }
    }
}
```

### do...while

```cs
using UnityEngine;
using System.Collections;

public class DoWhileLoop : MonoBehaviour 
{
    void Start()
    {
        bool shouldContinue = false;
        
        do
        {
            print ("Hello World");
            
        }while(shouldContinue == true);
    }
}
```

### for

```cs
using UnityEngine;
using System.Collections;

public class ForLoop : MonoBehaviour
{
    int numEnemies = 3;

    void Start ()
    {
        for(int i = 0; i < numEnemies; i++)
        {
            Debug.Log("Creating enemy number: " + i);
        }
    }
}
```

### foreach

```cs
using UnityEngine;
using System.Collections;

public class ForeachLoop : MonoBehaviour
{
    void Start ()
    {
        string[] strings = new string[3];

        strings[0] = "First string";
        strings[1] = "Second string";
        strings[2] = "Third string";

        foreach(string item in strings)
        {
            print (item);
        }
    }
}
```


## 访问域与访问修饰符

Understanding variable & function scope and accessibility.

https://unity3d.com/learn/tutorials/topics/scripting/scope-and-access-modifiers?playlist=17117

https://youtu.be/XKGT_4mR1C4

### ScopeAndAccessModifiers

```cs
using UnityEngine;
using System.Collections;

public class ScopeAndAccessModifiers : MonoBehaviour
{
    public int alpha = 5;


    private int beta = 0;
    private int gamma = 5;


    private AnotherClass myOtherClass;


    void Start ()
    {
        alpha = 29;

        myOtherClass = new AnotherClass();
        myOtherClass.FruitMachine(alpha, myOtherClass.apples);
    }


    void Example (int pens, int crayons)
    {
        int answer;
        answer = pens * crayons * alpha;
        Debug.Log(answer);
    }


    void Update ()
    {
        Debug.Log("Alpha is set to: " + alpha);
    }
}
```

### AnotherClass

```cs
using UnityEngine;
using System.Collections;

public class AnotherClass
{
    public int apples;
    public int bananas;


    private int stapler;
    private int sellotape;


    public void FruitMachine (int a, int b)
    {
        int answer;
        answer = a + b;
        Debug.Log("Fruit total: " + answer);
    }


    private void OfficeSort (int a, int b)
    {
        int answer;
        answer = a + b;
        Debug.Log("Office Supplies total: " + answer);
    }
}
```

## Data types 数据类型

Learn the important differences between Value and Reference data types, in order to better understand how variables work.

https://unity3d.com/learn/tutorials/topics/scripting/data-types?playlist=17117

https://youtu.be/6jMFsO6ldcw


DatatypeScript

```cs
using UnityEngine;
using System.Collections;

public class DatatypeScript : MonoBehaviour
{
    void Start ()
    {
        //Value type variable
        Vector3 pos = transform.position;
        pos = new Vector3(0, 2, 0);

        //Reference type variable
        Transform tran = transform;
        tran.position = new Vector3(0, 2, 0);
    }
}
```

## 什么是结构(struct)

A struct is a blueprint, just like a class. The difference is that whatever it creates is treated as a simple value, like an integer, instead of an object. It doesn't have the same sense of identity as an object.



## classes 类

How to use Classes to store and organise your information, and how to create constructors to work with parts of your class.

https://unity3d.com/learn/tutorials/topics/scripting/classes?playlist=17117

https://youtu.be/ZlJ2iH1jsCo

A class is a blueprint that can be used to create objects that reside in your computer's memory. The blueprint defines what data these objects contain and how they behave.

### new

The new keyword is used to construct a new instance of an object or a struct. It's followed by calling a special constructor method, which has the same name as the class or struct it belongs to.

### this

The this keyword refers to the current object or struct whose method is being called. It's being used implicitly all the time when referring to stuff from the same class. For example, whenever we've access depth we could have also done it via this.depth.
 
You typically only use this when you need to pass along a reference to the object itself, like we do for Initialize. Why? Because we're calling the Initialize method of the new child object, not of the parent object.



SingleCharacterScript

```cs
using UnityEngine;
using System.Collections;

public class SingleCharacterScript : MonoBehaviour
{
    public class Stuff
    {
        public int bullets;
        public int grenades;
        public int rockets;

        public Stuff(int bul, int gre, int roc)
        {
            bullets = bul;
            grenades = gre;
            rockets = roc;
        }
    }


    public Stuff myStuff = new Stuff(10, 7, 25);
    public float speed;
    public float turnSpeed;
    public Rigidbody bulletPrefab;
    public Transform firePosition;
    public float bulletSpeed;


    void Update ()
    {
        Movement();
        Shoot();
    }


    void Movement ()
    {
        float forwardMovement = Input.GetAxis("Vertical") * speed * Time.deltaTime;
        float turnMovement = Input.GetAxis("Horizontal") * turnSpeed * Time.deltaTime;

        transform.Translate(Vector3.forward * forwardMovement);
        transform.Rotate(Vector3.up * turnMovement);
    }


    void Shoot ()
    {
        if(Input.GetButtonDown("Fire1") && myStuff.bullets > 0)
        {
            Rigidbody bulletInstance = Instantiate(bulletPrefab, firePosition.position, firePosition.rotation) as Rigidbody;
            bulletInstance.AddForce(firePosition.forward * bulletSpeed);
            myStuff.bullets--;
        }
    }
}
```

Inventory

```cs
using UnityEngine;
using System.Collections;

public class Inventory : MonoBehaviour
{
    public class Stuff
    {
        public int bullets;
        public int grenades;
        public int rockets;
        public float fuel;

        public Stuff(int bul, int gre, int roc)
        {
            bullets = bul;
            grenades = gre;
            rockets = roc;
        }

        public Stuff(int bul, float fu)
        {
            bullets = bul;
            fuel = fu;
        }

        // Constructor
        public Stuff ()
        {
            bullets = 1;
            grenades = 1;
            rockets = 1;
        }
    }


    // Creating an Instance (an Object) of the Stuff class
    public Stuff myStuff = new Stuff(50, 5, 5);

    public Stuff myOtherStuff = new Stuff(50, 1.5f);

    void Start()
    {
        Debug.Log(myStuff.bullets); 
    }
}
```

MovementControls

```cs
using UnityEngine;
using System.Collections;

public class MovementControls : MonoBehaviour
{
    public float speed;
    public float turnSpeed;


    void Update ()
    {
        Movement();
    }


    void Movement ()
    {
        float forwardMovement = Input.GetAxis("Vertical") * speed * Time.deltaTime;
        float turnMovement = Input.GetAxis("Horizontal") * turnSpeed * Time.deltaTime;

        transform.Translate(Vector3.forward * forwardMovement);
        transform.Rotate(Vector3.up * turnMovement);
    }
}
```

Shooting

```cs
using UnityEngine;
using System.Collections;

public class Shooting : MonoBehaviour
{
    public Rigidbody bulletPrefab;
    public Transform firePosition;
    public float bulletSpeed;


    private Inventory inventory;


    void Awake ()
    {
        inventory = GetComponent<Inventory>();
    }


    void Update ()
    {
        Shoot();
    }


    void Shoot ()
    {
        if(Input.GetButtonDown("Fire1") && inventory.myStuff.bullets > 0)
        {
            Rigidbody bulletInstance = Instantiate(bulletPrefab, firePosition.position, firePosition.rotation) as Rigidbody;
            bulletInstance.AddForce(firePosition.forward * bulletSpeed);
            inventory.myStuff.bullets--;
        }
    }
}
```

## Instantiate 实例化

How to use Instantiate to create clones of a Prefab during runtime.

https://unity3d.com/learn/tutorials/topics/scripting/instantiate?playlist=17117

https://youtu.be/4rZAAHevq1s

`Instantiate`函数是用于创建游戏对象的Clone对象。常用于克隆Prefab，

需要注意的点：

1. 复制的对象
2. 复制对象所在的位置和朝向
3. 复制对象的类型 as type
4. 什么时候销毁新的克隆对象？


UsingInstantiate

```cs
using UnityEngine;
using System.Collections;

public class UsingInstantiate : MonoBehaviour
{
    public Rigidbody rocketPrefab;
    public Transform barrelEnd;


    void Update ()
    {
        if(Input.GetButtonDown("Fire1"))
        {
            Rigidbody rocketInstance;
            rocketInstance = Instantiate(rocketPrefab, barrelEnd.position, barrelEnd.rotation) as Rigidbody;
            rocketInstance.AddForce(barrelEnd.forward * 5000);
        }
    }
}
```

RocketDestruction

```cs
using UnityEngine;
using System.Collections;

public class RocketDestruction : MonoBehaviour
{
    void Start()
    {
        Destroy (gameObject, 1.5f);
    }
}
```


## 数组

Using arrays to collect variables together into a more manageable form.

https://unity3d.com/learn/tutorials/topics/scripting/arrays?playlist=17117

https://youtu.be/GqLNQY1052A


Arrays

```cs
using UnityEngine;
using System.Collections;

public class Arrays : MonoBehaviour
{
    public GameObject[] players;

    void Start ()
    {
        players = GameObject.FindGameObjectsWithTag("Player");

        for(int i = 0; i < players.Length; i++)
        {
            Debug.Log("Player Number "+i+" is named "+players[i].name);
        }
    }
}
```

## enumerations 枚举

Enumerations allow you to create a collection of related constants. In this video you will learn how to declare and use enumerations in your code.

https://unity3d.com/learn/tutorials/topics/scripting/enumerations?playlist=17117

https://youtu.be/DG-QjqSDR6s


```cs
using UnityEngine;
using System.Collections;

public class EnumScript : MonoBehaviour 
{
    enum Direction {North, East, South, West};

    void Start ()
    {
        Direction myDirection;

        myDirection = Direction.North;
    }

    Direction ReverseDirection (Direction dir)
    {
        if(dir == Direction.North)
            dir = Direction.South;
        else if(dir == Direction.South)
            dir = Direction.North;
        else if(dir == Direction.East)
            dir = Direction.West;
        else if(dir == Direction.West)
            dir = Direction.East;

        return dir;
    }
}
```

## switch语句

Switch statements act like streamline conditionals. They are useful for when you want to compare a single variable against a series of constants. In this video you will learn how to write and use switch statements.

https://unity3d.com/learn/tutorials/topics/scripting/switch-statements?playlist=17117

https://youtu.be/JwhWiLlNz3U

在做条件判断时，常用的是if条件语句，或者几个if else，类似的，我们也可以使用switch语句，它相对而言比较单一，主要用于单个值与多个常量比较，经常配合枚举来使用


ConversationScript

```cs
using UnityEngine;
using System.Collections;

public class ConversationScript : MonoBehaviour
{
    public int intelligence = 5;


    void Greet()
    {
        switch (intelligence)
        {
            case 5:
                print ("Why hello there good sir! Let me teach you about Trigonometry!");
                break;
            case 4:
                print ("Hello and good day!");
                break;
            case 3:
                print ("Whadya want?");
                break;
            case 2:
                print ("Grog SMASH!");
                break;
            case 1:
                print ("Ulg, glib, Pblblblblb");
                break;
            default:
                print ("Incorrect intelligence level.");
                break;
        }
    }
}
```

## Properties属性

How to create properties to access the member variables (fields) in a class.

A property is a method that pretends to be a variable. It might be read-only or write-only.

Note that variables exposed by the Unity editor are also commonly called properties, but it's not the same thing.



https://unity3d.com/learn/tutorials/topics/scripting/properties?playlist=17117
https://youtu.be/BYwCXJ-J9dA

3:11

获取某个类的成员变量(我们通常也称为字段)是常见的需求，比较有效的方法就是我们将成员变量的访问修饰符设置为public，但此外我们还有更好的方法，就是使用属性，属性就像方法，它封装了成员变量并给我们很大的便利什么时候以及如何访问。

假设，现在我们有个变量`experience`，我们用它表示经验值，那么它的属性可以这样写，可以看到属性的定义流程是这样的，先给出`public`的访问修饰符，然后给出类型，名称，对于名称比较好的惯例就是将首字母大写，其他保持一致，然后是像函数一样的大括号，里面是属性访问器？？ get， set，用于设置和获取对应的字段。完整的写法如下：

```cs
using UnityEngine;
using System.Collections;

public class Player:MonoBehaviour{

    private int experience;

    public int Experience{
        set{ experience = value; }
        get{ return experience; }
    }

}
```

这里多了一个值 `value` 这是需要特别注意的。这样就算是一个很正常的属性的定义方法了。在其他代码中我们就可以获取或设置属性了。

那么问题是：既然我能直接通过设置成员变量就可以完成的任务，为什么还要使用属性呢？

主要有两个功能是成员变量不具备的：

1. 通过忽略set或get，我们就可以实现该属性是只读还是只写？
2. setter和getter和函数一样，这意味着我们在读取或者设置的时候可以有更多的空间和可能性。我们可以在里面添加代码或调用其他函数，经典的就是设置属性的时候调用coroutine。

使用属性封装不一定需要直接对应某个成员变量，比如，这里我们可以定义一个`Level`属性，只要用户的experience值达到1000，`Level`值就升一级，那么我们的代码可以如下：

```cs
using UnityEngine;
using System.Collections;

public class Player:MonoBehaviour{

    private int experience;

    public int Experience{
        set{ experience = value; }
        get{ return experience; }
    }

    public int Level{
        set { experience = value * 1000; }
        get { return experience / 1000; }
    }
}

```

这里主要是在setter上可能有些疑惑，其实它的功能是通过设置玩家Level，从而计算Player的经验值。


### 自动实现

我们可以通过简写的语法来实现，就是在set或get之后直接加上分号，这样的属性和普通的字段基本功能相似，区别就是我们可以通过只留下set或get从而实现只读或只写。

```cs
using UnityEngine;
using System.Collections;

public class Player: MonoBehaviour{

    public int Health{
        set; get;
    }
}
```

问题：如果我的代码如下，设置Health属性值的时候，会修改health值吗？

```cs
using UnityEngine;
using System.Collections;

public class Player: MonoBehaviour{

    private int health;

    public int Health{
        set; get;
    }
}
```

答案是不会，它们之间没有任何关系。


## 三元运算符

How to utilize the Ternary Operator to build simple, shorthand IF-ELSE logical conditions.

https://unity3d.com/learn/tutorials/topics/scripting/ternary-operator?playlist=17117

https://youtu.be/ALXYkk34bq8

1:03

```cs
using UnityEngine;
using System.Collections;

public class TernaryOperator : MonoBehaviour
{
    void Start ()
    {
        int health = 10;
        string message;

        //This is an example Ternary Operation that chooses a message based
        //on the variable "health".
        message = health > 0 ? "Player is Alive" : "Player is Dead";
    }
}
```

三元运算符的结构就是，condition ? [true return value] : [false return value]，根据条件判断最终返回一个值，当然，我们可以嵌套使用，例如

```cs
message = health > 0 ? "Player is Alive" : health == 0 ? "Player is Barely Alive": "Player is Dead";
```

不过这样代码的可读性就不是很好，最佳的实践方式是在一个三元运算符里不嵌套其他三元运算符。


## Statics静态

How to create static variables, methods, and classes.

如何创建静态变量，方法和类？

https://unity3d.com/learn/tutorials/topics/scripting/statics?playlist=17117

https://youtu.be/nraOAaYLdRQ

3:18


静态变量或函数在所有类的示例中都可以访问，也就是说它们可以直接获取和调用而不用实例化它们所在的类。

通常情况下，每个类的成员变量都是相互独立的，虽然某个类的不同实例有都有相同的成员变量，但是它们的值仅仅属于该实例而不是共享的。但静态变量，就是由相同的成员变量有统一且唯一的值，也就是说，在一个地方修改了值，其他地方都会改变。


举例来说，我们想知道在游戏过程中，我们到底实例化了多少个敌人。我们在Enemy类中创建一个静态变量，那么就意味着该变量仅属于该类，而不是任何该类的实例。然后在实例化该类的时候递增该变量。

Enemy

```cs
using UnityEngine;
using System.Collections;

public class Enemy
{
    //Static variables are shared across all instances
    //of a class.
    public static int enemyCount = 0;

    public Enemy()
    {
        //Increment the static variable to know how many
        //objects of this class have been created.
        enemyCount++;
    }
}
```

获取到底实例化了多少个Enemy，直接调用 `Enemy.enemyCount`即可

Game

```cs
using UnityEngine;
using System.Collections;

public class Game
{
    void Start ()
    {
        Enemy enemy1 = new Enemy();
        Enemy enemy2 = new Enemy();
        Enemy enemy3 = new Enemy();

        //You can access a static variable by using the class name
        //and the dot operator.
        int x = Enemy.enemyCount;
    }
}
```

静态的这种特性对于Unity中的GameObject组件同样有效，例如我们想知道到在场景中创建了多少个Player

Player

```cs
using UnityEngine;
using System.Collections;

public class Player : MonoBehaviour
{
    //Static variables are shared across all instances
    //of a class.
    public static int playerCount = 0;

    void Start()
    {
        //Increment the static variable to know how many
        //objects of this class have been created.
        playerCount++;
    }
}
```


PlayerManager

```cs
using UnityEngine;
using System.Collections;

public class PlayerManager : MonoBehaviour
{
    void Start()
    {
        //You can access a static variable by using the class name
        //and the dot operator.
        int x = Player.playerCount;
    }
}
```


### 静态方法

和静态变量一样，静态方法也是仅仅属于当期类而不属于它的实例，我们可以直接调用而不用实例化该类。

Utilities

```cs
using UnityEngine;
using System.Collections;

public class Utilities
{
    //A static method can be invoked without an object
    //of a class. Note that static methods cannot access
    //non-static member variables.
    public static int Add(int num1, int num2)
    {
        return num1 + num2;
    }
}
```

UtilitiesExample

```cs
using UnityEngine;
using System.Collections;

public class UtilitiesExample : MonoBehaviour
{
    void Start()
    {
        //You can access a static method by using the class name
        //and the dot operator.
        int x = Utilities.Add (5, 6);
    }
}
```

其实我们已经使用过多次静态变量，和静态方法了，例如  `Input.GetAxis`等等，为什么知道它们是静态的呢？原因就是我们从来没有实例化过它们。事实上Unity有很静态变量和方法提供了相当丰富的内容和特性供我们调用。

> 需要注意的是，我们不能在静态函数中使用非静态的变量和非静态的方法，为什么？
> 因为静态方法和静态变量属于类，而非静态变量和方法属于类的实例。


### 静态类

我们可以在class前面加上static修饰符，该类是不可以被实例化的。它适用于我们期望类中的所有变量和函数都是静态的，例如Input Class。

Utilities

```cs
using UnityEngine;
using System.Collections;

public static class Utilities
{
    //A static method can be invoked without an object
    //of a class. Note that static methods cannot access
    //non-static member variables.
    public static int Add(int num1, int num2)
    {
        return num1 + num2;
    }
}
```

> 注意：在类前面加上了static关键字之后，静态变量和方法同样需要加上static关键字。


## methods-overloading方法重载

How to overload methods to create different methods with the same name.

https://unity3d.com/learn/tutorials/topics/scripting/method-overloading?playlist=17117

https://youtu.be/SSTMu8QomkE

1:52

重载可以让我们对一个方法有多个定义，也就是说我们可以用同一个方法名称去做不同的事儿，例如，我们现在想把样东西加在一起，对于数字，我们可以使用AddNumbers，对于字符串我们可以使用AddStrings，那现在我们就有两个方法名称需要记住了，我们有更好的办法，只用Add，它可以执行数值相加或者字符串相加。

每个方法都有签名，签名是由方法名和参数列表组成，例如`Add(int num1, int num2)` 的签名是`Add(int,int)`，`Add(string str1, string str2)`对应的签名就是 `Add(string, string)`

在同一个域下方法签名必须是唯一的，而我们的重写方法就是使用同样的方法名，但使用不同的参数列表。


Some Class

```cs
using UnityEngine;
using System.Collections;

public class SomeClass
{
    //The first Add method has a signature of
    //"Add(int, int)". This signature must be unique.
    public int Add(int num1, int num2)
    {
        return num1 + num2;
    }

    //The second Add method has a sugnature of
    //"Add(string, string)". Again, this must be unique.
    public string Add(string str1, string str2)
    {
        return str1 + str2;
    }
}
```

SomeOtherClass

```cs
using UnityEngine;
using System.Collections;

public class SomeOtherClass : MonoBehaviour
{
    void Start ()
    {
        SomeClass myClass = new SomeClass();

        //The specific Add method called will depend on
        //the arguments passed in.
        myClass.Add (1, 2);
        myClass.Add ("Hello ", "World");
    }
}

```


那么系统是怎么决定到底应该使用重载方法呢？以下三件事会发生其一：

1. 完全匹配参数列表，则对应的重载函数会运行
2. Least Conversion，如果没有完全匹配的参数列表，系统会查找所有可能的匹配，需要适度类型转换可以执行的方法。 will requrie the least amount of conversion，
3. Error，如果没有完全匹配的或者 multiple versions taht require the same amount of conversion，就会报错。


## Generics泛型

https://unity3d.com/learn/tutorials/topics/scripting/generics?playlist=17117

https://youtu.be/BOC-Z3WEbsg

4:08


SomeClass

```cs
using UnityEngine;
using System.Collections;

public class SomeClass
{
    //Here is a generic method. Notice the generic
    //type 'T'. This 'T' will be replaced at runtime
    //with an actual type. 
    public T GenericMethod<T>(T param)
    {
        return param;
    }
}
```

SomeOtherClass

```cs
using UnityEngine;
using System.Collections;

public class SomeOtherClass : MonoBehaviour
{
    void Start ()
    {
        SomeClass myClass = new SomeClass();

        //In order to use this method you must
        //tell the method what type to replace
        //'T' with.
        myClass.GenericMethod<int>(5);
    }
}
```

GenericClass

```cs
using UnityEngine;
using System.Collections;

//Here is a generic class. Notice the generic type 'T'.
//'T' will be replaced with an actual type, as will also 
//instances of the type 'T' used in the class.
public class GenericClass <T>
{
    T item;

    public void UpdateItem(T newItem)
    {
        item = newItem;
    }
}
```

GenericClassExample

```cs
using UnityEngine;
using System.Collections;

public class GenericClassExample : MonoBehaviour 
{
    void Start ()
    {
        //In order to create an object of a generic class, you must
        //specify the type you want the class to have.
        GenericClass<int> myClass = new GenericClass<int>();

        myClass.UpdateItem(5);
    }
}
```



## inheritance继承

How to use inheritance to reuse code and build a strong relationship between related classes.

https://unity3d.com/learn/tutorials/topics/scripting/inheritance?playlist=17117

5:28


```cs
using UnityEngine;
using System.Collections;

//This is the base class which is
//also known as the Parent class.
public class Fruit
{
    public string color;

    //This is the first constructor for the Fruit class
    //and is not inherited by any derived classes.
    public Fruit()
    {
        color = "orange";
        Debug.Log("1st Fruit Constructor Called");
    }

    //This is the second constructor for the Fruit class
    //and is not inherited by any derived classes.
    public Fruit(string newColor)
    {
        color = newColor;
        Debug.Log("2nd Fruit Constructor Called");
    }

    public void Chop()
    {
        Debug.Log("The " + color + " fruit has been chopped.");
    }

    public void SayHello()
    {
        Debug.Log("Hello, I am a fruit.");
    }
}
```

```cs
using UnityEngine;
using System.Collections;

//This is the derived class whis is
//also know as the Child class.
public class Apple : Fruit
{
    //This is the first constructor for the Apple class.
    //It calls the parent constructor immediately, even
    //before it runs.
    public Apple()
    {
        //Notice how Apple has access to the public variable
        //color, which is a part of the parent Fruit class.
        color = "red";
        Debug.Log("1st Apple Constructor Called");
    }

    //This is the second constructor for the Apple class.
    //It specifies which parent constructor will be called
    //using the "base" keyword.
    public Apple(string newColor) : base(newColor)
    {
        //Notice how this constructor doesn't set the color
        //since the base constructor sets the color that
        //is passed as an argument.
        Debug.Log("2nd Apple Constructor Called");
    }
}
```

```cs
using UnityEngine;
using System.Collections;

public class FruitSalad : MonoBehaviour
{
    void Start ()
    {
        //Let's illustrate inheritance with the 
        //default constructors.
        Debug.Log("Creating the fruit");
        Fruit myFruit = new Fruit();
        Debug.Log("Creating the apple");
        Apple myApple = new Apple();

        //Call the methods of the Fruit class.
        myFruit.SayHello();
        myFruit.Chop();

        //Call the methods of the Apple class.
        //Notice how class Apple has access to all
        //of the public methods of class Fruit.
        myApple.SayHello();
        myApple.Chop();

        //Now let's illustrate inheritance with the 
        //constructors that read in a string.
        Debug.Log("Creating the fruit");
        myFruit = new Fruit("yellow");
        Debug.Log("Creating the apple");
        myApple = new Apple("green");

        //Call the methods of the Fruit class.
        myFruit.SayHello();
        myFruit.Chop();

        //Call the methods of the Apple class.
        //Notice how class Apple has access to all
        //of the public methods of class Fruit.
        myApple.SayHello();
        myApple.Chop();
    }
}
```

## polymorphism多态

How to use Polymorphism, Upcasting, and Downcasting to create powerful and dynamic functionality between inherited classes.

https://unity3d.com/learn/tutorials/topics/scripting/polymorphism?playlist=17117

https://youtu.be/vLFx4-i3zqM

3:12

```cs
using UnityEngine;
using System.Collections;

public class Fruit
{
    public Fruit()
    {
        Debug.Log("1st Fruit Constructor Called");
    }

    public void Chop()
    {
        Debug.Log("The fruit has been chopped.");
    }

    public void SayHello()
    {
        Debug.Log("Hello, I am a fruit.");
    }
}
```

```cs
using UnityEngine;
using System.Collections;

public class Apple : Fruit
{
    public Apple()
    {
        Debug.Log("1st Apple Constructor Called");
    }

    //Apple has its own version of Chop() and SayHello(). 
    //When running the scripts, notice when Fruit's version
    //of these methods are called and when Apple's version
    //of these methods are called.
    //In this example, the "new" keyword is used to supress
    //warnings from Unity while not overriding the methods
    //in the Apple class.
    public new void Chop()
    {
        Debug.Log("The apple has been chopped.");
    }

    public new void SayHello()
    {
        Debug.Log("Hello, I am an apple.");
    }
}
```

```cs
using UnityEngine;
using System.Collections;

public class FruitSalad : MonoBehaviour
{
    void Start ()
    {
        //Notice here how the variable "myFruit" is of type
        //Fruit but is being assigned a reference to an Apple. This
        //works because of Polymorphism. Since an Apple is a Fruit,
        //this works just fine. While the Apple reference is stored
        //in a Fruit variable, it can only be used like a Fruit
        Fruit myFruit = new Apple();

        myFruit.SayHello();
        myFruit.Chop();

        //This is called downcasting. The variable "myFruit" which is 
        //of type Fruit, actually contains a reference to an Apple. Therefore,
        //it can safely be turned back into an Apple variable. This allows
        //it to be used like an Apple, where before it could only be used
        //like a Fruit.
        Apple myApple = (Apple)myFruit;

        myApple.SayHello();
        myApple.Chop();
    }
}
```

## Member Hiding

How to implement the hiding of base members in derived classes.

https://unity3d.com/learn/tutorials/topics/scripting/member-hiding?playlist=17117

https://youtu.be/iW5rrLWEggc

1:51

```cs
using UnityEngine;
using System.Collections;

public class Humanoid
{
    //Base version of the Yell method
    public void Yell()
    {
        Debug.Log ("Humanoid version of the Yell() method");
    }
}
```


```cs
using UnityEngine;
using System.Collections;

public class Enemy : Humanoid
{
    //This hides the Humanoid version.
    new public void Yell()
    {
        Debug.Log ("Enemy version of the Yell() method");
    }
}
```

```cs
using UnityEngine;
using System.Collections;

public class Orc : Enemy
{
    //This hides the Enemy version.
    new public void Yell()
    {
        Debug.Log ("Orc version of the Yell() method");
    }
}
```

```cs
using UnityEngine;
using System.Collections;

public class WarBand : MonoBehaviour 
{
    void Start () 
    {
        Humanoid human = new Humanoid();
        Humanoid enemy = new Enemy();
        Humanoid orc = new Orc();
        
        //Notice how each Humanoid variable contains
        //a reference to a different class in the
        //inheritance hierarchy, yet each of them
        //calls the Humanoid Yell() method.
        human.Yell();
        enemy.Yell();
        orc.Yell();
    }
}
```

## Overriding重写

https://unity3d.com/learn/tutorials/topics/scripting/overriding?playlist=17117

https://youtu.be/zmf6-VIv31w

2:37

How to override a base classes members with the members of a child class.

```cs
using UnityEngine;
using System.Collections;

public class Fruit 
{
    public Fruit ()
    {
        Debug.Log("1st Fruit Constructor Called");
    }
    
    //These methods are virtual and thus can be overriden
    //in child classes
    public virtual void Chop ()
    {
        Debug.Log("The fruit has been chopped.");     
    }
    
    public virtual void SayHello ()
    {
        Debug.Log("Hello, I am a fruit.");
    }
}
```

```cs
using UnityEngine;
using System.Collections;

public class Apple : Fruit 
{
    public Apple ()
    {
        Debug.Log("1st Apple Constructor Called");
    }
    
    //These methods are overrides and therefore
    //can override any virtual methods in the parent
    //class.
    public override void Chop ()
    {
        base.Chop();
        Debug.Log("The apple has been chopped.");     
    }
    
    public override void SayHello ()
    {
        base.SayHello();
        Debug.Log("Hello, I am an apple.");
    }
}
```


```cs
using UnityEngine;
using System.Collections;

public class FruitSalad : MonoBehaviour 
{   
    void Start () 
    {
        Apple myApple = new Apple();
        
        //Notice that the Apple version of the methods
        //override the fruit versions. Also notice that
        //since the Apple versions call the Fruit version with
        //the "base" keyword, both are called.
        myApple.SayHello();
        myApple.Chop(); 
        
        //Overriding is also useful in a polymorphic situation.
        //Since the methods of the Fruit class are "virtual" and
        //the methods of the Apple class are "override", when we 
        //upcast an Apple into a Fruit, the Apple version of the 
        //Methods are used.
        Fruit myFruit = new Apple();
        myFruit.SayHello();
        myFruit.Chop();
    }
}
```

## 接口

How to create interfaces and implement them in classes.

https://unity3d.com/learn/tutorials/topics/scripting/interfaces?playlist=17117

https://youtu.be/sQfS4w0wvcc

3:55

Interfaces

```cs
using UnityEngine;
using System.Collections;

//This is a basic interface with a single required
//method.
public interface IKillable
{
    void Kill();
}

//This is a generic interface where T is a placeholder
//for a data type that will be provided by the 
//implementing class.
public interface IDamageable<T>
{
    void Damage(T damageTaken);
}
```

Avatar Class

```cs
using UnityEngine;
using System.Collections;

public class Avatar : MonoBehaviour, IKillable, IDamageable<float>
{
    //The required method of the IKillable interface
    public void Kill()
    {
        //Do something fun
    }
    
    //The required method of the IDamageable interface
    public void Damage(float damageTaken)
    {
        //Do something fun
    }
}
```

## Extension Methods 扩展方法

How to create, implement, and call an extension method.

https://unity3d.com/learn/tutorials/topics/scripting/extension-methods?playlist=17117
https://youtu.be/v2ONQAqqLDc

1:54


```cs
using UnityEngine;
using System.Collections;

//It is common to create a class to contain all of your
//extension methods. This class must be static.
public static class ExtensionMethods
{
    //Even though they are used like normal methods, extension
    //methods must be declared static. Notice that the first
    //parameter has the 'this' keyword followed by a Transform
    //variable. This variable denotes which class the extension
    //method becomes a part of.
    public static void ResetTransformation(this Transform trans)
    {
        trans.position = Vector3.zero;
        trans.localRotation = Quaternion.identity;
        trans.localScale = new Vector3(1, 1, 1);
    }
}
```

```cs
using UnityEngine;
using System.Collections;

public class SomeClass : MonoBehaviour 
{
    void Start () {
        //Notice how you pass no parameter into this
        //extension method even though you had one in the
        //method declaration. The transform object that
        //this method is called from automatically gets
        //passed in as the first parameter.
        transform.ResetTransformation();
    }
}
```

## namespaces命名空间

How to create and use namespaces to organize your classes.

https://unity3d.com/learn/tutorials/topics/scripting/namespaces?playlist=17117

https://youtu.be/OybNcBJINKg

2:49

It's like a website domain, but for code stuff. For example, MonoBehaviour is defined inside the UnityEngine namespace, so it can be addressed with UnityEngine.MonoBehaviour.

Just like domains, namespaces can be nested. The big difference is that it's written the other way around. So instead of forum.unity3d.com it would be com.unity3d.forum. For example, the ArrayList type exists inside the Collections namespace, which in turn exists inside the System namespace. So you need to write System.Collections.ArrayList to get hold of it.

By declaring that we're using a namespace, we don't need to write it again each time we want to use something from it. So, when using UnityEngine, instead of having to write UnityEngine.MonoBehaviour we can suffice with MonoBehaviour.

```cs
using UnityEngine;
using System.Collections;

namespace SampleNamespace
{
    public class SomeClass : MonoBehaviour
    {
        void Start ()
        {

        }
    }
}
```


## 列表和字典

How to create and use the List and Dictionary collections.

https://unity3d.com/learn/tutorials/modules/intermediate/scripting/lists-and-dictionaries?playlist=17117

https://youtu.be/qVgrJkdampw

5:41

```cs
using UnityEngine;
using System.Collections;
using System; //This allows the IComparable Interface

//This is the class you will be storing
//in the different collections. In order to use
//a collection's Sort() method, this class needs to
//implement the IComparable interface.
public class BadGuy : IComparable<BadGuy>
{
    public string name;
    public int power;
    
    public BadGuy(string newName, int newPower)
    {
        name = newName;
        power = newPower;
    }
    
    //This method is required by the IComparable
    //interface. 
    public int CompareTo(BadGuy other)
    {
        if(other == null)
        {
            return 1;
        }
        
        //Return the difference in power.
        return power - other.power;
    }
}
```

```cs
using UnityEngine;
using System.Collections;
using System.Collections.Generic;

public class SomeClass : MonoBehaviour
{
    void Start () 
    {
        //This is how you create a list. Notice how the type
        //is specified in the angle brackets (< >).
        List<BadGuy> badguys = new List<BadGuy>();
        
        //Here you add 3 BadGuys to the List
        badguys.Add( new BadGuy("Harvey", 50));
        badguys.Add( new BadGuy("Magneto", 100));
        badguys.Add( new BadGuy("Pip", 5));
        
        badguys.Sort();
        
        foreach(BadGuy guy in badguys)
        {
            print (guy.name + " " + guy.power);
        }
        
        //This clears out the list so that it is
        //empty.
        badguys.Clear();
    }
}
```

```cs
using UnityEngine;
using System.Collections;
using System.Collections.Generic;

public class SomeOtherClass : MonoBehaviour 
{
    void Start ()
    {
        //This is how you create a Dictionary. Notice how this takes
        //two generic terms. In this case you are using a string and a
        //BadGuy as your two values.
        Dictionary<string, BadGuy> badguys = new Dictionary<string, BadGuy>();
        
        BadGuy bg1 = new BadGuy("Harvey", 50);
        BadGuy bg2 = new BadGuy("Magneto", 100);
        
        //You can place variables into the Dictionary with the
        //Add() method.
        badguys.Add("gangster", bg1);
        badguys.Add("mutant", bg2);
        
        BadGuy magneto = badguys["mutant"];
        
        BadGuy temp = null;
        
        //This is a safer, but slow, method of accessing
        //values in a dictionary.
        if(badguys.TryGetValue("birds", out temp))
        {
            //success!
        }
        else
        {
            //failure!
        }
    }
}
```

## 相关文档

* [Reference Homepage](http://docs.unity3d.com/Documentation/ScriptReference/index.html?_ga=1.138685178.838993178.1480250241) (Script Reference)
* [Scripting in Unity](http://docs.unity3d.com/Manual/ScriptingSection.html?_ga=1.176885100.838993178.1480250241) (Manual)
* [Awake](http://docs.unity3d.com/Documentation/ScriptReference/MonoBehaviour.Awake.html?_ga=1.139151098.838993178.1480250241) (Script Reference)
* [Start](http://docs.unity3d.com/Documentation/ScriptReference/MonoBehaviour.Start.html?_ga=1.139151098.838993178.1480250241) (Script Reference)
* [Update](http://docs.unity3d.com/Documentation/ScriptReference/MonoBehaviour.Update.html?_ga=1.210043612.838993178.1480250241) (Script Reference)
* [FixedUpdate](http://docs.unity3d.com/Documentation/ScriptReference/MonoBehaviour.FixedUpdate.html?_ga=1.210043612.838993178.1480250241) (Script Reference)
* [Vector2](http://docs.unity3d.com/Documentation/ScriptReference/Vector2.html?_ga=1.184306415.838993178.1480250241) (Script Reference)
* [Vector3](http://docs.unity3d.com/Documentation/ScriptReference/Vector3.html?_ga=1.184306415.838993178.1480250241) (Script Reference)
* [Dot](http://docs.unity3d.com/Documentation/ScriptReference/Vector3.Dot.html?_ga=1.184306415.838993178.1480250241) (Script Reference)
* [Cross](http://docs.unity3d.com/Documentation/ScriptReference/Vector3.Cross.html?_ga=1.184306415.838993178.1480250241) (Script Reference)
* [GetComponent](https://unity3d.com/learn/tutorials/topics/scripting/getcomponent) (Lesson)
* [Get Component](http://docs.unity3d.com/Documentation/ScriptReference/Component.GetComponent.html?_ga=1.154920561.838993178.1480250241) (Script Reference)
* [Set Active](http://docs.unity3d.com/Documentation/ScriptReference/GameObject.SetActive.html?_ga=1.120728257.838993178.1480250241) (Script Reference)
* [Active Self](http://docs.unity3d.com/Documentation/ScriptReference/GameObject-activeSelf.html?_ga=1.120728257.838993178.1480250241) (Script Reference)
* [Active In Hierarchy](http://docs.unity3d.com/Documentation/ScriptReference/GameObject-activeInHierarchy.html?_ga=1.120728257.838993178.1480250241) (Script Reference)
* [Deactivating GameObjects](http://docs.unity3d.com/Documentation/Manual/DeactivatingGameObjects.html?_ga=1.120728257.838993178.1480250241) (Manual)
* [Adding Physics Forces](https://unity3d.com/learn/tutorials/topics/physics/adding-physics-forces) (Lesson)
* [Adding Physics Torque](https://unity3d.com/learn/tutorials/topics/physics/adding-physics-torque) (Lesson)
* [Transform](http://docs.unity3d.com/Documentation/ScriptReference/Transform.html?_ga=1.108710219.838993178.1480250241) (Script Reference)
* [Translate](http://docs.unity3d.com/Documentation/ScriptReference/Transform.Translate.html?_ga=1.108710219.838993178.1480250241) (Script Reference)
* [Rotate](http://docs.unity3d.com/Documentation/ScriptReference/Transform.Rotate.html?_ga=1.108710219.838993178.1480250241) (Script Reference)
* [AddForce](http://docs.unity3d.com/Documentation/ScriptReference/Rigidbody.AddForce.html?_ga=1.108710219.838993178.1480250241) (Script Reference)
* [AddTorque](http://docs.unity3d.com/Documentation/ScriptReference/Rigidbody.AddTorque.html?_ga=1.108710219.838993178.1480250241) (Script Reference)
* [LookAt](http://docs.unity3d.com/Documentation/ScriptReference/Transform.LookAt.html?_ga=1.114100686.838993178.1480250241) (Script Reference)
* [Vector Maths](https://unity3d.com/learn/tutorials/topics/scripting/vector-maths) (Lesson)
* [Vector3 Lerp](http://docs.unity3d.com/Documentation/ScriptReference/Vector3.Lerp.html?_ga=1.209404507.838993178.1480250241)  (Script Reference)
* [Mathf Lerp](http://docs.unity3d.com/Documentation/ScriptReference/Mathf.Lerp.html?_ga=1.172630506.838993178.1480250241)  (float) (Script Reference)
* [Color Lerp](http://docs.unity3d.com/Documentation/ScriptReference/Color.Lerp.html?_ga=1.172630506.838993178.1480250241)  (Script Reference)
* [Light intensity](http://docs.unity3d.com/Documentation/ScriptReference/Light-intensity.html?_ga=1.172630506.838993178.1480250241)  (Script Reference)
* [Vector3 Smooth Damp](http://docs.unity3d.com/ScriptReference/Vector3.SmoothDamp.html?_ga=1.172630506.838993178.1480250241)  (Script Reference)
* [Destroy](http://docs.unity3d.com/Documentation/ScriptReference/Object.Destroy.html?_ga=1.118112576.838993178.1480250241) (Script Reference)
* [GetKey](http://docs.unity3d.com/Documentation/ScriptReference/Input.GetKey.html?_ga=1.121392449.838993178.1480250241) (Script Reference)
* [GetKeyUp](http://docs.unity3d.com/Documentation/ScriptReference/Input.GetKeyUp.html?_ga=1.121392449.838993178.1480250241) (Script Reference)
* [GetKeyDown](http://docs.unity3d.com/Documentation/ScriptReference/Input.GetKeyDown.html?_ga=1.121392449.838993178.1480250241) (Script Reference)
* [GetButton](http://docs.unity3d.com/Documentation/ScriptReference/Input.GetButton.html?_ga=1.121392449.838993178.1480250241) (Script Reference)
* [GetButtonUp](http://docs.unity3d.com/Documentation/ScriptReference/Input.GetButtonUp.html?_ga=1.187903457.838993178.1480250241) (Script Reference)
* [GetButtonDown](http://docs.unity3d.com/Documentation/ScriptReference/Input.GetButtonDown.html?_ga=1.187903457.838993178.1480250241) (Script Reference)
* [The Input Manager](http://docs.unity3d.com/Documentation/Components/class-InputManager.html?_ga=1.187903457.838993178.1480250241) (Manual)
* [GetAxis](http://docs.unity3d.com/Documentation/ScriptReference/Input.GetAxis.html?_ga=1.146074109.838993178.1480250241) (Script Reference)
* [GetAxisRaw](http://docs.unity3d.com/Documentation/ScriptReference/Input.GetAxisRaw.html?_ga=1.146074109.838993178.1480250241) (Script Reference)
* [OnMouseDown](http://docs.unity3d.com/Documentation/ScriptReference/MonoBehaviour.OnMouseDown.html?_ga=1.184887904.838993178.1480250241) (Script Reference)
* [OnMouseDrag](http://docs.unity3d.com/Documentation/ScriptReference/MonoBehaviour.OnMouseDrag.html?_ga=1.184887904.838993178.1480250241) (Script Reference)
* [OnMouseEnter](http://docs.unity3d.com/Documentation/ScriptReference/MonoBehaviour.OnMouseEnter.html?_ga=1.184887904.838993178.1480250241) (Script Reference)
* [OnMouseExit](http://docs.unity3d.com/Documentation/ScriptReference/MonoBehaviour.OnMouseExit.html?_ga=1.184887904.838993178.1480250241) (Script Reference)
* [OnMouseOver](http://docs.unity3d.com/Documentation/ScriptReference/MonoBehaviour.OnMouseOver.html?_ga=1.184887904.838993178.1480250241) (Script Reference)
* [OnMouseUp](http://docs.unity3d.com/Documentation/ScriptReference/MonoBehaviour.OnMouseUp.html?_ga=1.184887904.838993178.1480250241) (Script Reference)
* [OnMouseUpAsButton](http://docs.unity3d.com/Documentation/ScriptReference/MonoBehaviour.OnMouseUpAsButton.html?_ga=1.184887904.838993178.1480250241) (Script Reference)
* [Enabling and Disabling Components](https://unity3d.com/learn/tutorials/topics/scripting/enabling-and-disabling-components) (Lesson)
* [Classes](https://unity3d.com/learn/tutorials/topics/scripting/classes) (Lesson)
* [GetComponent](http://docs.unity3d.com/Documentation/ScriptReference/Component.GetComponent.html?_ga=1.117778752.838993178.1480250241) (Script Reference)
* [Delta Time](http://docs.unity3d.com/Documentation/ScriptReference/Time-deltaTime.html?_ga=1.218827344.838993178.1480250241) (Script Reference)
* [Variables and Functions](https://unity3d.com/learn/tutorials/topics/scripting/variables-and-functions) (Lesson)
* [Instantiate](http://docs.unity3d.com/Documentation/ScriptReference/Object.Instantiate.html?_ga=1.219425104.838993178.1480250241) (Script Reference)
* [Prefabs](http://docs.unity3d.com/Documentation/Manual/Prefabs.html?_ga=1.219425104.838993178.1480250241) (Manual)
* [Array](http://docs.unity3d.com/Documentation/ScriptReference/Array.html?_ga=1.185396576.838993178.1480250241) (Script Reference)
* [Variables and Functions](https://unity3d.com/learn/tutorials/topics/scripting/variables-and-functions) (Lesson)
* [Integral Types](http://msdn.microsoft.com/en-us/library/exx3b86w.aspx)
* [Enumerations](https://unity3d.com/learn/tutorials/topics/scripting/enumerations) (Lesson)
* [Classes](https://unity3d.com/learn/tutorials/topics/scripting/classes) (Lesson)
* [Variables and Functions](https://unity3d.com/learn/tutorials/topics/scripting/variables-and-functions) (Lesson)
* [Lists and Dictionaries](https://unity3d.com/learn/tutorials/modules/intermediate/scripting/lists-and-dictionaries) (Lesson)
* [Interfaces](https://unity3d.com/learn/tutorials/topics/scripting/interfaces) (Lesson)
* [Polymorphism](https://unity3d.com/learn/tutorials/topics/scripting/polymorphism) (Lesson)
* [Overriding](https://unity3d.com/learn/tutorials/topics/scripting/overriding) (Lesson)
* [Classes](https://unity3d.com/learn/tutorials/topics/scripting/classes) (Lesson)
* [Overriding](https://unity3d.com/learn/tutorials/topics/scripting/overriding) (Lesson)
* [Classes](https://unity3d.com/learn/tutorials/topics/scripting/classes) (Lesson)
* [Polymorphism](http://msdn.microsoft.com/en-us/library/ms173152.aspx) - MSDN
* [Overriding](https://unity3d.com/learn/tutorials/topics/scripting/overriding) (Lesson)
* [Classes](https://unity3d.com/learn/tutorials/topics/scripting/classes) (Lesson)
* [Inheritance](https://unity3d.com/learn/tutorials/topics/scripting/inheritance) (Lesson)
* [Classes](https://unity3d.com/learn/tutorials/topics/scripting/classes) (Lesson)
* [Inheritance](https://unity3d.com/learn/tutorials/topics/scripting/inheritance) (Lesson)
* [Member](https://unity3d.com/learn/tutorials/topics/scripting/member-hiding) Hiding (Lesson)
* [Classes](https://unity3d.com/learn/tutorials/topics/scripting/classes) (Lesson)
* [Polymorphism](https://unity3d.com/learn/tutorials/topics/scripting/polymorphism) (Lesson)
* [Overriding](https://unity3d.com/learn/tutorials/topics/scripting/overriding) (Lesson)
* [Generics](https://unity3d.com/learn/tutorials/topics/scripting/generics) (Lesson)
* [Lists](http://msdn.microsoft.com/en-us/library/6sh2ey19.aspx) - MSDN
* [Dictionaries](http://msdn.microsoft.com/en-us/library/xfhwa508.aspx)
* [ExecuteInEditMode Attribute Script Reference](http://docs.unity3d.com/412/Documentation/ScriptReference/ExecuteInEditMode.html?_ga=1.213584221.838993178.1480250241)
* [Delegates](https://unity3d.com/learn/tutorials/topics/scripting/delegates) (Lesson)


* http://www.cnblogs.com/kissdodog/archive/2013/01/29/2882195.html
* https://msdn.microsoft.com/zh-cn/library/9ct4ey7x(v=vs.90).aspx


## 社区资源

* [Answers - Difference of assigning a variable outside any function, in Awake or in Start?](http://answers.unity3d.com/questions/8794/Difference-of-assigning-a-variable-outside-any-function-in-Awake-or-in-Start.html?_ga=1.72016346.838993178.1480250241) (Answers)
* [Using the Xbox Controller in Unity](http://answers.unity3d.com/questions/8019/how-do-i-use-an-xbox-360-controller-in-unity.html?_ga=1.187903457.838993178.1480250241) (Answers)