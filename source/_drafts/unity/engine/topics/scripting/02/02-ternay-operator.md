# 三元运算符

How to utilize the Ternary Operator to build simple, shorthand IF-ELSE logical conditions.

https://unity3d.com/learn/tutorials/topics/scripting/ternary-operator?playlist=17117

https://youtu.be/ALXYkk34bq8

1:03

```cs
using UnityEngine;
using System.Collections;

public class TernaryOperator : MonoBehaviour
{
    void Start ()
    {
        int health = 10;
        string message;

        //This is an example Ternary Operation that chooses a message based
        //on the variable "health".
        message = health > 0 ? "Player is Alive" : "Player is Dead";
    }
}
```

三元运算符的结构就是，condition ? [true return value] : [false return value]，根据条件判断最终返回一个值，当然，我们可以嵌套使用，例如

```cs
message = health > 0 ? "Player is Alive" : health == 0 ? "Player is Barely Alive": "Player is Dead";
```

不过这样代码的可读性就不是很好，最佳的实践方式是在一个三元运算符里不嵌套其他三元运算符。