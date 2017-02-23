---
title: Unity 简单基准测试
categories:
  - Unity
tags:
  - Unity
date: 2017-02-08 10:27:27
updated: 2017-02-08 10:27:27
---

## 目标:
You want to test your code or compare two pieces of code to see which goes faster. This is just a short article that will show quickly how to get an efficient benchmark.

<!--more-->

## 内容

* Using the time of your computer
* Using the counter of your computer
* Using Stopwatch

## 使用主机的Time

从某种程度上来说这是最简单直接的方法。也就是简单的获取两个操作之间的流逝时间。
The easy way in some way. We are simple using how much time has elapsed between two operations.

我们将用Unity的GetComponent方法来做测试。

你可能之前听过不少人说不要将GetComponent方法放到Update方法中，甚至应当在程序一开始的时候就保存要用的组件的引用，这里我们就看看这样的说法是否正确。

首先我们需要设置基准测试环境。首先要说的是，我们将不使用`Thread`，为什么？因为也许结果不准确。并不是说进程有什么问题，只是不适用于我们的情况，你应当知道不要在游戏开发时使用`thread`，那我们也没有理由在基准测试中使用。

测试代码看起来如下：

```cs
void Start(){
    // Take time 0
    // Run code A
    // Take time 1
    // Result A = time 1 - time 0
 
    // Take time 2
    // Run code B
    // Take time 3 
    // Result B = time 3 - time 2
 
    // Compare result
}
```

In order to get a better view on the test, it is required to run the code a high number of time so that the little different between the two codes starts to show up. You won’t get much of two codes with 0.000001s difference.

```cs
using UnityEngine;
using System.Collections;
using System;
 
public class Test : MonoBehaviour {
 
    void Start () 
    {
        double totalA = 0;
        double totalB = 0;
        int counter = 1000000;
        // Caching our transform
        Transform tr = GetComponent<Transform>();
 
        var currentTime = DateTime.Now;
        // First test with cache
        for (int i = 0; i < counter;i++)
        {
            Vector3 vec = tr.position;
            vec.x = 10;
            tr.position = vec;
        }
        totalA = (DateTime.Now - currentTime).TotalMilliseconds;
        // Second test with no cache
        currentTime = DateTime.Now;
        for (int k = 0; k < counter; k++)
        {
            Vector3 vec = transform.position;
            vec.x = 10;
            transform.position = vec;
        }   
        totalB = (DateTime.Now - currentTime).TotalMilliseconds;
        print (totalA);
        print (totalB);
    }
}
```

With this configuration I get around 232 for the first one and 335 for the second clearly showing our cache principle to be faster.

If I modify the first loop for :

```cs
for (int i = 0; i < counter;i++)
{
    Transform tr = GetComponent<Transform>();
    Vector3 vec = tr.position;
    vec.x = 10;
    tr.position = vec;
}
```

I get 430 / 340.

As a conclusion we see that the best way is to cache in the start and use the reference we created.

Also, you can see that the value tend to fluctuate from one run to another, one good practice could be to nest the loop inside another loop (make counter smaller in that case or you are up all night waiting for a result…). Each loop adds the result in a total and at the end the total is divided by the amount of outside loop to get an average.

## Using the counter of our computer

The previous test was using the `DateTime` structure. Another way to perform test is to use the counter from the computer. This is a little more advanced and use some C functions.

First we need to set up our program. To access the inner system, we need to use some Windows methods out of some old libraries.

Here is how you do it:

```cs
using System.Runtime.InteropServices;
public class NewBehaviourScript : MonoBehaviour {
    [DllImport("kernel32")]
    private static extern bool QueryPerformanceCounter(ref long perfCount);
}
```

Noticed the namespace reference? It is necessary so add it. The attribute is needed to tell where the method is stored, it is used for unmanaged native code. For more info see [there](https://msdn.microsoft.com/en-us/library/system.runtime.interopservices.dllimportattribute.aspx) .

```cs
using UnityEngine;
using System.Collections;
using System.Runtime.InteropServices;
using System;
 
public class NewBehaviourScript : MonoBehaviour {
    [DllImport("kernel32")]
    private static extern bool QueryPerformanceCounter(ref long perfCount);
 
    void Start () 
    {
        long start = 0;
        long totalA = 0;
        long totalB = 0;
        int counter = 1000;
        // Caching the component 
        Transform tr = GetComponent<Transform>();
        // Store the value and run the test with component
        QueryPerformanceCounter(ref start);
        for (int i = 0; i < counter;i++)
        {
            for (int j = 0; j < counter;j++)
            {
                Vector3 vec = tr.position;
                vec.x = 10;
                tr.position = vec;
            }
            // a takes the current value
            // totalA adds the difference between now and starting
            long a = 0;
            QueryPerformanceCounter(ref a);
            totalA += (a - start);
         }
 
        // Starting test with no cache
        QueryPerformanceCounter(ref start);
        for (int k = 0; k < counter; k++)
        {
            for (int l = 0; l < counter; l++)
            {
                Vector3 vec = transform.position;
                vec.x = 10;
                transform.position = vec;
            }   
            long a = 0 ;
            QueryPerformanceCounter(ref a);
            totalB += (a - start);
        }
        // Values are divided by amount of loop to get average
        totalA /= counter;
        totalB /= counter;
 
        print (totalA);
        print (totalB);
    }
}
```

This code above includes the principle of average I introduced earlier. My result is now

```
364897 for non-cached
246048 for cached
```

## 3 – 使用`Stopwatch`

It is also possible to use the Stopwatch Class. Since it is a class we need first to instantiate it. The little plus about it is that it has a bunch of members we can use directly, it does pretty much what we have done above but in a neater way.

For the sake of this test, since we have proven (prove, proved,proven, french people will understand…) already that caching is better, lets’s run another that was asked from me lately. Accessing a public variable or a static variable. The basic starting is that we have one object that gets accessed by many. Should we used a static variable or should we get the component (cached as we saw before) and access the public member?

So we need to create a simple script:

除了上面提到的两中方法，也可以使用`Stopwatch`类，我们需要实例化它，好处是，它有一系列的成员我们可以直接使用，它基本提供了我们需要的内容。

由于之前我们已经证明了，对于GetComponent这样的操作，缓存的效率更高一点，本例中，我们就测试另外一个常见的问题，访问公共变量还是静态变量？背景是我们有一个对象被多个对象访问，我们是该使用静态变量呢，还是使用公共的变量呢？

创建一个简单的脚本：
```cs
public class OtherScript : MonoBehaviour {
 
    public int pubVar;
    public static int statVar;
    public void SetVar(int value){
        pubVar = value;
    }
    public static int SetStaticVar( int n){
          statVar = n;
    }
    public int PropVar {get; set;}
}
```

We can start benching (inappropriate use of the verb).

```cs
using UnityEngine;
using System.Collections;
using System;
using System.Diagnostics;
 
public class Test : MonoBehaviour {
    void Start () 
    {
        int counter = 100000;
        // Caching the component 
        OtherScript script = GetComponent<OtherScript>();
        // Create the Stopwatch object
        Stopwatch swA = new Stopwatch();
 
        // Start the stopwatch
        swA.Start();
        for (int j = 0; j < counter;j++)
        {
            script.pubVar = 10;
        }
        // Stop the stopwatch
        swA.Stop();
 
        Stopwatch swB = new Stopwatch();
        swB.Start();
        for (int l = 0; l < counter; l++)
        {
            script.SetVar(10);
        }   
        swB.Stop();
 
        Stopwatch swC = new Stopwatch();
        swC.Start();
        for (int n = 0; n < counter; n++)
        {
            OtherScript.statVar = 10;
        }   
        swC.Stop();
 
        Stopwatch swD = new Stopwatch();
        swD.Start();
        for (int p = 0; p < counter; p++)
        {
             OtherScript.SetStaticVar( 10);
        }   
        swD.Stop();
 
        Stopwatch swE = new Stopwatch();
        swE.Start();
        for (int r = 0; r < counter; r++)
        {
            script.PropVar = 10;
        }   
        swE.Stop();
 
        print ("Public variable: " + swA.Elapsed);
        print ("Public method: " + swB.Elapsed);
        print ("Static variable: " + swC.Elapsed);
        print ("Static method: " + swD.Elapsed);
        print ("Property: " + swE.Elapsed);
    }
}
```

![Untitled-300x163](../_images/unity/untitled-300x163.png)

The result tells us the best way is static variable, congratulation!!! But before rushing into static for performance, you need to know how they work, check out our memory management article for this.

The reason why static is faster lies in the nature of the variable, since it is static it belongs to the class and not to any object. As a result the compiler is sure to find the variable and hence does not need to perform a check on if the object exists or not. Method are always a little slower since they require storing some info about the program before jumping to the location of the method, running and taking out back those info we stored. Properties shows to be a little faster despite being themselves methods.

## All in one method

As you can see a test can quickly get really long. This is going to make it down to one line (and the method…).

First we need a method that will run our methods in a loop and keep track of all info we need:

```cs
public class Test{
    public static void CompareMethods(int round,Action printMessage, params Action[] actions) 
    {
        int length = actions.Length;
        Stopwatch sw = new Stopwatch();
        float [] ms = new float[length];
        float [] ticks = new float[length];
 
        for(int i = 0; i < length; i++) 
        {
            sw.Start();
            long tick = sw.ElapsedTicks;
            for(int j = 0; j < round; j++)
            {
                actions[i]();
            }
            sw.Stop();
            ms[i] = sw.ElapsedMilliseconds;
            ticks[i] = (float)(sw.ElapsedTicks - tick);
            sw.Reset();
        }
        printMessage(new TestResult(length, ms, ticks));
    }
}
```

The first parameter takes how many rounds we want to perform. Some comparison will require a larger amount of call to detect which is most likely to be the best. The second parameter is the printing message. You are meant to print the info you need. In this case, a TestData class is needed. The last parameter is the list of method we want to pass. Using params, we are not limited to a finite amount of methods.

```cs
public class TestResult
{
    private float [] ms = null;
    private float [] ticks = null;
    private int length = 0;
    public int Length { get {return length; } }
    public float[] ElapsedMs { get { return ms; } }
    public float [] Ticks { get { return ticks; } }
 
    public TestResult(int length,float [] ms, float [] ticks)
    {
        this.length = length;
        this.ms = ms;
        this.ticks = ticks;
    }
}
```

And here is a basic test case:

```cs
void Start()
{   
    Test.CompareMethods(1000,PrintInfo, MethodA, MethodB, MethodC);
}
void MethodA() { for(int i = 0; i < 100;i++) {} }
void MethodB() { for(int i = 0; i < 1000;i++) {} }
void MethodC() { for(int i = 0; i < 10;i++) {} }
 
void PrintInfo(TestResult td)
{
    float [] t = td.Ticks;
    float [] ms = td.ElapsedMs;
    for(int k = 0; k < td.Length ; k++)
    {
        Debug.Log ("Printing "+ ms[k]+" ms, "+ t[k]);
    }
}
```

The methods are not doing anything, this is not the point here. The PrintInfo method allows us to print the way we want. We could add more data to the TestResult class and assign the value in CompareMethods. So far, you get the amount of ticks and the elapsed milliseconds.

The parameter is params Action[]. It means it only receive a method returning void and getting no parameter. The reason is simple, if you had to consider any parameter combination, even using generic, you end up with a couple of dozens.

So instead we limit to provide all possibilities:


```cs
float MethodToTestF(float param1, float param2, int extraUselessParam){
    float result = param1 + param2 * 1.05f;
    // extraUselessParam is useless so we do not use it
    return result;
}
double MethodToTestD(double param1, double param2, int extraUselessParam){
    double result = param1 + param2 * 1.05;
    // extraUselessParam is useless so we do not use it
    return result;
}
void TestMethodFloat(){
   MethodToTestF(10f,20f,1);
}
void TestMethodDouble(){
   MethodToTestD(10,20,1);
}
void Start(){
   Test.CompareMethods(1000,PrintInfo, TestMethodFloat, TestMethodDouble);
}
```

Get it?

## Conclusion

Whether you choose one way or another the result will not change the outcome.

Our little test clearly demonstrate that the best way is caching the component when accessed regularly like in the Update.

It also shows that using GetComponent is even worse that simply accessing the component by its name.

So for all your CharacterController, Transform, Rigidbody and other components being used in the Update, the way to go is the cached component.

Now, we did our test for GetComponent but you do understand that you can do that with any piece of code. You do understand that don’t you? So anytime you wonder, mmmm would this go faster since it has less code, go for a benchmark since less written code does not always mean faster code.

And for static vs non-static, make sure you know what you are doing and do not get tempted to save a few cycles and get all of your enemies killed at once.

## 原文链接

[Quick Benchmark](https://unitygem.wordpress.com/quick-benchmark/)