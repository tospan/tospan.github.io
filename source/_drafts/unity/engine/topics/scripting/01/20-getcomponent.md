# GetComponent

How to use the GetComponent function to address properties of other scripts or components.

https://unity3d.com/learn/tutorials/topics/scripting/getcomponent?playlist=17117

https://youtu.be/PBqTrK3z_KM

比较消耗资源，所以尽量在Awake或Start方法中调用一次，然后保存引用以方便调用而不是频繁获取。

## 示例代码

UsingOtherComponents

```cs
using UnityEngine;
using System.Collections;

public class UsingOtherComponents : MonoBehaviour
{
    public GameObject otherGameObject;


    private AnotherScript anotherScript;
    private YetAnotherScript yetAnotherScript;
    private BoxCollider boxCol;


    void Awake ()
    {
        anotherScript = GetComponent<AnotherScript>();
        yetAnotherScript = otherGameObject.GetComponent<YetAnotherScript>();
        boxCol = otherGameObject.GetComponent<BoxCollider>();
    }


    void Start ()
    {
        boxCol.size = new Vector3(3,3,3);
        Debug.Log("The player's score is " + anotherScript.playerScore);
        Debug.Log("The player has died " + yetAnotherScript.numberOfPlayerDeaths + " times");
    }
}
```

AnotherScript

```cs
using UnityEngine;
using System.Collections;

public class AnotherScript : MonoBehaviour
{
    public int playerScore = 9001;
}
```

YetAnotherScript

```cs
using UnityEngine;
using System.Collections;

public class YetAnotherScript : MonoBehaviour
{
    public int numberOfPlayerDeaths = 3;
}
```

## 相关教程

* [Enabling and Disabling Components](https://unity3d.com/learn/tutorials/topics/scripting/enabling-and-disabling-components) (Lesson)
* [Classes](https://unity3d.com/learn/tutorials/topics/scripting/classes) (Lesson)

## 相关文档

* [GetComponent](http://docs.unity3d.com/Documentation/ScriptReference/Component.GetComponent.html?_ga=1.117778752.838993178.1480250241) (Script Reference)