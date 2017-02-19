---
title: Unity游戏编程模式:Flyweight Pattern
categories: 
  - Unity
tags:
  - Unity
date: 2017-02-05 15:14:28
updated: 2017-02-05 15:14:28
---

## What's the flyweight pattern?

We begin with [Wikipedia](https://en.wikipedia.org/wiki/Flyweight_pattern)'s defintion of the flyweight pattern:

> In computer programming, flyweight is a software design pattern. A flyweight is an object that minimizes memory use by sharing as much data as possible with other similar objects; it is a way to use objects in large numbers when a simple repeated representation would use an unacceptable amount of memory.

<!--more-->

So the basic idea is that if you have a lot of objects in your game, there's a high probability that you can increase the performance of your game by optimizing memory usage by making those objects "lighter." If this is going to work, your objects have to share some data which is the same for all objects. If they do share some data, then you can create the data once and then store a reference to the data in the object. This might sound complicated, so we need an example.

One common example to use is trees. Each tree in a game is the same to all other trees. So what's being shared among all trees is:

* They have the same mesh
* They have the same textures

But the trees don't have the same position, so that's the only thing in the tree object that's different from all other trees.

## Before the flyweight pattern

Trees are kinda dull, so we are here going to use Aliens with thousands of eyes, arms, and legs. So we need an alien class that describes an alien:

```cs
using UnityEngine;
using System.Collections;
using System.Collections.Generic;

namespace FlyweightPattern
{
    //Class that includes lists with position of body parts
    public class Alien
    {
        public List<Vector3> eyePositions;
        public List<Vector3> legPositions;
        public List<Vector3> armPositions;
    }
}
```

Before the flyweight pattern we will create all aliens and generate new body-part-positions for each alien:

```cs
using UnityEngine;
using System.Collections;
using System.Collections.Generic;

//Flyweight design pattern main class
namespace FlyweightPattern
{
    public class Flyweight : MonoBehaviour
    {
        //The list that stores all aliens
        List<Alien> allAliens = new List<Alien>();

        List<Vector3> eyePositions;
        List<Vector3> legPositions;
        List<Vector3> armPositions;


        void Start()
        {
            //List used when flyweight is enabled
            eyePositions = GetBodyPartPositions();
            legPositions = GetBodyPartPositions();
            armPositions = GetBodyPartPositions();
            
            //Create all aliens
            for (int i = 0; i < 10000; i++)
            {
                Alien newAlien = new Alien();

                //Add eyes and leg positions
                //Without flyweight
                newAlien.eyePositions = GetBodyPartPositions();
                newAlien.armPositions = GetBodyPartPositions();
                newAlien.legPositions = GetBodyPartPositions();

                //With flyweight
                //newAlien.eyePositions = eyePositions;
                //newAlien.armPositions = legPositions;
                //newAlien.legPositions = armPositions;

                allAliens.Add(newAlien);
            }
        }


        //Generate a list with body part positions
        List<Vector3> GetBodyPartPositions()
        {
            //Create a new list
            List<Vector3> bodyPartPositions = new List<Vector3>();

            //Add body part positions to the list
            for (int i = 0; i < 1000; i++)
            {
                bodyPartPositions.Add(new Vector3());
            }

            return bodyPartPositions;
        }
    }
}
```

If you add the `Flyweight.cs` to an empty gameobject, open the profiler, press play, click on the Memory box, you will see this image:

![Unity profiler memory without flyweight pattern](https://raw.githubusercontent.com/tospan/tospan.github.io/source/source/_images/unity/memory-without-flyweight.png)

The box you should pay attention to is Mono, which, [according to Unity](http://docs.unity3d.com/Manual/ProfilerMemory.html), is the "total heap size and used heap size used by Managed Code - this memory is garbagecollected." It's currently at 0.63 Gb, which is the number we are going to lower with the flyweight pattern.

## After the flyweight pattern

To add the flyweight pattern, we just assume that all aliens have the same body-part-positions. It might not be realistic, but this is just a test to see how the flyweight pattern is working. So modify the code so it looks like this:

```cs
//Add eyes and leg positions
//Without flyweight
//newAlien.eyePositions = GetBodyPartPositions();
//newAlien.armPositions = GetBodyPartPositions();
//newAlien.legPositions = GetBodyPartPositions();

//With flyweight
newAlien.eyePositions = eyePositions;
newAlien.armPositions = legPositions;
newAlien.legPositions = armPositions;
```

So we generate the body-part-positions once in the beginning and then add a reference to them to each Alien object. If you now open the profiler, press play, click on the Memory box, you will see this image:

![Unity profiler memory with flyweight pattern](https://raw.githubusercontent.com/tospan/tospan.github.io/source/source/_images/unity/memory-with-flyweight.png)

So the Memory has gone from 0.63 Gb to 17 Mb!