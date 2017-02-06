# 四元数

How to utilize the quaternion system to manage the rotation of game objects.

https://unity3d.com/learn/tutorials/topics/scripting/quaternions?playlist=17117
https://youtu.be/TdjoQB43EsQ

5:10

```cs
using UnityEngine;
using System.Collections;

public class MotionScript : MonoBehaviour 
{   
    public float speed = 3f;
    
    
    void Update () 
    {
        transform.Translate(-Input.GetAxis("Horizontal") * speed * Time.deltaTime, 0, 0);
    }
}
```

```cs
using UnityEngine;
using System.Collections;

public class LookAtScript : MonoBehaviour 
{
    public Transform target;
    
    
    void Update () 
    {
        Vector3 relativePos = target.position - transform.position;
        transform.rotation = Quaternion.LookRotation(relativePos);
    }
}
```

```cs
using UnityEngine;
using System.Collections;

public class GravityScript : MonoBehaviour 
{
    public Transform target;
    
    
    void Update () 
    {
        Vector3 relativePos = (target.position + new Vector3(0, 1.5f, 0)) - transform.position;
        Quaternion rotation = Quaternion.LookRotation(relativePos);
        
        Quaternion current = transform.localRotation;
        
        transform.localRotation = Quaternion.Slerp(current, rotation, Time.deltaTime);
        transform.Translate(0, 0, 3 * Time.deltaTime);
    }
}
```

```cs
using UnityEngine;
using System.Collections;

public class SomeClass : MonoBehaviour 
{
    void Start () 
    {
        transform.rotation = Quaternion.identity;
    }
}
```
