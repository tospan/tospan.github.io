# 移动和旋转

How to use the two transform functions Translate and Rotate to effect a non-rigidbody object's position and rotation.

https://unity3d.com/learn/tutorials/topics/scripting/translate-and-rotate?playlist=17117

https://youtu.be/Ywn7n0SXB4M

如何使用transform的两个方法Translate和Rotate来影响一个非Rigidbody对象的位置和朝向。

## 示例代码

```cs
using UnityEngine;
using System.Collections;

public class TransformFunctions : MonoBehaviour
{
    public float moveSpeed = 10f;
    public float turnSpeed = 50f;


    void Update ()
    {
        if(Input.GetKey(KeyCode.UpArrow))
            transform.Translate(Vector3.forward * moveSpeed * Time.deltaTime);

        if(Input.GetKey(KeyCode.DownArrow))
            transform.Translate(-Vector3.forward * moveSpeed * Time.deltaTime);

        if(Input.GetKey(KeyCode.LeftArrow))
            transform.Rotate(Vector3.up, -turnSpeed * Time.deltaTime);

        if(Input.GetKey(KeyCode.RightArrow))
            transform.Rotate(Vector3.up, turnSpeed * Time.deltaTime);
    }
}
```

## 相关教程

* [Adding Physics Forces](https://unity3d.com/learn/tutorials/topics/physics/adding-physics-forces) (Lesson)
* [Adding Physics Torque](https://unity3d.com/learn/tutorials/topics/physics/adding-physics-torque) (Lesson)

## 相关文档

* [Transform](http://docs.unity3d.com/Documentation/ScriptReference/Transform.html?_ga=1.108710219.838993178.1480250241) (Script Reference)
* [Translate](http://docs.unity3d.com/Documentation/ScriptReference/Transform.Translate.html?_ga=1.108710219.838993178.1480250241) (Script Reference)
* [Rotate](http://docs.unity3d.com/Documentation/ScriptReference/Transform.Rotate.html?_ga=1.108710219.838993178.1480250241) (Script Reference)
* [AddForce](http://docs.unity3d.com/Documentation/ScriptReference/Rigidbody.AddForce.html?_ga=1.108710219.838993178.1480250241) (Script Reference)
* [AddTorque](http://docs.unity3d.com/Documentation/ScriptReference/Rigidbody.AddTorque.html?_ga=1.108710219.838993178.1480250241) (Script Reference)