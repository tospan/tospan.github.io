# 变量与函数

What are Variables and Functions, and how do they store and process information for us?

https://unity3d.com/learn/tutorials/topics/scripting/variables-and-functions?playlist=17117

https://youtu.be/WQnpeR7u7HM

```c
using UnityEngine;
using System.Collections;

public class VariablesAndFunctions : MonoBehaviour
{
    int myInt = 5;


    void Start ()
    {
        myInt = MultiplyByTwo(myInt);
        Debug.Log (myInt);
    }


    int MultiplyByTwo (int number)
    {
        int ret;
        ret = number * 2;
        return ret;
    }
}
```