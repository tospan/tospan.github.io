# Delta Time

What is Delta Time and how can it be used in your games to smooth and interpret values.

https://unity3d.com/learn/tutorials/topics/scripting/delta-time?playlist=17117

https://youtu.be/a-w7w8x_moE

Delta的意思是两个值之间的差异

UsingDeltaTimes

```cs
using UnityEngine;
using System.Collections;

public class UsingDeltaTime : MonoBehaviour
{
    public float speed = 8f; 
    public float countdown = 3.0f;


    void Update ()
    {
        countdown -= Time.deltaTime;
        if(countdown <= 0.0f)
            light.enabled = true;

         if(Input.GetKey(KeyCode.RightArrow))
            transform.position += new Vector3(speed * Time.deltaTime, 0.0f, 0.0f);
    }
}
```

## 相关文档

* [Delta Time](http://docs.unity3d.com/Documentation/ScriptReference/Time-deltaTime.html?_ga=1.218827344.838993178.1480250241) (Script Reference)