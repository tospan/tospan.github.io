---
title: Unity协同进程(Coroutine)
categories:
  - Unity
tags:
  - Unity
  - Scripting
  - C#
date: 2014-04-15 16:56:14
updated: 2017-02-07 16:56:14
---


什么是协同进程？为什么要用它？怎么用？使用过程中会遇到哪些问题？又怎么解决？

与此同时，什么是多线程？为什么用它？怎么用？使用过程会遇到哪些问题？又怎么解决？

协同进程和多线程之间有什么关系？异同点？


Following the coroutine article, this one will present a few actions that could be done better with coroutine.

## Make something happen in the future
There are several ways to delay an action, you may think of Invoke which is the most simple way I would recommend. Yet, simplicity often brings restriction, for instance Invoke does not allow to pass arguments but it may be enough in some cases.

<!--more-->

```cs
void Start(){
    Invoke(2.0f, "MyMethod");
    StartCoroutine(Delay(2.0f,MyMethod))
}
void MyMethod(){Debug.Log("Call");}
IEnumerator Delay(float timer, Action action){
    while(timer > 0f){
        timer-=Time.deltaTime;
        yield return null;
    }
    if(action != null){
        action();
    }
}
```

Using the script [from there](https://raw.githubusercontent.com/fafase/unity-utilities/master/Scripts/DelayCoroutine) we can delay actions and pass parameters as well:

```cs
public class MyClass:MonoBehaviour{
    void MyMethod(int a, string s){
        Debug.Log(a+" " +s);
    }
    void Start(){
        this.DelayedInvoke(2.0f, "MyMethod");
    }
}
```

The method needs to be a member of the calling class. The order of parameters have to match the method order or you get an exception.

## Make an object live for a few seconds

If you want your object to only appear on screen for a few seconds, just destroy it in Start!  Destroy can take a parameter which is the delay before it actually gets killed.

```cs
private void Start()
{
       //Destroy the game object in 5 seconds
       Destroy(this.gameObject, 5f);
}
```

## Delaying an action

You want something to happen and another action after a defined amount of time. UnityScript and C# use different action for the same result. In those example, the check variable is necessary to enter the statement. When entering it is set to false, after 2 seconds it is set back to true.

```cs
using UnityEngine;
using System.Collections;
 
public class Wait : MonoBehaviour 
{
    private bool check = true;
    private int i =0;
 
    private void Update () 
        {
        if(Input.GetKeyDown(KeyCode.A) && this.check)
                {
            this.check = false;
            Debug.Log("Inside" + i++);
            StartCoroutine(WaitForIt());
        }
    }
    private IEnumerator WaitForIt()
        {
        yield return new WaitForSeconds(2.0f);
        this.check = true;
    }
}
```

Running those scripts, you see that you can press A and it prints out ”Inside” and the value of i. But you will have to wait for 2 seconds before being able to print again.

## Do something in a few seconds time

If you want something to happen a couple of seconds after something else then you can just use Invoke.   Write a function containing the script you want to run, then call Invoke(“NameOfYourFunction”, delayInSeconds). In the example below, the TurnMeBlue function is called after 2 seconds.

```cs
private void Start()
{
      //Turn the object blue in 2 seconds
      Invoke("TurnMeBlue", 2f);
}
 
private void TurnMeBlue()
{
       renderer.material.color = Color.blue;
}
```

You might even want to use multiple invokes to make things happen in a sequence:

```cs
private Vector3 startPosition = Vector3.zero;
private float health = 100.0;
private bool respawning = false;
private Renderer renderer = null;
 
private void Start()
{
    //Cache were this object started
    this.startPosition = this.transform.position;
    this.renderer = GetComponent<Renderer>();
}
 
//Reset the object
private void Respawn()
{
      this.transform.position = this.startPosition;
      this.health = 100f;
      this.renderer.enabled = true;
      this.respawning = false;
}
 
//Hide the character
private void Hide()
{
     this.renderer.enabled = false;
     this.transform.position = new Vector3(1000f,1000f,1000f);
}
 
private void Update()
{
    if(health < 0 && this.respawning == false)
    {
         this.respawning = true;
         animation.Play("die");
         Invoke("Hide", 3f);
         Invoke("Respawn", 10f);
    }
}
```

This script uses invoke to play a death animation and then respawn the character.  When the health drops below 0 a “die” animation is played.  The model is hidden (and moved out of the way) in 3 seconds and then respawned in 10 seconds.

## Do something every few seconds

Rather than writing a coroutine with a loop you can simply write a function that contains the script you want and use InvokeRepeating(“YourFunctionName”, delayForFirstCall, timeBetweenCalls).

You can always cancel this using CancelInvoke(“YourFunctionName”) or without any parameters to cancel all of them.

```cs
private float health = 100f;
 
private void Start()
{
    InvokeRepeating("Heal", 2f, 2f);
}
 
private void Heal()
{
    this.health = Mathf.Clamp(health + 1f, 0f, 100f);
}
```

## When to use a coroutine
 
A coroutine can be useful when you need something that would otherwise be in an Update call and you don’t want to make Update too complicated.
 
Be aware that having Update and multiple coroutines all changing the same variables is the source of hard to spot errors.
 
For example you might want to rotate an object to a new angle over time.  This could be a use for a coroutine.  In the following example calling RotateBy rotates the object to a new angle over a period of time.

```cs
private void OnMouseUp()
{
     RotateBy(new Vector3(0f,90f,0f),2f);
}
 
private void RotateBy(Vector3 angle, float time)
{
     Quaternion currentRotation = transform.rotation;
     Vector3 targetAngle = currentRotation.eulerAngles + angle;
     Quaternion targetRotation = Quaternion.Euler(targetAngle);
     float t = 0f;
     while(t < 1)
     {
          transform.rotation = Quaternion.Slerp(currentRotation, targetRotation, t);
          t += Time.deltaTime / time;
          yield null;
     }  
     this.transform.rotation = targetRotation;
}
```

In Javascript you make a function a coroutine simply by adding a yield to it. Never do this in Update as its the one place this doesn’t work.

## 原文链接

* [Coroutine usage – Delay](https://unitygem.wordpress.com/coroutine-usage-invoke-delay/)


## Coroutines

How to create coroutines and use them to achieve complex behaviors.

https://unity3d.com/learn/tutorials/topics/scripting/coroutines?playlist=17117

https://youtu.be/bM3CXzj5xM4

4:15

```cs
using UnityEngine;
using System.Collections;

public class CoroutinesExample : MonoBehaviour
{
    public float smoothing = 1f;
    public Transform target;


    void Start ()
    {
        StartCoroutine(MyCoroutine(target));
    }


    IEnumerator MyCoroutine (Transform target)
    {
        while(Vector3.Distance(transform.position, target.position) > 0.05f)
        {
            transform.position = Vector3.Lerp(transform.position, target.position, smoothing * Time.deltaTime);

            yield return null;
        }

        print("Reached the target.");

        yield return new WaitForSeconds(3f);

        print("MyCoroutine is now finished.");
    }
}
```

```cs
using UnityEngine;
using System.Collections;

public class PropertiesAndCoroutines : MonoBehaviour
{
    public float smoothing = 7f;
    public Vector3 Target
    {
        get { return target; }
        set
        {
            target = value;

            StopCoroutine("Movement");
            StartCoroutine("Movement", target);
        }
    }


    private Vector3 target;


    IEnumerator Movement (Vector3 target)
    {
        while(Vector3.Distance(transform.position, target) > 0.05f)
        {
            transform.position = Vector3.Lerp(transform.position, target, smoothing * Time.deltaTime);

            yield return null;
        }
    }
}
```

```cs
using UnityEngine;
using System.Collections;

public class ClickSetPosition : MonoBehaviour
{
    public PropertiesAndCoroutines coroutineScript;


    void OnMouseDown ()
    {
        Ray ray = Camera.main.ScreenPointToRay(Input.mousePosition);
        RaycastHit hit;

        Physics.Raycast(ray, out hit);

        if(hit.collider.gameObject == gameObject)
        {
            Vector3 newTarget = hit.point + new Vector3(0, 0.5f, 0);
            coroutineScript.Target = newTarget;
        }
    }
}
```