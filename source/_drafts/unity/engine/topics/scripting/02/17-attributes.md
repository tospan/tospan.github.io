# 属性(attributes)

Attributes allow you to attach additional behavior to the methods and variables you create. In this video you will learn the format of attributes as well as how to use the "Range" and "ExecuteInEditMode" attributes.

https://unity3d.com/learn/tutorials/topics/scripting/attributes?playlist=17117
https://youtu.be/WG_zoE5sZE4

4:17

属性可以让我们附加到函数、变量或类


## 示例代码

SpinScript

```cs
using UnityEngine;
using System.Collections;

public class SpinScript : MonoBehaviour
{
    [Range(-100, 100)] public int speed = 0;

    void Update ()
    {
        transform.Rotate(new Vector3(0, speed * Time.deltaTime, 0));
    }
}
```


ColorScript

```cs
using UnityEngine;
using System.Collections;

[ExecuteInEditMode]
public class ColorScript : MonoBehaviour
{
    void Start()
    {
        renderer.sharedMaterial.color = Color.red;
    }
}
```

## 相关文档

* [ExecuteInEditMode Attribute Script Reference](http://docs.unity3d.com/412/Documentation/ScriptReference/ExecuteInEditMode.html?_ga=1.213584221.838993178.1480250241)