# C#语法 VS JavaScript语法

Some of the core differences between C# and Javascript script syntax in Unity.

https://unity3d.com/learn/tutorials/topics/scripting/c-vs-js-syntax?playlist=17117

https://youtu.be/GPpw_ZE1LVc

## 脚本的基本结构

## 声明变量的方法

## 声明函数的方法

## 默认访问级别（公开还是私有）

```c
using UnityEngine;
using System.Collections;

public class ExampleSyntax : MonoBehaviour
{
    int myInt = 5;
    
    int MyFunction (int number)
    {
        int ret = myInt * number;
        return ret;
    }
}
```

```js
#pragma strict

var myInt : int = 5;

function MyFunction (number : int) : int
{
    var ret = myInt * number;
    return ret;
}

```