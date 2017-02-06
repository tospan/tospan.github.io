# 属性

How to create properties to access the member variables (fields) in a class.

https://unity3d.com/learn/tutorials/topics/scripting/properties?playlist=17117
https://youtu.be/BYwCXJ-J9dA

3:11

获取某个类的成员变量(我们通常也称为字段)是常见的需求，比较有效的方法就是我们将成员变量的访问修饰符设置为public，但此外我们还有更好的方法，就是使用属性，属性就像方法，它封装了成员变量并给我们很大的便利什么时候以及如何访问。

假设，现在我们有个变量`experience`，我们用它表示经验值，那么它的属性可以这样写，可以看到属性的定义流程是这样的，先给出`public`的访问修饰符，然后给出类型，名称，对于名称比较好的惯例就是将首字母大写，其他保持一致，然后是像函数一样的大括号，里面是属性访问器？？ get， set，用于设置和获取对应的字段。完整的写法如下：

```cs
using UnityEngine;
using System.Collections;

public class Player:MonoBehaviour{

    private int experience;

    public int Experience{
        set{ experience = value; }
        get{ return experience; }
    }

}
```

这里多了一个值 `value` 这是需要特别注意的。这样就算是一个很正常的属性的定义方法了。在其他代码中我们就可以获取或设置属性了。

那么问题是：既然我能直接通过设置成员变量就可以完成的任务，为什么还要使用属性呢？

主要有两个功能是成员变量不具备的：

1. 通过忽略set或get，我们就可以实现该属性是只读还是只写？
2. setter和getter和函数一样，这意味着我们在读取或者设置的时候可以有更多的空间和可能性。我们可以在里面添加代码或调用其他函数，经典的就是设置属性的时候调用coroutine。

使用属性封装不一定需要直接对应某个成员变量，比如，这里我们可以定义一个`Level`属性，只要用户的experience值达到1000，`Level`值就升一级，那么我们的代码可以如下：

```cs
using UnityEngine;
using System.Collections;

public class Player:MonoBehaviour{

    private int experience;

    public int Experience{
        set{ experience = value; }
        get{ return experience; }
    }

    public int Level{
        set { experience = value * 1000; }
        get { return experience / 1000; }
    }
}

```

这里主要是在setter上可能有些疑惑，其实它的功能是通过设置玩家Level，从而计算Player的经验值。


## 自动实现

我们可以通过简写的语法来实现，就是在set或get之后直接加上分号，这样的属性和普通的字段基本功能相似，区别就是我们可以通过只留下set或get从而实现只读或只写。

```cs
using UnityEngine;
using System.Collections;

public class Player: MonoBehaviour{

    public int Health{
        set; get;
    }
}
```

问题：如果我的代码如下，设置Health属性值的时候，会修改health值吗？

```cs
using UnityEngine;
using System.Collections;

public class Player: MonoBehaviour{

    private int health;

    public int Health{
        set; get;
    }
}
```

答案是不会，它们之间没有任何关系。

## 相关教程

* [Classes](https://unity3d.com/learn/tutorials/topics/scripting/classes) (Lesson)