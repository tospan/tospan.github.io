---
title: Unity Input
categories:
  - Unity
tags:
  - Scripting
date: 2014-02-28 14:30:36
updated: 2017-02-10 14:30:36
---

**概述** 游戏是一门交互的艺术，如何接收和处理用户的输入是一个很好的课题。

<!--more-->

## GetButton 与 GetKey

https://unity3d.com/learn/tutorials/topics/scripting/getbutton-and-getkey?playlist=17117

https://youtu.be/qf3Oo3BW83Y

How to get button or key for input and how these axes behave / can be modified with the Input manager

**KeyInput**

```cs
using UnityEngine;
using System.Collections;

public class KeyInput : MonoBehaviour
{
    public GUITexture graphic;
    public Texture2D standard;
    public Texture2D downgfx;
    public Texture2D upgfx;
    public Texture2D heldgfx;

    void Start()
    {
        graphic.texture = standard;
    }

    void Update]() ()
    {
        bool down = Input.GetKeyDown(KeyCode.Space);
        bool held = Input.GetKey(KeyCode.Space);
        bool up = Input.GetKeyUp(KeyCode.Space);

        if(down)
        {
            graphic.texture = downgfx;
        }
        else if(held)
        {
            graphic.texture = heldgfx;
        }
        else if(up)
        {
            graphic.texture = upgfx;
        }
        else
        {
            graphic.texture = standard;
        }

        guiText.text = " " + down + "\n " + held + "\n " + up;
    }
}
```

**ButtonInput**

```cs
using UnityEngine;
using System.Collections;

public class ButtonInput : MonoBehaviour
{
    public GUITexture graphic;
    public Texture2D standard;
    public Texture2D downgfx;
    public Texture2D upgfx;
    public Texture2D heldgfx;

    void Start()
    {
        graphic.texture = standard;
    }

    void Update]() ()
    {
        bool down = Input.GetButtonDown("Jump");
        bool held = Input.GetButton("Jump");
        bool up = Input.GetButtonUp("Jump");

        if(down)
        {
            graphic.texture = downgfx;
        }
        else if(held)
        {
            graphic.texture = heldgfx;
        }
        else if(up)
        {
            graphic.texture = upgfx;
        }
        else
        {
            graphic.texture = standard;
        }

        guiText.text = " " + down + "\n " + held + "\n " + up;
    }
}
```


## GetAxis

How to "get axis" based input for your games in Unity and how these axes can be modified with the Input manager

GetAxis和GetKey，GetButton大体相似，区别在于后两者总是返回boolean值，但GetAxis返回0~1之间的值。

且在Input Setting中，通过设置Gravity用来控制从+/- 1 回到0的速度。

至于手柄控制的时候有哪些细节，需要再看视频研究。

https://unity3d.com/learn/tutorials/topics/scripting/getaxis?playlist=17117

https://youtu.be/XZAxgH1dqXw

**AxisExample**

```cs
using UnityEngine;
using System.Collections;

public class AxisExample : MonoBehaviour
{
    public float range;
    public GUIText textOutput;

    void Update ()
    {
        float h = Input.GetAxis("Horizontal");
        float xPos = h * range;

        transform.position = new Vector3(xPos, 2f, 0);
        textOutput.text = "Value Returned: "+h.ToString("F2");
    }
}
```

**AxisRawExample**

```cs
using UnityEngine;
using System.Collections;

public class AxisRawExample : MonoBehaviour
{
    public float range;
    public GUIText textOutput;


    void Update ()
    {
        float h = Input.GetAxisRaw("Horizontal");
        float xPos = h * range;

        transform.position = new Vector3(xPos, 2f, 0);
        textOutput.text = "Value Returned: "+h.ToString("F2");
    }
}
```

**DualAxisExample**

```cs
using UnityEngine;
using System.Collections;

public class DualAxisExample : MonoBehaviour
{
    public float range;
    public GUIText textOutput;


    void Update ()
    {
        float h = Input.GetAxis("Horizontal");
        float v = Input.GetAxis("Vertical");
        float xPos = h * range;
        float yPos = v * range;

        transform.position = new Vector3(xPos, yPos, 0);
        textOutput.text = "Horizontal Value Returned: "+h.ToString("F2")+"\nVertical Value Returned: "+v.ToString("F2");    
    }
}
```


## OnMouseDown

How to detect mouse clicks on a Collider or GUI element.

https://unity3d.com/learn/tutorials/topics/scripting/onmousedown?playlist=17117

https://youtu.be/DgpHOTuUJ58

OnMouseDown以及其相关的一系列方法可以用于侦测Collider或者UI上的事件。

问题，如果Collider启用了IsTrigger有没有作用呢？

## 示例代码

```cs
using UnityEngine;
using System.Collections;

public class MouseClick : MonoBehaviour
{
    void OnMouseDown ()
    {
        rigidbody.AddForce(-transform.forward * 500f);
        rigidbody.useGravity = true;
    }
}
```