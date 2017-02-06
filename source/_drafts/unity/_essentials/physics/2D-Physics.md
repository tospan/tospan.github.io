<!-- TOC -->

- [2D 物理](#2d-%E7%89%A9%E7%90%86)
    - [Rigidbody 2D](#rigidbody-2d)
    - [Collider 2D](#collider-2d)
    - [Bouncing & Sliding in 2D](#bouncing--sliding-in-2d)
    - [Joint 2D 关节](#joint-2d-%E5%85%B3%E8%8A%82)
        - [Hinge Joint 2D](#hinge-joint-2d)
        - [Spring Joint 2D](#spring-joint-2d)
        - [Distance Joint 2D](#distance-joint-2d)
    - [Effector 2D](#effector-2d)
        - [Area Effector 2D](#area-effector-2d)
        - [Point Effector 2D](#point-effector-2d)
        - [Surface Effector 2D](#surface-effector-2d)
        - [Platform Effector 2D](#platform-effector-2d)
    - [相关教程](#%E7%9B%B8%E5%85%B3%E6%95%99%E7%A8%8B)
    - [相关文档](#%E7%9B%B8%E5%85%B3%E6%96%87%E6%A1%A3)

<!-- /TOC -->

# 2D 物理

## Rigidbody 2D

简介Rigidbody 2D以及其属性

常见的物理属性，例如

* 质量，质量越大，越难移动，与其他物体碰撞时产生的影响越大。
* Linear Drag，线性移动阻力，会影响速度或速率
* Angular Drag，旋转阻力
* Gravity Scale 会以全局重力值 X Scale 值来设置当前物体所受到的重力
* Fixed Angle 固定角度从而物体不能旋转
* Kinematic 会认为当前刚体是运动刚体，不会受到2D Physics forces，包括gravity和collision，它是需要关注一下的，游戏世界里某些物体需要移动，且与其他物体有碰撞，但不能物理影响因素，例如，受到重力，物体撞到会移动等，而是由脚本控制。
* Interpolate 是用于让物体移动更加平滑，当游戏比较卡的时候，使用它会有帮助，它有两种形式，Interpolate和Extrapolate，Interpolate会依据上一帧的位置，而Extrapolate会推测下一帧所在的位置
* Sleeping Mode 睡眠模式，主要用于节省运算时间，为啥？请看相关文档
* Collision Detection 碰撞检测模式，有三种：
    - Discrete，最好使用它，除非有什么问题
    - Continuous，用于检测快速移动的物体


The default is Discrete and it is best to use discrete unless there are problems. In this case collisions are registered if the game object's 2D collider is in contact with another during a physics update. Continuous is for fast moving game objects and collisions are registered if the game object appears to have collided with another between updates.


## Collider 2D

Collider 2D是用于定义游戏场景中游戏对象的物理外形用于处理2D碰撞和触发的组件

Circle
Box
Polygon
Edge

都继承自Collider2D

Is Trigger & Material

如何编辑这些Collider呢？

## Bouncing & Sliding in 2D

Sliding and Bouncing are all controlled by a Physics Material applied to a Collider Component. In this assignment we set up both Bouncy and Slippery 2D Physics Materials, and apply them to GameObjects.

## Joint 2D 关节

### Hinge Joint 2D

合页关节2D 可以让Sprite被2D Physics引擎控制从而围绕一个点来旋转。

### Spring Joint 2D

如何在2D Rigidbody 物理对象之间创建弹簧？


### Distance Joint 2D

距离关节可以让2D物理控制一个Sprite围绕一个点旋转，但和该点保持特定的距离。

## Effector 2D

### Area Effector 2D

In this lesson we explore the Area Effector 2D component which allows you to add 2D physics forces to objects which enter a trigger volume.
不是很理解……

### Point Effector 2D

In this lesson we explore the Point Effector 2D component which allows you to add 2D physics forces to objects which enter a trigger volume.

### Surface Effector 2D

In this lesson we explore the Surface Effector 2D and Platform Effector 2D component. 
The Surface Effector 2D creates conveyor belt style effects and the Platform Effector 2D allows you to create one sided platforms that the player can jump through.

### Platform Effector 2D


## 相关教程

* [Game Objects and Components](https://unity3d.com/learn/tutorials/topics/interface-essentials/game-objects-and-components) (Lesson)
* [The Sprite Type](https://unity3d.com/learn/tutorials/topics/2d-game-creation/sprite-type) (Lesson)
* [2D Physics: Fun with Effectors](https://unity3d.com/learn/tutorials/topics/2d-game-creation/2d-physics-fun-effectors) (Lesson)
* [01. 2D Physics Overview](https://unity3d.com/learn/tutorials/topics/2d-game-creation/2d-physics-overview) (Lesson)
* [02. Collider 2D](https://unity3d.com/learn/tutorials/topics/2d-game-creation/collider-2d?playlist=17120)
* [03. Rigidbody 2D](https://unity3d.com/learn/tutorials/topics/2d-game-creation/rigidbody-2d) (Lesson)
* [Distance Joint 2D](https://unity3d.com/learn/tutorials/topics/2d-game-creation/distance-joint-2d) (Lesson)
* [Spring Joint 2D](https://unity3d.com/learn/tutorials/topics/2d-game-creation/spring-joint-2d) (Lesson)
* [Hinge Joint 2D](https://unity3d.com/learn/tutorials/topics/2d-game-creation/hinge-joint-2d) (Lesson)
* [Area Effector 2D](https://unity3d.com/learn/tutorials/topics/2d-game-creation/area-effector-2d) (Lesson)
* [Point Effector 2D](https://unity3d.com/learn/tutorials/topics/2d-game-creation/point-effector-2d) (Lesson)
* [Bouncing & Sliding in 2D](https://unity3d.com/learn/tutorials/topics/2d-game-creation/bouncing-sliding-2d?playlist=17120)
* [Physics Materials](https://unity3d.com/learn/tutorials/topics/physics/physics-materials) (Lesson)
* [Colliders](https://unity3d.com/learn/tutorials/topics/physics/colliders) (Lesson)
* [Colliders as Triggers](https://unity3d.com/learn/tutorials/topics/physics/colliders-triggers) (Lesson)
* [Detecting Collisions with OnCollisionEnter](https://unity3d.com/learn/tutorials/topics/physics/detecting-collisions-oncollisionenter) (Lesson)



## 相关文档

* [2D Components](http://docs.unity3d.com/Documentation/Components/index.html?_ga=1.114560462.838993178.1480250241) (Manual)
* [Rigidbody 2D](http://docs.unity3d.com/Documentation/Components/class-Rigidbody2D.html?_ga=1.214052318.838993178.1480250241) (Manual)
* [Box Collider 2D](http://docs.unity3d.com/Documentation/Components/class-BoxCollider2D.html?_ga=1.214052318.838993178.1480250241) (Manual)
* [Physics Overview](http://docs.unity3d.com/Documentation/Manual/Physics.html?_ga=1.214052318.838993178.1480250241) (Manual)
* [Circle Collider 2D](http://docs.unity3d.com/Documentation/Components/comp-2DGroup.html?_ga=1.109768268.838993178.1480250241) (Manual)
* [Polygon Collider 2D](http://docs.unity3d.com/Documentation/Components/class-PolygonCollider2D.html?_ga=1.109768268.838993178.1480250241) (Manual)
* [Edge Collider 2D](http://docs.unity3d.com/Documentation/Components/class-EdgeCollider2D.html?_ga=1.105048778.838993178.1480250241) (Manual)
* [Physics Material 2D](http://docs.unity3d.com/Documentation/Components/class-PhysicsMaterial2D.html?_ga=1.105048778.838993178.1480250241) (Manual)
* [Physics 2D](http://docs.unity3d.com/Documentation/ScriptReference/Physics2D.html?_ga=1.105048778.838993178.1480250241) (Script Reference)
* [Collider 2D](http://docs.unity3d.com/Documentation/ScriptReference/Collider2D.html?_ga=1.105048778.838993178.1480250241) (Script Reference)
* [Hinge Joint 2D](http://docs.unity3d.com/Documentation/Components/class-HingeJoint2D.html?_ga=1.118760896.838993178.1480250241) (Manual)
* [Spring Joint 2D class](http://docs.unity3d.com/Documentation/ScriptReference/SpringJoint2D.html?_ga=1.120794817.838993178.1480250241) (Script Reference)
* [Spring Joint 2D Component](http://docs.unity3d.com/Documentation/Components/class-SpringJoint2D.html?_ga=1.120794817.838993178.1480250241) (Manual)
* [Distance Joint 2D](http://docs.unity3d.com/Documentation/Components/class-DistanceJoint2D.html?_ga=1.218376784.838993178.1480250241) (Manual)