# 启用或禁用组件

How to enable and disable components via script during runtime.

https://unity3d.com/learn/tutorials/topics/scripting/enabling-and-disabling-components?playlist=17117


首先要获取组件，然后组件的属性 `enabled`

## 示例代码

```cs
using UnityEngine;
using System.Collections;

public class EnableComponents : MonoBehaviour
{
    private Light myLight;


    void Start ()
    {
        myLight = GetComponent<Light>();
    }


    void Update ()
    {
        if(Input.GetKeyUp(KeyCode.Space))
        {
            myLight.enabled = !myLight.enabled;
        }
    }
}
```

## 相关教程

* [GetComponent](https://unity3d.com/learn/tutorials/topics/scripting/getcomponent) (Lesson)

## 相关文档

* [Get Component](http://docs.unity3d.com/Documentation/ScriptReference/Component.GetComponent.html?_ga=1.154920561.838993178.1480250241) (Script Reference)

