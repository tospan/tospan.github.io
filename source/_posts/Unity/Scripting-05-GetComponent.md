---
title: Unity编程之GetComponent
categories:
  - Unity
tags:
  - Unity
  - Scripting
date: 2014-02-18 16:05:09
updated: 2017-02-07 15:55:39
---

该教程中将讨论：

* Accessing another script from inside the object
* How does GetComponent work? GetComponent的工作原理
* Getting many components 获取多个组件
* Interaction between objects. 对象之间的交互；
* Interaction between objects within the hierarchy 层级面板中对象交互
* `SendMessage` 和 `BroadcastMessage` 
* Interactive functions 交互函数

<!--more-->

## 简介

One recurrent issue when starting with Unity is how to access the members in one script from another script. Many would think that dereferencing from the script name would be enough, they quickly realise it is not.

When developing a program, variables are stored in the memory at different locations. If an object could see any other object members, there would be a risk to modify them even though it was not intended. If for instances two objects hold the same script with the same variable names, the compiler would not be able to figure out which one we are referring to.

To prevent this issue, each object in memory cannot see other objects. It is then necessary to tell one object where the variable it needs is in memory. The whole pointer principle has been dropped long ago and now we are using reference instead (which actually uses pointer internally).

There are various way to access variables, some are better than others and some are simply to be used in particular situations.

有好几种方式来访问变量，有些方式更好，有些适用于特定的情况。

* `GetComponent`, 最常用，也最容易让人困惑the most common, also the one that confuses most at first
* `SendMessage`, 看起来简单易用，但效率较低it might look easier to grasp but it is also less efficient.
* `静态变量`, 最简单易用但不太容易理解the easiest but also the most complicated to fully understand at first.

## 搭建场景

Create a new scene, add an empty game objects and name it “StormTrooper”. Create two scripts named “Health” and “Force”. Add both scripts to StormTrooper.

创建一个新项目和新场景，添加一个新的空游戏对象，命名为`StormTrooper`，然后给该对象添加两个脚本组件`Health`和`Force`，也就是C#脚本，脚本内容如下：

Health.cs

```cs
using UnityEngine;
using System.Collections;
public class Health : MonoBehaviour
{
    [SerializeField] 
    private int health = 5;
    private bool hasForce = false;
    private void Update()
    {
        if (Input.GetKeyDown(KeyCode.Space))
        {
            this.hasForce = Force.ReportForce();
            if(hasForce == true)
            {
                this.health = 10;
            }
        }
    }
}
```

Force.cs

```cs
using UnityEngine;
using System.Collections;
 
public class Force : MonoBehaviour
{
     private bool force = false;
     private void Start()
     {
          this.force = true;  
     }
 
     private void Update()
     {
         if (Input.GetKeyDown(KeyCode.P))
         {
              string str = (this.force == true) ? " the": " no");
              Debug.Log("I have" + str + " force");
         }
     }
 
     public bool ReportForce()
     {
         return force;
     }
}
```

保存之后，会报错，"An object reference is required to access non-static members"。我们需要告诉`Health`组件，`Force`组件在哪儿以便访问其成员变量。由于两个脚本都附加在同一个游戏对象上，我们可以直接声明脚本类型的变量，然后使用`GetComponent`方法获取脚本组件赋给变量。

## 在游戏对象中访问另外一个脚本Accessing another script inside the object

修改`Health.cs`脚本

```cs
using UnityEngine;
using System.Collections;
 
public class Health : MonoBehaviour
{
    [SerializeField] 
    private Force forceScript= null;
    public int health = 5; 
    private bool hasForce = false;
    private void Start()
    {
        this.forceScript= GetComponent<Force>();
    } 
    private void Update() 
    { 
        if (Input.GetKeyDown(KeyCode.Space)) 
        { 
             this.hasForce = this.forceScript.ReportForce(); 
             if(this.hasForce == true)
             {
                 this.health = 10; 
             }
        } 
    }
}
```

With the modifications, run the game, press Space and then press P, your StormTrooper now has 10 of health (Note that a StormTrooper does not have the force, this is only for the example). You access variables the same way you accessed methods. Just make sure the members you are trying to reach are declared public or you will get an error.

If the members were private, it would not be possible to access them. Anything that should not be reached from outside the class/script should be private to cope with the encapsulation principle.

## GetComponent的工作原理

First we declare an object of the type of the script we want to reach.

```cs
private Force forceScript = null;
```

Force is our type and forceScript is just a name we provide for using the reference in the script. We could give it any name.

The GetComponent function will look inside the object (the StormTrooper game object) to find a component corresponding to the type we passed. If none is found a null reference is returned and our forceScript variable will not do anything. If a component of type Force is found then the address of that component is passed to the variable forceScript, the variable now points to the Force component stored somewhere in memory. In order to access the public members of Force, we just need to dereference the script variable using the dot operator. It is said that the forceScript variable is a handle to the object. It gives access to all public members (and internal ones).

`GetComponent`函数会从游戏对象上（也就是`StormTrooper`）查找，看是否能找到我们所提供的类型的组件。如果找不到则返回空引用。如果找到`Force`类型的组件，则`Force`组件的地址就会传递给`forceScript`变量

```cs
script.PublicMembers;
```

以`private`和`protected`访问修饰符修饰的成员变量不可访问。

Using the SerializeField attribute allows to see the variables in inspector despute being private. Your forceScript variable starts with value of None(Force) and turns into a Force reference. If you click on it, it will highlight the target object.

使用`SerializeField`属性标记的变量，哪怕是私有变量都可以让我们在`Inspector`面板上看到它。

![reference_component](../_images/unity/prc3a9sentation12.png)

## 那么我们可以通过`GetComponent`来保存组件吗？

当然，对于频繁使用的组件推荐这样做！

当获取内置组件时，例如`Rigidbody`，`Transform`，`Render`……等等，以前Unity可以支持直接访问：

```cs
private void Update()
{
    this.transform.position += this.transform.forward;
    this.rigidbody.AddForce(force);
}
```
后来除了`transform`外，其他的组件都移除了，因为对于一个游戏对象而言，无论什么情况下都有`Transform`组件，而其他的组件不一定有。

应该完全避免在`Update`方法中使用`GetComponent`，如果需要在`Update`方法中使用组件，推荐先在`Start`或`Awake`方法中保存好引用：

```cs
private Transform myTransform = null;
private Rigidbody rigidbody = null;
 
private void Start()
{
     this.myTransform = this.gameObject.GetComponent<Transform>();
     this.rigidbody = this.gameObject.GetComponent<Rigidbody>();
}
private void Update()
{
     this.myTransform.position += this.myTransform.forward;
     this.rigidbody.AddForce(force);
}
```

这样可以节省一些运算资源。

## 获取多个组件

如果`StormTrooper`有多个`Force`组件（很大程度上是不可能的)。为了举例，这里我们多添加两个。

`Health.cs`脚本就成了:

```cs
using UnityEngine;
using System.Collections;
 
public class Health : MonoBehaviour
{
     private Force [] scripts = null;
     
     [SerializeField] 
     private int health = 5;
 
     private void Start()
     {
         this.scripts = this.gameObject.GetComponents<Force>();
     }
 
     private void Update()
     {
         if (Input.GetKeyDown(KeyCode.Space))
         {
             for (int i = 0; i < this.scripts.Length; i++)
             {
                 if(this.scripts[i].ReportForce())
                 {
                     this.health += 5;
                 }
             }                              
         }
     }
}
```

Run the game, press Space, our StormTrooper is becoming a Jedi. GetComponents looks for all components of the type and returns an array of them. This is fine if you do not target a specific component of the type but more likely all of them.

运行游戏，按空格键，我们的暴风兵就成了绝地武士了。`GetComponents`会获取当前组件所在对象上的所有指定类型的组件并返回数组。

See the following would turn on and off the lights:

如下脚本将会打开和关闭灯光：

```cs
public class LightFlicker:MonoBehaviour{
    private Light lights = null;
    private float timer = 0.0f
    private float clockTimer = 0.0f;
     
    private void Start(){
        this.lights = this.gameObject.GetComponents<Light>();
        this.clockTimer =  Random.Range(0.5f, 2.0f);
    }
 
    private void Update(){
         this.timer += Time.deltaTime;

         if(this.timer > this.clockTimer){
              this.timer = 0.0f;
              this.clocktimer = Random.Range(0.5f, 2.0f);
              for(int i = 0; i < this.lights.Length; i++){
                  lights[i].enabled = !lights[i].enabled;
              }
         }
    }
}
```

## 对象交互Interaction between objects.

我们可以让两个不同的对象用类似的方式交互。创建一个新对象，命名为`Sith`，移除`StormTrooper`脚本上重复的`Force`组件，留下一个就可以了。

```cs
using UnityEngine;
using System.Collections;
 
public class DarkSide : MonoBehaviour 
{
    private const bool force = true;
    private Force script = null;

    private void Start () 
    {
        this.script = GameObject.Find ("StormTrooper").GetComponent<Force>();
    }

    private void Update () 
    {
        if(Input.GetKeyDown (KeyCode.Space))
        {
             UseForceToControl ();
        }     
    }

    private void UseForceToControl()
    {
        if(this.script.ReportForce() == false)
        {
            Debug.Log ("Do as I say.");
        }
    }
}
```

修改`StormTrooper`对象上的`Force`脚本，确保他没有原力(将force设为false，要不然Sith可能会起疑心……)，给你个提示，在`Start`方法里。


如下的代码是找到`StormTrooper`对象并获取其`Force`组件。

```cs
private void Start () 
{
     this.script = GameObject.Find ("StormTrooper").GetComponent<Force>();
}
```

GameObject.Find() first finds the object passed as parameter and then looks for the required component inside of it. It is recommended to perform those actions in the Start or at a time of the game where lags will not affect the gameplay. GameObject.Find and GetComponent are expensive functions since they need to iterate through the whole Hierarchy. If your scene only includes 10 objects, it will be fine but if it holds thousands of them (an army of clone) you will surely see a bad effect. Consider calling these in the Start function. Note that if your scene contains many StormTroopers, Uity cannot know which you meant to use and will return the first one it finds. Most likely, you hold a reference to the object you want to interact with, for example a collision callback.

`GameObject.Find()`。

建议

由于`GameObject.Find`和`GetComponent`需要遍历整个游戏场景层级，所以这两个方法都比较重。如果你的场景中只有10个对象，这没有什么问题，但如果有上千的游戏对象(军队)那性能就会受到很大的影响。考虑一下将这些调用放到`Start`方法中去。注意，如果你的场景中有多个`StormTrooper`对象，Unity并不知道你要找哪个，它只会返回给你它找到的第一个。

## 层级中游戏对象交互Interaction between objects within the hierarchy

Consider a vehicle,, when driving it, the character is attached to it as a child object. The vehicle would have special feature if the driver has the force (better handling, faster and so on). As the driver is attached, he is below in the hierarchy.
The vehicle can find the Force component of the driver with the same principle we used previously (GameObject.Find) but we can make it happen faster by indicating to the vehicle that the driver is in its own hierarchy.

考虑这样的情况，当玩家驾驶一辆战车，玩家角色作为子对象附加到战车上。如果玩家有原力，那么该战车就有有特殊属性（例如操控性更好，更快等等）。当玩家附加到战车上时，玩家处于战车的子层级。

我们可以使用之前的方式(GameObject.Find)找到玩家的`Force`组件，但我们可以为战车指定玩家在它的子层级中，从而更快地获取。

```cs
private Force script = null;
private void Start () 
{
    this.script = this.transform.Find ("Sith").GetComponent<DarkSideForce>();
}
```

GameObject is swapped for transform. Indeed, a child object belongs to the transform of the parent so we need to look into the transform to find our object. In the example, the object is right under the calling object, if your Sith is inside the cockpit object under the car object, you need to provide the path:

```cs
private void Start () 
{
    this.script = this.transform.Find ("Cockpit/Sith").GetComponent<DarkSideForce>();
}
```

The calling object is the Car and is refered as the this in the call. Then Cockpit is searched and Sith is searched. If the path is wrong, Find returns null and you get a Null Reference Exception. You can avoid that like this:

```cs
private void Start () 
{
    Transform tr = this.transform.Find ("Cockpit/Sith");
    if(tr != null){
        this.script = tr.GetComponent<DarkSideForce>();
    }else{Debug.LogWarning("No component found");}
}
```

transform.Find returns a Transform while GameObject.Find returns a GameObject.

还有更好的方法，我们可以使用`GetComponentInChildren`:

```cs
private void Start () 
{
     this.script = GetComponentInChildren<DarkSideForce>();
}
```

Now our vehicle can know that our Sith has the force and apply special feature.
One issue to know about, if the parent object contains a script of the type you are looking for, it will return it. So thinking you are dealing with the child component, you are actually dealing with the parent one.

In this example, we need to check for each kind of driver if it is a StormTrooper or a Sith. A better approach would have been to use inheritance so that the top base class would hold the force variable and one single function would be used for all type of driver. But inheritance and efficiency were not our main issue here.

## `SendMessage` 与 `BroadcastMessage`

SendMessage and BroadcastMeassage are two other possibilties that may be simpler but also more expensive. The principle remains the same, we have a reference to an object and we want to modify a script it contains.

With SendMessage, you can call a function on an object without having to find a reference to the script. Now you tell me: “What is all this confusion ride if you can make it simple?”

> SendMessage uses reflection and is extremely slow compared to GetComponent and should only be considered in cases where it will not affect the game or if you have no other solution.

And that should sound logical, with GetComponent we indicate exactly where the script is located in memory. With SendMessage, the compiler knows the object, but it will go through all the scripts and functions until it finds the lucky winner. This may affect your game if you use it often on multiple objects and if the object has a lot of components and scripts (which are components by the way).

SendMessage seeks only the object to which the call is made, BroadcastMessage launches a search on the target object but also on all of its children. Research will be even longer if the object contains a large hierarchy. A newcomer appeared with Unity4 SendMessageUpwards which you should have guessed does the same as BroadcastMessage but upwards so all of the parents of the object until root.

Ok let’s consider our StormTrooper has a function for receiving order

```cs
//Order.cs
 
using UnityEngine;
using System.Collections;
 
public class Order : MonoBehaviour
{ 
    private void ReceiveOrder(string order)
    {
        switch(order)
        {
            case "Fetch":
                 //Action
                 Debug.Log("Can you repeat Sir?");
                 break;
            // other cases            
        }    
    }
}
```

Now let’s consider our StormTrooper meets a commander from the Empire:

```cs
// Commander.cs
 
private void Update(){
   if(Input.GetKeyDown(KeyCode.Space))
   {
       GameObject.Find("StormTrooper").SendMessage("ReceiveOrder","Fetch");
   }
}
```

As you can see, the principle remains the same, I need a reference to the target object and then I send a message to it. Just like GetComponent it is better if you have cached the object reference.

It is also possible to add a parameter to make sure the message finds a receiver or not.

Note that it is not possible to use a function returning a value. You would then have to go back up and use GetComponent.

## 交互函数

Unity uses many features that make two or more objects interact. The advantage of these functions is that they store a lot of information about the other object.

1. Collision functions OnTriggerXXXX/OnCollisionXXXX
2. Physics functions XXXXcast

There we go with collision declaration:

```cs
private void OnCollisionEnter(Collision colReference){}
private void OnTriggerEnter(Collider colReference){}
```

The call of the function creates a variable of type Collision or Collider. The function will fill this instance with information on the collision and will hold a reference to the other object we are interacting with.

```cs
prviate void OnCollisionEnter(Collision colReference)
{
   GameObject goRef = colReference.gameObject;
   if(goRef.CompareTag("StormTrooper"))
   {
       Health script = goRef.GetComponent<Health>();
       script.health -= 5;
   }
}
```

Notice that I do not need to look for the other object as colReference is of type Collision and contains a reference to that object as member. Now I can perform my GetComponent as usual and access my public health variable.

Functions from the Physics class have a similar principle. The data are not stored in a Collision/Collider instance but in a RaycastHit instance:

```cs
using UnityEngine;
using System.Collections;
 
public class CheckFlat : MonoBehaviour 
{
    private void Update() 
    {
        Ray ray = Camera.main.ScreenPointToRay(Input.mousePosition);
        RaycastHit hit;
        if (Physics.Raycast(ray, out hit))
        {
            float result = Vector3.Dot(hit.normal,Vector3.up);
            if(Mathf.Abs(result) > 0.9)
            {
               Debug.Log("Ground is flat enough");
            }
            else
            {
               Debug.Log("Not Flat Enough");
            }
        }
    }
}
```

I declare a variable of type RaycastHit, I pass it to the function. RaycastHit is a structure and then a value type, I need the out keyword to create a reference to that structure so that the function can modify the structure within. If you are wondering about that process, check out the Memory Management article and the section on value/reference type.
What I am doing here is check how flat is the surface I am placing the cursor on top of. This is useful for strategic games where you want to place a building but not on a cliff.
I use the dot product of the terrain normal with the Vector3.Up and if the result is close enough to 1 meaning they are aligned, the ground is flat enough for construction.
Obviously, some more calculations are needed as this is only for one point and you would need to check the normal around but you get the idea. I created interaction between my mouse position and the terrain using a raycast.

A raycast contains a starting point and a direction. In our case, the starting point is the conversion of the mouse position to world position and then a direction based on the camera rotation and projection matrix. This line goes infinitely in the defined direction (you can also set a max distance) and if it hits a collider, the information of that collider and collision are stored in the RaycastHit structure. The structure contains then a reference to the hit object.

```cs
Collider objectCollider = hit.collider;
GameObject objectReference = hit.collider.gameObject; // or objectCollider.gameObject;
```

## 原文链接

[GetComponent in C#](https://unitygem.wordpress.com/getcomponent-in-c/)