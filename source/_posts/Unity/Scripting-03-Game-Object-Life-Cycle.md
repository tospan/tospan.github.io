---
title: Unity游戏对象的生命周期
categories:
  - Unity
tags:
  - Scripting
date: 2014-02-10 14:30:36
updated: 2017-02-10 14:30:36
---

**概述：**Unity中游戏对象的生命周期是怎样的呢？

<!--more-->

## Awake 和 Start 方法

How to use Awake and Start, two of Unity's initialisation functions.

https://unity3d.com/learn/tutorials/topics/scripting/awake-and-start?playlist=17117

https://youtu.be/M1KuIi5pCno

首先这两个方法在加载脚本的时候会调用，

### Awake方法

Awake会先调用，即使脚本组件没有启用。尤其适合用于设置各个脚本之间的引用关系以及初始化

### Start方法

Start方法会在Awake调用之后，首次调用Update方法之前调用，但仅仅在脚本组件启用了之后才会调用一次，这意味着我们可以使用Start方法来处理那些当且仅当脚本组件启用时发生的事儿。可以用于延迟处理实际需要才进行初始化的一些操作

> 有Start方法的脚本会在脚本组件启用时候调用，但是仅仅会调用一次，不能通过启用、禁用来不断调用该方法。

例如，敌人角色在Awake函数里设置它的子弹数量，但仅仅在Start方法才启用射击功能

### 示例代码

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

https://unity3d.com/learn/tutorials/topics/scripting/update-and-fixedupdate?playlist=17117

https://youtu.be/ZukcUv3pyXQ

### Update

Update 方法也许是Unity中最常使用的方法：

* 只要脚本中有该方法，则每一帧都会调用
* 主要用于一些常规的Update：
    - 移动非物理属性的对象
    - 简单计时器
    - 监听用户输入
* Update调用的间隔时间不规律，有可能一帧比另一帧处理所消耗的时间更长（短）

### FixedUpdate

和Update方法比较相似，但稍微有些区别：

* 在每个物理步长(Physics Step)调用
* FixedUpdate调用间隔时间是固定
* 主要用于调整物理属性（Rigidbody）的游戏对象

Immediately after FixedUpdate is called, any necessary physics calculations are made.

```cs
using UnityEngine;
using System.Collections;

public class UpdateAndFixedUpdate : MonoBehaviour
{
    void FixedUpdate ()
    {
        Debug.Log("FixedUpdate time :" + Time.deltaTime);
    }


    void Update ()
    {
        Debug.Log("Update time :" + Time.deltaTime);
    }
}
```

## Destroy 销毁

如何在游戏运行时使用`Destroy()`函数移除GameObject和Components。

https://unity3d.com/learn/tutorials/topics/scripting/destroy?playlist=17117

https://youtu.be/QxM0CfL3jQ8


`Destroy()` 可以移除GameObejct或者组件，它还有可选参数作为延迟时间，也就是等待多久后执行销毁操作。

**DestroyBasic**

```cs
using UnityEngine;
using System.Collections;

public class DestroyBasic : MonoBehaviour
{
    void Update ()
    {
        if(Input.GetKey(KeyCode.Space))
        {
            Destroy(gameObject);
        }
    }
}
```

**DestroyOther**

```cs
using UnityEngine;
using System.Collections;

public class DestroyOther : MonoBehaviour
{
    public GameObject other;


    void Update ()
    {
        if(Input.GetKey(KeyCode.Space))
        {
            Destroy(other);
        }
    }
}
```

**DestroyComponent**

```cs
using UnityEngine;
using System.Collections;

public class DestroyComponent : MonoBehaviour
{
    void Update ()
    {
        if(Input.GetKey(KeyCode.Space))
        {
            Destroy(GetComponent<MeshRenderer>());
        }
    }
}
```