---
title: Game programming patterns
categories: 
  - Unity
tags:
  - Unity
date: 2017-02-05 14:59:11
updated: 2017-02-05 14:59:11
---


## Introduction
In this tutorial you will learn everything about programming patterns in Unity with C# code. This is how [Wikipedia](https://en.wikipedia.org/wiki/Software_design_pattern) defines programming patterns:

> In software engineering, a software design pattern is a general reusable solution to a commonly occurring problem within a given context in software design. It is not a finished design that can be transformed directly into source or machine code. It is a description or template for how to solve a problem that can be used in many different situations. Design patterns are formalized best practices that the programmer can use to solve common problems when designing an application or system.

<!--more-->

Most of the basic ideas I've used here originate from the free book [Game Programming Patterns](http://gameprogrammingpatterns.com/), so if you want to read more about design patterns you should read it. But the code in that book is in C++ and neither is the code complete, so you can't just take the code, add it to a game engine, and run it. You will here find complete code in C# that you will be able to run in Unity with a few clicks.

The book includes the following design patterns:

1. **Command pattern**. Learn an easy way to rebind keys, how to undo old movements, and how to make a cool replay function.
2. **Flyweight pattern**. Learn how to save memory by reusing what objects have in common.
3. **Observer pattern**. Learn how to make boxes listen for events before they jump.
4. **Prototype**. The basic idea behind this pattern is that an object can spawn other objects similar to itself. But the conclusion from the author of the book is that you don't really need it: "I honestly can't say I've found a case where I felt the Prototype design pattern was the best answer." So I will not cover it here.
5. **Singleton**. With the singleton design pattern you will just be able to create one instance of the class. But the conclusion from the book is that this pattern usually does more harm than good, so it should be used sparingly. I haven't yet come up with a good game-application-example for the pattern, so I will not cover it here. If you need an example of a C# singleton class, there are several on Google.
6. **State pattern**. Learn how to make a simple Minecraft clone where creepers and skeletons are attacking you.
7. **Double Buffer**. According to [Wikipedia](https://en.wikipedia.org/wiki/Multiple_buffering), "multiple buffering is the use of more than one buffer to hold a block of data, so that a reader will see a complete (though perhaps old) version of the data, rather than a partially updated version of the data being created by a writer."

If you want to see it in action you should head to my tutorial on water wakes. There I'm using 2 arrays where I'm calculating the new heights of all points in a mesh by looking at the surrounding heights. I can't add the result of the calculations of a single point to the same array because that will affect the calculations of the other heights because they should use the old surrounding heights. So I'm saving the height in a new array, and then I flip the arrays when I'm done.

8. **Game Loop**. Unity has its own game loop built in and we can't really write our own. But you should read through the book's chapter to get a better understanding of how Unity is really working, like why Time.deltaTime is not constant.

9. **Update Method**. This design pattern is Unity's Update() method. The idea is that the game world has a collection of objects whose behavior has to be updated each frame. Each of these objects has to have an update method, and each frame the game updates every object in the collection.

10. **Bytecode**. Is the most complex pattern in the book. You develop your own programming language called bytecode. This is especially useful if you want your users to modify the game because bytecode can't accidentally reach weird parts of the game engine and it controls how much memory it uses.

11. **Subclass Sandbox**. Learn an easy way to handle all superpowers in your game.

12. **Type Object**. Let's say you have a monster base class and several different monsters inheriting from that base class. The basic idea behind the type object pattern is to give the monster base class a type of monster (or breed as in the book) instead of having several monsters inheriting from the base class. So you will just have two classes: monster and breed, instead of monster, dragon, troll, etc. So each monster in the game is an instance of the class monster, and the breed class contains the information shared between all monsters of the same breed, such as dragon breed.

It will be much easier to create a new breed object than creating yet another subclass. And if you have different types of dragons, then the type of dragon can inherit from a more general type of dragon. This design pattern is extremely useful if you let your users download new breeds of monsters. But the drawback is that it's more difficult to specialize different behaviors the monsters can have.

13. **Component**. The idea behind this pattern is why Unity has different components that you can attach to a gameobject. You can add a sound component, a rigidbody component, and/or a particle system component. What if all of those were a part of the same class? Now you can modify each of the components individually without having to give a single thought to one of the other components. The components are still allowed to talk to each other.

14. **Event Queue**. This design pattern is similar to the Observer pattern, and is used for high level communication between game systems that want to stay decoupled. The event queue is basically a list. When an event has happened (like a key has been pressed), you add the event to the list, and then another part of the game picks the oldest events from the queue and processes it.

This pattern is really useful if you are making a tutorial. You want to avoid mixing the tutorial code with the other game code. So the game code can send events to the event queue, and then the tutorial code picks events from the queue. You can also use it for playing sounds effects by merging the sound effects before adding them to the list if they are the same (and then playing only the loudest). The reason is we can't play several of the same sound effect at the same time. When the sound effects have the same waveform, it's the same as one sound effect played twice as loud.

15. **Service Locator**. A service could be something as simple as a random number generator. It's a system that needs to be available to the entire game. So you can say that the Service Locator pattern is similar to the Singleton pattern. You can also say that the Service Locator is similar to a phone book in which we can find both name and address. But use this pattern only if you have to because making something global is always asking for trouble. Unity is using this pattern together with the Component pattern in the GetComponent() method.

16. **Data Locality**. What you need to know here is that memory is slow while processors are fast. To try to compensate, the CPU is storing a little bit of memory on its own, which is called CPU caching. This is much faster than accessing the other memory, so you have to optimize that little bit of memory. You measure these improvements by measuring the "cache miss." To optimize the chache miss, you have to organize your data structures so that the things you're processing are next to each other in memory. This is easier said than done, but, according to the book, you should avoid unnecessary pointers, not send deactivated gameobjects or other unnecessary data to the cache, and avoid subclassing.

17. **Dirty Flag**. Let's say you have a gameobject with a certain position and rotation. This gameobject has also a child gameobject with its own position and rotation. If the parent gameobject is moving, then we have to recalculate the position of the child (this is costly from a performance point of view), otherwise we don't. To determine if we have to recalculate the position and rotation of the child, we set a boolean called Dirty Flag if the parent has moved. If the parent hasn't moved, we simply use the old position and rotation. Another example is if you let your players build something and then upload the build to the server. Then you can set Dirty Flags to what has actually changed, and upload only the changes.

18. **Object Pool**. If you have a gun that's firing bullets, then you should reuse the bullets instead of destroying them and creating new ones. So when a bullet has hit its target (or left the screen), you deactivate the bullet. When you then fire a new bullet, you activate an old bullet, move it to the gun's position, and give it a speed. It will improve the performance and memory of the game. This pattern has already been covered by Unity themselves: [Object Pooling](https://unity3d.com/learn/tutorials/modules/beginner/live-training-archive/object-pooling).

19. **[Spatial Partition]()**. Learn a fast way to find the closest enemy on a battlefield.


Other patterns than the ones in the book:

**Null Object**. If we can't return an object from a method, it's common to just return null. But that may break the game. So a Null Object is an object we can return instead of just null, but it does nothing, meaning it won't break the game. One example is the DoNothing class in the Command Pattern tutorial.