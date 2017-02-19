---
title: Unity GPP Observer pattern
categories: 
  - Unity
tags:
  - Unity
date: 2017-02-05 15:20:33
updated: 2017-02-05 15:20:33
---

## What's the observer pattern?

We begin with [Wikipedia](https://en.wikipedia.org/wiki/Observer_pattern)'s defintion of the observer pattern:

> The observer pattern is a software design pattern in which an object, called the subject, maintains a list of its dependents, called observers, and notifies them automatically of any state changes, usually by calling one of their methods.

<!--more-->

So the basic idea is that you should use the observer pattern when you need many objects to receive an update when another object changes. The observer pattern is actually the communication backbone of countless programs and app frameworks, so pay attention.

One example is when you have achievements in your game. This might be difficult to implement if you have several achievements each unlocked by a different behavior. But it's much easier to implement if you know the observer pattern.

## The observer pattern in Unity

To illustrate the observer pattern we will here have three boxes (not many but hey this is just an introduction) that will jump when a sphere is close to the center of the map (origo).

So begin by creating a sphere and three boxes. Each box should have a rigidbody attached to it so it can jump. Also add a plane. You also need an empty gameobject called _GameController which will hold the script GameController that will take care of everything. When you are done everything should now look like this:

![Observer pattern start scene](https://raw.githubusercontent.com/tospan/tospan.github.io/source/source/_images/unity/observer-pattern-scene.png)

The GameController script looks like this:

```cs
using UnityEngine;
using System.Collections;

namespace ObserverPattern
{
    public class GameController : MonoBehaviour
    {
        public GameObject sphereObj;
        //The boxes that will jump
        public GameObject box1Obj;
        public GameObject box2Obj;
        public GameObject box3Obj;

        //Will send notifications that something has happened to whoever is interested
        Subject subject = new Subject();


        void Start()
        {
            //Create boxes that can observe events and give them an event to do
            Box box1 = new Box(box1Obj, new JumpLittle());
            Box box2 = new Box(box2Obj, new JumpMedium());
            Box box3 = new Box(box3Obj, new JumpHigh());

            //Add the boxes to the list of objects waiting for something to happen
            subject.AddObserver(box1);
            subject.AddObserver(box2);
            subject.AddObserver(box3);
        }


        void Update()
        {
            //The boxes should jump if the sphere is cose to origo
            if ((sphereObj.transform.position).magnitude < 0.5f)
            {
                subject.Notify();
            }
        }
    }
}
```

## The observer pattern scripts

The basic observer pattern consists of two parts:

* The **subject (publisher)** that holds a list with all observers interested in getting information (events) when something has happened. When something has happened it sends a notification to all observers.
* The **observers** which are the objects interested in doing something when an event has happened.

We begin with the subject class that will send notifications to the observers:

```cs
using UnityEngine;
using System.Collections;
using System.Collections.Generic;

namespace ObserverPattern
{
    //Invokes the notificaton method
    public class Subject
    {
        //A list with observers that are waiting for something to happen
        List<Observer> observers = new List<Observer>();

        //Send notifications if something has happened
        public void Notify()
        {
            for (int i = 0; i < observers.Count; i++)
            {
                //Notify all observers even though some may not be interested in what has happened
                //Each observer should check if it is interested in this event
                observers[i].OnNotify();
            }
        }

        //Add observer to the list
        public void AddObserver(Observer observer)
        {
            observers.Add(observer);
        }

        //Remove observer from the list
        public void RemoveObserver(Observer observer)
        {
        }
    }
}
```

And then the observers which are interested in the events:

```cs
using UnityEngine;
using System.Collections;

namespace ObserverPattern
{
    //Wants to know when another object does something interesting 
    public abstract class Observer 
    {
        public abstract void OnNotify();
    }

    public class Box : Observer
    {
        //The box gameobject which will do something
        GameObject boxObj;
        //What will happen when this box gets an event
        BoxEvents boxEvent;

        public Box(GameObject boxObj, BoxEvents boxEvent)
        {
            this.boxObj = boxObj;
            this.boxEvent = boxEvent;
        }

        //What the box will do if the event fits it (will always fit but you will probably change that on your own)
        public override void OnNotify()
        {
            Jump(boxEvent.GetJumpForce());
        }

        //The box will always jump in this case
        void Jump(float jumpForce)
        {
            //If the box is close to the ground
            if (boxObj.transform.position.y < 0.55f)
            {
                boxObj.GetComponent().AddForce(Vector3.up * jumpForce);
            }
        }
    }
}
```

But we also need events. In this case the events will just determine with what force the boxes will jump.

```cs
using UnityEngine;
using System.Collections;


namespace ObserverPattern
{
    //Events
    public abstract class BoxEvents
    {
        public abstract float GetJumpForce();
    }


    public class JumpLittle : BoxEvents
    {
        public override float GetJumpForce()
        {
            return 30f;
        }
    }


    public class JumpMedium : BoxEvents
    {
        public override float GetJumpForce()
        {
            return 60f;
        }
    }


    public class JumpHigh : BoxEvents
    {
        public override float GetJumpForce()
        {
            return 90f;
        }
    }
}
```

If you add all these scripts to Unity and press play, you should see that all boxes begin to jump if you move the sphere close to origo (center of the map).

But when you are doing this in real life, you should probably send an event through **observers[i].OnNotify(Event event);**, and then you check for the specific event in the Observer class's **public override void OnNotify(Event event)** and see if it corresponds with the event the Observer is subscribing to.