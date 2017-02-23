---
title: Unity移动和旋转
categories:
  - Unity
tags:
  - Scripting
date: 2014-02-20 14:30:36
updated: 2017-02-10 14:30:36
---

## Translate 

How to use the two transform functions Translate and Rotate to effect a non-rigidbody object's position and rotation.

https://unity3d.com/learn/tutorials/topics/scripting/translate-and-rotate?playlist=17117

https://youtu.be/Ywn7n0SXB4M

如何使用transform的两个方法Translate和Rotate来影响一个非Rigidbody对象的位置和朝向。
<!--more-->

```cs
using UnityEngine;
using System.Collections;

public class TransformFunctions : MonoBehaviour
{
    public float moveSpeed = 10f;
    public float turnSpeed = 50f;


    void Update ()
    {
        if(Input.GetKey(KeyCode.UpArrow))
            transform.Translate(Vector3.forward * moveSpeed * Time.deltaTime);

        if(Input.GetKey(KeyCode.DownArrow))
            transform.Translate(-Vector3.forward * moveSpeed * Time.deltaTime);

        if(Input.GetKey(KeyCode.LeftArrow))
            transform.Rotate(Vector3.up, -turnSpeed * Time.deltaTime);

        if(Input.GetKey(KeyCode.RightArrow))
            transform.Rotate(Vector3.up, turnSpeed * Time.deltaTime);
    }
}
```

## Rotate

### 随机旋转值

[How to make a Random Quaternion.](http://answers.unity3d.com/questions/675338/how-to-make-a-random-quaternion.html)

```cs
yourSphere.rotation = Quaternion.Euler(Randon.Range(0.0f, 360.0f), Randon.Range(0.0f, 360.0f), Randon.Range(0.0f, 360.0f));
```

### 围绕某个物体旋转

[2D rotation around a object](http://answers.unity3d.com/questions/1133341/2d-rotation-around-a-object.html)

```cs
transform.RotateAround(target.position, zAxis, speed);
```
