# 数据类型

Learn the important differences between Value and Reference data types, in order to better understand how variables work.

https://unity3d.com/learn/tutorials/topics/scripting/data-types?playlist=17117

https://youtu.be/6jMFsO6ldcw

## 示例代码

DatatypeScript

```cs
using UnityEngine;
using System.Collections;

public class DatatypeScript : MonoBehaviour
{
    void Start ()
    {
        //Value type variable
        Vector3 pos = transform.position;
        pos = new Vector3(0, 2, 0);

        //Reference type variable
        Transform tran = transform;
        tran.position = new Vector3(0, 2, 0);
    }
}
```

## 相关教程

* [Variables and Functions](https://unity3d.com/learn/tutorials/topics/scripting/variables-and-functions) (Lesson)