# 激活游戏对象

How to handle the active status of gameobjects in the scene, both independently and within Hierarchies, using SetActive and activeSelf / activeInHierarchy.

要在场景中激活或者禁用一个游戏对象，可以使用`SetActive`方法。

如果一个物体没有子对象，几乎就没有什么事儿了。

因为禁用有两种情况：

1. 物体本身是否被禁用 `activeSelf`
2. 物体在Hierarchy中是否被禁用 `activeInHierarchy`

例如一辆车有4个轮子作为子对象，如果我们将车禁用，车轮在场景中也看不到，但如果我们切换到车轮对象，会发现它本身还是启用的，但在Hierarchy中是禁用的。


## 示例代码

ActiveObjects

```cs
using UnityEngine;
using System.Collections;

public class ActiveObjects : MonoBehaviour
{
    void Start ()
    {
        gameObject.SetActive(false);
    }
}
```

CheckState

```cs
using UnityEngine;
using System.Collections;

public class CheckState : MonoBehaviour
{
    public GameObject myObject;


    void Start ()
    {
        Debug.Log("Active Self: " + myObject.activeSelf);
        Debug.Log("Active in Hierarchy" + myObject.activeInHierarchy);
    }
}
```

## 相关文档

* [Set Active](http://docs.unity3d.com/Documentation/ScriptReference/GameObject.SetActive.html?_ga=1.120728257.838993178.1480250241) (Script Reference)
* [Active Self](http://docs.unity3d.com/Documentation/ScriptReference/GameObject-activeSelf.html?_ga=1.120728257.838993178.1480250241) (Script Reference)
* [Active In Hierarchy](http://docs.unity3d.com/Documentation/ScriptReference/GameObject-activeInHierarchy.html?_ga=1.120728257.838993178.1480250241) (Script Reference)
* [Deactivating GameObjects](http://docs.unity3d.com/Documentation/Manual/DeactivatingGameObjects.html?_ga=1.120728257.838993178.1480250241) (Manual)