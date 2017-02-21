
## 矢量计算

https://unity3d.com/learn/tutorials/topics/scripting/vector-maths?playlist=17117
https://youtu.be/7DK8aA2qee8

### 2D Vector

### 3D Vector

### Dot

### Cross Product

## 启用或禁用组件

How to enable and disable components via script during runtime.

https://unity3d.com/learn/tutorials/topics/scripting/enabling-and-disabling-components?playlist=17117

首先要获取组件，然后组件的属性 `enabled`

### 示例代码

```cs
using UnityEngine;
using System.Collections;

public class EnableComponents : MonoBehaviour
{
    private Light myLight;


    void Start ()
    {
        myLight = GetComponent<Light>();
    }


    void Update ()
    {
        if(Input.GetKeyUp(KeyCode.Space))
        {
            myLight.enabled = !myLight.enabled;
        }
    }
}
```

## activating gameobjects激活游戏对象

How to handle the active status of gameobjects in the scene, both independently and within Hierarchies, using SetActive and activeSelf / activeInHierarchy.

要在场景中激活或者禁用一个游戏对象，可以使用`SetActive`方法。

如果一个物体没有子对象，几乎就没有什么事儿了。

因为禁用有两种情况：

1. 物体本身是否被禁用 `activeSelf`
2. 物体在Hierarchy中是否被禁用 `activeInHierarchy`

例如一辆车有4个轮子作为子对象，如果我们将车禁用，车轮在场景中也看不到，但如果我们切换到车轮对象，会发现它本身还是启用的，但在Hierarchy中是禁用的。

**ActiveObjects**

```cs
using UnityEngine;
using System.Collections;

public class ActiveObjects : MonoBehaviour
{
    void Start ()
    {
        gameObject.SetActive(false);
    }
}
```

**CheckState**

```cs
using UnityEngine;
using System.Collections;

public class CheckState : MonoBehaviour
{
    public GameObject myObject;


    void Start ()
    {
        Debug.Log("Active Self: " + myObject.activeSelf);
        Debug.Log("Active in Hierarchy" + myObject.activeInHierarchy);
    }
}
```

## translate and rotate 移动和旋转

How to use the two transform functions Translate and Rotate to effect a non-rigidbody object's position and rotation.

https://unity3d.com/learn/tutorials/topics/scripting/translate-and-rotate?playlist=17117

https://youtu.be/Ywn7n0SXB4M

如何使用transform的两个方法Translate和Rotate来影响一个非Rigidbody对象的位置和朝向。

### 示例代码

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




## Loot at 看哪儿？！

How to make a game object's transform face another's by using the LookAt function.

https://unity3d.com/learn/tutorials/topics/scripting/look?playlist=17117

https://youtu.be/NDC9qafprwE

`transform`的`LookAt`方法可以用来让一个对象的面朝向游戏世界中另外一个物体transform。下面的案例可以附加到相机上，然后给target赋值，移动target，则会发现相机在`Local`模式下Z轴正方向一直朝向target

```cs
using UnityEngine;
using System.Collections;

public class CameraLookAt : MonoBehaviour
{
    public Transform target;

    void Update ()
    {
        transform.LookAt(target);
    }
}
```

##  Linear interpolation 线性插补

https://unity3d.com/learn/tutorials/topics/scripting/linear-interpolation?playlist=17117


When making games it can sometimes be useful to linearly interpolate between two values. This is done with a function called Lerp. Linearly interpolating is finding a value that is some percentage between two given values. For example, we could linearly interpolate between the numbers 3 and 5 by 50% to get the number 4. This is because 4 is 50% of the way between 3 and 5.

In Unity there are several Lerp functions that can be used for different types. For the example we have just used, the equivalent would be the Mathf.Lerp function and would look like this:

```cs
// In this case, result = 4
float result = Mathf.Lerp (3f, 5f, 0.5f);
```

The Mathf.Lerp function takes 3 float parameters: one representing the value to interpolate from; another representing the value to interpolate to and a final float representing how far to interpolate. In this case, the interpolation value is 0.5 which means 50%. If it was 0, the function would return the ‘from’ value and if it was 1 the function would return the ‘to’ value.

Other examples of Lerp functions include Color.Lerp and Vector3.Lerp. These work in exactly the same way as Mathf.Lerp but the ‘from’ and ‘to’ values are of type Color and Vector3 respectively. The third parameter in each case is still a float representing how much to interpolate. The result of these functions is finding a colour that is some blend of two given colours and a vector that is some percentage of the way between the two given vectors.

Let’s look at another example:

```cs
Vector3 from = new Vector3 (1f, 2f, 3f);
Vector3 to = new Vector3 (5f, 6f, 7f);

// Here result = (4, 5, 6)
Vector3 result = Vector3.Lerp (from, to, 0.75f);
```

In this case the result is (4, 5, 6) because 4 is 75% of the way between 1 and 5; 5 is 75% of the way between 2 and 6 and 6 is 75% of the way between 3 and 7.

The same principle is applied when using Color.Lerp. In the Color struct, colours are represented by 4 floats representing red, blue, green and alpha. When using Lerp, these floats are interpolated just as with Mathf.Lerp and Vector3.Lerp.

Under some circumstances Lerp functions can be used to smooth a value over time. Consider the following piece of code:

```cs
void Update ()
{
    light.intensity = Mathf.Lerp(light.intensity, 8f, 0.5f);
}
```

If the intensity of the light starts off at 0 then after the first update it will be set to 4. The next frame it will be set to 6, then to 7, then to 7.5 and so on. Thus over several frames, the lights intensity will tend towards 8 but the rate of it’s change will slow as it approaches its target. Note that this happens over the course of several frames. If we wanted this to not be frame rate dependent then we could use the following code:

```cs
void Update ()
{
    light.intensity = Mathf.Lerp(light.intensity, 8f, 0.5f * Time.deltaTime);
}
```

This would mean the change to intensity would happen per second instead of per frame.

Please note that when smoothing a value it is often best to use the SmoothDamp function. Only use Lerp for smoothing if you are sure of the effect you want.


## Destroy 销毁

如何在游戏运行时使用`Destroy()`函数移除GameObject和Components。

https://unity3d.com/learn/tutorials/topics/scripting/destroy?playlist=17117

https://youtu.be/QxM0CfL3jQ8


`Destroy()` 可以移除GameObejct或者组件，它还有可选参数作为延迟时间，也就是等待多久后执行销毁操作。

**DestroyBasic**

```cs
using UnityEngine;
using System.Collections;

public class DestroyBasic : MonoBehaviour
{
    void Update ()
    {
        if(Input.GetKey(KeyCode.Space))
        {
            Destroy(gameObject);
        }
    }
}
```

**DestroyOther**

```cs
using UnityEngine;
using System.Collections;

public class DestroyOther : MonoBehaviour
{
    public GameObject other;


    void Update ()
    {
        if(Input.GetKey(KeyCode.Space))
        {
            Destroy(other);
        }
    }
}
```

**DestroyComponent**

```cs
using UnityEngine;
using System.Collections;

public class DestroyComponent : MonoBehaviour
{
    void Update ()
    {
        if(Input.GetKey(KeyCode.Space))
        {
            Destroy(GetComponent<MeshRenderer>());
        }
    }
}
```

## GetButton and GetKey 获取按钮或键盘按键事件

https://unity3d.com/learn/tutorials/topics/scripting/getbutton-and-getkey?playlist=17117

https://youtu.be/qf3Oo3BW83Y

How to get button or key for input and how these axes behave / can be modified with the Input manager

**KeyInput**

```cs
using UnityEngine;
using System.Collections;

public class KeyInput : MonoBehaviour
{
    public GUITexture graphic;
    public Texture2D standard;
    public Texture2D downgfx;
    public Texture2D upgfx;
    public Texture2D heldgfx;

    void Start()
    {
        graphic.texture = standard;
    }

    void Update]() ()
    {
        bool down = Input.GetKeyDown(KeyCode.Space);
        bool held = Input.GetKey(KeyCode.Space);
        bool up = Input.GetKeyUp(KeyCode.Space);

        if(down)
        {
            graphic.texture = downgfx;
        }
        else if(held)
        {
            graphic.texture = heldgfx;
        }
        else if(up)
        {
            graphic.texture = upgfx;
        }
        else
        {
            graphic.texture = standard;
        }

        guiText.text = " " + down + "\n " + held + "\n " + up;
    }
}
```

**ButtonInput**

```cs
using UnityEngine;
using System.Collections;

public class ButtonInput : MonoBehaviour
{
    public GUITexture graphic;
    public Texture2D standard;
    public Texture2D downgfx;
    public Texture2D upgfx;
    public Texture2D heldgfx;

    void Start()
    {
        graphic.texture = standard;
    }

    void Update]() ()
    {
        bool down = Input.GetButtonDown("Jump");
        bool held = Input.GetButton("Jump");
        bool up = Input.GetButtonUp("Jump");

        if(down)
        {
            graphic.texture = downgfx;
        }
        else if(held)
        {
            graphic.texture = heldgfx;
        }
        else if(up)
        {
            graphic.texture = upgfx;
        }
        else
        {
            graphic.texture = standard;
        }

        guiText.text = " " + down + "\n " + held + "\n " + up;
    }
}
```


## GetAxis

How to "get axis" based input for your games in Unity and how these axes can be modified with the Input manager

GetAxis和GetKey，GetButton大体相似，区别在于后两者总是返回boolean值，但GetAxis返回0~1之间的值。

且在Input Setting中，通过设置Gravity用来控制从+/- 1 回到0的速度。

至于手柄控制的时候有哪些细节，需要再看视频研究。

https://unity3d.com/learn/tutorials/topics/scripting/getaxis?playlist=17117

https://youtu.be/XZAxgH1dqXw

**AxisExample**

```cs
using UnityEngine;
using System.Collections;

public class AxisExample : MonoBehaviour
{
    public float range;
    public GUIText textOutput;

    void Update ()
    {
        float h = Input.GetAxis("Horizontal");
        float xPos = h * range;

        transform.position = new Vector3(xPos, 2f, 0);
        textOutput.text = "Value Returned: "+h.ToString("F2");
    }
}
```

**AxisRawExample**

```cs
using UnityEngine;
using System.Collections;

public class AxisRawExample : MonoBehaviour
{
    public float range;
    public GUIText textOutput;


    void Update ()
    {
        float h = Input.GetAxisRaw("Horizontal");
        float xPos = h * range;

        transform.position = new Vector3(xPos, 2f, 0);
        textOutput.text = "Value Returned: "+h.ToString("F2");
    }
}
```

**DualAxisExample**

```cs
using UnityEngine;
using System.Collections;

public class DualAxisExample : MonoBehaviour
{
    public float range;
    public GUIText textOutput;


    void Update ()
    {
        float h = Input.GetAxis("Horizontal");
        float v = Input.GetAxis("Vertical");
        float xPos = h * range;
        float yPos = v * range;

        transform.position = new Vector3(xPos, yPos, 0);
        textOutput.text = "Horizontal Value Returned: "+h.ToString("F2")+"\nVertical Value Returned: "+v.ToString("F2");    
    }
}
```


## OnMouseDown

How to detect mouse clicks on a Collider or GUI element.

https://unity3d.com/learn/tutorials/topics/scripting/onmousedown?playlist=17117

https://youtu.be/DgpHOTuUJ58

OnMouseDown以及其相关的一系列方法可以用于侦测Collider或者UI上的事件。

问题，如果Collider启用了IsTrigger有没有作用呢？

### 示例代码

```cs
using UnityEngine;
using System.Collections;

public class MouseClick : MonoBehaviour
{
    void OnMouseDown ()
    {
        rigidbody.AddForce(-transform.forward * 500f);
        rigidbody.useGravity = true;
    }
}
```

## GetComponent

How to use the GetComponent function to address properties of other scripts or components.

https://unity3d.com/learn/tutorials/topics/scripting/getcomponent?playlist=17117

https://youtu.be/PBqTrK3z_KM

比较消耗资源，所以尽量在Awake或Start方法中调用一次，然后保存引用以方便调用而不是频繁获取。

### 示例代码

UsingOtherComponents

```cs
using UnityEngine;
using System.Collections;

public class UsingOtherComponents : MonoBehaviour
{
    public GameObject otherGameObject;


    private AnotherScript anotherScript;
    private YetAnotherScript yetAnotherScript;
    private BoxCollider boxCol;


    void Awake ()
    {
        anotherScript = GetComponent<AnotherScript>();
        yetAnotherScript = otherGameObject.GetComponent<YetAnotherScript>();
        boxCol = otherGameObject.GetComponent<BoxCollider>();
    }


    void Start ()
    {
        boxCol.size = new Vector3(3,3,3);
        Debug.Log("The player's score is " + anotherScript.playerScore);
        Debug.Log("The player has died " + yetAnotherScript.numberOfPlayerDeaths + " times");
    }
}
```

AnotherScript

```cs
using UnityEngine;
using System.Collections;

public class AnotherScript : MonoBehaviour
{
    public int playerScore = 9001;
}
```

YetAnotherScript

```cs
using UnityEngine;
using System.Collections;

public class YetAnotherScript : MonoBehaviour
{
    public int numberOfPlayerDeaths = 3;
}
```

## Delta Time

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
