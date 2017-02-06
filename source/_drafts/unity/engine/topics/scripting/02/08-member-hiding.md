# Member Hiding

How to implement the hiding of base members in derived classes.

https://unity3d.com/learn/tutorials/topics/scripting/member-hiding?playlist=17117

https://youtu.be/iW5rrLWEggc

1:51

```cs
using UnityEngine;
using System.Collections;

public class Humanoid
{
    //Base version of the Yell method
    public void Yell()
    {
        Debug.Log ("Humanoid version of the Yell() method");
    }
}
```


```cs
using UnityEngine;
using System.Collections;

public class Enemy : Humanoid
{
    //This hides the Humanoid version.
    new public void Yell()
    {
        Debug.Log ("Enemy version of the Yell() method");
    }
}
```

```cs
using UnityEngine;
using System.Collections;

public class Orc : Enemy
{
    //This hides the Enemy version.
    new public void Yell()
    {
        Debug.Log ("Orc version of the Yell() method");
    }
}
```

```cs
using UnityEngine;
using System.Collections;

public class WarBand : MonoBehaviour 
{
    void Start () 
    {
        Humanoid human = new Humanoid();
        Humanoid enemy = new Enemy();
        Humanoid orc = new Orc();
        
        //Notice how each Humanoid variable contains
        //a reference to a different class in the
        //inheritance hierarchy, yet each of them
        //calls the Humanoid Yell() method.
        human.Yell();
        enemy.Yell();
        orc.Yell();
    }
}
```

## 相关教程

* [Overriding](https://unity3d.com/learn/tutorials/topics/scripting/overriding) (Lesson)
* [Classes](https://unity3d.com/learn/tutorials/topics/scripting/classes) (Lesson)
* [Inheritance](https://unity3d.com/learn/tutorials/topics/scripting/inheritance) (Lesson)