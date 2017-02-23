---
title: 游戏编程模式:Subclass Sandbox pattern
categories: 
  - Game Programming Patterns
tags:
  - Unity
  - Patterns
date: 2017-02-05 15:28:27
updated: 2017-02-05 15:28:27
---

## What's the subclass sandbox pattern?

The subclass sandbox pattern describes a basic idea, while not having a lot of detailed mechanics. You will need the pattern when you have several similar subclasses. If you have to make a tiny change, then change the base class, while all subclasses shouldn't have to be touched. So the base class has to be able to provide all of the operations a derived class needs to perform.

<!--more-->

## The subclass sandbox pattern in Unity

We will here use the same code as in the book, but translated to C#. You will just need two scripts: a GameController and a Superpower script, which will include several classes. In your final game you should probably have one class per script, but we will here save space!

This is the GameController class:

```cs
using UnityEngine;
using System.Collections;
using System.Collections.Generic;

namespace SubclassSandbox
{
    public class GameController : MonoBehaviour
    {
        //A list that will store all superpowers
        List<Superpower> superPowers = new List<Superpower>();

        void Start()
        {
            superPowers.Add(new SkyLaunch());
            superPowers.Add(new GroundDive());
        }

        void Update()
        {
            //Activate each superpower each update
            for (int i = 0; i < superPowers.Count; i++)
            {
                superPowers[i].Activate();
            }
        }
    }
}
```

And these are the Superpower classes

```cs
using UnityEngine;
using System.Collections;
using System;

namespace SubclassSandbox
{
    //This is the base class
    public abstract class Superpower
    {        
        //This is the sandbox method that a subclass has to have its own version of
        public abstract void Activate();

        //All of the operations a derived class needs to perform - called from Activate()
        protected void Move(float speed)
        {
            Debug.Log("Moving with speed " + speed);
        }

        protected void PlaySound(string coolSound)
        {
            Debug.Log("Playing sound " + coolSound);
        }

        protected void SpawnParticles()
        {

        }
    }


    //Subclasses
    public class SkyLaunch : Superpower
    {
        //Has to have its own version of Activate()
        public override void Activate()
        {
            //Add operations this class has to perform
            Move(10f);
            PlaySound("SkyLaunch");
            SpawnParticles();
        }
    }

    public class GroundDive : Superpower
    {
        //Has to have its own version of Activate()
        public override void Activate()
        {
            //Add operations this class has to perform
            Move(15f);
            PlaySound("GroundDive");
            SpawnParticles();
        }
    }
}
```

If you add the GameController script to an empty gameobject in Unity and press play, you will see nothing exciting except a few lines in the console window. But as said before, this design pattern describes a basic idea.