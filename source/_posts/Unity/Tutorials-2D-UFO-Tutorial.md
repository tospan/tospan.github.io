# 2D UFO教程

通过创建一个简单的2D UFO游戏来告诉读者使用Unity以及创建2D游戏的一些基本原则，如果你是第一次使用Unity开发，创建这样一个游戏会告诉你很多关于`GameObject`, `Component`，`Prefab`，`Physics`和`Scripting`等各个方面的使用原则。

学习该教程，首先需要从`Asset Store`下载 [资源](https://www.assetstore.unity3d.com/en/content/52143?_ga=1.218910416.838993178.1480250241)

如果你有任何疑问，请到[对应的论坛](http://forum.unity3d.com/threads/2d-ufo-tutorial-q-a.381263/?_ga=1.183824111.838993178.1480250241)上发帖

## Introduction & Setup

### [1. Introduction to 2D UFO Project](https://unity3d.com/learn/tutorials/projects/2d-ufo-tutorial/introduction-2d-ufo-project?playlist=25844)

### [2. Setting Up The Play Field](https://unity3d.com/learn/tutorials/projects/2d-ufo-tutorial/setting-play-field?playlist=25844)

由于2D游戏是在一个面上的，所以我们需要分层来确保先后顺序，也就是通过Sort Layer值来设置，索引越小，越靠后。


## Movement Basics

### [1. Controlling the Player](https://unity3d.com/learn/tutorials/projects/2d-ufo-tutorial/controlling-player?playlist=25844)


### [2. Adding Collision](https://unity3d.com/learn/tutorials/projects/2d-ufo-tutorial/adding-collision?playlist=25844)

其实没有什么好说的，

再强调一遍，Collider 是物理引擎通过Shape或者Volume用于标识碰撞的。 物理引擎通过场景中的两个或多个Collider来检测碰撞。
要想物理引擎能检测到碰撞，至少一个碰撞对象（Colliding GameObject）需要有rigidbody2D组件。

这里我们使用的是2D物理引擎，所以这个项目中我们将会使用Collider2D 组件。

另外就是，由于我们的背景仅仅是一张图片，我们需要在上面附件4个BoxCollider2D
![](_assets/images/2d_ufo_game_01.png)

### [3. Following the Player with the Camera](https://unity3d.com/learn/tutorials/projects/2d-ufo-tutorial/following-player-camera?playlist=25844)

```c
public class CameraController : MonoBehaviour {

    public GameObject player;
    private Vector3 offset;

    void Start () {     }

    void LateUpdate () {
        transform.position = player.transform.position + offset;
    }
}
```

## Collectable Objects

### [1. Creating Collectable Objects](https://unity3d.com/learn/tutorials/projects/2d-ufo-tutorial/creating-collectable-objects?playlist=25844)

将Pickup Sprite拖拽到场景中，设置它的Sort Layer为`Pickup`，然后给它添加Circle Collider 2D，调节到适合大小，这不太好看，让它旋转起来吧，由于处于xy面，所以，就是沿着z轴方向旋转就好了。这里我们使用`transform.Rotate()` 方法来实现。


```c
public class Rotator : MonoBehaviour {

    // Update is called once per frame
    void Update () {
        // Rotate thet transform of the game object this is attached to by 45 degrees, taking into account the time elapsed since last frame.
        transform.Rotate (new Vector3 (0, 0, 45) * Time.deltaTime);
    }
}
```

### [2. Picking Up Collectables](https://unity3d.com/learn/tutorials/projects/2d-ufo-tutorial/picking-collectables?playlist=25844)


So how do collisions work in Unity's 2D physics engine? The 2D physics engine does not allow two 2D collider areas to overlap. Remember how the Player was ejected from the middle of the background sprite in our previous lesson. When the 2D physics engine detects that any two or more 2D colliders will overlap that frame the 2D physics engine will look at the objects and analyse their speed, rotation and shape and calculate a collision. One of the major factors in this calculation is whether the 2D colliders are static or dynamic. Static colliders are usually none moving parts of our scene like the walls, the floor, or other parts of the scenery. Dynamic colliders are things that move, like the Player's UFO, or a car. When calculating a collision the static geometry will not be affected by the collision. But the dynamic objects will be. In our case the Player's UFO is dynamic, or moving geometry, and it is bouncing off the static geometry of the pickups just as it bounces off the static geometry of the walls. However the 2D physics engine can allow the penetration or overlap of 2D collider areas. When it does this the 2D physics engine still calculates the 2D collider areas and keeps track of the collider overlap but it doesn't physically act on the overlapping objects. It doesn't cause a collision. We do this by making our 2D colliders in to triggers, or 2D Trigger Colliders. When we make our colliders in to a trigger we can detect the contact with that 2D trigger through the 2DOnTrigger event messages. When a 2D collider is a trigger we can do clever things like place a trigger in the middle of a doorway in for example an adventure game, and when the player enters the mini map updates and a message plays 'you have discovered this room'. Or every time our player walks in to an area a sound plays because the player has walked through a trigger. For more information on 2DOnCollision and 2DOnTrigger messages see the lessons linked below. We are using OnTriggerEnter2D in our code rather than OnCollisionEnter2D, so we need to change our collider 2D areas in to trigger areas. Let's select the prefab asset and look at the circle collider 2D component. I'm going to reactivate the two sprite renderers and highlight the Pickup prefab. In the Circle Collider 2D component we see a property called Is Trigger. Let's set this to True. Now let's enter play mode and test.

Excellent, let's exit play mode. So far everything looks great.

But we have one issue. We've made a small mistake, which is related to how Unity optimises it's 2D physics. When using 2D physics all of the static, or non-moving colliders are calculated as a single body. This increases performance. If we move a collider that the engine is not expecting to move the static collider or all our non-moving objects will have to be recalculated. If we do this every frame we risk our game losing performance. Where we've made our mistake is by rotating our pickups. We need to indicate to Unity which 2D colliders are dynamic before we move them. We do this by using the rigidbody2D component. Any game object with a 2D collider and a rigidbody2D is considered dynamic. Any game object with a 2D collider but no rigidbody2D is expected to be static. Currently our Pickup game objects have a circle collider but no rigidbody2D. So Unity is recreating the collider's for the pickups and walls each frame. The solution is to add a rigidbody2D to the Pickup objects. Let's enter play mode and test. As we can see, our Pickups fall off the screen. Gravity pulls them down and because they are triggers they don't collide with the walls. If we look at the rigidbody2D component we see that we could simply set Gravity Scale to 0, which would prevent the Pickups from falling downwards. This is only a partial solution however. If we did this even though our Pickups would not respond to gravity they would still respond to physics forces. There's a better solution. That is to select Is Kinematic. When we do this we set this rigidbody2D component to be a kinematic 2D rigidbody. A kinematic 2D rigidbody will not react to physics forces and can be animated and moved by it's transform. This is great for everything from objects with colliders like elevators and moving platforms to objects with triggers, like our collectables, that need to be animated or moved by their transform. For more information on the rigidbody2D and Is Kinematic see the lessons linked below. Let's save our scene and enter play mode to test. Now our behaviour is identical and performant. So static 2D colliders shouldn't move, like walls and floors, dynamic 2D colliders can move but should have a rigidbody2D attached. Standard 2D rigidbodies like our Player are moved using 2D physics forces. Kinematic 2D rigidbodies are moved using their transform.


### [3. Counting Collectables and Displaying Score](https://unity3d.com/learn/tutorials/projects/2d-ufo-tutorial/counting-collectables-and-displaying-score?playlist=25844)


需要注意的就是添加引用：

```c
using UnityEngine.UI;


```
## Creating a Playable Build

### [1. Building our 2D UFO Game](https://unity3d.com/learn/tutorials/projects/2d-ufo-tutorial/building-our-2d-ufo-game?playlist=25844)