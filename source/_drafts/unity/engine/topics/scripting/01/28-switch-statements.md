# switch语句

Switch statements act like streamline conditionals. They are useful for when you want to compare a single variable against a series of constants. In this video you will learn how to write and use switch statements.

https://unity3d.com/learn/tutorials/topics/scripting/switch-statements?playlist=17117

https://youtu.be/JwhWiLlNz3U

在做条件判断时，常用的是if条件语句，或者几个if else，类似的，我们也可以使用switch语句，它相对而言比较单一，主要用于单个值与多个常量比较，经常配合枚举来使用

## 示例代码

ConversationScript

```cs
using UnityEngine;
using System.Collections;

public class ConversationScript : MonoBehaviour
{
    public int intelligence = 5;


    void Greet()
    {
        switch (intelligence)
        {
            case 5:
                print ("Why hello there good sir! Let me teach you about Trigonometry!");
                break;
            case 4:
                print ("Hello and good day!");
                break;
            case 3:
                print ("Whadya want?");
                break;
            case 2:
                print ("Grog SMASH!");
                break;
            case 1:
                print ("Ulg, glib, Pblblblblb");
                break;
            default:
                print ("Incorrect intelligence level.");
                break;
        }
    }
}
```

## 相关教程

* [Enumerations](https://unity3d.com/learn/tutorials/topics/scripting/enumerations) (Lesson)