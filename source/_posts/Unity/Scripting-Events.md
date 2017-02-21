---
title: Events
categories:
  - Unity Scripting
tags:
  - default
date: 2017-02-10 14:30:36
updated: 2017-02-10 14:30:36
---

## 事件

https://unity3d.com/learn/tutorials/topics/scripting/events?playlist=17117
https://youtu.be/6qyR73EO68w

6:36

How to create a dynamic "broadcast" system using Events.

事件是一种特殊的委托（delegate），可以通知其他类某个特定的状况发生了。


事件可以看做是一个广播系统，任何对该事件感兴趣的类可以通过方法订阅它，那么在某种情况发生了，例如点击了按钮，玩家掉血，我们会调用事件，与此同时会调用所有订阅该事件的方法。


关于OnEnable和OnDisable

### 示例代码

EventManager

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

TeleportScript

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

TurnColorScript

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

