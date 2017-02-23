---
title: Unity Invoke方法
categories:
  - Unity
tags:
  - Scripting
date: 2014-03-05 14:30:36
updated: 2017-02-10 14:30:36
---

The Invoke functions allow you to schedule method calls to occur at a later time. In this video you will learn how to use the Invoke, InvokeRepeating, and CancelInvoke functions in your Unity scripts.

https://unity3d.com/learn/tutorials/topics/scripting/invoke?playlist=17117

https://youtu.be/sLYcVjWG2O8

可以根据想要的时间延迟来调用，Invoke只调用一次，InvokeRepeating可以多次调用，他们调用的方法只能是返回空值，不带参数的方法。同样，如果需要终止他们，我们可以使用CancelInvoke。
<!--more-->

### 示例脚本

InvokeScript

```cs
using UnityEngine;
using System.Collections;

public class InvokeScript : MonoBehaviour
{
    public GameObject target;


    void Start()
    {
        Invoke ("SpawnObject", 2);
    }

    void SpawnObject()
    {
        Instantiate(target, new Vector3(0, 2, 0), Quaternion.identity);
    }
}
```

InvokeRepeating

```cs
using UnityEngine;
using System.Collections;

public class InvokeRepeating : MonoBehaviour
{
    public GameObject target;


    void Start()
    {
        InvokeRepeating("SpawnObject", 2, 1);
    }

    void SpawnObject()
    {
        float x = Random.Range(-2.0f, 2.0f);
        float z = Random.Range(-2.0f, 2.0f);
        Instantiate(target, new Vector3(x, 2, z), Quaternion.identity);
    }
}
```