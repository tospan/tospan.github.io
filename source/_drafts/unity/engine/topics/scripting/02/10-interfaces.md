# 接口

How to create interfaces and implement them in classes.

https://unity3d.com/learn/tutorials/topics/scripting/interfaces?playlist=17117

https://youtu.be/sQfS4w0wvcc

3:55

Interfaces

```cs
using UnityEngine;
using System.Collections;

//This is a basic interface with a single required
//method.
public interface IKillable
{
    void Kill();
}

//This is a generic interface where T is a placeholder
//for a data type that will be provided by the 
//implementing class.
public interface IDamageable<T>
{
    void Damage(T damageTaken);
}
```

Avatar Class

```cs
using UnityEngine;
using System.Collections;

public class Avatar : MonoBehaviour, IKillable, IDamageable<float>
{
    //The required method of the IKillable interface
    public void Kill()
    {
        //Do something fun
    }
    
    //The required method of the IDamageable interface
    public void Damage(float damageTaken)
    {
        //Do something fun
    }
}
```

相关教程

* [Classes](https://unity3d.com/learn/tutorials/topics/scripting/classes) (Lesson)
* [Polymorphism](https://unity3d.com/learn/tutorials/topics/scripting/polymorphism) (Lesson)
* [Overriding](https://unity3d.com/learn/tutorials/topics/scripting/overriding) (Lesson)
* [Generics](https://unity3d.com/learn/tutorials/topics/scripting/generics) (Lesson)