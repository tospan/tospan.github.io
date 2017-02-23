---
title: Unity对象池(Object Pools)
categories:
  - Unity
tags:
  - Unity
  - Scripting
  - C#
date: 2014-05-01 17:09:23
updated: 2017-02-07 17:09:23
---

One issue was brought to my attention in the comment by broxxar. As mentioned in this thread http://forum.unity3d.com/threads/unityengine-object-name-allocates-for-each-access.237380/ and as I saw it in the profiler, the call gameObject.name creates an allocation of 40B each call.I will later come up with a new version that avoid that problem.
Most games tend to make heavy use of short-lived objects. The first reflex of any programmer is to create a new object when needed and get rid of needed when done with it. Problem is that creating is slow and releasing may lead to a garbage collection which is also slow. One solution would be reuse identical items to avoid those two problems or at least to minimize them.

In this article we will keep it short and simple so you can start using it right on. Later on, we may add some more features.

<!--more-->

## What is a pool?

In our case a pool is just a like a bag in which we store items/objects. Before we start implementing, let’s take a minute to think how and why. Our pool should be a class most obviously, no wonder about that. this class should not be inherited to avoid any wrong doing with the existing code. Finally, there should be only one of it since we want to have all items in one bag and accessible from anywhere. This latest statement is personal and could be argued. One could say that one pool could be for a certain type of object and then other types should have their own pool. Up to you. We will make one to rule them all.

```cs
public sealed class ObjectPool 
{
    private static ObjectPool instance = null;
    public static ObjectPool Instance 
    {
        get
        {
            if (instance==null)
            {
                instance = new ObjectPool();
            }
            return instance;
        }
    }
    public void Reset() 
    {
         instance = null;
    }
    private ObjectPool() {}
}
```

The class is sealed to avoid any sub class (one could argue to make it static), it has a basic singleton pattern and an explicit private constructor as we do not want the user to call many constructor. The Reset method is just there to set the instance to null, the GC will do the rest.

## The interface

Next, we should consider the interface of the class. In this case, we will not create an actual interface, we will simply make some methods public allowing the user to interact with the class. Just like an interface.

```cs
public bool AddToPool(GameObject prefab, int count, Transform parent = null) { }
public GameObject PopFromPool(string prefabName, bool forceInstantiate = false, 
    bool instantiateIfNone = false, Transform container = null) { return null;}
public GameObject PopFromPool(GameObject prefab, bool forceInstantiate = false, 
    bool instantiateIfNone = false, Transform container = null){ return null; }
public void PushToPool( ref GameObject obj, bool retainObject = true, 
    Transform parent = null) { }
public void ReleaseItems(GameObject prefab, bool destroyObject = false) { }
public void ReleasePool() { }
```

Each method represents a moment of the lifecycle of the object.

1. Creation -> AddToPool
2. Usage -> PopFromPool
3. End of usage -> PushToPool
4. Destruction -> ReleaseItems
5. Get rid of the pool -> ReleasePool

The pool is generic, not that it uses generic code but that it returns the highest level of abstraction in Unity, a GameObject reference. The reason is that we use prefab to determine the items so it would make sense to fetch the matching item instead of looking for a type of script. It also makes sense that grabbing a specific game object indirectly means grabbing a set of scripts that it contains.

## The concept

So the concept is simple, we want that providing a certain information, we are given a corresponding item. Somehow, we could say we give a key and we want a value. So the appropriate data collection to be used should be a dictionary since it is just what it does. Then via our interface, we add items to the dictionary, get items from it or reassigned items to it.

Our pool should not only be one key for one value so the best case is a dictionary with a prefab as key and a list of instantiated objects of that prefab as the value.

```cs
private Dictionary<string, Queue<GameObject>> container = new Dictionary<string, Queue<GameObject>>();
private Dictionary<string, GameObject> prefabContainer = new Dictionary<string, GameObject>();
```

Using a dictionary enables the fact we can extend freely the amount of key without constantly checking for a size.
Our dictionary will take a string as key and is linked to a Queue of GameObject instances.

The prefabContainer will store our prefab references so that we can add new items if needed later on in the project.

## Adding items

```cs
public bool AddToPool(GameObject prefab, int count, Transform parent = null)
{
    if(prefab == null || count <= 0){ return false; }
    string name = prefab.name;
    if (this.prefabContainer.ContainsKey(name) == false) 
    {
        this.prefabContainer.Add(name, prefab);
    }
    if (this.prefabContainer[name] == null) 
    {
        this.prefabContainer[name] = prefab;
    }
    for (int i = 0; i < count ; i++)
    {
        GameObject obj = PopFromPool(name, true);
        PushToPool(ref obj, true, parent);
    }
    return true;
}
```

The method returns bool to make sure it all went fine. It takes three parameters, the first one is the prefab we want to instantiate, some bullets for instance. The second parameter indicates how many we wantto begin with (this can be changed later on). The last parameter is not compulsory and is set to null by default. It allows to keep a neat scene hierarchy by storing items under a specific game object. So you may add an empty game object to your scene and call it BulletContainer. Then pass the transform of that object to the method and all newly created bullet will go under it.

First, we do a couple of checks to make sure the call is not done wrong, we make sure the prefab is registered in the prefabContainer, then start a loop to create each object. So far, nothing special since the method we are using are not yet implemented. The method can be used in the Awake or Start of some manager script.

```cs
[SerializeField] private GameObject bulletPrefab = null;
[SerializeField] private Transform bulletContainer = null;
private void Awake()
{
    ObjectPool.Instance.AddToPool(this.bulletPrefab, 20, this.bulletContainer);
}
```

## Getting to create

The first method we used in AddToPool may seem awkward since it tries to get from the pool while we are trying to add into the pool. Let’s look at the implementation.

```cs
public GameObject PopFromPool(GameObject prefab, bool forceInstantiate = false, 
    bool instantiateIfNone = false, Transform container = null)
{
    if(prefab == null) { return null; }
    return PopFromPool(prefab.name, forceInstantiate);
}

public GameObject PopFromPool(string prefabName, bool forceInstantiate = false, 
    bool instantiateIfNone = false, Transform container = null)
{
    if (prefabName == null|| this.prefabContainer.ContainsKey(prefabName) == false) 
    { 
        return null; 
    }
    if (forceInstantiate == true) { return CreateObject(prefabName, container); }
    GameObject obj = null;
    Queue<GameObject> queue = FindInContainer(prefabName);
    if (queue.Count > 0)
    {
        obj = queue.Dequeue();
        obj.transform.parent = container;
        obj.SetActive(true);
    }
    if (obj == null && instantiateIfNone == true)
    {
        return CreateObject(prefabName, container);
    }
    return obj;
}
```

The method returns a GameObject reference and takes the prefab as first parameter a boolean indicating whether we want to force the instantiation or not, then whether we want to create if we did not find any available item and finally a container transform. All those parameters are just there to be used in specific situation so they can be left as default. It is all up to the situation to define whether they are useful or not. You may consider the latest one for instance to be useless, it may be most of the time.

The first method is used when we have the reference to the prefab (probably in a manager), the second method will use the name of the prefab so a string will do, no need for a prefab reference.

Again, we first check if we got all we need. If we have forceInstantiate as true, it means we are simply creating a new object. Comes a call for a new method we will look at in a moment. This method returns a Queue<GameObject> reference. and if the collection is not empty, we dequeue an item out of it. In the case the queue was not empty but we did not get anything out (a reference may be null) or the queue was empty, it will get to the next stage where we check if the instantiateIfNone is set. Then we create a new instance.

You may wonder the difference between forceInstantiate and instantiateIfNone. The first is used when you want a new object whatever happens, at start or you are just adding new items to the level. Even though you have a full Queue, you would still get a new item. The second only creates if something went wrong or the Queue was empty. This comes for instance when you have been shooting all the bullets and you do not want to remain empty on a new shot of your player.

Next, let’s look at the FindInContainer method:

```cs
private Queue<GameObject> FindInContainer(string prefabName)
{
    if(this.container.ContainsKey(prefabName) == false)
    {
        this.container.Add(prefabName, new Queue<GameObject>());
    }
    return this.container[prefabName];
}
```

The FindInContainer is private since the user does not need it. Our container is the dictionary we already created. The purpose is just to get a reference to the Queue corresponding to the prefab (the prefab name in our case). Nothing too fancy here. Let’s move on.

And now the CreateObject method:

```cs
private GameObject CreateObject(string prefabName, Transform container)
{
    GameObject obj = (GameObject)Object.Instantiate(prefabContainer[prefabName]);
    obj.name = prefabName;
    obj.transform.parent = container;
    return obj;
}
```

This method does not do anything you should not understand by yourself.

## Give back what you are given

```cs
public void PushToPool( ref GameObject obj, bool retainObject = true, Transform parent = null)
{
    if(obj == null) { return; }
    if(retainObject == false)
    {
        Object.Destroy(obj);
        obj = null;
        return;
    }
    if(parent != null)
    {
        obj.transform.parent = parent;
    }
    Queue<GameObject> queue = FindInContainer(obj.name);
    queue.Enqueue(obj);
    obj.SetActive(false);
    obj = null;
}
```

The method returns void and takes three parameters. The first is a ref to a GameObject, second defines if we want to keep the object, the latest is the container to assign the object. This is quite simple to understand here, the only problem could come from the ref parameter. Why would we want that? Think about your own code in which you would use this one:

```cs
void MyMethod()
{
    // Some code and something happened with a bullet object
    GameObject obj = GetThatBulletReference();
    // Doing some stuff with obj reference
    if(condition == true){ // Any reason
    PushToPool(ref obj,false, containerTransform);
    else{ // other}
    // More code with obj => Wrong
}
```

In this code, based on a condition, the bullet is returned to the pool. But the code keeps on using the bullet later on regardless if it is still in use or not. Obviously, this code is wrong and may never happen but it is just for the sake of demonstration. Back to our code, it seems we still have a reference to our bullet object despite the fact it is back in the pool and deactivated. Using the ref keyword, it means we have access to the reference outside the method and we can set to null. As a result, we cannot modify the dead object since we have no link to it anymore.

In our method, the obj reference is at 0x0055 and contains the value 0x00AA where the bullet object is stored. When we call the PushToPool method, the parameter is given 0x0055 instead of 0x00AA, so we can control the obj reference directly. In the method it is set to null, so out of PushToPool, the obj reference inside MyMethod is null.

If you decide not to retain the item in the pool, then it gets destroyed. Now you may ask “What if we want to keep it without setting it in the pool?” well, do so. Just don’t call the method (I know some of you will think of that despite being obvious).

## Clean up after yourself

```cs
public void ReleaseItems(GameObject prefab, bool destroyObject = false)
{
     if( prefab == null){ return; }
     Queue<GameObject> queue = FindInContainer(prefab.name);
     if (queue == null)  { return; }
     while (queue.Count > 0) 
     {
         GameObject obj = queue.Dequeue();
         if (destroyObject == true) 
         {
              Object.Destroy(obj);
         }
     }
}
 
public void ReleasePool()
{
    foreach (var kvp in this.container)
    {
        Queue<GameObject> queue = kvp.Value;
        while(queue.Count > 0)
        {
            GameObject obj = queue.Dequeue();
            Object.Destroy(obj);
        }
    }
    this.container = null;
    this.container = new Dictionary<string, Queue<GameObject>>();
    this.prefabContainer.Clear();
    this.prefabContainer = null;
    this.prefabContainer = new Dictionary<string,GameObject>();
}
```

We are finally getting to the end, so ReleaseItems is somehow similar to PushToPool with retain as false. The only major difference is the fact that all items of a kind will be destroyed witht that method. Considers you want to get rid of all kind of bullets, use this one. ReleasePool does what it says, it empties the pool and set it ready for a new round.

## Practical case

To sum it up first let’s wrap the whole code and then use it quickly in an example. Please do keep in mind, there may have been some details you may find wrong, just let me know and I can explain my decision and maybe (I doubt it though) admit I could be wrong and improve the code.

Here it is:

[github-mark@1200x630](https://github.com/fafase/unity-utilities/blob/master/Scripts/ObjectPoolString.cs)

And a simplified use case:

```cs
[SerializeField] private GameObject enemyPrefab = null;
[SerializeField] private GameObject bulletPrefab = null;
[SerializeField] private Transform enemyContainer = null;
[SerializeField] private Transform weaponContainer = null;
// This needs to match the prefab name 
// I just made it simple here 
public const string PrefabName = "bulletPrefab"; 
private void Start()
{
    ObjectPool.Instance.AddToPool(this.enemyPrefab, 10 , this.enemyContainer);
    ObjectPool.Instance.AddToPool(this.bulletPrefab, 100, this.weaponContainer);
}
  
private void Shoot()
{
     // Using the prefab name
     var obj = ObjectPool.Instance.PopFromPool(PrefabName, false, true, this.transform);
}
  
private void Hit(GameObject bullet)
{
    ObjectPool.Instance.PushToPool(ref bulletPrefab, ref bullet, bulletContainer);
}
```


Following the previous article on object pool, some legit comments came up concerning memory allocation.

## The issue

When coding, it is required to find the most efficient code, that is the fastest way to solve a problem (fastest is not necessarily shortest). But sometimes in order to get fast, we hit some other trouble. In the previous article, the code could be considered fast (to some extents), but one line would be problematic. It happens that Unity runs some internal code when using

```cs
gameObject.name;
```

Despite the fact that name is just a property, Unity runs some deeper code that generates some memory allocation. Exactly, it seems to allocate 40B for each call.

The call happening during the creation of the object is no issue since it happens only once in the object lifetime, the problematic was during the pushing back in the pool:

```cs
public void PushToPool( ref GameObject obj, bool retainObject = true, Transform parent = null)
{
    if(obj == null) { return; }
    if(retainObject == false)
    {
        Object.Destroy(obj);
        obj = null;
        return;
    }
    if(parent != null)
    {
        obj.transform.parent = parent;
    }
    Queue<GameObject> queue = FindInContainer(obj.name);
    queue.Enqueue(obj);
    obj.SetActive(false);
    obj = null;
}
```

The FindInContainer method uses the name of the prefab as parameter and that is where it goes wrong.

So we need to figure out a way to get the prefab without using the name.

## Getting a solution

The solution is simple, instead of storing the prefab in a dictionary (string,prefab), we will add a component to the pooled object. That component will contain only one item, a reference to the prefab.

This way, we save the allocation generated by the call to .name. On the other hand, we have a bigger constant memory usage since the component will be stored in memory for the whole object lifetime. Also, we lose that tiny efficiency as well on the call to the component to get the prefab.

As you can see, one problem could not entirely be solved without adding something else. This is one of the main programming paradigm, finding the balance between efficiency, memory allocation and memory usage.

## The new interface

If you read the other object pooling article, you will see that this one is fairly similar but shorter. I would recommend to start with it first and back to this one.

The new interface is nothing more than a storage for a prefab reference and an init method:

```cs
public class IPoolObject : MonoBehaviour
{
   GameObject Prefab{ get; set; }
   void Init();
}
```

This is added to any component on an object that should be pooled. The compiler will then asked to implement the method and property. The Init method is there to reset the values on a pooled  component. If nothing should be done, just leave the method implementation empty. You can use to reset health, ammo or else.

## The pool

The class containing the pool is about to start just like the other article, so I wont repeat myself and just paste the code:

```cs
public sealed class ObjectPool
{
    private static ObjectPool instance = null;
    public static ObjectPool Instance
    {
        get
        {
            if (instance == null)
            {
                instance = new ObjectPool();
            }
            return instance;
        }
    }
    public void Reset()
    {
        instance = null;
    }
    private ObjectPool() { }
}
```

Here goes the interface of our pool (interface as in public interface not actual interface, well,…):

```cs
public bool AddToPool(GameObject prefab, int count, Transform parent = null) { return false; }
public GameObject PopFromPool(GameObject prefab, bool forceInstantiate = false,
        bool instantiateIfNone = false, Transform container = null) { return null; }
public void PushToPool(ref GameObject obj, bool retainObject = true, Transform parent = null) { }
public void ReleaseItems(GameObject prefab, bool destroyObject = false) { }
public void ReleasePool() { }
}
```

There is no more overload of the AddToPool method and we got rid of all string iterations.

## Creating items

The first method we want to call is the AddToPool method which will create our items.

```cs
public bool AddToPool(GameObject prefab, int count, Transform parent = null) 
{
   if (prefab == null || count <= 0) { return false; }
   string name = prefab.name;
   for (int i = 0; i < count; i++)
   {
       GameObject obj = PopFromPool(prefab, true);
       PushToPool(ref obj, true, parent);
   }
   return true;
}
```

The method just runs a loop to create all needed items. As soon as they get created, they also get pushed back to the pool. The return value is only there to inform it all went fine.
There is not much fancy happening so let’s move on.

```cs
public GameObject PopFromPool(GameObject prefab, bool forceInstantiate = false,
    bool instantiateIfNone = false, Transform container = null)
{
    if (forceInstantiate == true) { return CreateObject(prefab, container); }
    GameObject obj = null;
    Queue<GameObject> queue = FindInContainer(prefab);
    if (queue.Count > 0)
    {
        obj = queue.Dequeue();
        obj.transform.parent = container;
        obj.SetActive(true);
        IPoolObject poolObject =  (IPoolObject)obj.GetComponent(typeof(IPoolObject));
        poolObject.Init();
    }
    if (obj == null && instantiateIfNone == true)
    {
        return CreateObject(prefab, container);
    }
    return obj;
}
```

So if you read the previous article, you should notice some changes here. There is no usage of any string name and all happens with prefab reference. Mostly, we create the object and return it if it is forced. If we forced the creation, we mean to use it right away so there is no need to push it to the pool. In our initial run, we have to push it back manually. The FindInContainer receives a prefab reference and returns a queue of GameObject (if found). Then we dequeue from the queue, set as active, set the parent and return. If something went wrong with the queue, we try a new creation and return it. Notice how we get the interface and call the method. First off, we know that the object contains the interface so there is no need to check, then we call the Init method regardless of the object we are dealing with. The bonus of interfaces is that you don’t need to know what you are dealing with as long as it is a IPoolObject.

NOTE: I use the non-generic version of GetComponent with interfaces as it used to be that Unity would not support it. Unity5 seems to have fixed that issue and you can freely use your interface in generic methods. For those of you still using Unity4.x, I can’t guarantee that generic will work so use the shown version.

```cs
private Dictionary<GameObject, Queue<GameObject>>container = new Dictionary<GameObject, Queue<GameObject>>();
private Queue<GameObject> FindInContainer(GameObject prefab)
{
    if (container.ContainsKey(prefab) == false)
    {
        container.Add(prefab, new Queue<GameObject>());
    }
    return container[prefab];
}
```

The FindInContainer method adds the prefab to the dictionary with a new queue of GameObject and returns a reference to that queue.

## Creating the object

```cs
private GameObject CreateObject(GameObject prefab, Transform container)
{
    IPoolObject poolObjectPrefab = (IPoolObject)prefab.GetComponent(typeof(IPoolObject));
    if(poolObjectPrefab== null){Debug.Log ("Wrong type of object"); return null;}
     
    GameObject obj = (GameObject)Object.Instantiate(prefab);
    IPoolObject poolObject = (IPoolObject)obj.GetComponent(typeof(IPoolObject));
    obj.name = prefab.name;
    poolObject.Prefab = prefab;
    obj.transform.parent = container; 
    return obj   
}
```

The first part is to make sure the prefab that was passed contains a script that implemented the interface. Then it is just instantiating the object and setting some info. Mainly, we are also passing the current prefab to the prefab reference in the IPoolObject interface.

## Returning the item to the pool

Consider a bullet you have been taking out of the pool, it just hit an enemy player. Thanks to the collision callback (or any other means you may be using), you have a reference to the bullet:

```cs
void OnCollisionEnter(Collision col){
    if(col.collider.tag == "Bullet"){
        GameObject bulletRef = col.collider.gameObject;
        PushToPool(ref bulletRef, true, someContainer.transform);
        // bulletRef is nullified in the method, you cannot use it anymore!!
    }
}
```

Using that reference, we can push the item back in pool:

```cs
public void PushToPool(ref GameObject obj, bool retainObject = true, Transform parent = null)
{
    if (obj == null) { return; }
    if (retainObject == false)
    {
        Object.Destroy(obj);
        obj = null;
        return;
    }
    if (parent != null)
    {
        obj.transform.parent = parent;
    }
    IPoolObject poolObject = (IPoolObject)obj.GetComponent(typeof(IPoolObject));
    GameObject prefab = null;
    if(poolObject == null){ return;}
    prefab = poolObject.Prefab; 
    Queue<GameObject> queue = FindInContainer(prefab);
    queue.Enqueue(obj);
    obj.SetActive(false);
    obj = null;
}
```

We are passing the reference to the GameObject as ref, which means we get a reference to the reference and not to the object directly. This means we can modify the passed reference from within the method. This way we affect bulletRef in OnCollisionEnter. Without ref, the compiler would be copying the value stored in bulletRef and we would be working with that local copy, but we would not have any access to bulletRef itself. We want to make sure that bulletRef becomes unusable after the item has been pooled back. This prevents the consumer of the ObjectPool to push back and then continue using the bullet reference.

Back to the method implementation, if we set to not retain the item, it gets destroyed.

We parent to the container that has been defined and then we fetch the component containing the reference to the prefab. We make sure the object contains the component. If it fails, the method simply returns, it could (or should) inform further with returning false/null. Then we get the queue corresponding and enqueue the item. Finally we set the reference to null so that the item is no more usable outside.

## Cleaning

The cleaning of the pool is similar to the ones in the other article, except for the fact that only one container is remaining.

```cs
public void ReleaseItems(GameObject prefab, bool destroyObject = false)
{
    if (prefab == null) { return; }
    Queue<GameObject> queue = FindInContainer(prefab);
    if (queue == null) { return; }
    while (queue.Count > 0)
    {
        GameObject obj = queue.Dequeue();
        if (destroyObject == true)
        {
            Object.Destroy(obj);
        }
     }
}

public void ReleasePool() {
    foreach (var kvp in container)
    {
        Queue<GameObject> queue = kvp.Value;
        while (queue.Count > 0)
        {
            GameObject obj = queue.Dequeue();
            Object.Destroy(obj);
        }
    }
    container = null;
    container = new Dictionary<GameObject, Queue<GameObject>>();
}
```

The first ReleaseItems is used to get rid of a type of items, while ReleasePool is clearing the whole pool.

## Conclusion

This is it for the second version. We got rid of the memory allocation (at least I could not see any in the profiler). Obviously, there is a little more done under the hood but that should not affect the runtime. Also, the Init method got added which enables a nice reset feature.

Any comments, go ahead (only positif ones will make it through moderation).

Find the script on the link below.

[github-mark@1200x630](https://github.com/fafase/unity-utilities/blob/master/Scripts/ObjectPool.cs)


## 原文链接

* [Object pooling](https://unitygem.wordpress.com/object-pooling/)
* [Object polling v2](https://unitygem.wordpress.com/object-pooling-v2/)

另外，原文的评论也很赞！


--------------
另外一篇

Object Pools Keeping Things Alive

* Create a fountain of stuff with physics.
* Make objects poolable.
* Add functionality to prefabs.
* Generate pools on demand.
* Survive recompiles and scene transitions.

This tutorial is about objects pools. What are they, how do they work, and are they useful?

The tutorial follows Frames per Second. We will be spawning objects in a similar way, and the FPS counter is useful to measure performance.

![Create pools to feed your fountain](https://raw.githubusercontent.com/tospan/tospan.github.io/source/source/_images/unity/object-pools-tutorial-image.jpg)

<!--more-->

## Spawning Lots of Stuff

Object pools are a tool to reuse objects, which is relevant when you are creating and destroying lots of them. So we should begin by spawning lots of objects. And to measure performance, we can reuse the frame rate counter from the previous tutorial. You can grab just the counter bits and delete everything else. It is easiest if you open its scene, turn the counter object into a prefab, and then use that in a new scene for this tutorial.

![Reusing the frame rate counter:assets](https://raw.githubusercontent.com/tospan/tospan.github.io/source/source/_images/unity/counter-assets.png)  
![Reusing the frame rate counter:object](https://raw.githubusercontent.com/tospan/tospan.github.io/source/source/_images/unity/counter-object.png)  
![Reusing the frame rate counter:scene](https://raw.githubusercontent.com/tospan/tospan.github.io/source/source/_images/unity/counter-scene.png)

So we need stuff to spawn. And to make it interesting, let's make it physics stuff. We can start with a very simple component for that, similar to the nucleon of the previous tutorial.

```cs
using UnityEngine;

[RequireComponent(typeof(Rigidbody))]
public class Stuff : MonoBehaviour {

	Rigidbody body;

	void Awake () {
		body = GetComponent<Rigidbody>();
	}
}
```

Create a standard cube and sphere, and add this component to both of them. Then turn them into prefabs.

![cube prefab](https://raw.githubusercontent.com/tospan/tospan.github.io/source/source/_images/unity/cube-prefab.png)  
![stuff prefabs](https://raw.githubusercontent.com/tospan/tospan.github.io/source/source/_images/unity/stuff-prefabs.png)

The next thing we need is a spawner of stuff, much like the nucleon spawner.

```cs
using UnityEngine;

public class StuffSpawner : MonoBehaviour {

	public float timeBetweenSpawns;

	public Stuff[] stuffPrefabs;

	float timeSinceLastSpawn;

	void FixedUpdate () {
		timeSinceLastSpawn += Time.deltaTime;
		if (timeSinceLastSpawn >= timeBetweenSpawns) {
			timeSinceLastSpawn -= timeBetweenSpawns;
			SpawnStuff();
		}
	}

	void SpawnStuff () {
		Stuff prefab = stuffPrefabs[Random.Range(0, stuffPrefabs.Length)];
		Stuff spawn = Instantiate<Stuff>(prefab);
		spawn.transform.localPosition = transform.position;
	}
}
```

Create an object with this component. Set the time between spawns to something small, like 0.1. Then put references to the cube and sphere prefabs in the stuff array.

![spawner](https://raw.githubusercontent.com/tospan/tospan.github.io/source/source/_images/unity/spawner.png)  
![spawn](https://raw.githubusercontent.com/tospan/tospan.github.io/source/source/_images/unity/spawn.png)
A spawner of stuff to spawn stuff.

Now we have a spawner that create cubes and spheres at one point. It doesn't look like much. If we give the spawner access to the stuff's body, it can give them a starting velocity. So turn Stuff.body into a property.

```cs
	public Rigidbody Body { get; private set; }

	void Awake () {
		Body = GetComponent<Rigidbody>();
	}
```

Let's add a configurable velocity to StuffSpawner, which it will use to give its spawns an initial upward momentum.

```cs
	public float velocity;

	void SpawnStuff () {
		Stuff prefab = stuffPrefabs[Random.Range(0, stuffPrefabs.Length)];
		Stuff spawn = Instantiate<Stuff>(prefab);
		spawn.transform.localPosition = transform.position;
		spawn.Body.velocity = transform.up * velocity;
	}
```

![velocity](https://raw.githubusercontent.com/tospan/tospan.github.io/source/source/_images/unity/velocity.png)  
![a tower of spawn](https://raw.githubusercontent.com/tospan/tospan.github.io/source/source/_images/unity/tower-of-stuff.png)
Building a tower of stuff, bottom-up.

Now we get a neat rising tower of stuff, which quickly collapses, then rises again. If you tilt the spawner, then it will start to look more like a natural flow of stuff. In fact, if we were to put multiple spawners in a ring, it will start to resemble an elaborate fountain. Let's create a component to do just that.

```cs
using UnityEngine;

public class StuffSpawnerRing : MonoBehaviour {

	public int numberOfSpawners;
	
	public float radius, tiltAngle;
	
	public StuffSpawner spawnerPrefab;

	void Awake () {
		for (int i = 0; i < numberOfSpawners; i++) {
			CreateSpawner(i);
		}
	}
}
```

To position the spawners, it is easiest to create a rotater object for each. That way we can deal with the placement and rotations in isolation.

```cs
	void CreateSpawner (int index) {
		Transform rotater = new GameObject("Rotater").transform;
		rotater.SetParent(transform, false);
		rotater.localRotation =
			Quaternion.Euler(0f, index * 360f / numberOfSpawners, 0f);

		StuffSpawner spawner = Instantiate<StuffSpawner>(spawnerPrefab);
		spawner.transform.SetParent(rotater, false);
		spawner.transform.localPosition = new Vector3(0f, 0f, radius);
		spawner.transform.localRotation = Quaternion.Euler(tiltAngle, 0f, 0f);
	}
```

Now turn the spawner into a prefab and make a new spawner ring object.

![ring](https://raw.githubusercontent.com/tospan/tospan.github.io/source/source/_images/unity/ring.png)  
![Definitely a lot of stuff.](https://raw.githubusercontent.com/tospan/tospan.github.io/source/source/_images/unity/much-stuff.png)


We now have a fountain of stuff that just keeps on spawning more, resulting in an endless column of falling objects. To prevent our app from choking, we have to get rid of all this stuff at some point. We can do so by introducing a kill zone. All stuff that enters this zone should be destroyed.

Create an object with a box collider marked as a trigger, set it to a very large size, and place it somewhere below the fountain. Then give it a Kill Zone tag so we know its function. This tag doesn't exist, you'll have to add it yourself, which you can do via the Add Tag... option in the tag selector. Note that tags will not be included when importing a unitypackage file, you'll have to add them to your project so they show up correctly.

![kill zone](https://raw.githubusercontent.com/tospan/tospan.github.io/source/source/_images/unity/kill-zone.png)  
![A kill zone tag](https://raw.githubusercontent.com/tospan/tospan.github.io/source/source/_images/unity/tag.png)

Now Stuff can respond to entering a trigger by checking whether it is now in a kill zone, and if so terminate itself.

```cs
	void OnTriggerEnter (Collider enteredCollider) {
		if (enteredCollider.CompareTag("Kill Zone")) {
			Destroy(gameObject);
		}
	}
 
```
![](https://raw.githubusercontent.com/tospan/tospan.github.io/source/source/_images/unity/killed-spawn.png)
![Stuff gets terminated](https://raw.githubusercontent.com/tospan/tospan.github.io/source/source/_images/unity/PositiveRichHatchetfish.mp4)

> Why use a tag?
> Comparing tags is quick and our kill zone doesn't need to do anything, so it doesn't need a component of its own.

## Adding Variation

We have our fountain, but it's rather orderly. It could use some more randomness. We can do this by replacing fixed values with random values that fall inside some range. Because we can do this with multiple values, let's make a convenient structure for it.

```cs
using UnityEngine;

[System.Serializable]
public struct FloatRange {

	public float min, max;
	
	public float RandomInRange {
		get {
			return Random.Range(min, max);
		}
	}
}
```

Now we can mess up the timing of StuffSpawner.

```cs
	public FloatRange timeBetweenSpawns;

	float currentSpawnDelay;

	void FixedUpdate () {
		timeSinceLastSpawn += Time.deltaTime;
		if (timeSinceLastSpawn >= currentSpawnDelay) {
			timeSinceLastSpawn -= currentSpawnDelay;
			currentSpawnDelay = timeBetweenSpawns.RandomInRange;
			SpawnStuff();
		}
	}
```

![Irregular spawning](https://raw.githubusercontent.com/tospan/tospan.github.io/source/source/_images/unity/spawn-time-range.png)

Why not randomize the stuff's scale and rotation as well?

```cs
	public FloatRange timeBetweenSpawns, scale;

	void SpawnStuff () {
		Stuff prefab = stuffPrefabs[Random.Range(0, stuffPrefabs.Length)];
		Stuff spawn = Instantiate<Stuff>(prefab);
		
		spawn.transform.localPosition = transform.position;
		spawn.transform.localScale = Vector3.one * scale.RandomInRange;
		spawn.transform.localRotation = Random.rotation;
		
		spawn.Body.velocity = transform.up * velocity;
	}
```

![The little things also count](https://raw.githubusercontent.com/tospan/tospan.github.io/source/source/_images/unity/scale-range.png)

We can also vary the velocity, but let's make this a 3D randomization. We leave the base velocity unchanged, but also add a random velocity in an arbitrary direction.

```cs
	public FloatRange timeBetweenSpawns, scale, randomVelocity;

	void SpawnStuff () {
		…
		
		spawn.Body.velocity = transform.up * velocity +
			Random.onUnitSphere * randomVelocity.RandomInRange;
	}
```

![Extra random velocity](https://raw.githubusercontent.com/tospan/tospan.github.io/source/source/_images/unity/random-velocity-range.png)

And as physics allows for angular momentum as well, add a random angular velocity too. Keep in mind that the physics engine limits this velocity to keep the simulation stable. The default maximum is 7.

```cs
	void SpawnStuff () {
		…

		spawn.Body.velocity = transform.up * velocity +
			Random.onUnitSphere * randomVelocity.RandomInRange;
		spawn.Body.angularVelocity =
			Random.onUnitSphere * angularVelocity.RandomInRange;
	}
```

![angular velocity](https://raw.githubusercontent.com/tospan/tospan.github.io/source/source/_images/unity/angular-velocity-range.png)
![randomized](https://raw.githubusercontent.com/tospan/tospan.github.io/source/source/_images/unity/randomized-stuff.png) 
![stuff](https://raw.githubusercontent.com/tospan/tospan.github.io/source/source/_images/unity/MiniatureDefenselessGeese.mp4)

Adding some initial spin.

Our fountain of stuff is looking a lot more lively by now. Adding some color would make it even more interesting to watch. We could have spawners select randomly from multiple materials, but let's just give each spawner one material to work with.

```cs
	public Material stuffMaterial;

	void SpawnStuff () {
		…
		
		spawn.GetComponent<MeshRenderer>().material = stuffMaterial;
	}
```

![Configurable material](https://raw.githubusercontent.com/tospan/tospan.github.io/source/source/_images/unity/material.png)

> Why materials and not just colors?
> Assigning colors to the stuff's material would result in each instance getting its own material. This way all materials remain shared.

Of course we won't just use one material. Instead, create a bunch and let StuffSpawnerRing cycle through them while it creates the spawners.

```cs
	public Material[] stuffMaterials;

	void CreateSpawner (int index) {
		…

		spawner.stuffMaterial = stuffMaterials[index % stuffMaterials.Length];
	}
```

> What does % do?
> x % y computes the remainder of x divided by y. So we get repeating sequences going from 0 to stuffMaterials.Length - 1.

![materials](https://raw.githubusercontent.com/tospan/tospan.github.io/source/source/_images/unity/ring-materials.png)  
![A colored fountain of stuff](https://raw.githubusercontent.com/tospan/tospan.github.io/source/source/_images/unity/colored-stuff.png)



Our scene will look even more lively if the stuff has something to interact with besides itself. So add a large sphere at the center of the scene. I gave it a uniform scale of 15 and also increased the stuff's velocity to 17 so it has a better chance to land on top of the sphere.

![](https://raw.githubusercontent.com/tospan/tospan.github.io/source/source/_images/unity/sphere.png)
![](https://raw.githubusercontent.com/tospan/tospan.github.io/source/source/_images/unity/WeightyBronzeFawn.mp4)
More interesting physics interactions.

Now stuff can bounce around a bit and maybe spend some time on the sphere, before it inevitable ends up in the kill zone. You can add more obstacles to the scene, but the longer you delay stuff from getting to a kill zone, the more work the physics engine has to do.


## Making Heavier Stuff

Ok, so we're spawing stuff. How's the performance of our app? Check the profiler and see what's going on. Make a build and profile that as well, so you see how it runs as a standalone app. As builds performs better than playing in the editor, you could increase the number of spawners to 30 or higher, depending your your machine.

You will see continuous memory allocation and maybe garbage collection runs, which is not surprising. You could see anywhere from a hundred to a thousand bytes being allocated each frame, depending on randomness and the frame rate that your app is running at.

Is this memory allocation significant to warrant special attention? On desktops most likely not. Rendering and physics keep the machine busy, our scripts barely register on the profiler graph.

![Our code – the blue line – is not a bottleneck in this case](https://raw.githubusercontent.com/tospan/tospan.github.io/source/source/_images/unity/profiler.png)


Of course this picture might change for other devices. Mobiles and consoles typically have less and slower memory than desktops and laptops. You can end up with frequent garbage collection runs, which can wreak havoc on your app's frame rate. But you have to try and measure it before you can be sure.

For a desktop app, our current approach of simply creating and destroying objects as needed seems perfectly fine. There's no need to add complexity to our app trying to solve a non-exiting problem. But maybe our objects are just too simple? What if our stuff was more complex?

We need bigger stuff! Fox example, a cross made from cubes. Create three cubes, make them children of an empty game object, and set their scales to the three possible permutations of (0.5, 0.5, 2.5). Then add a Stuff component to the root object and turn the whole thing into a prefab. I set its mass to 3 because it's a larger object than the other stuff.

![cross](https://raw.githubusercontent.com/tospan/tospan.github.io/source/source/_images/unity/cross.png)  
![Cross prefab preview](https://raw.githubusercontent.com/tospan/tospan.github.io/source/source/_images/unity/cross-preview.png)

Add this new type of stuff to the spawner prefab's array so it will be included in the fountain.

![Cubes, spheres, and crosses](https://raw.githubusercontent.com/tospan/tospan.github.io/source/source/_images/unity/array-with-cross.png)

Entering play mode right now will give us trouble when trying to assigning materials. Our assumption that stuff objects always have a MeshRenderer is no longer true. So let's move the responsibility of setting the material to Stuff itself. Internally, it can collect all renderers inside itself when it awakens. Then it can forward material assignments to them all when needed.

```cs
	MeshRenderer[] meshRenderers;

	public void SetMaterial (Material m) {
		for (int i = 0; i < meshRenderers.Length; i++) {
			meshRenderers[i].material = m;
		}
	}
	
	void Awake () {
		Body = GetComponent<Rigidbody>();
		meshRenderers = GetComponentsInChildren<MeshRenderer>();
	}
```

Now StuffSpawner just has to invoke this method.

```cs
	void SpawnStuff () {
		…

		spawn.SetMaterial(stuffMaterial);
	}
```

![More complex stuff in the fountain](https://raw.githubusercontent.com/tospan/tospan.github.io/source/source/_images/unity/fountain-with-cross.png)

What about we add caltrops as well? A caltrop consists of four legs that point away from each other. You can put capsules inside those legs to visualize them, with a Y offset of 0.5. The first leg points upwards. The second has rotation (70.52878, 180, 180). The other two have the same X and Z rotation, but their Y rotation should be 300 and 60.

![caltrops](https://raw.githubusercontent.com/tospan/tospan.github.io/source/source/_images/unity/caltrop.png)  
![Caltrop prefab preview](https://raw.githubusercontent.com/tospan/tospan.github.io/source/source/_images/unity/caltrop-preview.png)

Add the caltrop to the spawner's stuff prefab array as well. You can increase the probability of an option being chosen by adding it more than once.

![prefabs](https://raw.githubusercontent.com/tospan/tospan.github.io/source/source/_images/unity/array-with-caltrop.png) 
![ A messy fountain fountain](https://raw.githubusercontent.com/tospan/tospan.github.io/source/source/_images/unity/fountain-with-caltrops.png)
.

How's our app's performance now? We're getting more memory allocations, but it is still not an issue for a desktop app. The physics calculations are much more important. Something drastic is needed to make the creation of stuff matter. For example, add the following line a few times to Stuff.Awake.

```cs
		FindObjectsOfType<Stuff>();
```

![This warrants attention](https://raw.githubusercontent.com/tospan/tospan.github.io/source/source/_images/unity/profiling-finding-objects.png)


That did it. Object creation is now significant for our desktop app. Invoking FindObjectsOfType just five times per stuff generates roughly 200KB of memory allocations per frame for me. But this is a ridiculous scenario. Object pooling isn't the solution for this, getting rid of those FindObjectsOfType invocations is. And have a chat with whoever thought it a good idea to put that code there.


## Pooling Objects

Suppose that we do want to use object pooling. How would we do that? We reuse objects by not destroying them, but instead deactivating them and placing them in a buffer. That's our pool. Then whenever we need a new object, we can reactivate one from the pool. If none are available, we create a new object as usual.

Let's create a component for these objects. It needs to know which pool it belongs to, so it can return to it when needed. And just in case it ends up without a pool, it should destroy itself instead.

```cs
using UnityEngine;

public class PooledObject : MonoBehaviour {

	public ObjectPool Pool { get; set; }

	public void ReturnToPool () {
		if (Pool) {
			Pool.AddObject(this);
		}
		else {
			Destroy(gameObject);
		}
	}
}
```

Now we can turn Stuff into a pooled object and have it return to its pool when entering a kill zone.

```cs
public class Stuff : PooledObject {
	
	…

	void OnTriggerEnter (Collider enteredCollider) {
		if (enteredCollider.CompareTag("Kill Zone")) {
			ReturnToPool();
		}
	}
}
```

Of course we now need an `ObjectPool` component. It should be possible to get an object from it and return objects to it. For now, let's just have it create and destroy objects the old-fashioned way and worry about actual reuse later.

While we are here though, let's use the pool as the parent for everything that it creates, so those objects don't clutter the root of the hierarchy.

```cs
using UnityEngine;
using System.Collections.Generic;

public class ObjectPool : MonoBehaviour {

	PooledObject prefab;

	public PooledObject GetObject () {
		PooledObject obj = Instantiate<PooledObject>(prefab);
		obj.transform.SetParent(transform, false);
		obj.Pool = this;
		return obj;
	}

	public void AddObject (PooledObject o) {
		Object.Destroy(o.gameObject) ;
	}
}
```

Next, we have to change `StuffSpawner` so it uses a pool to create stuff, instead of instantiating new stuff all the time. How can we do this? It has to somehow get hold of a pool for each prefab. And we don't want duplicate pools, so all spawners should share them.

It would be very convenient if we could directly get a pooled instance from a prefab, without having to worry about the pools themselves. So let's pretend that we can.

```cs
	void SpawnStuff () {
		Stuff prefab = stuffPrefabs[Random.Range(0, stuffPrefabs.Length)];
		Stuff spawn = prefab.GetPooledInstance<Stuff>();

		…
	}
```

Of course prefabs don't have this functionality, so we have to add it ourselves. What we are actually doing here is simply invoking a method of PooledObject. This object just happens to be a prefab, not an object in a scene.

> You can interact with prefabs?
> Yes. They are object instances, just like scene objects. But they're not part of the scene hierarchy. They are assets, which means that all changes to them in the editor will be permanent. Just like when you'd adjust a shared material, for example.

It is now up to the PooledObject component to take care of the pools. First, we need to give it the required method, which is a generic method, like Instantiate.

```cs
	public T GetPooledInstance<T> () where T : PooledObject {
	}
```

> How does a generic method work?
> They work like regular methods, except that they are also like a template. Instead of a concrete type, you use a placeholder, typically named T. You can then invoke this method with any type. The compiler will make sure that method versions for each requested type are available.
> 
> The where keyword after the method header is used to constrain the types that are allowed to be used with this method. In our case, we only support PooledObject and its subclasses.

All this method needs to do is get an object from some pool instance that belongs to this prefab, cast it to the required type, and return it.

```cs
	public T GetPooledInstance<T> () where T : PooledObject {
		return (T)poolInstanceForPrefab.GetObject();
	}
```

Of course we need to keep track of poolInstanceForPrefab, so it becomes a field of PooledObject. Note that we'll use this field to reference an object in the scene, even though a prefab is an asset and not part of any scene. So we cannot save it as part of the prefab, thus we have to make it non-serializable.

```cs
	[System.NonSerialized]
	ObjectPool poolInstanceForPrefab;
```

> What happens when you serialize it?
> Unity will try to save the pool instance as part of the prefab asset, but will fail. If the field were public, it would show up in the inspector as a type mismatch while in play mode, and as missing otherwise.
> 
> Note that all PooledObject instances have this field. It's just prefabs that should make use of it.

Finally, how do we actually get a reference to the correct pool? We'll ask the ObjectPool class for one when necessary, via a static method.

```cs
	[System.NonSerialized]
	ObjectPool poolInstanceForPrefab;

	public T GetPooledInstance<T> () where T : PooledObject {
		if (!poolInstanceForPrefab) {
			poolInstanceForPrefab = ObjectPool.GetPool(this);
		}
		return (T)poolInstanceForPrefab.GetObject();
	}
```

So off we go to ObjectPool and add the required method.

```cs
	public static ObjectPool GetPool (PooledObject prefab) {
	}
```

At this point we still don't have an actual object pool instance. That's all right, because until this method gets invoked we had no need for one anyway. So this is the right place to create a pool instance.

```cs
	public static ObjectPool GetPool (PooledObject prefab) {
		GameObject obj = new GameObject(prefab.name + " Pool");
		ObjectPool pool = obj.AddComponent<ObjectPool>();
		pool.prefab = prefab;
		return pool;
	}
```

Pools should now appear in play mode, one for each prefab, each neatly containing their own stuff.

![Tidy pools](https://raw.githubusercontent.com/tospan/tospan.github.io/source/source/_images/unity/pool-instances.png)


Unfortunately, duplicate pools will be created each time Unity recompiles the scripts while in play mode. While this isn't an issue for builds, it is annoying when working in the editor. This happens because PooledObject.poolInstanceForPrefab doesn't survive a recompile, as we indicated that it should not be serialized. We can work around this by having ObjectPool.GetPool check whether a pool with the same name already exists.

```cs
	public static ObjectPool GetPool (PooledObject prefab) {
		GameObject obj;
		ObjectPool pool;
		if (Application.isEditor) {
			obj = GameObject.Find(prefab.name + " Pool");
			if (obj) {
				pool = obj.GetComponent<ObjectPool>();
				if (pool) {
					return pool;
				}
			}
		}
		obj = new GameObject(prefab.name + " Pool");
		pool = obj.AddComponent<ObjectPool>();
		pool.prefab = prefab;
		return pool;
	}
```

With that problem solved, let's make ObjectPool actually reuse objects. We can do this with a simple list. We add objects to this list as they return to the pool, and take the last one out of the list whenever a new object is required.

```cs
	List<PooledObject> availableObjects = new List<PooledObject>();
	
	public PooledObject GetObject () {
		PooledObject obj;
		int lastAvailableIndex = availableObjects.Count - 1;
		if (lastAvailableIndex >= 0) {
			obj = availableObjects[lastAvailableIndex];
			availableObjects.RemoveAt(lastAvailableIndex);
			obj.gameObject.SetActive(true);
		}
		else {
			obj = Instantiate<PooledObject>(prefab);
			obj.transform.SetParent(transform, false);
			obj.Pool = this;
		}
		return obj;
	}

	public void AddObject (PooledObject obj) {
		obj.gameObject.SetActive(false);
		availableObjects.Add(obj);
	}
```

> Why take the last object out of the list?
> Lists internally use an array. Removing an item from the list requires all items after it to shift one index forward to fill the gap. This gets more expensive the longer the list gets. Removing the first item of the list is the worst case. Removing the last item from the list is the best case, because there are no items after it that need to be shifted.

> We could have also used a stack, but those aren't serialized by Unity so wouldn't survive a recompile.


Suddenly, we have no more constant memory allocations! New stuff is only created when a pool is empty, which will only happen occasionally once the initial burst of object creation is over.

So, does this improve performance? For desktop apps it most likely won't make a difference whether you use the pools or not. In my case the performance is identical. In other cases, it might be a big help. It might even result in worse performance. You'll have to try to find out.

## Pooling Across Scenes

What would happen to our pools if we were to load a different scene? Everything should be destroyed, right? Let's find out.

A very simple way to change scenes is with a little component that switches between consecutive scenes, wrapping back to the first scene when needed.

```cs
using UnityEngine;

public class SceneSwitcher : MonoBehaviour {

	public void SwitchScene () {
		int nextLevel = (Application.loadedLevel + 1) % Application.levelCount;
		Application.LoadLevel(nextLevel);
	}
}
```

Add a button to the canvas and add this component to it, then hook up its method to the On Click event. Also add an event system to the scene via GameObject / UI / Event System, if don't have one already.

![scene switcher](https://raw.githubusercontent.com/tospan/tospan.github.io/source/source/_images/unity/scene-switcher.png)  
![Scene switch button](https://raw.githubusercontent.com/tospan/tospan.github.io/source/source/_images/unity/button.png)


Now we need more scenes. Save and duplicate the current scene. Then change something in the duplicate so you can tell them apart, like replacing the sphere with a cube. Add both these scenes to the build, via File / Build Settings... and using the Add Current button while being inside the scene that you want to add.

![Including two scenes in the build](https://raw.githubusercontent.com/tospan/tospan.github.io/source/source/_images/unity/scenes.png)


The scene switcher is now functional. However, when you try it in the editor Unity might screw up the lighting after loading another scene. This is a problem with the editor only, builds are fine. You can work around this problem by disabling Continuous Baking via Window / Lighting and manually baking each scene once, even when you're not using any baked lighting.

![](https://raw.githubusercontent.com/tospan/tospan.github.io/source/source/_images/unity/wrong-lighting.png)
Lighting screws up after switching scenes in play mode.

Indeed, switching scenes destroys everything from the old scene, including the pools. As the stuff prefabs have lost their pools, they will simply request new ones and start fresh.

![Switching Scenes](https://raw.githubusercontent.com/tospan/tospan.github.io/source/source/_images/unity/EsteemedWarmheartedLadybird.mp4)

But maybe we can keep the pools alive? They're all about preventing object destruction and creation, and scene transitions do just that. We can keep them alive by instructing Unity to not destroy our ObjectPool instances when a new scene is loaded.

```cs
	public static ObjectPool GetPool (PooledObject prefab) {
		…
		
		obj = new GameObject(prefab.name + " Pool");
		DontDestroyOnLoad(obj);
		pool = obj.AddComponent<ObjectPool>();
		pool.prefab = prefab;
		return pool;
	}
```

Our pools now survive scene transitions, and so do all their child objects. So all our stuff remains exactly where it is, which means that our fountain's flow is no longer interrupted. Of course changes to the scene can cause some funny physics reactions.

![Not destroying pools keeps the flow intact](https://raw.githubusercontent.com/tospan/tospan.github.io/source/source/_images/unity/QueasyPessimisticDugong.mp4)

Ideally, we would return all stuff to their pools when loading a new scene, instead of either destroying them or keeping them active. Fortunately, we can do this by adding the OnLevelWasLoaded event method to Stuff.

```cs
	void OnLevelWasLoaded () {
		ReturnToPool();
	}
```

That's it! You have created an object pool system that is easy to use and survives both recompiles and scene transitions. Of course this is not the only way to pool objects. You could tweak it to instantiate a whole bunch of objects when a pool is created, or limit the maximum instances per pool, or make other adjustments that fit your use case. Or use an entirely different approach. The most important thing is that you understand what object pooling is, what it can be used for, and when it might or might not solve your problems.

## 原文链接

[Object Pools ](http://catlikecoding.com/unity/tutorials/object-pools/)