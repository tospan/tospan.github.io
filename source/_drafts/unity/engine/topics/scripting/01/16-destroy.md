# 销毁

如何在游戏运行时使用`Destroy()`函数移除GameObject和Components。

https://unity3d.com/learn/tutorials/topics/scripting/destroy?playlist=17117

https://youtu.be/QxM0CfL3jQ8


`Destroy()` 可以移除GameObejct或者组件，它还有可选参数作为延迟时间，也就是等待多久后执行销毁操作。

## 示例代码


DestroyBasic

```cs
using UnityEngine;
using System.Collections;

public class DestroyBasic : MonoBehaviour
{
    void Update ()
    {
        if(Input.GetKey(KeyCode.Space))
        {
            Destroy(gameObject);
        }
    }
}
```


DestroyOther

```cs
using UnityEngine;
using System.Collections;

public class DestroyOther : MonoBehaviour
{
    public GameObject other;


    void Update ()
    {
        if(Input.GetKey(KeyCode.Space))
        {
            Destroy(other);
        }
    }
}
```

DestroyComponent

```cs
using UnityEngine;
using System.Collections;

public class DestroyComponent : MonoBehaviour
{
    void Update ()
    {
        if(Input.GetKey(KeyCode.Space))
        {
            Destroy(GetComponent<MeshRenderer>());
        }
    }
}
```

## 相关文档

* [Destroy](http://docs.unity3d.com/Documentation/ScriptReference/Object.Destroy.html?_ga=1.118112576.838993178.1480250241) (Script Reference)