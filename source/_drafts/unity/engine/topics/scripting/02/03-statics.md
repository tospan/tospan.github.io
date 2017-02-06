# 静态

How to create static variables, methods, and classes.

如何创建静态变量，方法和类？

https://unity3d.com/learn/tutorials/topics/scripting/statics?playlist=17117

https://youtu.be/nraOAaYLdRQ

3:18


静态变量或函数在所有类的示例中都可以访问，也就是说它们可以直接获取和调用而不用实例化它们所在的类。

通常情况下，每个类的成员变量都是相互独立的，虽然某个类的不同实例有都有相同的成员变量，但是它们的值仅仅属于该实例而不是共享的。但静态变量，就是由相同的成员变量有统一且唯一的值，也就是说，在一个地方修改了值，其他地方都会改变。


举例来说，我们想知道在游戏过程中，我们到底实例化了多少个敌人。我们在Enemy类中创建一个静态变量，那么就意味着该变量仅属于该类，而不是任何该类的实例。然后在实例化该类的时候递增该变量。

Enemy

```cs
using UnityEngine;
using System.Collections;

public class Enemy
{
    //Static variables are shared across all instances
    //of a class.
    public static int enemyCount = 0;

    public Enemy()
    {
        //Increment the static variable to know how many
        //objects of this class have been created.
        enemyCount++;
    }
}
```

获取到底实例化了多少个Enemy，直接调用 `Enemy.enemyCount`即可

Game

```cs
using UnityEngine;
using System.Collections;

public class Game
{
    void Start ()
    {
        Enemy enemy1 = new Enemy();
        Enemy enemy2 = new Enemy();
        Enemy enemy3 = new Enemy();

        //You can access a static variable by using the class name
        //and the dot operator.
        int x = Enemy.enemyCount;
    }
}
```

静态的这种特性对于Unity中的GameObject组件同样有效，例如我们想知道到在场景中创建了多少个Player

Player

```cs
using UnityEngine;
using System.Collections;

public class Player : MonoBehaviour
{
    //Static variables are shared across all instances
    //of a class.
    public static int playerCount = 0;

    void Start()
    {
        //Increment the static variable to know how many
        //objects of this class have been created.
        playerCount++;
    }
}
```


PlayerManager

```cs
using UnityEngine;
using System.Collections;

public class PlayerManager : MonoBehaviour
{
    void Start()
    {
        //You can access a static variable by using the class name
        //and the dot operator.
        int x = Player.playerCount;
    }
}
```


## 静态方法

和静态变量一样，静态方法也是仅仅属于当期类而不属于它的实例，我们可以直接调用而不用实例化该类。

Utilities

```cs
using UnityEngine;
using System.Collections;

public class Utilities
{
    //A static method can be invoked without an object
    //of a class. Note that static methods cannot access
    //non-static member variables.
    public static int Add(int num1, int num2)
    {
        return num1 + num2;
    }
}
```

UtilitiesExample

```cs
using UnityEngine;
using System.Collections;

public class UtilitiesExample : MonoBehaviour
{
    void Start()
    {
        //You can access a static method by using the class name
        //and the dot operator.
        int x = Utilities.Add (5, 6);
    }
}
```

其实我们已经使用过多次静态变量，和静态方法了，例如  `Input.GetAxis`等等，为什么知道它们是静态的呢？原因就是我们从来没有实例化过它们。事实上Unity有很静态变量和方法提供了相当丰富的内容和特性供我们调用。

> 需要注意的是，我们不能在静态函数中使用非静态的变量和非静态的方法，为什么？
> 因为静态方法和静态变量属于类，而非静态变量和方法属于类的实例。


## 静态类

我们可以在class前面加上static修饰符，该类是不可以被实例化的。它适用于我们期望类中的所有变量和函数都是静态的，例如Input Class。

Utilities

```cs
using UnityEngine;
using System.Collections;

public static class Utilities
{
    //A static method can be invoked without an object
    //of a class. Note that static methods cannot access
    //non-static member variables.
    public static int Add(int num1, int num2)
    {
        return num1 + num2;
    }
}
```

> 注意：在类前面加上了static关键字之后，静态变量和方法同样需要加上static关键字。