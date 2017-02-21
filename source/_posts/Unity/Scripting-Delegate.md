---
title: C# 委托
categories:
  - Unity Scripting
tags:
  - default
date: 2017-02-10 14:27:47
updated: 2017-02-10 14:27:47
---

什么是委托？为什么要用它？怎么用？使用过程中有什么问题？怎么解决这些问题？


## delegates 委托


https://unity3d.com/learn/tutorials/topics/scripting/delegates?playlist=17117
https://youtu.be/RSN-A0NZTO0

4:44


委托可以看做是一个 “可以传递的函数” 的容器，或者当做变量来使用，只不过变量的类型是函数，和变量一样，delegate可以赋值，以及在运行时修改……区别是，变量可以放置数据，但delegate包含函数。

DelegateScript

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

MulticastScript

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



