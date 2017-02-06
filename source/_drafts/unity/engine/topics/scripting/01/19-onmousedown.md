# OnMouseDown

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

## 相关文档

* [OnMouseDown](http://docs.unity3d.com/Documentation/ScriptReference/MonoBehaviour.OnMouseDown.html?_ga=1.184887904.838993178.1480250241) (Script Reference)
* [OnMouseDrag](http://docs.unity3d.com/Documentation/ScriptReference/MonoBehaviour.OnMouseDrag.html?_ga=1.184887904.838993178.1480250241) (Script Reference)
* [OnMouseEnter](http://docs.unity3d.com/Documentation/ScriptReference/MonoBehaviour.OnMouseEnter.html?_ga=1.184887904.838993178.1480250241) (Script Reference)
* [OnMouseExit](http://docs.unity3d.com/Documentation/ScriptReference/MonoBehaviour.OnMouseExit.html?_ga=1.184887904.838993178.1480250241) (Script Reference)
* [OnMouseOver](http://docs.unity3d.com/Documentation/ScriptReference/MonoBehaviour.OnMouseOver.html?_ga=1.184887904.838993178.1480250241) (Script Reference)
* [OnMouseUp](http://docs.unity3d.com/Documentation/ScriptReference/MonoBehaviour.OnMouseUp.html?_ga=1.184887904.838993178.1480250241) (Script Reference)
* [OnMouseUpAsButton](http://docs.unity3d.com/Documentation/ScriptReference/MonoBehaviour.OnMouseUpAsButton.html?_ga=1.184887904.838993178.1480250241) (Script Reference)