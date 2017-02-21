---
title: Unity Object Pools 2
categories:
  - default
tags:
  - default
date: 2017-02-07 17:09:23
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