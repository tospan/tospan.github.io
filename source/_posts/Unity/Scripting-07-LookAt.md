---
title: Unity LookAt
categories:
  - Unity
tags:
  - Scripting
date: 2014-02-24 14:30:36
updated: 2017-02-10 14:30:36
---

## Loot at 看哪儿？！

How to make a game object's transform face another's by using the LookAt function.

https://unity3d.com/learn/tutorials/topics/scripting/look?playlist=17117

https://youtu.be/NDC9qafprwE

`transform`的`LookAt`方法可以用来让一个对象的面朝向游戏世界中另外一个物体transform。下面的案例可以附加到相机上，然后给target赋值，移动target，则会发现相机在`Local`模式下Z轴正方向一直朝向target

<!--more-->

```cs
using UnityEngine;
using System.Collections;

public class CameraLookAt : MonoBehaviour
{
    public Transform target;

    void Update ()
    {
        transform.LookAt(target);
    }
}
```

## 2D LookAt函数

[LootAt 2D Equivalent?](http://answers.unity3d.com/questions/585035/lookat-2d-equivalent-.html)

```cs
Vector3 dir = target.position - transform.position;
float angle = Mathf.Atan2(dir.y,dir.x) * Mathf.Rad2Deg;
transform.rotation = Quaternion.AngleAxis(angle, Vector3.forward);
```