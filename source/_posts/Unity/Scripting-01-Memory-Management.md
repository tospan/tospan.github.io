---
title: Unity内存管理
categories:
  - Unity
tags:
  - Unity
  - Scripting
date: 2014-02-08 16:05:09
updated: 2017-02-07 16:05:09
---

本文将介绍：

* 内存空间
* 值类型和栈(Stack)
* 引用类型和堆(Heap)
* 结构(Struct)与类(Class)
* 创建引用
* 静态
  - 静态类
  - 静态变量
  - 静态函数
* Heap Fragmentation, 对象池和垃圾回收(GC)

<!--more-->

This tutorial is especially useful for C/C++ programmers making the move to C# for Unity development.  C# handles memory in a very different way which can appear confusing or magical at first and like all systems comes with its own pitfalls and idiosyncrasies.

When starting programming with Unity and programming in general many falls upon the purpose of variable types. For instance, why sometimes some variables are updated and others remain the same, why is my variable gone when I still need it.

Another common issue for beginners is the use of static variables. You often meet them in the various tutorials on the Internet but the tutors barely take the time to clearly explain and only end up claiming the “easy access of the variables”.

The problem is, static variables (also called class variables) are no easy topic.

Some might even tell you to simply avoid using them as whatever is done with static can be done with other types. We will see that if that could be true, it is not necessarily the best solution.

Before kicking off, if you have a C background and no real object-orientated programming, keep in mind that what you know about memory management and storage keyword is not totally similar in OOP. Try not to relate C usage with C#’s.

In this tutorial, we will cover the different memory zones, the value types, the reference types, the dynamic memory allocation and the static variables.

## 内存空间

当我们启动一个程序时，操作系统会为该程序分配一部分内存，以前主要有4种内存空间：call stack， 堆，register，和静态内存。（There used to be four main zones, the call stack, the heap, the register and the static memory.
）

C#编程语言开发人员友善地减少到两种主要的空间，栈(Stack)和堆(Heap)。栈是有序的，快但空间有限。堆是随机的，空间较大但相对慢一些。


The developers of the C# language have been kind enough to narrow it to two main zones only, stack and heap. The first is ordered, fast but limited. The second is random, large and then slower.

It is possible for the programmer to decide which of these is used for each variable depending on the purpose and the fate of the variable.

There are three storage class keywords (there used to be a fourth one in C, register) to be used for memory allocation, auto, static (those are the ones we are about to cover) and extern.

extern is a keyword you might encounter. It just means you declare a variable that is declared outside of the code in your project – for instance it’s a variable in a plugin DLL written in a different, non .NET language.

If you are a C programmer note that the meaning of extern is subtlety different in C# – where it refers to a variable that is not allocated in *managed memory*.

```cs
extern int number;
```

The compiler will not allocate memory for number and expect the variable to be found somewhere else. We will not dwell any further on this. You may find more information [on this link](https://web.archive.org/web/20140702030338/http://msdn.microsoft.com/en-us/library/e59b22c5(v=vs.71).aspx).

## 值类型与栈(Stack)

Auto stands for automatic variable. You might have never seen the keyword though as it is not used in C# anymore. Back in the old days, auto would stand for the default variable:

```cs
int variable = 10;
auto int variable = 10;
```

Those two lines are exactly the same.

The feature of an automatic variable is that it is allocated in the call stack. A stack is a pile of variables, just like a pile of plates, you can only remove the top one or add on the top but you cannot remove the bottom one or add at the bottom. See picture below.


栈其实就是一摞变量，就像一摞碟子，只能移除最顶上的或者放到最顶上，但不能从最底下拿或放到最下面。


![Stack](../_images/unity/Stack-300x150.png)

A stack is said to be a LIFO (Last-In-First-Out), simply said what you put last will be taken out first.

栈，也被称为LIFO(后进先出)，简单来说，最后放进去的先取出来？？？！

In order to know what the top of the stack is, the program uses a stack pointer which is a variable in the CPU that keeps track of the current position in the stack represented by the black arrow on the picture. As we add a variable the stack pointer is incremented (or decremented depending which way  your OS implements memory management).

The stack is used for function calls, if you think about it the whole program is a main function that calls Update functions which in turn call other functions so it is logical that any variables in your script is an automatic variable placed on the stack if not defined otherwise.

You may also encounter the concept of the stack frame – simply put this is the collection of variables allocated locally by the currently executing function (and also contains the return address that is the instruction point that will be executed immediately after the function returns).  The compiler maintains a pointer to the end of the stack frame and then actually uses a negative offset from this pointer to find the memory that your local variables occupy.  The practical upshot of this is that after a function has returned then none of the space used by its local variables is used or allocated in any way – save for being spare space for the next function call.

If you write a recursive program (a function that calls itself) you could potentially run the stack out of space as each subsequent call requires all of the local variables and the return address need to have space made for them on the stack – you will also see the stack overflow message if a bug in your code makes your routines call themselves without stopping.

So the lifetime and scope of an automatic variable go hand in hand. A variable lives from its declaration until the next closing curly brace corresponding to the closest opening curly brace above. That might need a clarification.

```cs
private void Update(){
    int number = 10;
    Fct(number);
    Debug.Log(number);
} 
 
private void Fct(int n){
    int inside=20;
    n = n + inside;
    Debug.Log(n);
}
```

![Stack function](../_images/unity/Stack2-300x253.png)

The Update is called and the first command is a variable declaration. The stack pointer is pushed one up and the value of number is placed into the memory location.The second command is a function call. When calling a function, the program stops and jumps to the address of the function, some data are passed to the stack in order to retrieve the original position of the program. The parameters (if any) are also pushed to the stack by creating a copy of the original value.

The variable number is not pushed onto the stack to be used in the function but a new variable n is created, pushed onto the stack to which the value of number is assigned

We now have a variable n inside the function that has the value of number but number does not exist in Fct() and therefore cannot be seen and accessed.

Inside the function a new integer is declared. The stack looks something like the picture below (note that it is highly simplified).

By the end of the function, n is printed and has value 30. All the variables created inside the function are destroyed/lost. That includes n and inside. Even though the variables are still in memory, they are simply ignored and not considered anymore by the system.

Back in the Update, we print number that has the original value of 10 as it was not the variable itself which was used in the function but a copy of it. The stack pointer has lost the location of n or inside, if we tried to access them back in the Update, the compiler would return an error that the variable does not exist in this scope.

We just saw that the scope of an automatic variable is defined by the { } it was declared in. Outside of them, the variable does not exist and cannot be seen. Those curly brackets can mark the scope of a function, an if-statement or a loop.

### 变量的生命周期和访问域

Note that lifetime means the time in the program the variable exists, scope defines the zones of the program the variable is visible. In the case of automatic variables, both are similar.

注意这里变量的生命周期是指程序中变量所存在的时间，访问域定义了程序中的变量的可见范围。

值类型的变量都是存储在栈中的，这些值类型包括：

* int
* unsigned
* float
* double
* char
* struct
* bool
* byte
* enum
* long
* short

看起来挺多，其实除了bool类型外所有的类型都是从C语言继承而来的，当时还没有bool类型。
Pretty much, all the types inherited from C except for bool that did not exist back then.

For an automatic variable, its life is taken care by the compiler. The end of the scope in which it was declared marks also the end of the variable. The programmer does not have to care about releasing the memory location for further use, it is done automatically.

## 引用类型与堆(Heap)

A reference type variable is an object stored in memory as a reference. When creating a new instance of a class, the data are stored in the heap as well as a reference to the location of those data. This reference is a variable that has the address of the object as value.To create an instance of a class we use the keyword new which is a call to the OS for some space in memory corresponding to the type of object and an instruction to run any code that is required to initialize the object.  See below for the instantiation of an object of type dog.

引用类型变量是指一个对象以引用的方式存储在内存中。当我们创建类的实例时，实例的数据和这些数据所在位置的引用都存储在堆(Heap)中。该引用是一个对象的地址。

```cs
public class Dog{
     public string name { get; set;}
     public int age { get; set;}
     public Dog(string name, int age){
           this.name = name;
           this.age = age;
    }                          
}

public class Test : MonoBehaviour 
{
    Dog dog1 = new Dog("Rufus",10);
    Dog dog2 = new Dog("Brutus",8);
 
    private void Update (){                                                  
        if(Input.GetKeyDown(KeyCode.Alpha1))
            Debug.Log ("The dog1 is named "+dog1.name+" and is "+dog1.age);
        if(Input.GetKeyDown(KeyCode.Alpha2))
             Debug.Log ("The dog2 is named "+dog2.name+" and is "+dog2.age);
    }
}
```

Our *dog1* and *dog2* variables are references to the locations in memory where the data concerning these objects are stored.

![Class](../_images/unity/Diapositive8-300x182.png)

Let’s now see in action with the Instantiate function. Consider we have a prefab called Enemy which includes a 3D model, some components and some scripts.

```cs
using UnityEngine;
using System.Collections;
 
public class Test : MonoBehaviour{
    public GameObject enemyPrefab;
 
    private void Update()
    {
        if(Input.GeyKeyDown(KeyCode.Space))
               Instantiate(enemyPrefab,new Vector3(0,0,0), Quaternion.identity);
    }
}
```

We created a new instance of the enemyPrefab and it is up and running. Problem is we have no reference to it if we want to access it. Should we want to apply some actions to our newly created object we can create a variable that will be a reference to it.

```cs
void Update(){
    if(Input.GeyKeyDown(KeyCode.Space))
       GameObject enemyRef = Instantiate(enemyPrefab, new Vector3(0,0,0), Quaternion.identity);
        enemyRef.AddComponent(Rigidbody);
        Destroy(enemyRef);
     }
}
```

In this example, enemyRef is declared and assigned the address of the new object. enemyRef is now a reference to it. See how we can add a component t since the variable knows the location of the object, it is possible to add or access any public variables of the object. In the final line, the object is destroyed which, you may agree, is not really likely to be done this way but that is just for the sake of the explanation.

Note that the reference is an automatic variable that lives only within the { } of the if statement. Outside of it, the reference variable does not exist any longer. In order to retrieve our enemy object we would have to look for it using for instance:

```cs
GameObject enemyRefAgain = GameObject.Find(“Enemy”);
```

任何引用类型的变量都是存放在堆(Heap)中的，它们是：

* class
* object
* string (string看上去像值类型变量但实际上它是String类的实例) 

The main difference between the value type variables and the reference type variables is the addition of a reference variable to point where the data are stored. This reference needs first to be accessed before we can find the data. This makes our class a little slower than structures which can be accessed directly.

值类型和引用类型的最主要区别就是引用类型变量指定真实数据存放的位置，要获取真实数据我们首先要获取引用变量。这就导致了在获取结构类型变量比类类型变量慢一些，因为结构可以直接访问。

其他主要问题

Other main issue is operation like this:

```cs
using UnityEngine;
using System.Collections;
 
public class Memory : MonoBehaviour 
{
    DogC dogC1 = new DogC();
    DogC dogC2 = new DogC();
    int number1;
    int number2;
 
    private void Update () 
    {
        if(Input.GetKeyDown(KeyCode.Space)){
           number2=number1 = 10;
           number1 = 20;
           Debug.Log (number1+" "+number2);
           dogC1.name = "Rufus";
           dogC1.age = 12;
           dogC2 = dogC1;
           dogC1.name = "Brutus";
           Debug.Log (dogC1.name+" "+dogC1.age+" "+dogC2.name+" "+dogC2.age);
        }
    }
}
```

You will notice that the two numbers are independent as one changes the other does not get the modification.On the other hand, when assigning a class to another one, the modification to one affects also the other.

This is because when doing dogC2 = dogC1; we assign the address of dogC1 to dogC2 reference variable. The original address of the dogC2 data is lost in memory and altering dogC1 or dogC2 is the same. We now have two references to the same data location.

![Class reference](../_images/unity/Diapositive4-231x300.png)

### 引用类型的生命周期，访问域与垃圾回收(GC) 

In the case of a reference type variable, the lifetime is unclear as it is the job of the garbage collector. Back in the old days of C and C++, it was the duty of the programmer to make sure dynamically allocated variables are removed from memory (the free() function in C, the ~ in C++).

In C#, the garbage collector will check if the variables are still used, if no reference to it is found from executable code, it will mark the memory locations as free to use.  Programmers familiar with COM interfaces or other reference counting systems often find garbage collection a strange concept – because in reference counting systems having two objects refer to each other (like a parent having a reference to its children and those children having a reference to their parent) can cause memory to never be freed – garbage collection has no such limitation.

Garbage Collection is a big subject and Unity has a number of oddities when it comes to the implementation of the collection process – if you are used to traditional C# then you will basically expect that GC should be cheap for small short lived objects, but it is not in Unity.Every time a block of memory fills up the system needs to check for the reachability of all objects allocated on the heap – this means a check for whether they can be accessed from any code that is or could be running – if they can be then the object is retained otherwise it is released.  Normal .NET Garbage Collections happen in a thing called generations which seek to minimise the amount of work associated with reachability testing by trying newly created objects first and only continuing to older objects if there still isn’t enough memory available.  It appears that Unity always tests all objects and so Garbage Collection is a major overhead, far greater than would normally be expected in a .NET language.

Reachability testing is the magic behind Garbage Collection – it enables the collector to determine if any code that is currently running or that could be running, because there is a live reference to it, has a reference to the object which is a candidate for collection.  This includes the use of closures inside functions – it’s very complicated and very powerful, but it isn’t fast in the way games need things to be fast.
A closure is created by referring to some local variable or parameter inside an anonymous function defined inside a function.  The value of the variable is kept so that when the anonymous function is called the variable is the same as when the routine executed.

```cs
List<Action> actionsToPerformLater = new List<Action>();
 
void DisplayAMessage(string message)
{
      var t = System.DateTime.Now;
      actionsToPerformLater.Add(()=>{
          Debug.Log(message + " @ " + t.ToString());
      });
}
 
void SomeOtherFunction()
{
      DisplayAMessage("Hello");
      DisplayAMessage("World");
      SomeFunctionThatTakesAWhile();
      DisplayAMessage("From Unity Gems");
 
      foreach(var a in actionsToPerformLater)
      {
           a();
      }
 
}
```

In this code each time we call the DisplayAMessage function we form a closure over the message passed in and the current system time.  When we run the foreach loop at the end, the debug messages are logged with the value of the parameter passed at the time we called the earlier function to add it to the list.  Closures are a very powerful tool and will be the subject of a future article.

Garbage Collection happens when the system thinks that there isn’t enough memory available for some operation, this means that an object that has been discarded may not be collected for a considerable period of time – it is therefore common practice to explicitly close external connections or otherwise release external resources when an object is no longer required.There is no need to explicitly release internal objects (classes in your project), but there is a reason when using things outside your project including Streams (files) and database connections that might be having some impact on the system.  For example not closing a file may make a subsequent action that access that file fail because the file is still open by a discarded object.

Garbage Collection is an expensive operation and so it makes sense to manually trigger it when it won’t make any difference to game play – such as between level loads, when the player enters a pause menu or at other times that it is likely to be unnoticed by the player.  You can manually trigger Garbage Collection using System.GC.Collect();
Concerning the scope of a reference type variable, it can be accessed anywhere in the program providing we perform the appropriate action to find it, using GameObject.Find() for instance will return a reference to the object in any scripts.
See our GetComponent tutorial for more details on finding instances of classes during game play.


## 结构(Struct) vs 类(Class)

什么情况下用结构，什么情况下用类？

So which one to use in what situations? We found out that using a class adds an overhead variable. If we were to create a thousand objects we get a thousand overhead variables when our structure would not generate them.

Now see this example:

```cs
public class DogC{
    public string name { get; set;}
    public int age { get; set;}
}
 
public struct DogS {
    public string name;
    public int age;
}
 
using UnityEngine;
using System.Collections;
 
public class Memory : MonoBehaviour {
    DogC dogC = new DogC();
    DogS dogS = new DogS();
 
    void Update () {
        if(Input.GetKeyDown(KeyCode.Alpha1))
            Print (dogS);
        if(Input.GetKeyDown(KeyCode.Alpha2))
            Print (dogC);
        if(Input.GetKeyDown(KeyCode.Alpha3))
            print ("Struct: The dog's name is "+dogS.name +" and is "+dogS.age);
        if(Input.GetKeyDown(KeyCode.Alpha4))
            print ("Class: The dog's name is "+dogC.name +" and is "+dogC.age);
     }
 
    void Print(DogS d){
        d.age = 10;
        d.name = "Rufus";
        print ("Struct:The dog's name is "+d.name +" and is "+d.age);
     }
 
    void Print(DogC d){
        d.age = 10;
        d.name = "Rufus";
        print ("Class:The dog's name is "+d.name +" and is "+d.age);
    }
}
```

First off, you will notice that the third input does not print out what is expected, the name and age of the structure dog have gone. On the other hand, the class retains its values.

Remember what we said, a structure is a value type when a class is a reference type. When calling the function for the structure, we pass a copy of the structure but the copy is not the original data. When modifying the data, we are just modifying the copies on the stack. Those are lost at the end of the function.

The class is passed using a reference which can access the original data and modify them within the function, those modifications remains even when the function is over.

What should that make us think of? If my structure is really large with for instance 10 variables, when passing it to the function the stack grows of 10 locations. Now if I pass an array of structures with 50 of them, the stack is now 500 locations larger, the stack is limited is size and a stack overflow could happen if the structure and array are even larger.

When passing the reference of my class, I only pass one variable. Passing 50 instances of the class will grow my stack of 50 variables only.
![Struct vs Class](../_images/unity/Diapositive6-300x136.png)

简单地说也就是，类节省内存而结构更快。我们的问题也就是处理内存和访问速度之间的平衡。So, classes save memory but structures are faster, totally. So the point is to figure out a balance between the two. Consider using structures when you have less than for instance 5 to 8 variables. Above that, use a class.

You must also bear in mind that every time you access a variable which is a structure (for example by doing transform.position) you are creating a copy of that structure on the stack – that copy takes time to create.

与此同时你还需要意识到的是每次访问结构类型的变量（例如`transform.position`）都需要在栈(stack)中创建该结构的拷贝，创建拷贝的过程也是需要时间的。

Every time you create a class you are allocating memory on the heap which will quickly lead to a Garbage Collection cycle for discarded values – structures are always allocated on the stack and hence will never cause Garbage Collection.

每次创建类的实例时候，计算机都需要在堆中分配内存，从而进入了垃圾回收循环。结构类型的值则会占据栈并且无法被垃圾回收。

You will notice that in Unity structures are often used for 3 to 4 variables like position, color, quaternion, rotation, scale,…, all of those are structures.

你会注意到Unity中的结构类型中比较常见的是3到4个变量，例如`position`,`color`, `quaternion`, `rotation`, `scale`等等这些都是结构。

Short lived, frequently allocated objects are good candidates for structs because they do not cause Garbage Collection.  Long lived, large objects are good candidates for classes because they will not be copied each time that they are accessed and their continued existence will not cause the need for Garbage Collection.  You will see that lots of small objects like Vector3 and RaycastHit are structs in Unity for this very reason.

One last point before we move on, we kept for the end the cherry on top. It is possible to pass a structure as reference so that we can modify our structure and retain the value.

You cannot maintain a reference to a structure after the function which originally defined the variable has exited, there are lots of ways for this to cause a problem such as the use of closures (formed by lambda functions or inline functions) but they are quite advanced techniques.

```cs
void PrintRef(ref DogS d){
    d.age = 10;
    d.name = "Rufus";
    print ("Structure:The dog's name is "+d.name +" and is "+d.age);
}
```

we call it like this:

```cs
void Update(){
    PrintRef(ref dogS);
    Debug.Log ("Structure:The dog's name is "+dogS.name +" and is "+dogS.age);
}
```

You have noticed the keyword ref, this is telling the compiler to create a reference to the structure instead of copying it onto the stack. Outside the function, the structure has kept the values given inside of it. This keyword can be used with any value type.

## Creating a Reference

So we just saw that it is possible to create a reference to a value type variable. Consider we want to create an integer inside a function but we want to keep for later use. Simply declaring inside the function will create an automatic variable that is lost on completion of the function.

We can create a dynamically allocated variable that will be stored in the heap with a reference in our script.

```cs
using UnityEngine;
using System.Collections;
 
public class Test:MonoBehaviour{
    int number;
 
    void Update () {
        if(Input.GetKeyDown (KeyCode.C))
            CreateVariable();
        if(Input.GetKeyDown (KeyCode.Space))
            PrintVariable(number);
    }
 
    void CreateVariable(){
        //int number = new int();
        number = new int();
        number = 10;
        Debug.Log ("In function "+number);
    }
 
    void PrintVariable(int n){
        Debug.Log (n);
        n+=10;
    }
}
```

First we declare a variable number of type int that is global to our script. We use that variable inside CreateVariable and assign to it an integer variable reference using `new int();`

![Reference for automatic](../_images/unity/Heap-300x138.png)

To clarify this situation, the new keyword returns a reference to the variable that is created in the heap section. This reference is stored inside number which now holds the address of the newly created variable. Using number is indirectly using this variable.Our newly created variable still exists outside the function.
Consider the commented part, if we had down it this way, we would have lost our reference at the end of the function. On its next cleaning round, the garbage collector would have found our data with no reference and remove it from memory. Consider that when declaring a local variable like we would have done here inside the function, int number; would have hidden the global int number.

When you hold a primitive variable, like an int, inside a reference the compiler performs a function called boxing basically wrapping the primitive variable inside a class.  Variables declared in this fashion are allocated on the heap and cause Garbage Collection to occur when they are released, they also take more memory than the default primitive value.
Let’s consider a basic boxing situation. When using the function Debug.Log(object);, the parameter is an object meaning that any type of variable can be passed to the function. But when passing an integer, the compiler will box the integer into an object. It is then possible to optimize the boxing by anticipating it like so:

```cs
void PrintingManyTimes(int n){
    object obj = n;
    for(int i = 0;i < 500;i++)
        Debug.Log(obj);
}
```

This way the compiler does not need perform the boxing on each call of Debug.Log. It is done once and for all 500 calls.

---


## 静态(Static)

We are getting now to the last part of our tutorial. Again, if you have a C background, forget what you know about static as the use here is different.

A static variable in .NET is stored in a special part of the heap called the High Frequency Heap.

As we mentioned earlier, we often meet static variables in tutorials and beginners just praise the ease of use. The gentle tutor often does not take the time to fully explain the benefits and danger of static variables. We are about to try to clarify what is a static object, how to use, where to use it and not.

### 静态类

A static class is a class that does not have any object instantiation, you cannot create an object of a static class.

As a result, there is only one instantiation. You might be confused as I just said there cannot be any instance of it. The program will allocate memory in the heap for this object and will not allow any user instantiation.

Static objects are created on their first call and will be destroyed on completion of the program (or its crash…).

```cs
public static class GameManager{
    public static int score;  
    public static float amountOfTime;
}
```

You will not be able to declare an object of type GameManager.

```cs
GameManager GM = new GameManager();
```

will return an error.

You will be able to access the members of the static class simply using

```cs
GameManager.score = 10;
```

This can be done anywhere and anytime in the game. A static class or member has scope in every file and lifetime of the whole program.

#### 使用静态类的案例

When loading a new scene, all variables from the previous scene are destroyed. If we want to preserve one variable like the score for instance we need

`DontDestroyOnLoad(score);`

One great use for static class lies in their lifetime. Since they do not die, if we want our score to survive the load of a scene we can simply use a static class. At the end of our level we pass the value to the static member, starting our new scene we retrieve that value.

```cs
if(newLevel){
    GameManager.score = tempScore;
    Application.LoadLevel(nextLevel);
}
```

And in script of the new level:

```cs
void Start(){
    scoreTemp = gameManager.score;
}
```

A little trick for this use, do not use GameManager.score until the end.

Think of a game with 10 levels where you get 200pts maximum per level.

One player finishes the game in one run and gets 2000pts.

Another player finishes the game in many run and gets 5000pts.

The second player is not as good but gets more points because he was trying each level many times and accumulating the points. If we wait for the loading of the next level to pass the point to the static variable, retrying should cancel the accumulated points during the level.

While leaving the game, the data from the static class can be sent to a save.

Static classes and static variables are allocated when they are first encountered during the programs execution.  A static class is never created unless you actually access one of its variables or methods.

### 静态变量

Many beginners make this simple mistake:

```cs
public static int health;
void Start(){
    health=100;
}
```

in another script we might find:

```cs
void OnCollisionEnter(Collision other){
    if (other.gameObject.tag == “Enemy”){
        EnemyScript.health-=10;
        if(EnemyScript.health <= 0)Destroy(other);
    }
}
```

They try the game with one enemy and succeed to kill him. But then they add more enemies and wonder why everyone dies suddenly.

The reason is simple, you have many instances of the enemy class (which is not static) but you have only one instance of the health for all instances. Killing one kill them all!

In fact, a static variable declared in a class does not belong to the objects but to the class itself. That is why we access it using the class name and not the instance name.

It did not work for the enemy health because each enemy needs to have its own health variable.

A static variable in the same way as the static class is instantiated when the class that defines it is first accessed. That means even though there would not be any instance of the class in the game, the variable would exist if you tried to access it.

Consider an enemy counter, we want to keep track of the amount of enemies we have in the game.

```cs

static var counter;
 
public void CreateEnemy(){
    GameObject obj=Instantiate(enemyPrefab, new Vector3(0,0,0).Quaternion.identity);
 
    if(obj)counter++;
}
 
public void DestroyEnemy(){
    Destroy(gameObject);
    counter--;
}
```

This script is attached to each enemy. They all share the same counter variable even though they all have a different script.

Using this function in our game allows us keeping track of our enemy count. Note the use of a reference variable, in the case Instantiate would not succeed creating the object (heap fragmentation for example), Instantiate returns a null reference. If we were simply to increase our counter without checking, if for some reason an instantiation fails, we have a wrong counter. Now if we use that counter to end up our level and launch a new one as such:

```cs
if(counter <= 0)Application.LoadLevel(nextLevel);
```

Without the check, if we had a fail, after killing all enemies we have a counter of 1, our game has a bug.

What about our player, can I use static health? Yes, you can use a static variable for the player’s health. Actually, accessing a static variable is even more efficient than an instance member.

When calling the member of an instance, the compiler needs to call a tiny function that checks for the existence of the object. That makes sense since you should not be able to modify a data that does not exist.

### 静态函数

A static function can be implemented in a non static class, in this case only the static member of that class can be accessed inside the function. This is logical since the static function is called via the class and not an instance, it would be problematic to try accessing members that are not sure to be existing.

On the other hand it is possible to pass parameters and you probably have been using this type of function quite a lot.

```cs
Vector3.Distance(vec1.vec2);
vec1.Normalize();
 
Application.LoadLevel (1);
 
if (GUI.Button(Rect(10,10,50,50),btnTexture)){}
```

The list could go on and on. Note the second example is not a static method. It is called through the instance vec1 of type Vector3. The other examples show that the class is used to call the static function.

Static function tends to be faster than non-static since the compiler since a call of an instance function creates a tiny overhead to check if the instance exist, a static function uses a class which is guarantee to exist . Though this gain of time is small.

Another benefit of static classes is that their existence is permanent and known to the compiler hence they can be used, in C#, to define extension methods – these are methods that appear to be additional functions added to any other type of variable.  This is syntactic sugar, the classes extended in this fashion are not actually opened (their private and protected variables are not available to extension methods), but it can be a neat way to apparently give new functions to existing objects which often lead to greater code legibility and an easier API for developers.  For example you could apparently add a new function to Transformtransform.MyWeirdNewFunction(x);

## Example of extension function:

Let’s consider I want to create a function for arrays that will extend the existing ones (OrderBy,Sort,…), I can declare a static function that will extend the list.

```cs
using UnityEngine;
using System.Collections;
 
public static class ExtensionTest {
 
    public static int BiggestOfAll(this int[] integer){
        int length = integer.Length;
        int biggest = integer[0];
        for(int i = 1;i < length;i++){
            if(integer[i] > biggest)biggest = integer[i];
        }
        return biggest;
    }
}
```

We declare a static function,see how the parameter passed is the instance calling the function. Modifying the type of the parameter will allow working on other objects.

Now declare and fill up an array:

```cs
using UnityEngine;
using System.Collections;
 
public class Test : MonoBehaviour {
 
    private int[] array = {1,25,12,120,12,6};
    private void Start () {
        print(array.BiggestOfAll()); 
    }
}
```

As you type the dot after your array name, you will see your new function showing up.

Should you decide to create a more generic function that accept any array regardless its type, you would implement a static generic extension method.

```cs
using UnityEngine;
using System.Collections;
using System.Collections.Generic;
 
public static class ExtensionTest {
 
    public static T TypelessBiggestOfAll<T>(this T[] t){
        int length = t.Length;
        var biggest = t[0];
        for(int i = 1;i < length;i++){
            biggest =((Comparer<T>.Default.Compare(t[i], biggest) > 0)?t[i]:biggest);
        }
        return biggest;
    }
}
```

Note the addition of using System.Collections.Generic; . As the compiler do not know what the type of T is, we cannot simply compare the values with < or >. But we can use  Comparer<T> . In the example, the ternary operator is used. It tends to make coding a little confusing at first but it also save a few lines.

The ternary operator looks like:       a ? b : c;a is performed and if true b is sent out of the command, if false c is sent. The advantage compared to a regular if-statement lies in the value returned from the comparison.        result = a ? b : c;           result will receive the value b or c depending on a

It is now possible to declare different type of arrays and use the same function.

```cs
using UnityEngine;
using System.Collections;
 
public class Test : MonoBehaviour {
 
    private int[] arrayInt = {1, 25, 12, 120, 12, 6};
    private float[] arrayFl = {0.5f, 52.456f, 654.25f, 41.2f};
    private double[]arrayDb = {0.1254, -15487.258, 654, 8795.25, -2};
 
    private void Start () {
         print(arrayInt.TypelessBiggestOfAll());
         print (arrayFl.TypelessBiggestOfAll());    
         print (arrayDb.TypelessBiggestOfAll());
    }
}
```

This will print out the values  120, 654.25, 8795.25 .

Now I want to show you where this can get useful. Many Unity users ask about how to constraint an object inside an area. For instance should you want to keep an object inside the screen you can clamp the Transform of that object using this function:

```cs
using UnityEngine;
using System.Collections;
 
public static class GameManager{
 
    public static void ClampTransform(this Transform tr,Vector3 move, Vector3 min, Vector3 max){
        Vector3 pos = tr.position + move;
        if(pos.x < min.x ) pos.x = min.x;
        else if(pos.x > max.x) pos.x = max.x;
 
        if(pos.y  < min.y ) pos.y = min.y;
        else if(pos.y > max.y) pos.y = max.y;
 
        if(pos.z < min.z) pos.z = min.z;
        else if(pos.z > max.z) pos.z = max.z;
 
        tr.position = pos;
    }
}
```

And you use it this way anywhere you need it:

```cs
Vector3 minVector = new Vector3(-10,0,-10);
Vector3 maxVector = new Vector3(10,0,10);
transform.ClampTransform(move,minVector,maxVector);
```

In this example, the object is constraint in a 2D (y remains at 0) environment in a square of 20*20  centered at (0,0,0).

## Heap Fragmentation, Garbage Collection and Object Pooling

The heap is a large zone of memory where data are randomly stored. Actually it is not so random, the OS will look into the memory and search for the first memory zone large enough to fit the required data.  The size and location of the heap varies depending on the player platform you have selected.

For instance, when we ask for an integer, the OS looks for 4 consecutive bytes of free memory. Fact is, as the program runs, memory locations get used and freed without real logic and your program might end up with a heap fragmentation.

![Heap fragmentation](../_images/unity/Diapositive5-300x149.png)

The picture shows an example of heap fragmentation, obviously this one is drastically emphasizes but the idea remains.You might notice that we are trying to allocate a data3 but on state1 we had a data3 that was destroyed on state2.
Note that no data have been added, there are at the end the same amount of data than at state1. Only the movements have created a heap fragmentation.

Unity uses .NET managed memory.  In a C program heap fragmentation could create a circumstance where it was impossible to allocate a block of memory because no contiguous block could be found of a suitable size, even though there was actually enough memory free – managed memory does not suffer from this limitation.  If a block of memory cannot be found the real .NET memory management and Garbage Collection systems can be used to remove the fragmentation of the heap by moving items around.  This doesn’t happen in Unity due to the implementation of the Garbage Collector.  It is more likely that you will find space as more than one object can be cleared at once, but this isn’t as effective as moving around blocks until they fit.
A solution to the programmer can use consist of Object pooling. What if instead of destroying our data3 in state1 we simply deactivate it for later use. That would have avoided our heap fragmentation.

We do not destroy data, we keep it sleeping and when needed, we wake it up.

There are many advantages for this solution, first we avoid the case seen above, second we also avoid calling Instantiate and Destroy functions, we only need to use gameObject.SetActiveRecursevely(true/false);

Finally, since we do not destroy, we do not call the garbage collector which is quite an expensive action.

ObjectScript.cs

```cs
using UnityEngine;
using System.Collections;
 
public class Test : MonoBehaviour {
    public GameObject prefab;
    GameObject[] objectArray = new GameObject[5];  
    public static int counter; 
 
    private void Start(){
        for(int i = 0;i < objectArray.Length;i++){
            objectArray[i] = (GameObject)Instantiate(prefab,new Vector3(0,0,0),Quaternion.identity);
            objectArray[i].SetActiveRecursively(false);
        }
    }
    void Createobject(){
        if(counter < objectArray.Length){    
        // iterate through array
            for(int i = 0;i < objectArray.Length;i++){  
                 // Find an inactive object                                     
                 if(!objectArray[i].active){    
                     // Increase the counter, activate the object, position it                                              
                     counter++;                                             
                     objectArray[i].SetActiveRecursively(true);            
                     objectArray[i].transform.position = new Vector3(0,0,0);  
                     return;
                 }
             }
             return;
        }else return;
    }
}
void OnCollisionEnter(CollisionEnter){
    if(other.gameObject.tag=="Object"){
        other.gameobject.SetActiveRecursively(false);       //Deactivate the object
        ObjectScript.counter--;                                             //Decrease counter
    }
}
```

The scene never have more than 5 objects at the same time, when one is deactivated it can be reactivated anywhere we want it. No garbage collection is needed.

You can use this trick for your enemies, if you have a wave of enemies coming at you, instead of destroying a killed enemy, reposition him on the back, you are about to kill the same guy many time without noticing it.

Same for your ammunition, instead of destroying each bullet you or the other NPC are shooting, disable them. Enable them back when you shoot again with position in front of your gun and proper velocity.

Note that pooling at the memory level is a different process managed by the OS consisting in reserving always the same amount  of memory whatever size the object is. As a result there will never be tiny memory locations lost all over the memory.

## 数组复用

In addition to reusing objects it is also worth considering creating buffers that are reused by your code between different calls or repeatedly in the same call.  This is because an array or List is allocated on the heap and creating one every Update or in some other frequent circumstance can quickly lead to Garbage Collection cycles.

```cs
void Update()
{
      if(readyToFire && Input.GetKeyDown(KeyCode.F))
      {
          var nearestFiveEnemies = new Enemy[5];
 
          //Do something to fill out the list finding the nearest
          //enemies
 
          TargetEnemies(nearestFiveEnemies);
      }
}
```

In this example we are targeting the nearest 5 enemies and firing some automatic weapon at them.  The problem is that we are allocating memory every time we press the fire button.  If instead we made a long-lived array of enemies we could use anywhere in our code, then we would not allocate memory and hence avoid the slow down.

This would be the simple case:

```cs
Enemy[] _nearestFiveEnemies = new Enemy[5];
 
void Update()
{
      if(readyToFire && Input.GetKeyDown(KeyCode.F))
      {
          //Do something to fill out the list finding the nearest
          //enemies
 
          TargetEnemies(_nearestFiveEnemies);
      }
}
```

But perhaps we could make it even more general if we often needed arrays of enemies:

```cs
Enemy[] _enemies = new Enemy[100];
 
void Update()
{
      if(readyToFire && Input.GetKeyDown(KeyCode.F))
      {
          //Do something to fill out the list finding the nearest
          //enemies
 
          TargetEnemies(_enemies, 5);
      }
}
```
By rewriting our code for TargetEnemies to take the count we can effectively use a general purpose buffer of enemies anywhere in our code and avoid further need for allocations.

The second example here is a pretty extreme case, and you should only do this if you really have huge collections that might cause a memory issue – in general you should always write readable, comprehensible code in favour of saving a few bytes of memory.




## GetComponent

How to use the GetComponent function to address properties of other scripts or components.

https://unity3d.com/learn/tutorials/topics/scripting/getcomponent?playlist=17117

https://youtu.be/PBqTrK3z_KM

比较消耗资源，所以尽量在Awake或Start方法中调用一次，然后保存引用以方便调用而不是频繁获取。

## 示例代码

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


## 原文链接

* [Memory Management](https://unitygem.wordpress.com/memory-management/)