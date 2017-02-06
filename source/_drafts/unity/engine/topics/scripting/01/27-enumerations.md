# 枚举

Enumerations allow you to create a collection of related constants. In this video you will learn how to declare and use enumerations in your code.

https://unity3d.com/learn/tutorials/topics/scripting/enumerations?playlist=17117

https://youtu.be/DG-QjqSDR6s

## 示例代码

```cs
using UnityEngine;
using System.Collections;

public class EnumScript : MonoBehaviour 
{
    enum Direction {North, East, South, West};

    void Start ()
    {
        Direction myDirection;

        myDirection = Direction.North;
    }

    Direction ReverseDirection (Direction dir)
    {
        if(dir == Direction.North)
            dir = Direction.South;
        else if(dir == Direction.South)
            dir = Direction.North;
        else if(dir == Direction.East)
            dir = Direction.West;
        else if(dir == Direction.West)
            dir = Direction.East;

        return dir;
    }
}
```

## 相关文档

* [Integral Types](http://msdn.microsoft.com/en-us/library/exx3b86w.aspx)