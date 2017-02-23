---
title: 游戏编程设计模式:有限状态机(FSM)
categories: 
  - Game Programming Patterns
tags:
  - Unity
  - Scripting
  - C#
  - Patterns
date: 2017-02-07 17:37:36
updated: 2017-02-07 17:37:36
---


The following article will introduce a basic state machine (or finite state machine FSM). The point here being to introduce some programming topics that are needed to develop a more advanced FSM without going to deep into it and losing the reader.

The article will cover the basic of a FSM, explaining the basic required for the tool. It will cover the definition of a FSM and talk about delegate and reflection. Those last two often bring confusion so we’ll try to clarify them.

Create a new script and name it StateMachine, go to the link below and copy paste the content onto your new script.

<!--more-->

[github-mark@1200x630](https://github.com/fafase/unity-utilities/raw/master/Scripts/StateMachineA)

## Basic of FSM

So, how to define a FSM? Well, any program is likely to contain some method like this:

```cs
void ControlMethod(int currentState)
{
    switch(currentState){
        case 0:
            Action0();
            break;
        case 1:
            Action1();
            break;
        case 2:
            Action2();
            break;
        default:
            ActionError();
            break;
    }
}
```

This is already a FSM. It has define actions for the appropriate state. So from there, it could be possible to create a more robust and maintainable situation via a class that would handle the process.

When we write a if-statement, we allow the program to be in either one of the two states. The more conditions, the more complex the machine becomes and more states we allow. It is also possible to set the program in two different states at a time though it is not recommended and more likely one main and one inner state. The problem is that as you add more states, it also becomes harder to debug and maintain, particularly for a sole developer.

So the idea lies in the term finite meaning limited. When building a FSM, we define its limits in each state so that we don’t end up running two states at a time. This way we keep full control since only one method is run. Think of it like the Update method you are already familiar with, but there would be one for each state.
Let’s try to visualize a common FSM we deal with every day (if you are no amish):

|------|--------------------------|
|Mode  |	Remote Control Functions|
|------|--------------------------|
|Off	 | Only the power button has any function.|
|      | Pressing power moves the TV into the On state.|
|----- |---------------------------|
|On	   | Pressing the power button moves the TV to the Off state.
         Pressing channel buttons changes the programme being received.
         Pressing the up/down buttons surfs through the channels.
         Pressing the menu button moves the TV to the Menu state.
|------|---------------------------|
| Menu |	* Pressing the power button moves the TV to the Off state.
          * Pressing the up/down buttons moves through the menu.
          * Pressing the menu button moves the TV to the On state.
|------|---------------------------|

The logic contained in each state would probably be extended but nonetheless, we are given the user interface. We can also clearly see the limitations of the program and some functions are only available in some specific states.

It is also recommended to start your FSM on the paper, thinking what states and what transitions. There are some tools out there to help you with that, some free, some not.Draw.io is one I use every now and then (free).
![States](../_images/unity/states.png)

The picture above indicates each state of the object and the conditions triggering a transition. Those conditions can be checked in update or via an event system. For instance, “Hit?” is a condition triggered by collision with a projectile (or any kind of weapon)
subtracting health and performing a check. If it is less than 0, it transits to “Dying”. On the other hand, the “Player/Enemy Approaches” bubble would require some kind of Update to be performed regularly.

## Creating the FSM

The first step is to create the class, no surprise if we call it StateMachine and it could inherit from MonoBehaviour. Avoid StateMachineBehaviour as it is already used by UnityEngine (or create a namespace but well…).

It is not necessary to have the MonoBehaviour in the StateMachine but it will simplify a bit but just so you know, it could definitely be a non MB.

```cs
public class StateMachine:MonoBehaviour
{
}
```

Next, we are talking about state and machine so we can consider the machine to be the class and we need a state. Let’s create an inner class for that.

```cs
using UnityEngine;
using System;
public class StateMachine:MonoBehaviour
{
    class State{
        public Enum name = null;
 
        public State(Enum name)
        {
            this.name = name;
        }
    }
}
```

Our class is not public so we cannot screw with it from outside the StateMachine class. It only contains an enum and a constructor for now. The enum is there because we will define our state using an enumeration. We could also use strings, same same but different. Don’t forget the System namespace.

We should define what a state is at the point. So far it is a name only. What we want is that the machine has a pointer (a reference) to the current state and it runs its update. Also, for each changing of state, we want to run an entry and an exit method.
So we need three method pointer aka delegate.

```cs
using UnityEngine;
using System;
public class StateMachine:MonoBehaviour
{
    class State{
        public Enum name = null;
        public Action updateMethod = () =&gt; { };
        public Action&lt;Enum&gt; enterMethod = (state) =&gt; { };
        public Action&lt;Enum&gt; exitMethod = (state) =&gt; { };
 
        public State(Enum name)
        {
            this.name = name;
        }
    }
}
```

All items are public since we need them in the machine, the first is a basic Action delegate, the two others will receive a parameter of type Enum.

## Why delegate?

Back in C/C++, a delegate was a function pointer.
A method MyMethod would require to be passed another method as parameter so that second method can be called from within. The programmer defines a method pointer and a method address is passed to it. The pointer then contains the address and can jump to that address and find the actual method. Ok this is confusing and I won’t explain much of the C/C++ version [function pointer](http://www.cprogramming.com/tutorial/function-pointers.html).

Now in C#, the .NET developer pushed the concept one step further. In C/C++, the pointer was a 4bytes variable (in 32bit) meant to receive the address of a method matching its signature. In C#, a delegate is a class and it contains all kind of methods.

The original way still in use:

```cs
public delegate void MyDelegateDeclaration();
public MyDelegateDeclaration myDelegateInstance = null;
void Start(){
    myDelegateInstance = MyMethod;
    myDelegateInstance();
}
void MyMethod(){ Debug.Log(&quot;Hell...oh World...&quot;);}
```

The first line declares the delegate in a similar pattern you would declare a new class. First comes the access modifier, then the delegate keyword indicating we are using a special type of class declaration. Comes next the return type, your own naming and the parameter list. This is the signature of methods the delegate can be assigned. In our example, MyMethod returns void and has no parameter so it does match the signature of the delegate.

Second line is the object creation. We now have a reference called myDelegateInstance which is an object of type MyDelegateDeclaration. We can create many object of type MyDelegateDeclaration. We cannot reuse MyDelegateDeclaration for a different type of delegate (just like you cannot reuse a class name)

In the Start, we assign the MyMethod to it. Notice that the () are missing. This is because we do not want to assign the return value (which is void here) by calling the method but instead we want to pass the address and it is done as such (this is equivalent to &Method; in C++). The next line is the invoke of the delegate as we use the parenthesis meaning we are not trying to use the address but the content of the delegate. In this case, it will point towards MyMethod and will print “Hell! Oh World…”. Since a delegate is an object, it can also be null.

The new way:

```cs
Action myDelegateInstance = null;
void Start(){
    myDelegateInstance = MyMethod;
    if(myDelegateInstance != null){
        myDelegateInstance();
    }
}
```

This is the generic way and was introduced later in the .NET framework. It works the exact same way as the previous since it is basically a wrapping of the delegate creation. As it is generic, the parameter list is also different and if a return value is needed, Func is used. I would recommend to look at the msdn for more info so that we can move along folks.

## Starting with our Machine

The idea is that our machine will store a collection of state and will transfer from the current state to the required state. In this version of the FSM, there will not be any constrains so any state can jump to any other state.

The first new feature is the current state of the machine. Basically, this is a reference to a state. We also want to be able to retrieve that state from outside the FSM

```cs
using UnityEngine;
using System;
public class StateMachine:MonoBehaviour
{
    class State{ // Code }
 
    private State currentState = null;
    public Enum CurrentState { get { return this.currentState.name; } }
}
```

The currentState is private and a getter is implemented. You could redefine that property if you feel more relevant to return a string for instance.

Now let’s initialised, shall we?

```cs
private bool initialized = false;
private bool debugMode = false;
private Dictionary &lt; Enum, State &gt; states;
private Action OnUpdate = ( ) =&gt; { };
 
protected void InitializeStateMachine&lt;T&gt;(Enum initialState, bool debug)
{
   if (this.initialized == true)
   {
       Debug.LogError(&quot;The StateMachine component on &quot; + this.GetType().ToString() + &quot; is already initialized.&quot;);
       return;
   }
   this.initialized = true;
 
   var values = Enum.GetValues(typeof(T));
   this.states = new Dictionary&lt;Enum, State&gt;();
   for (int i = 0; i &lt; values.Length; i++)
   {
      this.initialized = this.CreateNewState((Enum)values.GetValue(i));
   }
   this.currentState = this.states[initialState];
   this.inTransition = false;
   this.debugMode = debug;
 
   this.currentState.enterMethod(currentState.name);
   this.OnUpdate = this.currentState.updateMethod;
}
```

Ok, quite a bunch of things added here. First, the method is generic and should be passed an enum as generic parameter. We will see soon how to do that. The initialized variable prevents many initializations if by mistake you would call that method many times.

The debugMode variable will allow us to print info on the console if the mode is on. The states dictionary creates a map between enum and state so that we can pass an enum and get the matching state fast. Finally, the OnUpdate delegate is the machine delegate pointing to the current update method. It is just a shortcut to the update in the current state.

In the method now, first we check if already initialized and set it if not yet. Then, we grab all members of the enumeration that was passed and create the dictionary.
The for loop iterates all enum values and create a new state (method is coming next). The method returns true if all fine and false if something wrong. This is stored in the initialized variable so that if something went wrong, you cannot use the FSM in a wrong way you would not be expecting.

Next, we set the debugMode and call the enterMethod on the currentState and set the OnUpdate with the update of the currentState.
Notice the parameter on the enterMethod, this is because the method is expecting to be passed the previous state but as we are starting, there is no previous state, so we pass the current one.

This is how you would use it in a simplified example:

```cs
public class MyClassExample : StateMachine
{
    void Start(){
        InitializeStateMachine &lt;MyClassState&gt; (MyClassState.Start, true);
    }
}
public enum MyClassState{ Start, Move, Run, Die }
```

And nothing else. At this point, the FSM has registered four states. Let’s jump to the creation of those states.

```cs
private bool CreateNewState(Enum newstate)
{
    if (this.Initialized() == false) { return false; }
 
    if (this.states.ContainsKey(newstate) == true)
    {
        Debug.Log(newstate + &quot; is already registered in &quot; + this.GetType().ToString());
        return false;
    }
 
    State s = new State(newstate);
    Type type = this.GetType();
    s.enterMethod = StateTest.GetMethodInfo&lt;Action&lt;Enum&gt;&gt; (this, type, &quot;Enter&quot; + newstate, DoNothingEnterExit);
    s.updateMethod = StateTest.GetMethodInfo &lt;Action&gt; (this, type, &quot;Update&quot; + newstate, DoNothingUpdate);
    s.exitMethod = StateTest.GetMethodInfo &lt;Action&lt;Enum&gt;&gt;  (this, type, &quot;Exit&quot; + newstate, DoNothingEnterExit);
 
    this.states.Add(newstate, s);
 
    return true;
}
```

The beginning of the method holds no secret. We then create a new State object. We pass the type of our class into a type reference. What you need to understand there is that the value of the type will not be StateMachine but the calling class. In our example, type would contain info about MyClassExample, since the this pointer points to an object of that type.

Then comes three calls to GetMethodInfoInheritance (coming soon). That method returns a reference to a method that is passed to the delegates of the State object we created. The purpose of that method is that if a specific method is found on the class then a delegate is created towards that method. We will need reflection for this. Think of the Awake/Start/Update/… of a MonoBehaviour, Unity needs to check if you implemented those methods in the script so that it can call them, we will do the same with our own method naming.

Once done, we add the state to the dictionary so that we can fetch a state by enum. This is it for this one.

## Reflection

The purpose of reflection is to provide inner information on a type, an assembly or any meta data. In our case, we want to know if a class was implemented with methods that would carry specific names.

Consider you want to know if a class contains the method Start, there we go:

```cs
Type ourType = this.GetType();
MethodInfo methodInfo = ourType.GetMethod(&quot;Start&quot;, BindingFlags.NonPublic | BindingFlags.Public | BindingFlags.Instance);
if(methodInfo != null)
{
    Debug.Log(&quot;The given type contains a Start method&quot;);
}
```

You will need an extra namespace:

```cs
using System.Reflection;
```


And tadaaa. This is it. So again, no magic, if you know what you need and you look for the appropriate method, you get it. In our case, we want to look for a method so we use GetMethod. Then, we need to feed the correct parameters. We want a Start method that is public or private and an instance (non-static), this is done with the second parameter.

```cs
BindingFlags.NonPublic | BindingFlags.Public | BindingFlags.Instance
```

This performs a OR operation creating a bit value that will contain all three settings.

If the compiler finds a match in the [assembly manifest](https://msdn.microsoft.com/en-us/library/1w45z383(v=vs.110).aspx) (the file containing all the program info), it returns an information summary else you get null. The baseType reference is of type Type. It is in no way linked to the object it came from. In our example, we used this.GetType(), the call returned an object that contained the info about this object but you cannot get back to the object using baseType. Just like a handler’s manual contains information about a specific item, it is not the item itself. [Here](https://msdn.microsoft.com/en-us/library/System.Type(v=vs.110).aspx), you can find the documentation on Type from msdn.

Using the reflection method, we can find and register the FSM methods.

```cs
private static T GetMethodInfo&lt;T&gt;(object obj, Type type, string method, T Default) where T : class
{
    Type ourType = type;
    MethodInfo methodInfo = ourType.GetMethod(method, BindingFlags.NonPublic | BindingFlags.Public | BindingFlags.Instance);
    if (methodInfo != null)
    {
        return Delegate.CreateDelegate(typeof(T), obj, methodInfo) as T;
    }
    return Default;
}
```

The method is generic with a constrain (the generic has to be a class). The first parameter is the object on which we are working, the second parameter is the type we are working with. The string parameter is for the method name and the last parameter is the default method in case none was found.

Looking at the method, we find the same pattern we looked at earlier. If GetMethod found a method, then we create a delegate with [Delegate.CreateDelegate](https://msdn.microsoft.com/en-us/library/s3860fy3(v=vs.110).aspx) to which we pass the needed info with a cast as T. if none was found Default is returned.

This might require an extra explanation. When you think of a method on a class, you have to understand the inner mechanism. If you have a class Dog with a method Eat and you create 10 instances of Dog, the compiler actually only creates one version of the Eat method that is stored in memory. When an instance calls the method, the compiler passes a this pointer as first parameter that is hidden. That is how you can use this keyword within a method. So think you have a whole bunch of dogs and when one eats, it passes a reference to himself to a unique Eat method. That unique method is able to know the instance members of the Dog object using that this pointer. The same mechanism is explicitly used in extension methods.

Back to our CreateDelegate method, it creates a delegate, so simply said a pointer to the method location, it takes a type as first parameter so that it can instantiate the delegate (remember a delegate is an object of a type). The second parameter is a reference to an actual instance of an object and will create the this pointer relation between the delegate and that object. The delegate is now able to look into members of that instance. The third parameter uses the content of the method info to know where the delegate should point at.

* First -> Creates the delegate object
* Second -> Gets the reference to the object it should deal with
* Third -> Gets the method it should point at

Where does Default comes from?  Back up, when we called the method, we passed DoNothingUpdate and DoNothingEnterExit. Those are just empty method we use to fill the gap if nothing exists. So we can declare them:

```cs
public class StateMachine : MonoBehaviour
{
    private static void DoNothingUpdate() { }
    private static void DoNothingEnterExit(Enum state) { }
}
```

Now, let’s get back to the GetMethodInfo to look at the parameters again:

```cs
s.enterMethod = StateTest.GetMethodInfo&lt;Action&lt;Enum&gt;&gt;(this, type, &quot;Enter&quot; + newstate, DoNothingEnterExit);
s.updateMethod = StateTest.GetMethodInfo&lt;Action&gt;(this, type, &quot;Update&quot; + newstate, DoNothingUpdate);
s.exitMethod = StateTest.GetMethodInfo&lt;Action&lt;Enum&gt;&gt;(this, type, &quot;Exit&quot; + newstate, DoNothingEnterExit);
```

Pay attention to the third parameter, it shows

```cs
&quot;Enter&quot;+newState
&quot;Update&quot;+newState
&quot;Exit&quot; + newState&quot;
```
Our reflection process will look for methods that are made of Enter/Update/Exit and the name of the state. So consider we have


`public enum MyClassState{ Start, Move, Die }`

We need to implement like this:

```cs
public class MyClassExample : StateMachine
{
    private void EnterStart(Enum previous){}
    private void UpdateStart(){}
    private void ExitStart(Enum nextState){}
 
    private void EnterMove(Enum previous){}
 
    private void UpdateDie(){}
}
```

Enter and exit are taking parameters. This may allow the user to perform specific actions based on previous or next state. Update does not require this process.

Finally concerning reflection, it tends to be fairly slow. So best is to use a pool if you plan on using it on object that gets regularly destroyed and instantiate. You can use my ObjectPool article.

## Changing state

The final step is to be able to change state.

```cs
private bool inTransition = false;
 
protected bool ChangeCurrentState(Enum newstate)
{
    if (this.Initialized() == false) { return false; }
 
    if (this.inTransition)
    {
        if (this.debugMode == true)
        {
            Debug.LogWarning(this.GetType().ToString() + &quot; requests transition to state &quot; + newstate + &quot; when still transitioning to state &quot;);
        }
        return false;
    }
    if (this.debugMode == true)
    {
        Debug.Log(this.GetType().ToString() + &quot; transition: &quot; + this.currentState.name + &quot; =&gt; &quot; + newstate);
    }
    this.inTransition = true;
    State transitionTarget = this.states[newstate];
    if (this.currentState.name == transitionTarget.name)
    {
       Debug.LogError(this.GetType().ToString() + &quot; transition: &quot; + this.currentState.name + &quot; =&gt; &quot; + newstate +&quot; is same state&quot;);
       return false;
    }
    if (transitionTarget == null || this.currentState == null)
    {
        Debug.LogError(this.GetType().ToString() + &quot; cannot finalize transition; source or target state is null!&quot;);
        return false;
    }
    this.currentState.exitMethod(transitionTarget.name);
    transitionTarget.enterMethod(this.currentState.name);
    this.currentState = transitionTarget;
    this.inTransition = false;
    this.OnUpdate = this.currentState.updateMethod;
    return true;
}
```

The point of the ChangeCurrentState method is mainly to make sure we are not doing anything wrong first (do the states exist? are we not going from and to the same state?). The inTransition variable makes sure we are not trying to switch to a new state while already switching to a new state. It means you are not trying to call ChangeCurrentState while calling a Enter/Exit method.

Once all conditions are ok, we call the exit of the current state with the next state as parameter and then the enter of the next state with the current state as parameter

```
this.currentState.exitMethod(transitionTarget.name);
transitionTarget.enterMethod(this.currentState.name);
```

The final step is to reset the inTransition to false and set the update delegate to the new state update. Finally, we return true to say that it all went fine.

This last bit tells us we are missing one last method in our FSM, the update.

```cs
protected virtual void Update()
{
    this.OnUpdate();
}
```

Simply, we call the OnUpdate delegate that thanks to our ChangeCurrentState will always refer to the update of the current state.

This also means that our subclass using the FSM should not hide the Update and instead use an override that calls the base within it.

If you want to also implement FixedUpdate and LateUpdate, it goes the same way as we do for Update.

## Practical use

```cs
using UnityEngine;
using System.Collections;
using System;
 
public class MyClassExample: StateTest
{
     protected virtual void Awake()
    {
        InitializeStateMachine&lt;StateClass&gt;(StateClass.Start, true);
    }
 
    void EnterStart(Enum previous) { Debug.Log(&quot;Enter Start&quot;); }
    void ExitStart(Enum next) { Debug.Log(&quot;Exit Start&quot;); }
 
    void EnterRun(Enum previous) { Debug.Log(&quot;Enter Run&quot;); }
    void EnterDie(Enum next) { Debug.Log(&quot;Enter Die&quot;); }
    void UpdateRun() { Debug.Log(&quot;update run&quot;);}
 
    protected override void Update()
    {
        base.Update();
        if (Input.GetKeyDown(KeyCode.S)) { ChangeCurrentState(StateClass.Start); }
        if (Input.GetKeyDown(KeyCode.R)) { ChangeCurrentState(StateClass.Run); }
        if (Input.GetKeyDown(KeyCode.D)) { ChangeCurrentState(StateClass.Die); }
    }
}
public enum StateClass
{
    Start, Run, Die
}
```

Note how the Update also calls the base.Update. Do not forget the override or the update in the base will be hidden. The example is really simple using some input to fake the start, the running state and the death of the object. If you switch to a new state, the transition is shown in the console.
If you try to jump from one state to the same state, you will see an error in the console. This is so far the only constrain. If you set the parameter in InitializeStateMachine to false, all printing will be removed.

## Conclusion

This is a really basic introduction to FSM meant to be easy to begin with. Later, I will add some more features to the FSM so that it gets more robust. It can already be used as it is but is missing a main detail, one stage should only be able to move to defined stage. This is coming in the next article.
Any comment, go ahead.

Continuing with our FSM, we are about to add some extra features in order to more robust. One key issue with the current state of our FSM is that it allows any movement of state. All are registered and there is no tree of movement and restrictions. This is not acceptable and will be fixed with this article. So before getting any further, you need to have gone through the previous article or at least copied the script at the end.

Create a new script and name it StateMachine, go to the link below and copy paste the content onto your new script.

[github-mark@1200x630](https://github.com/fafase/unity-utilities/raw/master/Scripts/StateMachineB)

## Making a proper FSM

A Finite-State-Machine is not just a bunch of states, it has to define some limitations. In the previous article, the result script does not have any, you can jump from any state to any other state. Actually, the only limitations are that both states have to be registered and the origin and target states cannot be the same.

![fsm (1)](../_images/unity/fsm-1.png)

Just to clarify, the picture shows that:

* Start state leads to Walk and will be our initial state
* Walk leads to Run, Attack, Pause and Die
* Run leads to Walk, Attack, Pause and Die
* Attack leads to Walk, Run, Pause and Die
* Pause leads to Walk, Run and Attack
* Die leads to Start

As you can see, some states are restricted to some targets and that sounds more like a plan. You could argue that you just have to be careful and avoid sending one state to the wrong. Trust me, this is not enough. Even with the following addition, your program will perform actions you expect to land somewhere and it happens to crash somewhere else. This will help you track down or stop the program before it runs in the wrong direction.

## Extending the State

First thing we need is to add some features to the state class.

```cs
class State
{
    // existing code
        
    public bool forceTransition = false;
    public List <Enum> transitions = null;
         
    public State(Enum name)
    {
        this.name = name;
        this.transitions = new List <Enum>();
    }
}
```

So we add the forceTransition boolean that allows to bypass the restrictions we are about to implement. You might find this a contradiction, you may have find yourself in situations where it comes useful.

The second addition is the list of transitions the user will define as legal. This way we create a one to many relation (or one to one, or one to none).

## Adding states

The next step is to enable the addition of user state.

```cs
protected bool AddTransitionsToState(Enum newState, Enum[] transitions, bool forceTransition = false)
{
    if (this.Initialized() == false) { return false; }
    if (this.states.ContainsKey(newState) == false) { return; }
 
    State s = states[newState];
    s.forceTransition = forceTransition;
    foreach (Enum t in transitions)
    {
        if (s.transitions.Contains(t) == true)
        {
            Debug.LogError("State: " + newState + " already contains a transition for " + t + " in " + this.GetType().ToString());
            continue;
        }
        s.transitions.Add(t);
    }
    return true;
}
```

And there it is, no big magic once again. The first parameter is the state we are creating, the second parameter is the array of states we allow to go to. The last parameter will define if the state should allow any transition, default is false. The method reuses Initialized method from previous article.

So for our Walk state:

```cs
public enum PlayerState{ Start, Walk, Run, Attack, Die, Pause }
public class PlayerController:StateMachine
{
    void Start()
    {
         InitializeStateMachine<PlayerState>(PlayerState.Walk, true);
         AddTransitionsToState(PlayerState.Walk, new Enum[]{ PlayerState.Run,PlayerState.Attack, PlayerState.Die, PlayerState,Pause });
    }
}
```

The last parameter is left blank as forceTransition should be false by default. We just added Run, Attack, Die and Pause to its transitions list.

## Changing state

First, we need a method that would tell us if a transition is legit.

```cs
protected bool IsLegalTransition(Enum fromstate, Enum tostate)
{
    if (this.Initialized() == false) { return false; }
 
    if (this.states.ContainsKey(fromstate) && this.states.ContainsKey(tostate))
    {
        if (this.states[fromstate].forceTransition == true || this.states[fromstate].transitions.Contains(tostate) == true)
        {
            return true;
        }
    }
    return false;
}
```

The method is made protected so you can actually check in sub classes if the change of state is legit. So the first check is always the initialization. Then comes the check to see if both states are registered and finally either the state allows any transition or the source state allows the transition to the target state.

We can now implement the changing of state.

```cs
protected bool ChangeCurrentState(Enum newstate, bool forceTransition = false)
{
    if (this.Initialized() == false) { return false; }
 
    if (this.inTransition)
    {
        if (this.debugMode == true)
        {
            Debug.LogWarning(this.GetType().ToString() + " requests transition to state " + newstate +
                                    " when still transitioning");
        }
        return false;
    }
 
    if (forceTransition || this.IsLegalTransition(this.currentState.name, newstate))
    {
        if (this.debugMode == true)
        {
            Debug.Log(this.GetType().ToString() + " transition: " + this.currentState.name + " => " + newstate);
        }
 
        State transitionSource = this.currentState;
        State transitionTarget = this.states[newstate];
        this.inTransition = true;
        this.currentState.exitMethod(transitionTarget.name);
        transitionTarget.enterMethod(transitionSource.name);
        this.currentState = transitionTarget;
 
        if (transitionTarget == null || transitionSource == null)
        {
            Debug.LogError(this.GetType().ToString() + " cannot finalize transition; source or target state is null!");
        }
        else
        {
            this.inTransition = false;
        }
    }
    else
    {
        Debug.LogError(this.GetType().ToString() + " requests transition: " + this.currentState.name + " => " + newstate + " is not a defined transition!");
        return false;
    }
 
    this.OnUpdate = this.currentState.updateMethod;
    return true;
}
```

The first parameter takes the state to which we want to move. The second parameter will force the transition in case we do need to get there whatever it takes and we did not set the current state to accept the target state. It could be for instance a crash state while we already have 20 states and we do not want to manually add it to each single state. The rest is pretty much the same as in the previous article apart from the call to the if statement checking if we HAVE to transit or if it is a legal transition.

## Using the FSM

```cs
public class PlayerController : StateTest 
{
     private float timer = 0f;
     private Enum previousState;
     protected virtual void Awake() 
     {
        InitializeStateMachine<PlayerState>(PlayerState.Start, true);
        AddTransitionsToState(PlayerState.Start, 
              new Enum[]{PlayerState.Walk});
        AddTransitionsToState(PlayerState.Walk, 
              new Enum[] { PlayerState.Run, PlayerState.Attack, PlayerState.Pause, PlayerState.Die });
        AddTransitionsToState(PlayerState.Run, 
              new Enum[] { PlayerState.Walk, PlayerState.Attack, PlayerState.Pause, PlayerState.Die });
        AddTransitionsToState(PlayerState.Attack, 
              new Enum[] { PlayerState.Walk, PlayerState.Run, PlayerState.Pause, PlayerState.Die });
        AddTransitionsToState(PlayerState.Pause, 
              new Enum[] { PlayerState.Walk, PlayerState.Attack, PlayerState.Run});
        AddTransitionsToState(PlayerState.Die, 
              new Enum[] { PlayerState.Start });
    }
 
    void EnterStart(Enum previous) { 
         if((PlayerState)previous == PlayerState.Die)
         {
              Debug.Log("I am back from the dead");
         }
         Debug.Log("Starting game"); 
    }
 
    void EnterWalk(Enum previous) { Debug.Log("Setting walk"); }
    void UpdateWalk() { Debug.Log("I am walking");}
 
    void EnterRun(Enum previous) { Debug.Log("EnterRun"); }
    void UpdateRun() { Debug.Log("I am running");}
    void EnterRun(Enum previous) { Debug.Log("EnterRun"); }
     
    void EnterAttack(Enum previous)
    {
         timer = 0f;
         this.previousState = previous;
    }
    void UpdateAttack() { Debug.Log("I am attacking");
         timer += Time.deltaTime;
         if(timer >= 1.0f){ ChangeCurrentState(previous); }
    }
    void EnterPause(Enum previous)
    {
         previousState = previous;
         // Stopping the game!!
    }
    void UpdatePause()
    {
        if (Input.GetKeyDown(KeyCode.P) && (PlayerState)Current != PlayerState.Pause) { ChangeCurrentState(previous); }
    }
    void ExitPause(Enum nextState)
    {
         // Restarting the game!!
    }
 
    void EnterDie(Enum next) { Debug.Log("I am so dead right now"); }
    void UpdateDie(){
        // Should we restart
        if (Input.GetKeyDown(KeyCode.S)) { ChangeCurrentState(StateParent.Start); }
    }
    protected override void Update() 
    {
        base.Update();
        if (Input.GetKeyDown(KeyCode.P) && (PlayerState)Current != PLayerState.Pause) { ChangeCurrentState(StateParent.Pause); }
        if (Input.GetKeyDown(KeyCode.W)) { ChangeCurrentState(StateParent.Walk); }
        if (Input.GetKeyDown(KeyCode.A)) { ChangeCurrentState(StateParent.Attack); }
        if (Input.GetKeyDown(KeyCode.R)) { ChangeCurrentState(StateParent.Run); }
        if (Input.GetKeyDown(KeyCode.D)) { ChangeCurrentState(StateParent.Die); }
    }
}


public enum PlayerState 
{
    Start, Walk, Run, Attack, Die , Pause
}

```

The example is again simple, I leave to you to come up with your own version. Maybe you can share it in the comments. Basically, it is meant to print what happens. You will notice that some actions are only performed for the specific state. For instance, the S key is only check when dead. A better and more thorough implementation would have been to place the input in methods that are called within the appropriate state updates.

## Conclusion
This is it for this part. We can now prevent wrong state transition so our FSM is more robust avoiding transition that should not be. Again, if you see some errors, problems or if there would be some features you would think of, let us know.

## StateMachine – FSM – Part 3

![J-Alves-impossible-gears](../_images/unity/j-alves-impossible-gears.png)

The article is currently being reviewed so it might contain some leftovers of previous version that are outdated.
Final part of the FSM, in this article we should add some methods that will give us some more flexibility and information.
At the moment, we can know the current state but there will be situations when we need to know where we came from, or if a state exists based on what state we have already registered.
The FSM does not allow us to create what could be considered multi-transition so we would need that too.
How about adding and removing state at runtime? There could be cases where we want and need that.
And finally, adding a transition to an existing state will enable inheritance.

The extra step we could add but I will leave it up to you, is to override the methods containing debug line. You may want to create your own versions, shorter, longer with precise info and so on.

Create a new script and name it StateMachine, go to the link below and copy paste the content onto your new script.

[github-mark@1200x630](https://github.com/fafase/unity-utilities/raw/master/Scripts/StateMachineC)

## Adding helper methods

Let’s consider a UI system based on Model-View-Controller (MVC). In this situation, you have a class containing the methods for the UI buttons, mainly the action that should be performed on press of a button. This action is send to the UIController which contains most of the “intelligent” methods and also the FSM of the GUI. The last one contains all the view methods, mostly some methods called from the controller to switch on and off the UI items.
The Action class has a reference to the controller and the controller has a reference to the view.

Let’s think of the Pause button. It can be pressed anytime in the game and should return to the same state. When the action is pressed, the action class sends the info to the controller and set the FSM to pause, spreading the info to whatever is interested. Now once the pause is pressed again, either we recorded the previous state or we create it in the FSM.

Remember our transitionSource variable we create in the ChangeCurrentState, well, we just need to make it global.

```cs
public class StateMachine : MonoBehaviour{
    private State transitionSource = null;
    public Enum PreviousState { get { return this.transitionSource.name; } }
    protected ChangeCurrentState(Enum newstate, bool forceTransition = false)
    {
         // Code
          if (this.IsLegalTransition(this.currentState.name, newstate))
        {
            // Code
 
            this.transitionSource = this.currentState;
            State transitionTarget = this.states[newstate];
            
            // Code
            if (transitionTarget == null || this.transitionSource == null)
            {
                Debug.LogError(this.GetType().ToString() + " cannot finalize transition; source or target state is null!");
            }
            else
            {
                this.inTransition = false;
            }
        }
    }
}
```

I stripped the code to a minimum of lines so that you can see where it happens. Before we were creating the transition state and destroying it at the end of the method since it was local. Now that we made it global it will remain and contain the previous state.

Next helper method I could think of would be getting all the states currently registered as well as does a state exist then all the transitions registered in a state. We already can check if a state and transition are legit.

You should understand that those methods are only useful in some specific cases. IF we would have made the members public, this would be easier but also breaking encapsulation. So we need methods to get info without risking to break the machine.

```cs
protected bool ContainsState(Enum state)
{
    return this.states.ContainsKey(state);
}
protected Enum[] GetTransitionsFromState(Enum state) 
{
    if (this.states.ContainsKey(state) == false)
    {
        if (this.debugMode)
        {
            Debug.LogError("The given state " + state + " is not a registered state");
        }
        return null;
    }
    return this.states[state].transitions.ToArray(); 
}
protected State[] GetAllStates() 
{
    List<State>list = new List<State>();
    foreach(var kvp in this.states)
    {
        list.Add(kvp.Value);
    }
    return list.ToArray();
}
```

In order to enable the GetAllStates method, we need to make the State class public or the State type is not available outside the FSM.
This is it for the basic helper methods, let’s now move on to another topic.

## Multi-transition

The way the FSM has been defined, we restricted the possibility to move to a new state while transitioning.
To explain the needs for that feature, consider the user can see a tutorial before entering a level, if he already saw the tutorial, then switch to game:

```cs
protected void EnterTutorial(Enum oldState)
{
    if(AppController.Instance.HasSeenTutorial == true)
    {
        this.ChangeCurrentState(AppState.GameOn);
        return;
    }
    // More code
}
```

As we enter, if the AppController tells the tutorial was already seen, then we switch state to game. Obviously, we could have checked for that earlier when the change of state was triggered. Or we can just allow this feature and make it easy.
I would consider the feature only available in EnterMethod. Think that you define state A to go to state B and this one defines that on some conditions, it goes directly to state C. The state was entered and left but it was run.
Other situation, state A goes to state B but in the exit method of A, it switch to state C. Now you were expecting B to happen but something made it jump to C. Now should we be queuing B so that it runs after C or should B be first and then C? Well, nope, let’s not make it happen, end of discussion.

It will also require to modify a couple of methods and a few new variables.

```cs
private State transitionQueue = null;
private bool allowMultiTransition = false;
private bool inExitMethod = false;
 
protected void InitializeStateMachine<T>(Enum initialState, bool debug, bool multiTransition = false) 
{
    // Code
    this.allowMultiTransition = multiTransition;
    // Code
}
```

We add three variables, transitionQueue is of type State and will inform if we should trigger a new transition. allowMultiTransition defines if we allowed the multi-transition feature (it is set in the initialization method) and the inExitMethod is tracking if we are trying to switch while in exit method (never underestimate how wrong a coder can code).

```cs
protected bool ChangeCurrentState(Enum newstate, bool forceTransition = false)
{
    if (this.Initialized() == false) { return false; }
 
    if (this.inTransition == true)
    {
        if (this.allowMultiTransition == false) 
        {
            // Code
        } else if (this.allowMultiTransition == true)
        {
            if (this.inExitMethod == true)
            {
                Debug.LogWarning(this.GetType().ToString() + " requests new state in exit method is not recommended");
                return false;
            }
            this.transitionQueue = states[newstate]; // here
            return true;
        }
    }
 
    if (this.IsLegalTransition(this.currentState.name, newstate))
    {
        if (this.debugMode == true)
        {
            Debug.Log(this.GetType().ToString() + " transition: " + this.currentState.name + " => " + newstate);
        }
 
        this.transitionSource = this.currentState;
        State transitionTarget = this.states[newstate];
        this.inTransition = true;
        this.inExitMethod = true; // Added line here
        this.currentState.exitMethod(transitionTarget.name);
        this.inExitMethod = false; // Added line here
        transitionTarget.enterMethod(this.currentState.name);
        this.currentState = transitionTarget;
 
        if (transitionTarget == null || this.transitionSource == null)
        {
            Debug.LogError(this.GetType().ToString() + " cannot finalize transition; source or target state is null!");
        } else {
            this.inTransition = false;
        }
    } else {
        Debug.LogError(this.GetType().ToString() + " requests transition: " + this.currentState.name + " => " + newstate + " is not a defined transition!");
        return false;
    }
    // Added code here 
    if (this.allowMultiTransition == true && this.transitionQueue !=  null)
    {
        State temp = this.transitionQueue;
        this.transitionQueue = null;
        this.ChangeCurrentState(temp.name);
    }
 
    this.OnUpdate = this.currentState.updateMethod;
 
    return true;
}
```

ChangeCurrentState is a little longer. The idea is that if the FSM is transiting while this is called, it means while we were in the Enter method of a state, a ChangeCurrentState was found. So we are stacking up state transition. If we allow multi-transition, we check that we ar not in the exit method and register the new state into transitionQueue and return to avoid calling the rest of the method. This is the snippet below:

```cs
else if (this.allowMultiTransition == true)
{
    if (this.inExitMethod == true)
    {
        Debug.LogWarning(this.GetType().ToString() + " requests new state in exit method is not recommended");
        return false;
    }
    this.transitionQueue = states[newstate]; // here
    return true;
}
```

In order to prevent the switch in exit method, here is what we do:

```cs
this.inExitMethod = true; // Added line here
this.currentState.exitMethod(transitionTarget.name);
this.inExitMethod = false; // Added line here
transitionTarget.enterMethod(this.currentState.name);
```

The inExitMethod is set tot true, then we call the exiting method via the delegate. This jumps to the ExitMethod of the current state and if there was a call for ChangCurrentState within it, then the inExitMethod is true, it fails. Once the exit method returns fine without any failure, we reset the inExitMethod to false and call the enter method delegate of the new state. Now if there is a call for ChangeCurrentState in this one, no problem, we just run the method again but the new state will be added to the transitionQueue reference.

The final part of the method checks if any transiting state is pending:

```cs
// Added code here 
if (this.allowMultiTransition == true && this.transitionQueue !=  null)
{
    State temp = this.transitionQueue;
    this.transitionQueue = null;
    this.ChangeCurrentState(temp.name);
}
```

If the transitionQueue reference is not null, we have a pending state. We asiign it to a temporary state and set it to null (this prevent recursive call, a queue would have done it as well). Then we call for ChangeCurrentState with the new state and the process goes on again. Once all transition are done, the end of the method is run and finally, we give control back to the application.

## Adding/Removing states

The final step for our FSM is to allow addition and removal of transitions into an existing state. Consider you have an object that evolve in time, so you may want to add attribute in time. This you can already do. But what we cannot do is remove states.

```cs
protected bool RemoveTransitionToExistingState(Enum state, Enum[]transitions) 
{
    if (this.states.ContainsKey(state) == false)
    {
        if (this.debugMode)
        {
            Debug.LogError("The given state " + state + " is not a registered state");
        }
        return false;
    }
    State existingState = this.states[state];
    foreach (Enum transition in transitions)
    {
        if (this.states.ContainsKey(transition) == false)
        {
            Debug.LogError(transition + " is not a registered state");
            return false;
        }
        if (existingState.transitions.Contains(transition) == false)
        {
            if (this.debugMode)
            {
                Debug.LogWarning(existingState.name + " does not contain " + transition);
            }
            continue;
        }
        existingState.transitions.Remove(transition);
    }
    return true;
}
```

We can now remove states if our object is losing ability for instance. We already know how to add states. We have only done it in Start but we can do it anytime.

Note that this allows us to consider upgrade as state additions. A newly collected item may add a new state that we can use.

## Inheritance

In order to enable inheritance, we first need to update our GetMethodInfo and add a new method that will take care of adding new states later on when initialization is already done and cannot be called again:

```cs
private static T GetMethodInfo<T> (object obj, Type type, string method, T Default) where T : class
{
    Type baseType = type;
    while (baseType != typeof(MonoBehaviour))
    {
        MethodInfo methodInfo = baseType.GetMethod(method, BindingFlags.NonPublic | BindingFlags.Public | BindingFlags.Instance);
        if (methodInfo != null)
        {
            return Delegate.CreateDelegate(typeof(T), obj, methodInfo) as T;
        }
        baseType = baseType.BaseType;
    }
    return Default;
}
protected bool CreateNewStateWithTransitions(Enum newState, Enum[] transitions, bool forceTransition = false)
{
    if (this.Initialized() == false) { return false; }
    if (this.CreateNewState(newState) == false) { return false; }
 
    State s = states[newState];
    s.forceTransition = forceTransition;
    foreach (Enum t in transitions)
    {
        if (s.transitions.Contains(t) == true)
        {
            Debug.LogError("State: " + newState + " already contains a transition for " + t + " in " + this.GetType().ToString());
            continue;
        }
        s.transitions.Add(t);
    }
    return true;
}
```

If our base class contains private FSM methods, our sub class would not contain them, so if we want the FSM to find them, we need to iterate through the class inheritance. When the method info is null, we search the class above for the missing class.

This may seem confusing so look at this:

```cs
public class Top : StateMachine{
    void Start(){
        InitializeStateMachine<TopState>(TopState.Init, true);
    }
    private void UpdateInit(){}
}
public class Sub :Top{}
```

We have a problem. When the initialization method will run, it will consider the calling type, and that is Sub. But Sub has no UpdateInit since it is private in Top. One solution is to make it protected but maybe that is not intended or make the FSM search for it as we did.

The Add/Remove feature actually opens a whole new universe. Until now, we were not able to consider inheritance. If we add an Enemy class with a FSM, we were not able to create a Archer class with specific states to add to Enemy and another Cavalery class with different states to add to Enemy. Now we simply can.


```cs
public class Enemy : StateMachine{
    protected virtual void Awake() 
    {
        InitializeStateMachine<StateEnemy>(StateEnemy.Start, true, true);
        AddTransitionsToState(StateEnemy.Start, new Enum[]{StateEnemy.Walk});
        AddTransitionsToState(StateEnemy.Run, new Enum[] { StateEnemy.Walk });
        AddTransitionsToState(StateEnemy.Walk, new Enum[] { StateEnemy.Start, StateEnemy.Run });    
    }
}
public class Archer : Enemy
{
   protected virtual void Awake() 
    {
        // Call this one first
        base.Awake();
        // Do not call that one it was already in the base
        // InitializeStateMachineWithTransitions(true, true); 
        CreateNewStateWithTransitions(StateArcher.Shoot, new Enum[]{StateEnemy.Walk}); 
        AddTransitionsToState(StateEnemy.Walk, new Enum[]{StateArcher.Shoot});
    }
}
public class Cavalery :Enemy
{
   protected virtual void Awake() 
    {
        // Call this one first
        base.Awake();
        // Do not call that one it was already in the base
        // InitializeStateMachineWithTransitions(true, true); 
        CreateNewStateWithTransitions(StateCavalery.Ride, new Enum[]{StateEnemy.Walk}); 
        AddTransitionsToState(StateEnemy.Walk, new Enum[]{StateCavalery.Ride});
        ChangeCurrentState(StateCavalery.Ride);  // Set starting state
    }
}
```

Alright, so the base.Awake() has to be called before anything else so that it initializes the FSM. Calling it again in sub class would crash. If you want to know whether a class has a FSM above, make the IsInitialized method protected and call it to see if it is already done.
Each sub class registers its own states and add them to the Enemy states. Notice the Cavalery also sets the entry state. We would not be able to do that if the base.Awake was not called first, we would always end up in the state defined in Enemy.

Conclusion
This is now the end of the series, we went from a simple FSM to a more advanced. Based on what you are doing, the first one might be enough, if you do not consider inheritance for instance. In this case, I would use the second version for development and the first article once I got it all working.

Here is the interface of our FSM:

```cs
public Enum PreviousState{ get; }
public Enum CurrentState{ get; }
protected bool Initialized();
protected void Update();
protected void InitializeStateMachineWithTransitions(bool debug, bool multiTransition = false);
protected bool CreateNewStateWithTransitions(Enum newState, Enum[] transitions, bool forceTransition = false);
protected bool IsLegalTransition(Enum fromstate, Enum tostate);
protected bool ChangeCurrentState(Enum newstate, bool forceTransition = false);
protected bool AddTransitionToExistingState(Enum state, Enum[] transitions);
protected bool RemoveTransitionToExistingState(Enum state, Enum[]transitions);
protected Enum[] GetTransitionsFromState(Enum state);
protected State[] GetAllState();
protected bool ContainsState(Enum state);
```

## 原文链接

* [State Machine – Basic](https://unitygem.wordpress.com/state-machine-basic/)
* [State Machine – FSM – Part 2](https://unitygem.wordpress.com/state-machine-fsm-part-2/)
* [StateMachine – FSM – Part 3](https://unitygem.wordpress.com/fsm-part-3/)


----

另外一篇教程

## What's the state pattern?

We begin with [Wikipedia](https://en.wikipedia.org/wiki/State_pattern)'s defintion of the state pattern:

> The state pattern, which closely resembles Strategy Pattern, is a behavioral software design pattern, also known as the objects for states pattern. This pattern is used in computer programming to encapsulate varying behavior for the same object based on its internal state.

<!--more-->

The basic idea behind the pattern is that what you are trying to control has different states. If you have a car, it can either drive forward, brake, reverse, or you could have parked it. To write this behavior of a car in code is not that simple as it first might seem. If we are moving forward and braking, then we don't want to reverse even though the reverse button and the brake button is often the same key. And what if we are reversing and pressing the forward key, should we brake or accelerate? And what if we have more states than just four? Then you will need the state pattern to avoid nested if-else-statements with several booleans.

So the first thing you have to ask yourself is: What are all the states or possible situations the object is going to find itself in? This idea to divide the behavior into a fixed set of states that the object can be in is called a [Finite State Machine](https://en.wikipedia.org/wiki/Finite-state_machine), or FSM. The object can only be in one state at a time: you shouldn't accelerate and press the brake pedal at the same time!

It's common to use enums when coding a state machine in C#, and it looks like this with our car example:

```cs
enum CarFSM
{
	Forward,
	Brake,
	Reverse,
	Park
}
//Default state
CarFSM carMode = CarFSM.Park; 

//And then you change the state with something like
if (carMode == CarFSM.Park && IsPressingGasPedal())
{
	carMode = CarFSM.Forward;
}
else if (carMode == CarFSM.Forward && IsPressingBrakePedal())
{
	carMode == CarFSM.Brake;
}
//...and so on
```

## The state pattern in Unity

To illustrate the state pattern we will here make a Mincecraft clone. In Minecraft you have nasty creatures called Skeletons and Creepers. A Skeleton will attack you from distance with a bow and arrow, and the Creeper will attack you by close combat. If the player is far away from the Skeleton and Creeper, they will simply walk around in random directions, waiting for you to come closer.

So begin by creating:

* A plane which will act as our ground
* A sphere which will be the player being killed by enemies
* A green box which will symbolize a creeper
* A white box which will symbolize a skeleton
* An empty gameobject called _GameController

Everything should now look like this:

![State pattern start scene](https://raw.githubusercontent.com/tospan/tospan.github.io/source/source/_images/unity/state-pattern-scene.png)

The GameController script looks like this:

```cs
using UnityEngine;
using System.Collections;
using System.Collections.Generic;


namespace StatePattern
{
    public class GameController : MonoBehaviour
    {
        public GameObject playerObj;
        public GameObject creeperObj;
        public GameObject skeletonObj;

        //A list that will hold all enemies
        List<Enemy> enemies = new List<Enemy>();

        
        void Start()
        {
            //Add the enemies we have
            enemies.Add(new Creeper(creeperObj.transform));
            enemies.Add(new Skeleton(skeletonObj.transform));
        }


        void Update()
        {
            //Update all enemies to see if they should change state and move/attack player
            for (int i = 0; i < enemies.Count; i++)
            {
                enemies[i].UpdateEnemy(playerObj.transform);
            }
        }
    }
}
```

## The state pattern scripts

Our state pattern scripts consists of an Enemy parent class, and two child classes for each enemy. The basic idea is that we first update the state of the enemy, such as Attack, Flee, Stroll, or Move-towards-the-player-to-kill, by checking if we should change to another state from the current state the enemy is in. So if the enemy is strolling around and the player is within a certain distance, then the enemy should begin hunting the player.

The main difference between the Creeper and the Skeleton is that the Skeleton can attack from distance, while the Creeper has to be very close to the player to attack. I've not implemented everything, such as the health system, so neither of the enemies will run away when they are damaged.

The base enemy class looks like this:

```cs
using UnityEngine;
using System.Collections;


namespace StatePattern
{
    //The enemy base class
    public class Enemy 
    {
        protected Transform enemyObj;

        //The different states the enemy can be in
        protected enum EnemyFSM
        {
            Attack,
            Flee,
            Stroll,
            MoveTowardsPlayer
        }


        //Update the enemy by giving it a new state
        public virtual void UpdateEnemy(Transform playerObj)
        {

        }


        //Do something based on a state
        protected void DoAction(Transform playerObj, EnemyFSM enemyMode)
        {
            float fleeSpeed = 10f;
            float strollSpeed = 1f;
            float attackSpeed = 5f;

            switch (enemyMode)
            {
                case EnemyFSM.Attack:
                    //Attack player
                    break;
                case EnemyFSM.Flee:
                    //Move away from player
                    //Look in the opposite direction
                    enemyObj.rotation = Quaternion.LookRotation(enemyObj.position - playerObj.position);
                    //Move
                    enemyObj.Translate(enemyObj.forward * fleeSpeed * Time.deltaTime);
                    break;
                case EnemyFSM.Stroll:
                    //Look at a random position
                    Vector3 randomPos = new Vector3(Random.Range(0f, 100f), 0f, Random.Range(0f, 100f));
                    enemyObj.rotation = Quaternion.LookRotation(enemyObj.position - randomPos);
                    //Move
                    enemyObj.Translate(enemyObj.forward * strollSpeed * Time.deltaTime);
                    break;
                case EnemyFSM.MoveTowardsPlayer:
                    //Look at the player
                    enemyObj.rotation = Quaternion.LookRotation(playerObj.position - enemyObj.position);
                    //Move
                    enemyObj.Translate(enemyObj.forward * attackSpeed * Time.deltaTime);
                    break;
            }
        }
    }
}
```

The Skeleton class looks like this:

```cs
using UnityEngine;
using System.Collections;


namespace StatePattern
{
    //The skeleton class
    public class Skeleton : Enemy
    {
        EnemyFSM skeletonMode = EnemyFSM.Stroll;

        float health = 100f;


        public Skeleton(Transform skeletonObj)
        {
            base.enemyObj = skeletonObj;
        }


        //Update the creeper's state
        public override void UpdateEnemy(Transform playerObj)
        {
            //The distance between the Creeper and the player
            float distance = (base.enemyObj.position - playerObj.position).magnitude;

            switch (skeletonMode)
            {
                case EnemyFSM.Attack:
                    if (health < 20f)
                    {
                        skeletonMode = EnemyFSM.Flee;
                    }
                    else if (distance > 6f)
                    {
                        skeletonMode = EnemyFSM.MoveTowardsPlayer;
                    }
                    break;
                case EnemyFSM.Flee:
                    if (health > 60f)
                    {
                        skeletonMode = EnemyFSM.Stroll;
                    }
                    break;
                case EnemyFSM.Stroll:
                    if (distance < 10f)
                    {
                        skeletonMode = EnemyFSM.MoveTowardsPlayer;
                    }
                    break;
                case EnemyFSM.MoveTowardsPlayer:
                    //The skeleton has bow and arrow so can attack from distance
                    if (distance < 5f)
                    {
                        skeletonMode = EnemyFSM.Attack;
                    }
                    else if (distance > 15f)
                    {
                        skeletonMode = EnemyFSM.Stroll;
                    }
                    break;
            }

            //Move the enemy based on a state
            DoAction(playerObj, skeletonMode);
        }
    }
}
```

The Creeper class looks like this:

```cs
using UnityEngine;
using System.Collections;


namespace StatePattern
{
    //The creeper class
    public class Creeper : Enemy
    {
        EnemyFSM creeperMode = EnemyFSM.Stroll;

        float health = 100f;


        public Creeper(Transform creeperObj)
        {
            base.enemyObj = creeperObj;
        }


        //Update the creeper's state
        public override void UpdateEnemy(Transform playerObj)
        {
            //The distance between the Creeper and the player
            float distance = (base.enemyObj.position - playerObj.position).magnitude;

            switch (creeperMode)
            {
                case EnemyFSM.Attack:
                    if (health < 20f)
                    {
                        creeperMode = EnemyFSM.Flee;
                    }
                    else if (distance > 2f)
                    {
                        creeperMode = EnemyFSM.MoveTowardsPlayer;
                    }
                    break;
                case EnemyFSM.Flee:
                    if (health > 60f)
                    {
                        creeperMode = EnemyFSM.Stroll;
                    }
                    break;
                case EnemyFSM.Stroll:
                    if (distance < 10f)
                    {
                        creeperMode = EnemyFSM.MoveTowardsPlayer;
                    }
                    break;
                case EnemyFSM.MoveTowardsPlayer:
                    if (distance < 1f)
                    {
                        creeperMode = EnemyFSM.Attack;
                    }
                    else if (distance > 15f)
                    {
                        creeperMode = EnemyFSM.Stroll;
                    }
                    break;
            }

            //Move the enemy based on a state
            DoAction(playerObj, creeperMode);
        }
    }
}
```

If you now press play, move the camera to a from-above-position, and move the player by changing its coordinates in Unity's window, you should see something like this:

![State pattern in action scene](https://raw.githubusercontent.com/tospan/tospan.github.io/source/source/_images/unity/state-pattern-in-action.png)

Both boxes will move towards you if you are close enough, but only the green box will move towards your exact position, because the white box can attack you from a certain distance. But if the blue sphere is far away, both boxes will vibrate at their position. You should maybe implement a time parameter so they are actually strolling and not just changing direction each update, but changing random direction after a certain time!