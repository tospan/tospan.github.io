---
title: C# 委托(Delegate)和事件(Event)
categories:
  - Unity
tags:
  - Scripting
date: 2014-04-20 14:27:47
updated: 2017-02-10 14:27:47
---

**概述：** “你能帮我做点事吗？”， “好！”，这就是一个委托的过程。本文我们将讨论C#中什么是委托(Delegate)？与此同时，我们也将了解什么是事件(Event)，它们的区别，以及使用过程中要注意的一些问题。

<!--more-->

<!-- TOC -->

- [委托](#委托)
    - [委托类型](#委托类型)
    - [委托实例](#委托实例)
- [事件](#事件)
- [委托和事件的区别](#委托和事件的区别)
- [常见问题](#常见问题)
    - [若多次添加同一个事件处理函数时，触发时处理函数是否也会多次触发？](#若多次添加同一个事件处理函数时触发时处理函数是否也会多次触发)
    - [若添加了一个事件处理函数，却执行了两次或多次”取消事件“，是否会报错？](#若添加了一个事件处理函数却执行了两次或多次取消事件是否会报错)
    - [如何认定两个事件处理函数是一样的？ 如果是匿名函数呢？](#如何认定两个事件处理函数是一样的-如果是匿名函数呢)
    - [如果不手动删除事件函数，系统会帮我们回收吗？](#如果不手动删除事件函数系统会帮我们回收吗)
    - [在多线程环境下，挂接事件时和对象创建所在的线程不同，那事件处理函数中的代码将在哪个线程中执行？](#在多线程环境下挂接事件时和对象创建所在的线程不同那事件处理函数中的代码将在哪个线程中执行)
    - [当代码的层次复杂时，开放委托和事件是不是会带来更大的麻烦？](#当代码的层次复杂时开放委托和事件是不是会带来更大的麻烦)
- [参考](#参考)

<!-- /TOC -->

## 委托

我们可以举一个现实生活中的例子，现在有个业主想把房子租出去，在网上发了个小广告，由于房子条件不错，很多人联系他看法，没有两天，业主就被这些事儿烦了，于是他就决定找个中介帮他把房子租出去，以后中介就负责接待看房子客户了，业主松了一口气。

这里面就涉及了委托哦。

其实业主的需求就是**将房子租给合适的人，把租金给我**，我们用伪代码这么表示`delegate money Rent(roomer)`， 他可以自己完成这件事儿，但是浪费时间，所以他想找中介来替他完成这个需求，也就是委托的对象 `Rent Agency`。 中介得找个人具体去负责这件事儿，比如小王，那小王就表示具体完成的方法。

### 委托类型

### 委托实例

为什么要用它？怎么用？使用过程中有什么问题？怎么解决这些问题？

委托可以看做是一个 “可以传递的函数” 的容器，或者当做变量来使用，只不过变量的类型是函数，和变量一样，delegate可以赋值，以及在运行时修改……区别是，变量可以放置数据，但delegate包含函数。

**DelegateScript**

```cs
using UnityEngine;
using System.Collections;

public class DelegateScript : MonoBehaviour 
{   
    delegate void MyDelegate(int num);
    MyDelegate myDelegate;
    

    void Start () 
    {
        myDelegate = PrintNum;
        myDelegate(50);
        
        myDelegate = DoubleNum;
        myDelegate(50);
    }
    
    void PrintNum(int num)
    {
        print ("Print Num: " + num);
    }
    
    void DoubleNum(int num)
    {
        print ("Double Num: " + num * 2);
    }
}
```

**MulticastScript**

```cs
using UnityEngine;
using System.Collections;

public class MulticastScript : MonoBehaviour 
{
    delegate void MultiDelegate();
    MultiDelegate myMultiDelegate;
    
    void Start () 
    {
        myMultiDelegate += PowerUp;
        myMultiDelegate += TurnRed;
        
        if(myMultiDelegate != null)
        {
            myMultiDelegate();
        }
    }
    
    void PowerUp()
    {
        print ("Orb is powering up!");
    }
    
    void TurnRed()
    {
        renderer.material.color = Color.red;
    }
}
```


## 事件

事件和委托的关系有点像Property和Variable之间的关系，它对委托进行了封装，还拿上面的例子来说，业主让中介把房子租出去，中介误解以为要把房子卖出去。

How to create a dynamic "broadcast" system using Events.

事件是一种特殊的委托（delegate），可以通知其他类某个特定的状况发生了。

<!--more-->

事件可以看做是一个广播系统，任何对该事件感兴趣的类可以通过方法订阅它，那么在某种情况发生了，例如点击了按钮，玩家掉血，我们会调用事件，与此同时会调用所有订阅该事件的方法。




关于OnEnable和OnDisable

**EventManager**

```cs
using UnityEngine;
using System.Collections;

public class EventManager : MonoBehaviour
{
    public delegate void ClickAction();
    public static event ClickAction OnClicked;


    void OnGUI()
    {
        if(GUI.Button(new Rect(Screen.width / 2 - 50, 5, 100, 30), "Click"))
        {
            if(OnClicked != null)
                OnClicked();
        }
    }
}
```

**TeleportScript**

```cs
using UnityEngine;
using System.Collections;

public class TeleportScript : MonoBehaviour
{
    void OnEnable()
    {
        EventManager.OnClicked += Teleport;
    }

    
    void OnDisable()
    {
        EventManager.OnClicked -= Teleport;
    }
    
    
    void Teleport()
    {
        Vector3 pos = transform.position;
        pos.y = Random.Range(1.0f, 3.0f);
        transform.position = pos;
    }
}
```

**TurnColorScript**

```cs
using UnityEngine;
using System.Collections;

public class TurnColorScript : MonoBehaviour 
{
    void OnEnable()
    {
        EventManager.OnClicked += TurnColor;
    }
    
    
    void OnDisable()
    {
        EventManager.OnClicked -= TurnColor;
    }
    
    
    void TurnColor()
    {
        Color col = new Color(Random.value, Random.value, Random.value);
        renderer.material.color = col;
    }
}
```

## 委托和事件的区别
An Event declaration adds a layer of abstraction and protection on the delegate instance. This protection prevents clients of the delegate from resetting the delegate and its invocation list and only allows adding or removing targets from the invocation list.

In brief, you can think of an event as being a bit like a property - but instead of having get/set operations, it has add/remove. The value being added/removed is always a delegate reference.

Delegates themselves support operations of:

* Combine (chain multiple delegate instances together)
* Remove (split them up again)
* Invoke (synchronous or asynchronous)
* Various things to do with finding out the target, invocation list etc

Note that delegates themselves are immutable(不变的), so combine/remove operations return a new delegate instance rather than modifying the existing ones.

## 常见问题

### 若多次添加同一个事件处理函数时，触发时处理函数是否也会多次触发？
### 若添加了一个事件处理函数，却执行了两次或多次”取消事件“，是否会报错？
### 如何认定两个事件处理函数是一样的？ 如果是匿名函数呢？
### 如果不手动删除事件函数，系统会帮我们回收吗？
### 在多线程环境下，挂接事件时和对象创建所在的线程不同，那事件处理函数中的代码将在哪个线程中执行？
### 当代码的层次复杂时，开放委托和事件是不是会带来更大的麻烦？



## 参考

* [Delegates and Events](http://csharpindepth.com/Articles/Chapter2/Events.aspx)
* [What are the differences between delegates and events?](http://stackoverflow.com/questions/29155/what-are-the-differences-between-delegates-and-events)
* [你可能不知道的陷阱：C#委托和事件的困惑](http://www.cnblogs.com/buptzym/archive/2013/03/15/2962300.html)
* [Handling and Raising Events](https://msdn.microsoft.com/en-us/library/edzehd2t(v=vs.110).aspx)
* [Delegates and Events in Unity](http://www.unitygeek.com/delegates-events-unity/)
