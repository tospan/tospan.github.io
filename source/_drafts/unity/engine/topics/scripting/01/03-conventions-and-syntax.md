# 语法

https://unity3d.com/learn/tutorials/topics/scripting/conventions-and-syntax?playlist=17117

https://youtu.be/xtvPpR0YdQs

## 点

就像一个地址。

## 分号



## 缩进

```c

using UnityEngine;
using System.Collections;

public class BasicSyntax : MonoBehaviour
{
    void Start ()
    {
        Debug.Log(transform.position.x);
        
        if(transform.position.y <= 5f)
        {
            Debug.Log ("I'm about to hit the ground!");
        }
    }
}
```