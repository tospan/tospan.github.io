# 数组

Using arrays to collect variables together into a more manageable form.

https://unity3d.com/learn/tutorials/topics/scripting/arrays?playlist=17117

https://youtu.be/GqLNQY1052A

## 示例代码

Arrays

```cs
using UnityEngine;
using System.Collections;

public class Arrays : MonoBehaviour
{
    public GameObject[] players;

    void Start ()
    {
        players = GameObject.FindGameObjectsWithTag("Player");

        for(int i = 0; i < players.Length; i++)
        {
            Debug.Log("Player Number "+i+" is named "+players[i].name);
        }
    }
}
```

## 相关文档

* [Array](http://docs.unity3d.com/Documentation/ScriptReference/Array.html?_ga=1.185396576.838993178.1480250241) (Script Reference)