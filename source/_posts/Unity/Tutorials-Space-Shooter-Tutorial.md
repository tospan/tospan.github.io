---
title: Unity教程:Space Shooter
categories: 
  - Unity Tutorials
tags:
  - Unity
  - Patterns
date: 2017-02-01 15:29:58
updated: 2017-02-05 15:29:58
---

## Introduction

### [1. Introduction to Space Shooter](https://unity3d.com/learn/tutorials/projects/space-shooter/introduction?playlist=17147)

An introduction to the Space Shooter project, showing the final game and describing what will be covered in this project.

<!--more-->
## 1. Game setup, Player and Camera

### [1. Setting up the project](https://unity3d.com/learn/tutorials/projects/space-shooter/setting-up-the-project?playlist=17147)

Creating a new project, importing assets, setting the build target and other project related details.

创建新项目，命名为 `SpaceShooter`，然后从`AssetStore`中导入资源：[Space Shooter](https://www.assetstore.unity3d.com/en/#!/content/13866?_ga=1.139739514.838993178.1480250241)


游戏默认是全屏设置，通过Edit -> Project Settings -> Player，默认是`PC, Mac & Linux Standalone`

取消 `Default Is Full Screen` 选项，然后设置 `Default Screen Width` 为 `600`， `Default Screen Height`为`900`

### [2. The player GameObject](https://unity3d.com/learn/tutorials/projects/space-shooter/the-player-gameobject?playlist=17147)

先设置角色。

有模型直接把模型拖进场景，位置重置，一个模型游戏对象需要最基本的组件：

* Mesh Filter
* Mesh Render
    * Materials

游戏过程中我们是需要控制我们的角色的，这里我们使用Rigidbody，直接添加组件就可以了，由于角色在太空中，所以就不用启用重力选项了。

Rigidbody的主要功能是什么呢？

接下来还需要检测碰撞，这主要用于处理两种情况：

* 角色与其他对象碰撞了
* 角色与其他对象重叠了

这主要就和物体的外形(Volume)有关，有些时候要求不严谨，我们可以使用一些常用的的Collider，例如Box，Sphere，或Capsule Collider，抑或是它们的组合，如果实在不行，我们可以考虑使用Mesh Collider，默认它是基于模型的结构来创建各个面的，这个时候就会就会很复杂（为什么会复杂呢？）这个时候就该考虑做一个简化模型来作为Mesh Collider。

> 在Unity 5中一个物体可以有多个Collider，而不是之前只允许有一个Collider，所以要根据实际情况来处理。另外，Mesh Collider必须启用Convex才有用。

最后，我们给角色添加了引擎。


### [3. Camera and lighting](https://unity3d.com/learn/tutorials/projects/space-shooter-tutorial/camera-and-lighting?playlist=17147)

#### 设置相机

这是个j街机游戏，所以Skybox没有价值，直接设置`Clear Flags`为 `Solid Color`，然后将背景色设置为全黑色。另外，Projection改为Orthographic（正交视图），而非透视



#### 设置光源

添加背景，新的Unity中默认每个场景都有skybox，可以通过Window/Lighting窗口找到Skybox。

Lighting选项卡是针对全局设置的，Camera是局部设置，例如在没有设置Skybox的情况下，我们将Clear Flags设置为Skybox，默认还是Solid Color

Unity 5之后，每个新建的场景都有Skybox和Directional Light。

如果发现即使没有了直射光，我们依旧可以看到我们的角色，主要原因在于存在环境光(Ambient Light)，新版本的Unity，默认的Ambient Source是Skybox，那么我们需要在Lighting窗口的Scene 选项卡，找到Skybox然后删除即可，由于没有Skybox，它就会出现Ambient Color，这下就和老版本的Unity一样（通过Edit -> RenderSettings -> Ambient Light）

Ambient Light是来自于非固定光源，而是场景中物体表面光源，默认值是(51,51,51,255)，虽然我们可以调节它，但是由于不是直接光源，会导致结果不可控，还是直接设置为(0,0,0,255)吧。

这样一来我们的角色就全黑了！！！

我们需要添加main light, Fill light, Rim Light，至于为什么要这三种光……并不是很明白。

### [4. Adding a background](https://unity3d.com/learn/tutorials/projects/space-shooter/adding-a-background?playlist=17147)

现在我们的游戏场景就是一片漆黑，很无趣！

需要给游戏添加点背景，Create -> 3D -> Quad（四边形），鉴于我们只是使用它来作为背景，那么没有必要使用Mesh Collider，可以直接移除。

然后我们给它加上贴图，直接将贴图拖拽到Quad上面就可以了，实际上背后的流程是Unity会为创建一个Material，然后使用该贴图。并使用默认标准的Shader。

然后就是调节大小了，问题是使用标准的Shader我们的背景色会很灰暗，尽管我们可以使用光（放于不同层的光，可以避免光之间互相影响）。其实只要调节下Shader就可以了，使用Unlit/Texture，对于Shader完全不理解啊……

基本的舞台和主角都好了，接下来该准备Gameplay了！

### [5. Moving the player](https://unity3d.com/earn/tutorials/projects/space-shooter/moving-the-player?playlist=17147)

首先是能控制我们的飞机哇！

我们可以直接控制它的速度来完成，这个需要它的Rigidbody来完成（我可以直接设置position啊！为什么一定要使用Rigidbody呢？？？？！）。 另外在Unity 5之后，不能再使用快捷方式，例如直接获取rigidbody了，必须使用GetComponent后操作。

```C

private Rigidbody rb;

void Start(){
    rb = getComponent<Rigidbody>();
}

void FixedUpdate(){
    float moveHorizontal = Input.GetAxis("Horizontal");
    float moveVertical = Input.GetAxis("Vertical");

    rg.velocity = new Vector3(moveHorizontal, 0.0f, moveVertical);
}

```

就这样，我们就可以通过方向键来控制我们的灰机移动了，问题是：moveHorizontal属于0~1，moveVertical属于 0~1，rigidbody.velocity到底是什么呢？

#### 设置移动范围

如果上面的移动速度太慢，我们可以设置一个速度值乘以向量。另外又有一个问题，一不小心我们的飞机就会飞到外面去了。这个时候我们就需要个飞机的活动范围设置界限。

对于y轴方向，没有什么好说的，我们根本不会改变它，对于x和y方向则不然。我们需要规定position的x，y的最大最小值。这时候可以使用Mathf.Clamp(value, min, max)。当然我们可以直接使用

```c
public float xMin, xMax, zMin, zMax;

rg.position = new Vector3(
    Mathf.Clamp(rg.position.x, xMin, xMax),
    0.0f,
    Mathf.Clamp(rg.position.z, zMin, zMax)
);

```

但这里的问题是，会直接在Inspector面板上占用4行的空间，不太好，我们可以创建一个类。

```c
public class Boundary{
    public float xMin, xMax, zMin, zMax;
}

public PlayerController: MonoBehavior{
    public Boundary boundary;

    void FixedUpdate(){
        ... ...
        rg.position = new Vector3(
            Mathf.Clamp(rg.position.x, boundary.xMin, boundary.xMax),
            0.0f,
            Mathf.Clamp(rg.position.z, boundary.zMin, boundary.zMax)
        );
    }
}
```

这样虽好，但我们无法直接在Inspector上设置了，需要进行序列化，给Boundary类加上修饰 `[System.Serializable]` ，这样我们就可以直接在Inspector上进行编辑了，也可以折叠，是不是很吊？！

#### 让飞机倾斜！

当飞机左右飞行的时候，如果能够稍微倾斜一点则更加有趣一点。

```C
    public float tilt;

    void FixedUpdate()
    {
        rg.rotation = Quaternion.Euler(0.0f, 0.0f, rg.velocity.x * -tilt);
    }
```

对，我们只要旋转z方向就可以了。 rotation是什么？需要好好理解！！！！！！！！

### [6. Creating shots](https://unity3d.com/learn/tutorials/projects/space-shooter-tutorial/creating-shots?playlist=17147)

### [7. Shooting shots](https://unity3d.com/learn/tutorials/projects/space-shooter/shooting-shots?playlist=17147)

## 2. Boundaries, Hazards and Enemies

### [1. Boundary](https://unity3d.com/learn/tutorials/projects/space-shooter-tutorial/boundary?playlist=17147)

接着上一节，如果我们持续射击，那么我们的场景中会增加很多子弹它们会一直沿着Z轴方向飞去。肯定会影响性能的。

直接的想法就是当这些子弹飞出我们的游戏场景的时候，我们就直接销毁它们。（当然还有其他方式，就是子弹池，避免不断实例化新的GameObject）。

那么怎么怎么判断它们是否在场景中呢？这就需要Collider了，关于Collider是如何工作的，可以看看 a brief of 2D game Development.

我们需要使用Collider的trigger功能。

Collider有两个事件：OnTriggerEnter, OnTriggerExit。

其实我们都可以拿来用，比如，OnTriggerEnter，我们将场景四周围绕Box Collider，用于检测。但这里更简单的是：OnTriggerExit

创建一个Box，让它和我们的场景一样大（主要只宽高）然后，启用Is Trigger

```c
void OnTriggerExit(Collider other){
    Destroy(other.gameObject);
}
```


### [2. Creating hazards](https://unity3d.com/learn/tutorials/projects/space-shooter-tutorial/creating-hazards?playlist=17147)

现在设置基本齐全了，我们该考虑怎么让我们的角色接受点挑战了！

首先来点陨石。

先创建一个空的父对象，然后将陨石模型拖到下面，重置所有Transform参数。

给父对象添加Rigidbody，由于在太空中，不需要重力。
同时还需要添加一个Collider，这里我们使用Capsule Collider，可以使用Inspector面板上的编辑器来修改Collider的大小。

这样我们的一个陨石基本就完成了。

可是很丑陋啊……

让它自己旋转吧！

创建脚本 RandomRotator.cs

直接获取Rigidbody，然后设置它的AngularVelocity，什么是AngularVelocity，就是RigidBody的旋转速度，另外Unity自带很多随机值库`Random`，这里我们使用`Random.insideUnitSphere`(Returns a random point inside a sphere with radius 1 (Read Only)) 然后乘以一个标量值tumble。运行，就可以看到我们的陨石随机转动了。问题是转动一会儿就停止了……

这是由于Rigidbody中有两个参数模拟摩擦力的，例如空气阻力，它们是Drag和Angular Drag，从名字上看一个是方向上的阻力，一个是旋转时候的阻力，默认Angular Drag的值是0.5，我们设置为0就好了。


现在我们直接用我们的子弹射击陨石，然而并没有什么用途。

其实所有的Collider默认有两个用途：

1. 碰撞检测；
2. Trigger

若果我们启用了Is Trigger，那么它就只有 Trigger的功能了，但是Trigger只会触发事件，我们还需要完成事件的具体处理功能才可以实现想要的功能。例如子弹与陨石触碰后，子弹和陨石都销毁，当然陨石碰到了我们的飞机，它们也都销毁，所以创建一个脚本：

DestroyByContact.cs

```c
void OnTriggerEnter(Collider other){
    if(other.tag == "Bounday"){
        return;
    }

    Destroy(other.gameObject);
    Destroy(gameObject);
}
```

这里有两个点需要强调：

1. 为什么需要排除Boundary，其实因为陨石和子弹都在Boundary之内，那么游戏一旦启动，Boundary就会与陨石触发OnTriggerEnter的事件，那么就会导致我们的Boundary和陨石都消逝了。
2. Destroy的顺序问题，它们不应该是同时销毁的么？其实Destroy方法只会标记特定的gameObject会被销毁而不会立即执行，所有的操作会在当前帧都处理完成后执行。

这下再试试，就会发现，duang，子弹和陨石消逝了！



### [3. Explosions](https://unity3d.com/learn/tutorials/projects/space-shooter-tutorial/explosions?playlist=17147)

陨石被销毁怎么能没有爆炸呢？？！

这个其实很简单（只要不是你自己创建爆炸效果就好）

在DestroyByContact.cs脚本中添加如下代码：

```c
public GameObject explosion;
public GameObject playerExplosion;

void OnTriggerEnter(){
    if (other.tag == "Boundary") {
        return;

    }

    Instantiate (explosion, transform.position, transform.rotation);

    if (other.tag == "Player") {
        Instantiate (explosion, other.transform.position, other.transform.rotation);
    }

    Destroy (other.gameObject);
    Destroy (gameObject);
}
```

然后直接在Inspector面板将explosion的效果直接拖进去就可以了啊！

另外，我们还需要陨石是可以移动的，这就需要给陨石添加Mover.cs脚本了。让后设置speed为-5，那么它就会向我们的飞船飞过来了！

### [4. Game Controller](https://unity3d.com/learn/tutorials/projects/space-shooter-tutorial/game-controller?playlist=17147)

到目前来看，游戏需要的角色基本上场了。现在就需要游戏规则了。这个时候就是需要GameController的时候。

我们创建一个空对象，命名为GameController，并设置它的Tag为GameController，并添加一个脚本为GameController。

我们用它做什么？当然是生成各种陨石啦！

不过这里主要的问题就是在那里生成陨石呢？

教程的作者厉害的地方在于倒推。

他很清楚自己需要什么东西（这也许就是经验），然后拆分为对应的代码，再倒推到具体的参数来源及其组合方式（比如组合使用随机数，默认值，数学函数等等）。




### [5. Spawning waves](https://unity3d.com/learn/tutorials/projects/space-shooter/spawning-waves?playlist=17147)

这是重要课程！！！！！！！非常重要之非常重要！！！！！

```c
public GameObject hazard;
public Vector3 SpawnValue;
public int hazardCount;
public float spawnWait;
public float startWait;
public float waveWait;

// Use this for initialization
void Start () {
    StartCoroutine(SpawnWaves ());
}

IEnumerator SpawnWaves(){
    yield return new WaitForSeconds (startWait);

    while(true){
        for (int i = 0; i<hazardCount; i++) {
            Vector3 spawnPosition = new Vector3 (Random.Range(-SpawnValue.x, SpawnValue.x) ,SpawnValue.y, SpawnValue.z);
            Quaternion spawnRotation = Quaternion.identity;
            Instantiate (hazard, spawnPosition, spawnRotation);
            yield return new WaitForSeconds (spawnWait);
        }

        yield return new WaitForSeconds (waveWait);
    }
}
```


## 3. Scoring, Finishing and building the game

### [1. Audio](https://unity3d.com/learn/tutorials/projects/space-shooter-tutorial/audio?playlist=17147)

没有声音是一件很枯燥的事情啊！

三种Audio组件：

* Audio Clips       容器：用于承载各种音频
* Audio Source      播放器： 用于播放各种音频
* Audio Listener


这里的流程是，先导入声音，然后调节，这里我们省去了这个流程。对于声音来说，最重要的就是，声音添加到哪里，然后什时候播放，音量是多少？是否循环，是否支持立体声？


### [2. Counting points and displaying the score](https://unity3d.com/learn/tutorials/projects/space-shooter-tutorial/counting-points-and-displaying-score?playlist=17147)

现在我们的游戏基本已经可以了嘛！需要计分系统了呢！

这个可以分成几个步骤

#### 添加GUIText用于显示分数

#### 在GameController提供更新分数的方法

#### 在陨石中获取GameController并在销毁的时候更新分数


### [3. Ending the game](https://unity3d.com/learn/tutorials/projects/space-shooter-tutorial/ending-game?playlist=17147)


重新加载场景时，原来的Application LoadLevel不能用了，现在必须使用SceneManager，这就需要我们先添加命名空间：UnityEngine.SceneManagement

 SceneManagement.SceneManager.GetActiveScene().name

### [4. Building the game](https://unity3d.com/learn/tutorials/projects/space-shooter-tutorial/building-game?playlist=17147)


## 4. Extending Space Shooter

### [1. Extending Space Shooter: Enemies, More Hazards, Scrolling BG...](https://unity3d.com/learn/tutorials/projects/space-shooter-tutorial/extending-space-shooter-enemies-more-hazards?playlist=17147)

### [2. Mobile Development: Converting Space Shooter to Mobile](https://unity3d.com/learn/tutorials/topics/mobile-touch/mobile-development-converting-space-shooter-mobile?playlist=17147)