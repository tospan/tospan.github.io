---
title: Unity编程 2D脚本常用代码集合
categories:
  - Unity
tags:
  - Unity
  - C#
date: 2017-02-06 14:08:57
updated: 2017-02-06 14:08:57
---


## 2D LookAt函数

[LootAt 2D Equivalent?](http://answers.unity3d.com/questions/585035/lookat-2d-equivalent-.html)

```cs
Vector3 dir = target.position - transform.position;
float angle = Mathf.Atan2(dir.y,dir.x) * Mathf.Rad2Deg;
transform.rotation = Quaternion.AngleAxis(angle, Vector3.forward);
```

## 随机旋转值

[How to make a Random Quaternion.](http://answers.unity3d.com/questions/675338/how-to-make-a-random-quaternion.html)

```cs
yourSphere.rotation = Quaternion.Euler(Randon.Range(0.0f, 360.0f), Randon.Range(0.0f, 360.0f), Randon.Range(0.0f, 360.0f));
```

## 围绕某个物体旋转

[2D rotation around a object](http://answers.unity3d.com/questions/1133341/2d-rotation-around-a-object.html)

```cs
transform.RotateAround(target.position, zAxis, speed);
```