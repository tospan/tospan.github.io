---
title: Unity GPP State Pattern
categories: 
  - Unity
tags:
  - Unity
date: 2017-02-05 15:24:22
updated: 2017-02-05 15:24:22
---

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