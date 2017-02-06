# 实例化

How to use Instantiate to create clones of a Prefab during runtime.

https://unity3d.com/learn/tutorials/topics/scripting/instantiate?playlist=17117

https://youtu.be/4rZAAHevq1s

`Instantiate`函数是用于创建游戏对象的Clone对象。常用于克隆Prefab，

需要注意的点：

1. 复制的对象
2. 复制对象所在的位置和朝向
3. 复制对象的类型 as type
4. 什么时候销毁新的克隆对象？

## 示例代码

UsingInstantiate

```cs
using UnityEngine;
using System.Collections;

public class UsingInstantiate : MonoBehaviour
{
    public Rigidbody rocketPrefab;
    public Transform barrelEnd;


    void Update ()
    {
        if(Input.GetButtonDown("Fire1"))
        {
            Rigidbody rocketInstance;
            rocketInstance = Instantiate(rocketPrefab, barrelEnd.position, barrelEnd.rotation) as Rigidbody;
            rocketInstance.AddForce(barrelEnd.forward * 5000);
        }
    }
}
```

RocketDestruction

```cs
using UnityEngine;
using System.Collections;

public class RocketDestruction : MonoBehaviour
{
    void Start()
    {
        Destroy (gameObject, 1.5f);
    }
}
```


## 相关文档

* [Instantiate](http://docs.unity3d.com/Documentation/ScriptReference/Object.Instantiate.html?_ga=1.219425104.838993178.1480250241) (Script Reference)
* [Prefabs](http://docs.unity3d.com/Documentation/Manual/Prefabs.html?_ga=1.219425104.838993178.1480250241) (Manual)