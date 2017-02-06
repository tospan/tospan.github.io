# 泛型

https://unity3d.com/learn/tutorials/topics/scripting/generics?playlist=17117

https://youtu.be/BOC-Z3WEbsg

4:08

## 示例代码

SomeClass

```cs
using UnityEngine;
using System.Collections;

public class SomeClass
{
    //Here is a generic method. Notice the generic
    //type 'T'. This 'T' will be replaced at runtime
    //with an actual type. 
    public T GenericMethod<T>(T param)
    {
        return param;
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

        //In order to use this method you must
        //tell the method what type to replace
        //'T' with.
        myClass.GenericMethod<int>(5);
    }
}
```

GenericClass

```cs
using UnityEngine;
using System.Collections;

//Here is a generic class. Notice the generic type 'T'.
//'T' will be replaced with an actual type, as will also 
//instances of the type 'T' used in the class.
public class GenericClass <T>
{
    T item;

    public void UpdateItem(T newItem)
    {
        item = newItem;
    }
}
```

GenericClassExample

```cs
using UnityEngine;
using System.Collections;

public class GenericClassExample : MonoBehaviour 
{
    void Start ()
    {
        //In order to create an object of a generic class, you must
        //specify the type you want the class to have.
        GenericClass<int> myClass = new GenericClass<int>();

        myClass.UpdateItem(5);
    }
}
```


## 相关教程

* [Lists and Dictionaries](https://unity3d.com/learn/tutorials/modules/intermediate/scripting/lists-and-dictionaries) (Lesson)
* [Interfaces](https://unity3d.com/learn/tutorials/topics/scripting/interfaces) (Lesson)