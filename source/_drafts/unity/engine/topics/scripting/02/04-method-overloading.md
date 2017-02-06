# 方法重载

How to overload methods to create different methods with the same name.

https://unity3d.com/learn/tutorials/topics/scripting/method-overloading?playlist=17117

https://youtu.be/SSTMu8QomkE

1:52

重载可以让我们对一个方法有多个定义，也就是说我们可以用同一个方法名称去做不同的事儿，例如，我们现在想把样东西加在一起，对于数字，我们可以使用AddNumbers，对于字符串我们可以使用AddStrings，那现在我们就有两个方法名称需要记住了，我们有更好的办法，只用Add，它可以执行数值相加或者字符串相加。

每个方法都有签名，签名是由方法名和参数列表组成，例如`Add(int num1, int num2)` 的签名是`Add(int,int)`，`Add(string str1, string str2)`对应的签名就是 `Add(string, string)`

在同一个域下方法签名必须是唯一的，而我们的重写方法就是使用同样的方法名，但使用不同的参数列表。

## 示例代码

Some Class

```cs
using UnityEngine;
using System.Collections;

public class SomeClass
{
    //The first Add method has a signature of
    //"Add(int, int)". This signature must be unique.
    public int Add(int num1, int num2)
    {
        return num1 + num2;
    }

    //The second Add method has a sugnature of
    //"Add(string, string)". Again, this must be unique.
    public string Add(string str1, string str2)
    {
        return str1 + str2;
    }
}
```

SomeOtherClass

```cs
using UnityEngine;
using System.Collections;

public class SomeOtherClass : MonoBehaviour
{
    void Start ()
    {
        SomeClass myClass = new SomeClass();

        //The specific Add method called will depend on
        //the arguments passed in.
        myClass.Add (1, 2);
        myClass.Add ("Hello ", "World");
    }
}

```


那么系统是怎么决定到底应该使用重载方法呢？以下三件事会发生其一：

1. 完全匹配参数列表，则对应的重载函数会运行
2. Least Conversion，如果没有完全匹配的参数列表，系统会查找所有可能的匹配，需要适度类型转换可以执行的方法。 will requrie the least amount of conversion，
3. Error，如果没有完全匹配的或者 multiple versions taht require the same amount of conversion，就会报错。


## 相关教程

* [Variables and Functions](https://unity3d.com/learn/tutorials/topics/scripting/variables-and-functions) (Lesson)