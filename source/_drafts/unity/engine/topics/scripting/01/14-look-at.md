# 看哪儿？！

How to make a game object's transform face another's by using the LookAt function.

https://unity3d.com/learn/tutorials/topics/scripting/look?playlist=17117

https://youtu.be/NDC9qafprwE

`transform`的`LookAt`方法可以用来让一个对象的面朝向游戏世界中另外一个物体transform。下面的案例可以附加到相机上，然后给target赋值，移动target，则会发现相机在`Local`模式下Z轴正方向一直朝向target

## 示例代码

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

## 相关文档

* [LookAt](http://docs.unity3d.com/Documentation/ScriptReference/Transform.LookAt.html?_ga=1.114100686.838993178.1480250241) (Script Reference)