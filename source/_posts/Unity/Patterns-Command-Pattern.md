---
title: Unity GPP Command Pattern
categories: 
  - Unity
tags:
  - Unity
date: 2017-02-05 15:07:34
updated: 2017-02-05 15:07:34
---

## What's the command pattern?

We begin with [Wikipedia](https://en.wikipedia.org/wiki/Command_pattern)'s defintion of the command pattern:

> In object-oriented programming, the command pattern is a behavioral design pattern in which an object is used to encapsulate all information needed to perform an action or trigger an event at a later time. This information includes the method name, the object that owns the method and values for the method parameters.

<!--more-->

...and according to the author of the book Game Programming Patterns, the command pattern is one of his favorite patterns, and he's often using it somewhere in most large programs he has written.

One example where the command pattern is really useful is when you let the users of your game change the keys. In Unity, you can write a simple fire function like this:

```cs
if (Input.GetKeyDown(KeyCode.A))
{
	FireWeapon();
}
```
So what are you going to do if the user would like to fire the weapon with U and not A? To make that happen we have to find a way to replace FireWeapon(); with something more general, and that's why we need the command pattern. We begin by defining a base/parent class called Command:

```cs
public abstract class Command
{
	public abstract void Execute();
}
```

...and then the FireWeapon will inherit from that base class. We also need a class that will do nothing, so we can switch to U from A when firing the weapon.

```cs
public class FireWeapon : Command
{
	public override void Execute()
	{
		FireTheWeapon();
	}
}
public class DoNothing : Command
{
	public override void Execute()
	{
	}
}
```

Where we earlier was just firing the weapon when pressing A, we can now easily replace FireWeapon(); with the more general Execute(); when we press the U key. So you have to come up with a clever GUI so your user can replace buttonA with buttonU, but that will be much easier than using the FireWeapon(); when pressing a certain key.

```cs
Command buttonU = new FireWeapon();
Command buttonA = new DoNothing();

if (Input.GetKeyDown(KeyCode.U))
{
	buttonU.Execute(); //Will now call the FireTheWeapon() method in the Execute() method in the FireWeapon class 
}
if (Input.GetKeyDown(KeyCode.A))
{
	buttonA.Execute(); //Will do nothing because the Execute() method in DoNothing class is empty
}
```
But that's not it! If you are saving the commands you are sending in a list, you will also be able to undo the commands. Let's say you have a level editor and are placing trees. Then you can use the command pattern to undo the last (or all) commands in the case you placed the tree at the wrong spot.

By using the same list as when undoing something you will also be able to make a replay function. If you remember the start positions, then you just loop over the commands you have saved and Unity will reproduce everything you did. This is a much better way than saving the position and orientation of all your objects in the scene each frame and then repeat the positions while looping the list. Or as the author of the book said:

> Many games record the set of commands every entity performed each frame. To replay the game, the engine just runs the normal game simulation, executing pre-recorded commands.

## The command pattern in Unity

The idea behind these scripts is that you have a gameobject, like a box, and then you can move the box with WASD. But thanks to the command pattern you will also be able to undo movements with Z key and replay all movements from the beginning with the R key.

What you need is a scene with a box and an empty gameobject. Add the script InputHandler.cs to the empty gameobject and drag the box to the script. That's it!

```cs
using UnityEngine;
using System.Collections;
using System.Collections.Generic;

namespace CommandPattern
{
    public class InputHandler : MonoBehaviour
    {
        //The box we control with keys
        public Transform boxTrans;
        //The different keys we need
        private Command buttonW, buttonS, buttonA, buttonD, buttonB, buttonZ, buttonR;
        //Stores all commands for replay and undo
        public static List<Command> oldCommands = new List<Command>();
        //Box start position to know where replay begins
        private Vector3 boxStartPos;
        //To reset the coroutine
        private Coroutine replayCoroutine;
        //If we should start the replay
        public static bool shouldStartReplay;
        //So we cant press keys while replaying
        private bool isReplaying;


        void Start()
        {
            //Bind keys with commands
            buttonB = new DoNothing();
            buttonW = new MoveForward();
            buttonS = new MoveReverse();
            buttonA = new MoveLeft();
            buttonD = new MoveRight();
            buttonZ = new UndoCommand();
            buttonR = new ReplayCommand();

            boxStartPos = boxTrans.position;
        }



        void Update()
        {
            if (!isReplaying)
            {
                HandleInput();
            }

            StartReplay();
        }


        //Check if we press a key, if so do what the key is binded to 
        public void HandleInput()
        {
            if (Input.GetKeyDown(KeyCode.A))
            {
                buttonA.Execute(boxTrans, buttonA);
            }
            else if (Input.GetKeyDown(KeyCode.B))
            {
                buttonB.Execute(boxTrans, buttonB);
            }
            else if (Input.GetKeyDown(KeyCode.D))
            {
                buttonD.Execute(boxTrans, buttonD);
            }
            else if (Input.GetKeyDown(KeyCode.R))
            {
                buttonR.Execute(boxTrans, buttonZ);
            }
            else if (Input.GetKeyDown(KeyCode.S))
            {
                buttonS.Execute(boxTrans, buttonS);
            }
            else if (Input.GetKeyDown(KeyCode.W))
            {
                buttonW.Execute(boxTrans, buttonW);
            }
            else if (Input.GetKeyDown(KeyCode.Z))
            {
                buttonZ.Execute(boxTrans, buttonZ);
            }
        }


        //Checks if we should start the replay
        void StartReplay()
        {
            if (shouldStartReplay && oldCommands.Count > 0)
            {
                shouldStartReplay = false;

                //Stop the coroutine so it starts from the beginning
                if (replayCoroutine != null)
                {
                    StopCoroutine(replayCoroutine);
                }

                //Start the replay
                replayCoroutine = StartCoroutine(ReplayCommands(boxTrans));
            }
        }


        //The replay coroutine
        IEnumerator ReplayCommands(Transform boxTrans)
        {
            //So we can't move the box with keys while replaying
            isReplaying = true;
            
            //Move the box to the start position
            boxTrans.position = boxStartPos;

            for (int i = 0; i < oldCommands.Count; i++)
            {
                //Move the box with the current command
                oldCommands[i].Move(boxTrans);

                yield return new WaitForSeconds(0.3f);
            }

            //We can move the box again
            isReplaying = false;
        }
    }
}
```

This script has collected all the commands we need. You should probably have one class per script when you are making your own command pattern.

```cs
using UnityEngine;
using System.Collections;
using System.Collections.Generic;

namespace CommandPattern
{
    //The parent class
    public abstract class Command
    {
        //How far should the box move when we press a button
        protected float moveDistance = 1f;

        //Move and maybe save command
        public abstract void Execute(Transform boxTrans, Command command);

        //Undo an old command
        public virtual void Undo(Transform boxTrans) { }

        //Move the box
        public virtual void Move(Transform boxTrans) { }
    }


    //
    // Child classes
    //

    public class MoveForward : Command
    {
        //Called when we press a key
        public override void Execute(Transform boxTrans, Command command)
        {
            //Move the box
            Move(boxTrans);
            
            //Save the command
            InputHandler.oldCommands.Add(command);
        }

        //Undo an old command
        public override void Undo(Transform boxTrans)
        {
            boxTrans.Translate(-boxTrans.forward * moveDistance);
        }

        //Move the box
        public override void Move(Transform boxTrans)
        {
            boxTrans.Translate(boxTrans.forward * moveDistance);
        }
    }


    public class MoveReverse : Command
    {
        //Called when we press a key
        public override void Execute(Transform boxTrans, Command command)
        {
            //Move the box
            Move(boxTrans);

            //Save the command
            InputHandler.oldCommands.Add(command);
        }

        //Undo an old command
        public override void Undo(Transform boxTrans)
        {
            boxTrans.Translate(boxTrans.forward * moveDistance);
        }

        //Move the box
        public override void Move(Transform boxTrans)
        {
            boxTrans.Translate(-boxTrans.forward * moveDistance);
        }
    }


    public class MoveLeft : Command
    {
        //Called when we press a key
        public override void Execute(Transform boxTrans, Command command)
        {
            //Move the box
            Move(boxTrans);

            //Save the command
            InputHandler.oldCommands.Add(command);
        }

        //Undo an old command
        public override void Undo(Transform boxTrans)
        {
            boxTrans.Translate(boxTrans.right * moveDistance);
        }

        //Move the box
        public override void Move(Transform boxTrans)
        {
            boxTrans.Translate(-boxTrans.right * moveDistance);
        }
    }


    public class MoveRight : Command
    {
        //Called when we press a key
        public override void Execute(Transform boxTrans, Command command)
        {
            //Move the box
            Move(boxTrans);

            //Save the command
            InputHandler.oldCommands.Add(command);
        }

        //Undo an old command
        public override void Undo(Transform boxTrans)
        {
            boxTrans.Translate(-boxTrans.right * moveDistance);
        }

        //Move the box
        public override void Move(Transform boxTrans)
        {
            boxTrans.Translate(boxTrans.right * moveDistance);
        }
    }


    //For keys with no binding
    public class DoNothing : Command
    {
        //Called when we press a key
        public override void Execute(Transform boxTrans, Command command)
        {
            //Nothing will happen if we press this key
        }
    }


    //Undo one command
    public class UndoCommand : Command
    {
        //Called when we press a key
        public override void Execute(Transform boxTrans, Command command)
        {
            List<Command> oldCommands = InputHandler.oldCommands;

            if (oldCommands.Count > 0)
            {
                Command latestCommand = oldCommands[oldCommands.Count - 1];

                //Move the box with this command
                latestCommand.Undo(boxTrans);

                //Remove the command from the list
                oldCommands.RemoveAt(oldCommands.Count - 1);
            }
        }
    }


    //Replay all commands
    public class ReplayCommand : Command
    {
        public override void Execute(Transform boxTrans, Command command)
        {
            InputHandler.shouldStartReplay = true;
        }
    }
}
```
