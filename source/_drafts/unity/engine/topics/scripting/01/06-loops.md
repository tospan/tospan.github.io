# 循环

How to use the For, While, Do-While, and For Each Loops to repeat actions in code.

https://unity3d.com/learn/tutorials/topics/scripting/loops?playlist=17117

https://youtu.be/NiLqfxSw5WA

## while

```c
using UnityEngine;
using System.Collections;

public class WhileLoop : MonoBehaviour
{
    int cupsInTheSink = 4;
    
    
    void Start ()
    {
        while(cupsInTheSink > 0)
        {
            Debug.Log ("I've washed a cup!");
            cupsInTheSink--;
        }
    }
}
```

## do...while

```c
using UnityEngine;
using System.Collections;

public class DoWhileLoop : MonoBehaviour 
{
    void Start()
    {
        bool shouldContinue = false;
        
        do
        {
            print ("Hello World");
            
        }while(shouldContinue == true);
    }
}
```

## for

```cs
using UnityEngine;
using System.Collections;

public class ForLoop : MonoBehaviour
{
    int numEnemies = 3;

    void Start ()
    {
        for(int i = 0; i < numEnemies; i++)
        {
            Debug.Log("Creating enemy number: " + i);
        }
    }
}
```

## foreach

```cs
using UnityEngine;
using System.Collections;

public class ForeachLoop : MonoBehaviour
{
    void Start ()
    {
        string[] strings = new string[3];

        strings[0] = "First string";
        strings[1] = "Second string";
        strings[2] = "Third string";

        foreach(string item in strings)
        {
            print (item);
        }
    }
}
```

