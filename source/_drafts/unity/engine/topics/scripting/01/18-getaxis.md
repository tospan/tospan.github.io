# GetAxis

How to "get axis" based input for your games in Unity and how these axes can be modified with the Input manager

GetAxis和GetKey，GetButton大体相似，区别在于后两者总是返回boolean值，但GetAxis返回0~1之间的值。

且在Input Setting中，通过设置Gravity用来控制从+/- 1 回到0的速度。

至于手柄控制的时候有哪些细节，需要再看视频研究。

https://unity3d.com/learn/tutorials/topics/scripting/getaxis?playlist=17117

https://youtu.be/XZAxgH1dqXw

## 示例代码

AxisExample

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

AxisRawExample

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

DualAxisExample

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

## 相关文档

* [GetAxis](http://docs.unity3d.com/Documentation/ScriptReference/Input.GetAxis.html?_ga=1.146074109.838993178.1480250241) (Script Reference)
* [GetAxisRaw](http://docs.unity3d.com/Documentation/ScriptReference/Input.GetAxisRaw.html?_ga=1.146074109.838993178.1480250241) (Script Reference)