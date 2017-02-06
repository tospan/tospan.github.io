# Awake 和 Start 方法

How to use Awake and Start, two of Unity's initialisation functions.

https://unity3d.com/learn/tutorials/topics/scripting/awake-and-start?playlist=17117

https://youtu.be/M1KuIi5pCno

首先这两个方法在加载脚本的时候会调用，

## Awake方法

Awake会先调用，即使脚本组件没有启用。尤其适合用于设置各个脚本之间的引用关系以及初始化

## Start方法

Start方法会在Awake调用之后，首次调用Update方法之前调用，但仅仅在脚本组件启用了之后才会调用一次，这意味着我们可以使用Start方法来处理那些当且仅当脚本组件启用时发生的事儿。可以用于延迟处理实际需要才进行初始化的一些操作

> 有Start方法的脚本会在脚本组件启用时候调用，但是仅仅会调用一次，不能通过启用、禁用来不断调用该方法。

例如，敌人角色在Awake函数里设置它的子弹数量，但仅仅在Start方法才启用射击功能

## 示例代码

```cs
using UnityEngine;
using System.Collections;

public class AwakeAndStart : MonoBehaviour
{
    void Awake ()
    {
        Debug.Log("Awake called.");
    }


    void Start ()
    {
        Debug.Log("Start called.");
    }
}
```


## 相关文档

* [Awake](http://docs.unity3d.com/Documentation/ScriptReference/MonoBehaviour.Awake.html?_ga=1.139151098.838993178.1480250241) (Script Reference)
* [Start](http://docs.unity3d.com/Documentation/ScriptReference/MonoBehaviour.Start.html?_ga=1.139151098.838993178.1480250241) (Script Reference)

## 社区资源

* [Answers - Difference of assigning a variable outside any function, in Awake or in Start?](http://answers.unity3d.com/questions/8794/Difference-of-assigning-a-variable-outside-any-function-in-Awake-or-in-Start.html?_ga=1.72016346.838993178.1480250241) (Answers)