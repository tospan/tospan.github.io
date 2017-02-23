---
title: Interview 2
categories:
  - default
tags:
  - default
date: 2017-02-09 13:31:21
updated: 2017-02-09 13:31:21
---

Difference between Update,Fixed Update and Late Update.
What is Prefabs in Unity 3D?
What is the use of AssetBundle in Unity?
What is difference between Resources and StreamingAssets Folder.
What is Batching and what is the use of Batching?
Difference between Destroy and DestroyImmediate unity function
Difference between Start and Awake Unity Events
What is the use of deltatime?
Is this possible to collide two mesh collider,if yes then How?
Difference between Static and Dynamic Batching.
What is the use of Occlusion Culling?
How can you call C# from Javascript, and vice versa?
Arrange the event functions listed below in the order in which they will be invoked when an application is closed:
* Update()
* OnGUI()
* Awake()
* OnDisable()
* Start()
* LateUpdate()
* OnEnable()
* OnApplicationQuit()
* OnDestroy()

11. .Net与Mono的关系？
  答：Mono官网主页
  Mono is a software platform designed to allow developers to easily create cross platform applications. Sponsored by Xamarin, Mono is an open source implementation of Microsoft's .NET Framework based on the ECMA standards for C# and the Common Language Runtime.

  mono是.net的一个开源跨平台工具，就类似java虚拟机，java本身不是跨平台语言，但运行在虚拟机上就能够实现了跨平台。.net只能在windows下运行，mono可以实现跨平台跑，可以运行于linux，Unix，Mac OS等。

12. 简述Unity3D支持的作为脚本的语言的名称
  答：Unity的脚本语言基于Mono的.Net平台上运行，可以使用.NET库，这也为XML、数据库、正则表达式等问题提供了很好的解决方案。Unity里的脚本都会经过编译，他们的运行速度也很快。这三种语言实际上的功能和运行速度是一样的，区别主要体现在语言特性上。JavaScript：和网页中常用的JavaScript不一样，它编译后的运行速度很快，语法方面也会有不少区别。C#，Boo：可以看做是Python语言的变种，又糅合了Ruby和C#的特性，它是静态类型语言

13. Unity3D是否支持写成多线程程序？如果支持的话需要注意什么？
  答：参考http://www.unitymanual.com/3821.html
  仅能从主线程中访问Unity3D的组件，对象和Unity3D系统调用支持：如果同时你要处理很多事情或者与Unity的对象互动小可以用thread,否则使用coroutine。
  注意：C#中有lock这个关键字,以确保只有一个线程可以在特定时间内访问特定的对象

14. Unity3D的协程和C#线程之间的区别是什么？
  答：http://blog.csdn.net/kongbu0622/article/details/8775037
  多线程程序同时运行多个线程 ，而在任一指定时刻只有一个协程在运行，并且这个正在运行的协同程序只在必要时才被挂起。
  除主线程之外的线程无法访问Unity3D的对象、组件、方法。
  Unity3d没有多线程的概念，不过unity也给我们提供了StartCoroutine（协同程序）和LoadLevelAsync（异步加载关卡）后台加载场景的方法。 StartCoroutine为什么叫协同程序呢，所谓协同，就是当你在StartCoroutine的函数体里处理一段代码时，利用yield语句等待执行结果，这期间不影响主程序的继续执行，可以协同工作。而LoadLevelAsync则允许你在后台加载新资源和场景，所以再利用协同，你就可以前台用loading条或动画提示玩家游戏未卡死，同时后台协同处理加载的事宜asynchronous[e ɪˈ s ɪŋ kr ə n ə s] .synchronous同步。 

15. U3D中用于记录节点空间几何信息的组件名称，及其父类名称
  答：Transform 父类是 Component

16. 简述四元数的作用，四元数对欧拉角的优点？
  答：四元数用于表示旋转
  相对欧拉角的优点：
  1）能进行增量旋转
  2）避免万向锁
  3）给定方位的表达方式有两种，互为负（欧拉角有无数种表达方式）

17. 向量的点乘、叉乘以及归一化的意义？
  1）点乘描述了两个向量的相似程度，结果越大两向量越相似，还可表示投影
  2）叉乘得到的向量垂直于原来的两个向量
  3）标准化向量：用在只关系方向，不关心大小的时候

18. 矩阵相乘的意义及注意点
  用于表示线性变换：旋转、缩放、投影、平移、仿射
  注意矩阵的蠕变：误差的积累

19. 为何大家都在移动设备上寻求U3D原生GUI的替代方案
  不美观，OnGUI很耗费时间，使用不方便 

20. 请简述如何在不同分辨率下保持UI的一致性
  NGUI很好的解决了这一点，屏幕分辨率的自适应性，原理就是计算出屏幕的宽高比跟原来的预设的屏幕分辨率求出一个对比值，然后修改摄像机的size。
  原生GUI http://unity3d.9ria.com/?p=2587
  NGUI http://blog.csdn.net/mfc11/article/details/17681429

21. 为什么dynamic font在unicode环境下优于static font
  Unicode是国际组织制定的可以容纳世界上所有文字和符号的字符编码方案。使用动态字体时，Unity将不会预先生成一个与所有字体的字符纹理。当需要支持亚洲语言或者较大的字体的时候，若使用正常纹理，则字体的纹理将非常大。

22. Render的作用？描述MeshRender和SkinnedMeshRender的关系与不同
  A renderer is what makes an object appear on the screen。Mesh就是指模型的网格（同名组件是用于调整网格属性的），MeshFilter一般是用于获得模型网格的组件，而MeshRender是用于把网格渲染出来的组件，

23. 简述SkinnedMesh的实现原理
  http://blog.csdn.net/n5/article/details/3105872

24. 在场景中放置多个Camera并同时处于活动状态会发生什么
  答：游戏界面可以看到很多摄像机的混合，实际看到的画面由多个camera的画面组成，由depth、Clear Flag、Culling Mask都会影响最终合成效果。

25. Prefab的作用？如何在移动环境的设备下恰当地使用它？
  答：Prefab在实例化的时候用到，主要用于经常会用到的物体，属性方便修改
    在游戏运行时实例化，prefab相当于一个模板，对你已经有的素材、脚本、参数做一个默认的配置，以便于以后的修改，同事prefab打包的内容简化了导出的操作，便于团队的交流。
  http://www.cnblogs.com/88999660/archive/2013/03/15/2961663.html 

26. 如何销毁一个UnityEngine.Object及其子类
  答：使用Destroy()方法;

27. 为什么Unity3D中会发生在组件上出现数据丢失的情况？
  答：组件上绑定的对象被删除了

28. 如何安全的在不同工程间安全地迁移asset数据？三种
  答：
  将Assets目录和Library目录一起迁移
  导出包
  用unity自带的assets Server功能

29. MeshCollider和其他Collider的一个主要不同点？
  答：Convex？Meshcollider再快也是基于V3顶点~~ 建议还是用boxcollider，boxcollider本身是基于算法，没有面的概念。

30. 当一个细小的高速物体撞向另一个较大的物体时，会出现什么情况？如何避免？
  穿透（碰撞检测失败）
  http://forum.unity3d.com/threads/3353-collision-detection-at-high-speed

31. OnEnable、Awake、Start运行时的发生顺序？哪些可能在同一个对象周期中反复的发生？
  答Awake -》OnEnable-》Start，OnEnable在同一周期中可以反复地发生
  http://answers.unity3d.com/questions/217941/onenable-awake-start-order.html

32. 请简述OnBecameVisible及OnBecameInvisible的发生时机，以及这一对回调函数的意义？
  答: 当物体是否可见切换之时。可以用于只需要在物体可见时才进行的计算。

33. Unity3D如何获知场景中需要加载的数据？
  题目是获取的意思？Resource.Load，AssetBundle

34. MeshRender中material和sharedmaterial的区别？
  修改sharedMaterial将改变所有物体使用这个材质的外观，并且也改变储存在工程里的材质设置。不推荐修改由sharedMaterial返回的材质。如果你想修改渲染器的材质，使用material替代。

1. 请描述游戏动画有哪几种，以及其原理。
  答：主要有关节动画、骨骼动画、单一网格模型动画(关键帧动画)。 
  关节动画：把角色分成若干独立部分，一个部分对应一个网格模型，部分的动画连接成一个整体的动画，角色比较灵活，Quake2中使用这种动画；
  骨骼动画，广泛应用的动画方式，集成了以上两个方式的优点，骨骼按角色特点组成一定的层次结构，有关节相连，可做相对运动，皮肤作为单一网格蒙在骨骼之外，决定角色的外观；
  单一网格模型动画由一个完整的网格模型构成，在动画序列的关键帧里记录各个顶点的原位置及其改变量，然后插值运算实现动画效果，角色动画较真实。

2. alpha blend 工作原理
  答：Alpha Blend 实现透明效果，不过只能针对某块区域进行alpha操作，透明度可设。实际显示颜色 = 前景颜色*Alpha/255 + 背景颜色*(255-Alpha)/255

  alpha blend 用于做半透明效果。Color = (源颜色 *    源系数) OP (   目标颜色* 目标系数);其中OP（混合方式）有加，减，反减，取最小，取最大;

3. 写光照计算中的diffuse的计算公式
  答：diffuse = Kd x colorLight x max(N*L,0)；Kd 漫反射系数、colorLight 光的颜色、N 单位法线向量、L 由点指向光源的单位向量、其中N与L点乘，如果结果小于等于0，则漫反射为0。实际光照强度 I= 环境光(Iambient) + 漫反射光(Idiffuse) + 镜面高光(Ispecular);环境光：Iambient= Aintensity* Acolor; (Aintensity表示环境光强度，Acolor表示环境光颜色)
  漫反射光：Idiffuse = Dintensity*Dcolor*N.L;(Dintensity表示漫反射强度，Dcolor表示漫反射光颜色，N为该点的法向量，L为光源向量)
  镜面反射光：Ispecular = Sintensity*Scolor*(R.V)^n;(Sintensity表示镜面光照强度，Scolor表示镜面光颜色，R为光的反射向量，V为观察者向量，n称为镜面光指数),
  漫反射光(diffuse)计算公式为：Idiffuse = Dintensity*Dcolor*N.L ; (Dintensity表示漫反射强度，Dcolor表示漫反射光颜色，N为该点的法向量，L为光源向量)
  其他，3D渲染中，物体表面的光照计算公式为：
  I = 环境光(Iambient) + 漫反射光(Idiffuse) + 镜面高光(Ispecular);
    其中，环境光(ambient)计算公式为：
  Iambient= Aintensity* Acolor; (Aintensity表示环境光强度，Acolor表示环境光颜色)
    漫反射光(diffuse)计算公式为：
  Idiffuse = Dintensity*Dcolor*N.L ; (Dintensity表示漫反射强度，Dcolor表示漫反射光颜色，N为该点的法向量，L为光源向量)
  镜面光照(specular)计算公式为：
  Ispecular = Sintensity*Scolor*(R.V)n; (Sintensity表示镜面光照强度，Scolor表示镜面光颜色，R为光的反射向量，V为观察者向量)
  综上所得：整个光照公式为：
  I = Aintensity* Acolor+ Dintensity*Dcolor*N.L + Sintensity*Scolor*(R.V)n ;
  将一些值合并，并使用白色作为光照颜色，则上述公式可简化为：I = A + D*N.L + (R.V)n

  实际光照强度 I= 环境光(Iambient) + 漫反射光(Idiffuse) + 镜面高光(Ispecular);

  环境光：Iambient= Aintensity* Acolor; (Aintensity表示环境光强度，Acolor表示环境光颜色)

  漫反射光：Idiffuse = Dintensity*Dcolor*N.L;

  (Dintensity表示漫反射强度，Dcolor表示漫反射光颜色，N为该点的法向量，L为光源向量)

  镜面反射光：Ispecular = Sintensity*Scolor*(R.V)^n;

  (Sintensity表示镜面光照强度，Scolor表示镜面光颜色，R为光的反射向量，V为观察者向量，n称为镜面光指数)

4. LOD是什么，优缺点是什么？
  答：LOD(Level of detail)多层次细节，是最常用的游戏优化技术。它按照模型的位置和重要程度决定物体渲染的资源分配，降低非重要物体的面数和细节度，从而获得高效率的渲染运算。LOD技术即Levels of Detail的简称，意为多细节层次。LOD技术指根据物体模型的节点在显示环境中所处的位置和重要度，决定物体渲染的资源分配，降低非重要物体的面数和细节度，从而获得高效率的渲染运算。
  优点：可根据距离动态地选择渲染不同细节的模型
  缺点：加重美工的负担，要准备不同细节的同一模型，同样的会稍微增加游戏的容量。

5. 两种阴影判断的方法工作原理
  本影和半影：
  本影：景物表面上那些没有被光源直接照射的区域（全黑的轮廓分明的区域）。
  半影：景物表面上那些被某些特定光源直接照射但并非被所有特定光源直接照射的区域（半明半暗区域）
  工作原理：从光源处向物体的所有可见面投射光线，将这些面投影到场景中得到投影面，再将这些投影面与场景中的其他平面求交得出阴影多边形，保存这些阴影多边形信息，然后再按视点位置对场景进行相应处理得到所要求的视图（利用空间换时间，每次只需依据视点位置进行一次阴影计算即可，省去了一次消隐过程）
  阴影由两部分组成：本影与半影
  本影：景物表面上那些没有被光源直接照射的区域（全黑的轮廓分明的区域）
  半影：景物表面上那些被某些特定光源直接照射但并非被所有特定光源直接照射的区域（半明半暗区域）
  求阴影区域的方法：做两次消隐过程
  一次对每个光源进行消隐，求出对于光源而言不可见的区域L；
  一次对视点的位置进行消隐，求出对于视点而言可见的面S；
  shadow area= L ∩ S
  阴影分为两种：自身阴影和投射阴影
  自身阴影：因物体自身的遮挡而使光线照射不到它上面的某些可见面
  工作原理：利用背面剔除的方法求出，即假设视点在点光源的位置。
  投射阴影：因不透明物体遮挡光线使得场景中位于该物体后面的物体或区域受不到光照照射而形成的阴影
  工作原理：从光源处向物体的所有可见面投射光线，将这些面投影到场景中得到投影面，再将这些投影面与场景中的其他平面求交得出阴影多边形，保存这些阴影多边形信息，然后再按视点位置对场景进行相应处理得到所要求的视图（利用空间换时间，每次只需依据视点位置进行一次阴影计算即可，省去了一次消隐过程）
  若是动态光源此方法就无效了。

## Vertex Shader是什么？怎么计算？
  顶点着色器是一段执行在GPU上的程序，用来取代fixed pipeline中的transformation和lighting，Vertex Shader主要操作顶点。
  Vertex Shader对输入顶点完成了从local space到homogeneous space（齐次空间）的变换过程，homogeneous space即projection space的下一个space。在这其间共有world transformation, view transformation和projection transformation及lighting几个过程。


## MipMap是什么？作用？
  答：MipMapping：在三维计算机图形的贴图渲染中有常用的技术，为加快渲染进度和减少图像锯齿，贴图被处理成由一系列被预先计算和优化过的图片组成的文件，这样的贴图被称为MipMap。在三维计算机图形的贴图渲染中有一个常用的技术被称为Mipmapping。为了加快渲染速度和减少图像锯齿，贴图被处理成由一系列被预先计算和优化过的图片组成的文件,这样的贴图被称为 MIP map 或者 mipmap。

## 用u3d实现2d游戏，有几种方式？
  答：一种用UI实现(GUI,NGUI...)，一种是采用3d实体对象（plane），绘制在3d对象上，调节摄像机，采用平行投影模式或则固定视角。   1.利用引擎自带的GUI
   2.把摄像机设为Orthographic，用面片作为2d元素
   3.利用第三方插件：NGUI、2dToolkit

## Unity中碰撞器(Collider)和触发器(Trigger)的区别?
  碰撞器（Collider）有碰撞效果，IsTrigger=false，可以调用OnCollisionEnter/Stay/Exit函数
  触发器(Trigger)没有碰撞效果，isTrigger=true，可以调用OnTriggerEnter/Stay/Exit函数
  碰撞器是触发器的载体，而触发器只是碰撞器身上的一个属性。
  当Is Trigger=false时，碰撞器根据物理引擎引发碰撞，产生碰撞的效果，可以调用OnCollisionEnter/Stay/Exit函数；
  当Is Trigger=true时，碰撞器被物理引擎所忽略，没有碰撞效果，可以调用OnTriggerEnter/Stay/Exit函数。
  如果既要检测到物体的接触又不想让碰撞检测影响物体移动或要检测一个物件是否经过空间中的某个区域这时就可以用到触发器
  碰撞器会有碰撞的效果，IsTrigger = false,可以调用OnCollisionEnter/Stay/Exit函数。    触发器没有碰撞效果，isTrigger = true,可以调用OnTriggerEnter/stay/exit函数
  碰撞器是触发器的载体，而触发器只是碰撞器身上的一个属性。当Is Trigger=false时，碰撞器根据物理引擎引发碰撞，产生碰撞的效果，可以调用OnCollisionEnter/Stay/Exit函数;

  当Is Trigger=true时，碰撞器被物理引擎所忽略，没有碰撞效果，可以调用OnTriggerEnter/Stay/Exit函数。

  如果既要检测到物体的接触又不想让碰撞检测影响物体移动或要检测一个物件是否经过空间中的某个区域这时就可以用到触发器


## 物体发生碰撞的必要条件
  答：需要检测碰撞的物体身上存在刚体组件（或被检测物体），也要碰撞器collider
  必须带有collider碰撞器和rigibody刚体属性或者人物控制器，其实人物控制器就包含了前两者，另外一个物体也要必须带有Collider，Collider分类：网格碰撞器，盒子碰撞器，胶囊碰撞器，球型碰撞器，地形碰撞器！
  两个物体都必须带有碰撞器Collider，其中一个物体还必须带有Rigidbody刚体。
  其中至少一个物体（运动的）必须带有碰撞器（collider）+刚体(Rigidbody)或者CharacterController，另一个物体也必须至少带有collider。　　两个物体都必须带有碰撞器(Collider)，其中一个物体还必须带有Rigidbody刚体，而且必须是运动的物体带有Rigidbody脚本才能检测到碰撞。

## CharacterController和Rigidbody的区别
  Rigidbody具有完全真实物理的特性，而CharacterController可以说是受限的的Rigidbody，具有一定的物理效果但不是完全真实的。
  CharacterController自带胶囊碰撞器，里面好像封装了一个刚体,Rigidbody就是刚体，使物体带有物理的特性?
  Rigidbody具有完全真实物理的特性，Unity中物理系统最基本的一个组件，包含了常用的物理特性。而CharacterController可以说是受限的的Rigidbody，具有一定的物理效果但不是完全真实的，是Unity为了使开发者能方便的开发第一人称视角的游戏而封装的一个组件

## 物体发生碰撞时，有几个阶段，分别对应的函数
  答：三个阶段， OnCollisionEnter， OnCollisionStay，OnCollisionExit

## u3d中，几种施加力的方式，描述出来。
  rigidbody.AddForce/AddForceAtPosition,都是rigidbody的成员函数
  rigidbody.AddForce，rigidbody.AddForceAtPosition
  a)爆炸力（AddExplosionForce(force : float, forcePos : Vector3，radius : float, upwards : float, mode : ForceMode)），应用一个力到刚体来模拟爆炸效果,就是在爆炸力中心坐标position,搜索在radius范围内的刚体，对其释放力作用，超出radius范围的刚体不受力作用，爆炸力将随着刚体的距离线性减弱。
  b)力AddForce(force : Vector3, mode : ForceMode),主要施力给一个刚，使其移动。
  c)位置力AddForceAtPosition(force : Vector3, position : Vector3, mode : ForceMode), 在position施加一个力，施力的主体将会受到一个力和力矩。
  d)相对力AddRelativeForce(force : Vector3, mode : ForceMode),类似于AddForce；

## 什么叫做链条关节
  Hinge Joint，可以模拟两个物体间用一根链条连接在一起的情况，能保持两个物体在一个固定距离内部相互移动而不产生作用力，但是达到固定距离后就会产生拉力。

## 物体自旋转使用的函数叫什么
  答：transform.Rotate(eulerAngles : Vector3, relativeTo : Space = Space.self);

## 物体绕某点旋转使用函数叫什么
  答：transform.RotateAround(point : Vector3, axis : Vector3, angles : float)

## u3d提供了一个用于保存读取数据的类，（playerPrefs），请列出保存读取整形数据的函数
  答：PlayerPrefs.SetInt()，PlayerPrefs.GetInt(key : string, defaultValue : int = 0)

## unity3d提供了几种光源，分别是什么
  答：平行光,点光源，聚光灯，环境光四种。
  平行光：Directional Light
  点光源：Point Light
  聚光灯：Spot Light
  区域光源：Area Light
  共4种，DirectionalLight、PointLight、SpotLight、AreaLight（用于烘焙）

## unity3d从唤醒到销毁有一段生命周期，请列出系统自己调用的几个重要方法。
  答：void Awake(),void Start(), void Update(), void FixedUpdate(),void LateUpdate(), void OnGUI() ，void Reset(), OnDisable(), void OnDestroy()
  Awake——>Start——>Update——>FixedUpdate——>LateUpdate——>OnGUI——>Reset——>OnDisable——>OnDestroy
  ![](../_images/unity/09594731b420081605.png)

## 物理更新一般在哪个系统函数里？
  答：void FixedUpdate()FixedUpdate,每固定帧绘制时执行一次，和update不同的是FixedUpdate是渲染帧执行，如果你的渲染帧效率低下的时候FixedUpdate调用次数就会跟着下降。FixedUpdate比较适合用于物理引擎的计算，因为是跟每帧的渲染有关。Update就比较适合做控制。

## 移动相机动作在哪个函数里，为什么在这个函数里。
  答：void LateUpdate(),因为这个函数是在Update执行完毕才执行的，不然的话就有可能出现摄像机里面什么都看到的情况。LateUpdate，是在所有的update结束后才调用，比较适合用于命令脚本的执行。官网上例子是摄像机的跟随，都是所有的update操作完才进行摄像机的跟进，不然就有可能出现摄像机已经推进了，但是视角里还未有角色的空帧出现。

## 当游戏中需要频繁创建一个物体对象时，我们需要怎么做来节省内存。
  做一个pool，游戏开始时预先实例化足够的数量，然后用的时候取不用的时候收回

## 什么是渲染管道？
  答：是指在显示器上为了显示出图像而经过的一系列必要操作。 渲染管道中的很多步骤，都要将几何物体从一个坐标系中变换到另一个坐标系中去。
  主要步骤有：本地坐标->视图坐标->背面裁剪->光照->裁剪->投影->视图变换->光栅化。

## 如何优化内存？
  答：有很多种方式，例如
  1.压缩自带类库；
  2.将暂时不用的以后还需要使用的物体隐藏起来而不是直接Destroy掉；
  3.释放AssetBundle占用的资源；
  4.降低模型的片面数，降低模型的骨骼数量，降低贴图的大小；
  5.使用光照贴图，使用多层次细节(LOD)，使用着色器(Shader)，使用预设(Prefab)。

## 动态加载资源的方式？
  1.Resources.Load();
  2.AssetBundle
  Unity5.1版本后可以选择使用Git:   https://github.com/applexiaohao/LOAssetFramework.git

## 什么是协同程序？
  答：在主线程运行时同时开启另一段逻辑处理，来协助当前程序的执行。换句话说，开启协程就是开启一个可以与程序并行的逻辑。可以用来控制运动、序列以及对象的行为。在主线程运行的同时开启另一段逻辑处理，来协助当前程序的执行，协程很像多线程，但是不是多线程，Unity的协程实在每帧结束之后去检测yield的条件是否满足。


1. 反向旋转动画的方法是什么？
2. 碰撞检测需要物体具备什么属性？
3. 用代码实现第三角色控制器
4. 实现吊机吊物体的功能
5. 获取、增加、删除组件的命令分别是什么？
  获取：GetComponent
  增加：AddComponent
  删除：Destroy

6. CrossFade命令作用是(C)
  A.动画放大 B.动画转换 C.动画的淡入为其他动画

7. Application.loadLevel命令为(A)
  A.加载关卡 B.异步加载关卡 C.加载动作

8. 调试记录到控制台的命令是什么？
  Debug.Log();

9. 编辑器类存放路径是什么？
  工程目录下的Assets/Editor文件夹下。

10. 使用原生GUI创建一个可以拖动的窗口命令是什么？
  GUI.DragWindow();

11. localPosition与Position的使用区别？
  localPosition：自身位置，相对于父级的变换的位置。 Position：在世界坐标transform的位置

12. 意义连线
  Mathf.Round  四舍五入
  Mathf.Clamp  限制
  Mathf.Lerp   插值

13. 写一个计时器工具，从整点开始计时，格式为：00:00:00

14. 写出Animation的五个方法

15. 怎么拿到一个对象上脚本的方法
  GameObject.GetComponent<>();

16. 上机题：用鼠标实现在场景中拖动物体，用鼠标滚轮实现缩放(用一个Cube即可)。

17. 请简述向量的点乘，向量的叉乘以及向量归一化的几何意义？
  点乘的几何意义是：计算两个向量之间的夹角，以及在某一方向上的投影；
  叉乘的几何意义是：创建垂直于平面，三角形，或者多边形的向量；参考：http://blog.sina.com.cn/s/blog_9283b6f601017hfw.html
 

5. 一个简单的游戏,怪物会走动\攻击\死亡,游戏角色会走动,跳跃\攻击\格挡\死亡,还会接受玩家从输入端输入的指令,NPC会走动,他们彼此之间可以互相通信.请画出以上三种角色的UML图示.
答：

6. NGUI Button怎样接受用户点击并调用函数,具体方法名称是什么
  OnClick()主要是在UICamera脚本中用射线判断点击的物体并通过SendMessage调用OnClick() OnPress()等函数，可以说NGUI的按钮是通过发消息这个方式调用的。

7. 怎么判断两个平面是否相交?不能用碰撞体,说出计算方法
  答：https://www.baidu.com/s?wd=怎么判断两个平面是否相交&rsv_spt=1&issp=1&f=3&rsv_bp=1&rsv_idx=2&ie=utf-8&tn=baiduhome_pg&rsv_enter=0&rsv_pq=abcac720000083b6&rsv_t=ecdctoXQNURcfX%2FVm8MQdPtiBLttYO4HqJrZ1yNsNcxUqDe6DM4ovlQT4HdHZJMh6guT&inputT=2590&oq=C%23%20lamb&rsv_n=2&bs=c%23%20lambda

8. <愤怒的小鸟>给予初速度以后,怎么让小鸟受到重力和空气阻力的影响而绘制抛物线轨迹,说出具体的计算方法.
 Vector3 v代表初速度v'代表现在的速度，假设小鸟是沿的z轴也就是transform.forward方向运动的质量为1，那么v‘=v-new Vector3(0,g*t,f*t)，transform.Translate(v')做的就是抛物线运动（g为重力加速度不要用现实中的需要自己调试，f为阻力也要自己调试设置，t为时间）


先识别一下，你到了公司会让你干什么：

1.UI（面试题会偏向NGUI等ui操作，会考到一些简单的排序算法，数据结构，问题处理思路等）
2.数据逻辑层（会考到数据结构的搭配，配置表的构造等）
3.游戏控制流程（会考到状态机的设计，与服务器协议的设计，以及少量lua或python脚本编写内容）
4.项目架构（主程内容。。你懂得。反正我还不够格面这个。。）

UI的没什么好说的，去了之后天天layout，毫无乐趣，但是吧，新手都得虐过一次，才可以。数据逻辑层，也就是统筹整个游戏的后台运算数据，依照网络模块给予的数据，维护整个数据基的稳定和swift。这个工作与 unity

基本无关，但是是必不可少的，一般简历上写过什么acm之类的，会让你搞这个吧。游戏控制流程，至此开始进入高级程序员的行列，你会接触很多unity相关的内容，比如技能释放，角色换装，角色状态控制等等有趣的内容，通常如果你不是自废武功转3d或者再原来公司本来就干这个，只是干腻了跳槽而已，是不会让新手干这个的。项目架构这个略掉，我没资格谈这个。

说道这里本来应该结束废话的，但是可能引导大家对UI有了新的看法）——它不值得去做。其实不是的，UI这东西虽然无聊，但是是新手接触引擎，熟悉代码的最佳通道，同时也是所有公司愿意开放给各位新手的一个免费培训通道吧，因为UI代码很不值钱，你走了谁都能接上，所以他们愿意用新手。

下面来谈谈，如果我时面试出卷子的那个【sb】，我会问什么。这些问题都是项目中坑过我，害过我，让我哭让我痛得问题。

UI方面：

1.你觉得为什么UI摄像机和场景摄像机能协同工作，而且工作的这么尽如人意呢？

答案：UI摄像机和场景摄像机分别属于两个渲染层（Layer），所以它们之间的渲染互不干扰。它们工作得尽如人意（没有发生先后错乱，UI永远位于场景之上层）的原因就是因为摄像机深度（depth）控制的好。

2.你觉得怎么防止UI控件被点穿（如何过滤掉点击事件）。

答案：使用一个可渲染的物体或者pannel，绑定boxcollider组件即可。如果你单用一个不可渲染的物体（这里点cao一下UIwiget），即使设定了大小和boxcollider，也是无法屏蔽的。（坑在这里）。

3.关于UIGrid问题

答案：我基本不会使用UIGrid，我会直接在代码里设置localpos等等。（这个控件是很坑的）。

4.你对UI功能模块之间相互通信有什么好看法。（或者问成Broadcast和sendMes的看法）

答案：UI模块之间尽量解耦合，使用BroadCast机智或者delegate机智。由于Unity自身的BroadCast和SendMsg效率是很低的，（务必百度一下这俩的区别，都有，我懒得复制粘贴了。）所以推荐使用NGUI自带的那个消息机智。具体的使用方法，下载任何一款NGUI3.5左右的版本，都能清楚的看到。更或者自己实现一个msgerpool也可以，思路是使用泛型写法+字典+delegate，效率也很高。

5.众里寻他千百度，你怎么样能迅速找到某一个UI控件。

答案：分情况处理，这是一个优化策略的题目。首先如果这个控件我在awake的时候能知道，那么我会把它存成一个private变量，代码中使用的时候直接使用即可。如果不行，这个子物体是动态生成的（他可能有或者没有），那尽量使用FindChild，得到之后加以判断。如果不得不全局找一个东西（比如找到角色物体），才会用Find。

6.你对遮挡关系有什么好的策略。

答案：这种问题的诞生是由于Ngui2采用了一种zorder+depth方式处理遮挡关系造成的，已经在ngui3里完全屏蔽掉了。只要维护depth即可。

7.你对屏幕适配有什么好主意。

答案：屏幕适配根本没有完美的解决方案，如果是全屏模块，那么锁定目标机型，将目标机型做成满屏，其他机型充满宽或高后留白（也叫留黑边）处理即可。如果是屏幕模块（比如战斗界面，主城界面等等，你会看到下边的场景那种），需要采用Anchor来解决。NGUI2 Anchor有独立的控件，NGUI3Anchor已经被搞到Wighet和Pannel中，这个自己下去好好研究研究吧。

数据逻辑层方面

1.请简述一下你对数据结构的选取有什么看法

答案：批量取，经常遍历的数据，会采取List来存储，经常查找的会采取字典存储。同时如果多个字段比较重要（比如一份配置表经常会id索引以及name索引），我会开辟多个字典进行存储，牺牲空间换取逆向查找效率。

2.请把这份配置文件解析成你想要的数据结构，给我看看

答案：这份配置文件如果是xml或者json，那么它肯定会被先转换成HashTable，然后你根据配置表的可能拥有的字段（比如有一个物体，它有“使用后增加体力”这个字段，有的没有，你要用ContainsKey来检测是否有字段），挨个拆出并且存入你想要的数据结构中，如果你想要的数据结构是字典，需要注意的是在加入key的时候，应该询问是否包含了这个key，如果包含，则修改，如果不包含，则添加，，大家智力都没有问题。。试一试就会很清楚。

如果这个配置文件为excell，那么它将会被转化成一个string数组，数组的每一个元素，都是一个字段数据，按照配置表的顺序挨个取出来存入你得数据结构中即可。

3.请简述一下C#中，结构体和class的用法

（再次鸣谢面试我得那位同事，容忍了我回答的很烂）

答案：如果你是自废武功，从C++转到C#，这俩东西会让你大跌眼镜。C++中结构体和类几乎没有区别。C#中，结构体属于对象，而类属于引用，结构体不需要new出来，类必须new出来。这是个巨大的坑。比如你搞了一个结构体链表：

List<structA> structtestlist = new List<structA>();

然后给这个链表中注入数据，注入数据之后，你想更改其中的一个链节，你是这么做得

structA structlink = structtestlist[2];

structlink.data = 3;

这样做，根本没有修改到structtestlist中得值！因为声明structlink那一句话，是个对象，并不是指向那个（链节）的“指针”！

但是如果是class，你这么做毫无烦恼。

这个东西建议大家试试。很坑比。

4.接受到网络发来的数据，你会怎么办

答案：着手做项目之前，会搞一个txt，客户端内部消息协议罗列到上边，这个消息协议是网络协议的处理映射。比如网络协议告诉我，“因为你刚才吃了一个药，吃成功了，现在你得给我刷新，让用户大大能够看到”，我就必须通知所牵连的模块：“你得数据被刷新了，再老地方重新取一次，刷新界面给用户大大看”。牵连到得模块，比如就是战斗UI，它要把血条给加上去，还有character信息的数据基，更改当前血量数据等等。这些模块收到协议之后，改变自己的值：比如UI就会从数据基里取，而数据基会从网络包或者配置表里取等等。

游戏控制流程：

1.你对资源加载有什么看法。

答案：首先资源加载这个东西，就是老大难，用户一遍遍的吐槽加载的慢，但是谁也不愿意让加载的少导致后边必须加载而变得卡顿。对于比较耗费的加载，属于IO操作，在游戏刚登陆的时候，进行初始化加载。强制加载一些重要模块，比如登陆窗口（用户输入用户名密码的地方）等等。选择性加载一些必要的模块：如果这人需要新手引导，则加载新手引导模块，否则不加载，反之亦然。这些用户一定会用（或者一定不用）的模块，在登陆时候处理好。对于应用模块：比如我打开一个很庞大的UI，这个怎么加载，如果这个模块很吸金，用户用得概率很大，那么必须在登陆时加载，否则那只能用户打开的时候加载了，加载的时候要转圈提示用户。。关于场景加载更加的莫衷一是。既然大家被面到这一步了，相信也不需要我废话了，一个程序有一个程序的活法不是吗？

2.请给我设计一个状态机，完成一个简单的xxxx情景。

答：状态机并不是单纯的switch结构，如果你是新手，你说switch我可以容忍，如果你干了好些年 2D

x，然后想自废武功搞U3d了，再说switch就直接pass了。可以去搜一下相关资料，水不是很深，但篇幅有限。大概意思就是，每个对象都维护自己的状态机，状态机的状态改变，靠状态触发器。所有的状态（不是状态机。。状态机是一个主管它负责调度各种状态，状态是各种。。。状态。。哎不可言传啊。。），都会重写begin，excute，end这三个函数，本来敲了一大堆。。发现说也说不清楚，大家自己去看看吧还是。毕竟这只提供一种面试方式而已。。

3.角色换装，技能释放你会怎么做。

角色换装这个，各有各的活法，尸块换装，纸娃娃贴图等，这个莫衷一是，可以自己百度去，找一个自己喜欢的实现一下试试吧。技能释放水是很深的，这个能答就答，答不上来也不丢人。估计被面这些的，肯定都比我强，也就不需要我比比什么的了。。

4.动态更新有什么看法。

答：这个题牵扯到assetbundle问题。其中assetbundle面对资源的处理方法是截然不同的。比如对于prefab，可以很好的搞上去，对于二进制文件，也能搞得不错，但是对于代码这个东西，往往是大家争论的焦点——毕竟代码这个东西，iosAppStore是肯定不让你搞得，因为unity本身也不受苹果待见，所以它怕被秒杀，也不给用户提供更新脚本的功能。这就导致比如你更新了一个prefab，没法使用它上边挂的脚本！那更新有个jb用。。。提供一下解决代码更新的一点看法，现在我再尝试搞这一块，一孔之见，轻喷

安卓上可以使用unityreflaction机制进行编译后代码的动态更新，这个广大google开发者肯定是知道的。但是问题就是，这种机制会导致很庞大的开发成本——从代码层面完全跟ios搞了个大分支，如果是想跨平台的，这恐怕是不可取的吧。。

第二就是可以采用lua脚本无缝编写方法，因为unity脚本并不是真正的脚本，真正脚本的威力是它可以动态编译到宿主上，这点太牛逼了。lua就是这种牛逼脚本。所以你可以无缝换lua。。。这个没问题。所以立项就应该想好是lua还是什么的。。别回来做了一半，想改lua了，那就sb了。。

这两种机制在App Store上肯定是不让你搞的，第一种，越狱平台也不让搞，因为unity不让你搞。。

其实还有神秘的第三种做法，这第三种做法，就不分享了，毕竟还没有实验，说出来怕坑杀大家。。

不定期会更新这个帖子，时而一个月，时而半年的。。。大家见谅，希望对大家有帮助

1. 哪种实时光源是Unity中没有的？ A:点光源 B:方向光 C:聚光灯 D:日光灯 
2. 如何在Unity中创建地形系统？ A：Terrain->Create Terrain B：Component->Create Terrain C：Asset->Create Terrain D：Windows->Create Terrain 
3. 以下哪种操作步骤可以在场景中添加“Wind Zone”？ A：Terrain -> Wind Zone B：GameObject -> Create Other -> Wind Zone C：Component -> Physics -> Wind Zone D：Assets -> Create -> Wind Zone 
4. 在Unity编辑器中创建一个Directional Light，以下步骤正确的是？ A：Edit -> Rendering Setting -> Directional Light B：GameObject -> Create Other -> Directional Light C：Component -> Rendering -> Directional Light D：Assets -> Directional Light 
5. 下列哪一项不属于Camera中的“Clear Flags”？ A：Skybox B：Solid Color C：Depth Only D：Background 
6. 以下哪种脚本语言是Unity编辑器所不支持的？ A：JavaScript B：C# C：Boo D：Perl 
7. 对于Prefab，以下说法错误的是？ A：Prefab资源可以在项目中多次重复使用 B：由Prefab实例出的GameObject，其在Hierarchy视图中表现为蓝色 C：Prefab上的组件信息一经改变，其实例出的GameObject也会自动改变 D：实例出的GameObject上的组件信息一经改变，其对应的Prefab也会自动改变 
8. 下面哪种做法可以打开Unity的Asset Store？ A：Windows -> Asset Store B：Edit -> Asset Store C：File -> Asset Store D：Assets -> Asset Store 
9. 在哪个面板中可以修改物体的空间属性，如位置、朝向、大小等？ A：Project B：Inspector C：Hierarchy D：Toolbar 
10. 如何为一个Asset资源设定一个Label，从而能够方便准确的搜索到? A：在Project窗口中选中一个Asset,右键->Create->Label B：在Project窗口中选中一个Asset,右键->Add Label C：在Project窗口中选中一个Asset,在Inspector窗口中点击添加Label的图标 D：在Project窗口中选中一个Asset,在Inspector窗口中点击按钮“Add Label” 
1. Mecanim系统中，Body Mask的作用是？ A:指定身体的某一部分是否参与骨骼动画 B:指定身体的某一部分是否参与物理模拟 C:指定身体的某一部分是否可以输出骨骼信息 D:指定身体的某一部分是否参与渲染 
2. 以下哪种操作步骤可以打开Unity编辑器的Lightmapping视图？ A：File --> Lightmapping B：Assets --> Lightmapping C：Windows --> Lightmapping D：Component --> Lightmapping 
3. 下列关于光照贴图，说法错误的是？ A：使用光照贴图比使用实时光源渲染要快 B：可以降低游戏内存消耗 C：可以增加场景真实感 D：多个物体可以使用同一张光照贴图 
4. 如何为物体添加光照贴图所使用的UV？ A：不用添加，任何时候都会自动生成 B：更改物体导入设置，勾选”Generate Lightmap UVs” C：更改物体导入设置，勾选 “Swap UVs” D：更改物体导入设置，在UVs 选项中选择” Use Lightmaps” 
5. 在哪个模块下可以修改Render Path？ A：Camera B：Light C：Render Settings D：Project Settings->Quality 
6. 以下哪项技术不是目前Unity所支持的Occlusion Culling技术？ A：PVS only B：PVS and dynamic objects C：Automatic Portal Generation D：Dynamic Only 
7. 关于Vector3的API，以下说法正确的是？ A：Vector3.normalize可以获取一个三维向量的法线向量； B：Vector3.magnitude可以获取一个三维向量的长度； C：Vector3.forward与Vector3（0,0,1）是一样的意思； D：Vector3.Dot（向量A，向量B）是用来计算向量A与向量B的叉积 
8. 以下哪个函数在游戏进入新场景后会被马上调用？ A：MonoBehaviour.OnSceneWasLoaded B：MonoBehaviour.OnSceneEnter C：MonoBehaviour.OnLevelWasLoaded D：MonoBehaviour.OnLevelEnter 9. 什么是导航网格（ NavMesh）？ A：一种用于描述相机轨迹的网格 B：一种被优化过的物体网格 C：一种用于物理碰撞的网格 D：一种用于实现自动寻路的网格 
10. 下列那些选项不是网格层属性的固有选项? A：Default B：Walkable C：Not Walkable D：Jump

九：简述一下对象池，你觉得在FPS里哪些东西适合使用对象池

　　对象池就存放需要被反复调用资源的一个空间，当一个对象回大量生成的时候如果每次都销毁创建会很费时间，通过对象池把暂时不用的对象放到一个池中(也就是一个集合)。当下次要重新生成这个对象的时候先去池中查找一下是否有可用的对象，如果有的话就直接拿出来使用，不需要再创建。如果池中没有可用的对象，才需要重新创建，利用空间换时间来达到游戏的高速运行效果，在FPS游戏中要常被大量复制的对象包括子弹，敌人，粒子等


## GPU的工作原理
简而言之，GPU的图形（处理）流水线完成如下的工作：（并不一定是按照如下顺序）
顶点处理：这阶段GPU读取描述3D图形外观的顶点数据并根据顶点数据确定3D图形的形状及位置关系，建立起3D图形的骨架。在支持DX8和DX9规格的GPU中，这些工作由硬件实现的Vertex Shader（定点着色器）完成。
光栅化计算：显示器实际显示的图像是由像素组成的，我们需要将上面生成的图形上的点和线通过一定的算法转换到相应的像素点。把一个矢量图形转换为一系列像素点的过程就称为光栅化。例如，一条数学表示的斜线段，最终被转化成阶梯状的连续像素点。
纹理帖图：顶点单元生成的多边形只构成了3D物体的轮廓，而纹理映射（texture mapping）工作完成对多变形表面的帖图，通俗的说，就是将多边形的表面贴上相应的图片，从而生成“真实”的图形。TMU（Texture mapping unit）即是用来完成此项工作。
像素处理：这阶段（在对每个像素进行光栅化处理期间）GPU完成对像素的计算和处理，从而确定每个像素的最终属性。在支持DX8和DX9规格的GPU中，这些工作由硬件实现的Pixel Shader（像素着色器）完成。
最终输出：由ROP（光栅化引擎）最终完成像素的输出，1帧渲染完毕后，被送到显存帧缓冲区。
总结：GPU的工作通俗的来说就是完成3D图形的生成，将图形映射到相应的像素点上，对每个像素进行计算确定最终颜色并完成输出。


## Unity3D是否支持写成多线程程序？如果支持的话需要注意什么？
答：仅能从主线程中访问Unity3D的组件，对象和Unity3D系统调用
支持：如果同时你要处理很多事情或者与Unity的对象互动小可以用thread,否则使用coroutine。
注意：C#中有lock这个关键字,以确保只有一个线程可以在特定时间内访问特定的对象

## Unity3D的协程和C#线程之间的区别是什么？
答：多线程程序同时运行多个线程 ，而在任一指定时刻只有一个协程在运行，并且这个正在运行的协同程序只在必要时才被挂起。
除主线程之外的线程无法访问Unity3D的对象、组件、方法。
Unity3d没有多线程的概念，不过unity也给我们提供了StartCoroutine（协同程序）和LoadLevelAsync（异步加载关卡）后台加载场景的方法。 StartCoroutine为什么叫协同程序呢，所谓协同，就是当你在StartCoroutine的函数体里处理一段代码时，利用yield语句等待执行结果，这期间不影响主程序的继续执行，可以协同工作。

## 什么叫动态合批？跟静态合批有什么区别？
答：如果动态物体共用着相同的材质，那么Unity会自动对这些物体进行批处理。动态批处理操作是自动完成的，并不需要你进行额外的操作。
区别：动态批处理一切都是自动的，不需要做任何操作，而且物体是可以移动的，但是限制很多。静态批处理：自由度很高，限制很少，缺点可能会占用更多的内存，而且经过静态批处理后的所有物体都不可以再移动了。



六十一：什么是LightMap？
答：LightMap:就是指在三维软件里实现打好光，然后渲染把场景各表面的光照输出到贴图上，最后又通过引擎贴到场景上，这样就使物体有了光照的感觉。

六十二：Unity和cocos2d的区别
答：

Unity3D支持C#、javascript等，cocos2d-x 支持c++、Html5、Lua等。
cocos2d 开源 并且免费
Unity3D支持iOS、Android、Flash、Windows、Mac、Wii等平台的游戏开发，cocos2d-x支持iOS、Android、WP等。


六十四:Unity3D Shader分哪几种，有什么区别？
答：表面着色器的抽象层次比较高，它可以轻松地以简洁方式实现复杂着色。表面着色器可同时在前向渲染及延迟渲染模式下正常工作。
顶点片段着色器可以非常灵活地实现需要的效果，但是需要编写更多的代码，并且很难与Unity的渲染管线完美集成。
固定功能管线着色器可以作为前两种着色器的备用选择，当硬件无法运行那些酷炫Shader的时，还可以通过固定功能管线着色器来绘制出一些基本的内容。

六十五：
已知strcpy函数的原型是：
char * strcpy(char * strDest,const char * strSrc);
1.不调用库函数，实现strcpy函数。
2.解释为什么要返回char *

    char * strcpy(char * strDest,const char * strSrc)
        {
                if ((strDest==NULL)||(strSrc==NULL)) //[1]
                        throw "Invalid argument(s)"; //[2]
                char * strDestCopy=strDest;  //[3]
                while ((*strDest++=*strSrc++)!='\0'); //[4]
                return strDestCopy;
        }
错误的做法：

    //不检查指针的有效性，说明答题者不注重代码的健壮性。
    //检查指针的有效性时使用((!strDest)||(!strSrc))或(!(strDest&&strSrc))，说明答题者对C语言中类型的隐式转换没有深刻认识。在本例中char *转换为bool即是类型隐式转换，这种功能虽然灵活，但更多的是导致出错概率增大和维护成本升高。所以C++专门增加了bool、true、false三个关键字以提供更安全的条件表达式。
    //检查指针的有效性时使用((strDest==0)||(strSrc==0))，说明答题者不知道使用常量的好处。直接使用字面常量（如本例中的0）会减少程序的可维护性。0虽然简单，但程序中可能出现很多处对指针的检查，万一出现笔误，编译器不能发现，生成的程序内含逻辑错误，很难排除。而使用NULL代替0，如果出现拼写错误，编译器就会检查出来。
//return new string("Invalid argument(s)");，说明答题者根本不知道返回值的用途，并且他对内存泄漏也没有警惕心。从函数中返回函数体内分配的内存是十分危险的做法，他把释放内存的义务抛给不知情的调用者，绝大多数情况下，调用者不会释放内存，这导致内存泄漏。
//return 0;，说明答题者没有掌握异常机制。调用者有可能忘记检查返回值，调用者还可能无法检查返回值（见后面的链式表达式）。妄想让返回值肩负返回正确值和异常值的双重功能，其结果往往是两种功能都失效。应该以抛出异常来代替返回值，这样可以减轻调用者的负担、使错误不会被忽略、增强程序的可维护性。
//忘记保存原始的strDest值，说明答题者逻辑思维不严密。
//循环写成while (*strDest++=*strSrc++);，同[1](B)。
//循环写成while (*strSrc!='\0') *strDest++=*strSrc++;，说明答题者对边界条件的检查不力。循环体结束后，strDest字符串的末尾没有正确地加上'\0'。
/**
 *返回strDest的原始值使函数能够支持链式表达式，增加了函数的“附加值”。同样功能的函数，如果能合理地提高的可用性，自然就更加理想。
    链式表达式的形式如：
        `nt iLength=strlen(strcpy(strA,strB));
    又如：
        char * strA=strcpy(new char[10],strB);
    返回strSrc的原始值是错误的。其一，源字符串肯定是已知的，返回它没有意义。其二，不能支持形如第二例的表达式。其三，为了保护源字符串，形参用const限定strSrc所指的内容，把const char *作为char *返回，类型不符，编译报错。
 */


七十三：协同程序的执行代码是什么？有何用处，有何缺点？

function Start() { 
    // - After 0 seconds, prints "Starting 0.0"
    // - After 0 seconds, prints "Before WaitAndPrint Finishes 0.0"
    // - After 2 seconds, prints "WaitAndPrint 2.0"
    // 先打印"Starting 0.0"和"Before WaitAndPrint Finishes 0.0"两句,2秒后打印"WaitAndPrint 2.0"
    print ("Starting " + Time.time );
    // Start function WaitAndPrint as a coroutine. And continue execution while it is running
    // this is the same as WaintAndPrint(2.0) as the compiler does it for you automatically
    // 协同程序WaitAndPrint在Start函数内执行,可以视同于它与Start函数同步执行.
    StartCoroutine(WaitAndPrint(2.0)); 
    print ("Before WaitAndPrint Finishes " + Time.time );
}

function WaitAndPrint (waitTime : float) {
    // suspend execution for waitTime seconds
    // 暂停执行waitTime秒
    yield WaitForSeconds (waitTime);
    print ("WaitAndPrint "+ Time.time );
}
作用：一个协同程序在执行过程中,可以在任意位置使用yield语句。yield的返回值控制何时恢复协同程序向下执行。协同程序在对象自有帧执行过程中堪称优秀。协同程序在性能上没有更多的开销。
缺点：协同程序并非真线程，可能会发生堵塞。

七十四：什么是里氏代换元则？
答：里氏替换原则(Liskov Substitution Principle LSP)面向对象设计的基本原则之一。 里氏替换原则中说，任何基类可以出现的地方，子类一定可以出现，作用方便扩展功能能

七十五：Mock和Stub有何区别？
Mock与Stub的区别：Mock:关注行为验证。细粒度的测试，即代码的逻辑，多数情况下用于单元测试。Stub：关注状态验证。粗粒度的测试，在某个依赖系统不存在或者还没实现或者难以测试的情况下使用，例如访问文件系统，数据库连接，远程协议等。



八十：射线检测碰撞物的原理是？
答：射线是3D世界中一个点向一个方向发射的一条无终点的线，在发射轨迹中与其他物体发生碰撞时，它将停止发射 。

八十一：客户端与服务器交互方式有几种？
答： socket通常也称作"套接字",实现服务器和客户端之间的物理连接，并进行数据传输，主要有UDP和TCP两个协议。Socket处于网络协议的传输层。
http协议传输的主要有http协议 和基于http协议的Soap协议（web service）,常见的方式是 http 的post 和get 请求，web 服务。

八十二：Unity和Android与iOS如何交互？

八十三：Unity中，照相机的Clipping Planes的作用是什么？调整Near、Fare两个值时，应该注意什么？
答：剪裁平面 。从相机到开始渲染和停止渲染之间的距离。

八十四：如何在Unity3D中查看场景的面试，顶点数和Draw Call数？如何降低Draw Call数？
答：在Game视图右上角点击Stats。降低Draw Call 的技术是Draw Call Batching

八十五：请问alpha test在何时使用？能达到什么效果？
Alpha Test,中文就是透明度测试。简而言之就是V&F shader中最后fragment函数输出的该点颜色值（即上一讲frag的输出half4）的alpha值与固定值进行比较。Alpha Test语句通常于Pass{}中的起始位置。Alpha Test产生的效果也很极端，要么完全透明，即看不到，要么完全不透明。

八十六：UNITY3d在移动设备上的一些优化资源的方法
答：
1.使用assetbundle，实现资源分离和共享，将内存控制到200m之内，同时也可以实现资源的在线更新
2.顶点数对渲染无论是cpu还是gpu都是压力最大的贡献者，降低顶点数到8万以下，fps稳定到了30帧左右
3.只使用一盏动态光，不是用阴影，不使用光照探头
粒子系统是cpu上的大头
4.剪裁粒子系统
5.合并同时出现的粒子系统
6.自己实现轻量级的粒子系统
animator也是一个效率奇差的地方
7.把不需要跟骨骼动画和动作过渡的地方全部使用animation，控制骨骼数量在30根以下
8.animator出视野不更新
9.删除无意义的animator
10.animator的初始化很耗时（粒子上能不能尽量不用animator）
11.除主角外都不要跟骨骼运动apply root motion
12.绝对禁止掉那些不带刚体带包围盒的物体（static collider ）运动
NUGI的代码效率很差，基本上runtime的时候对cpu的贡献和render不相上下
13每帧递归的计算finalalpha改为只有初始化和变动时计算
14去掉法线计算
15不要每帧计算viewsize 和windowsize
16filldrawcall时构建顶点缓存使用array.copy
17.代码剪裁：使用strip level ，使用.net2.0 subset
18.尽量减少smooth group
19.给美术定一个严格的经过科学验证的美术标准，并在U3D里面配以相应的检查工具

八十七：四元数有什么作用？
答：对旋转角度进行计算时用到四元数

八十八：将Camera组件的ClearFlags选项选成Depth only是什么意思？有何用处？
答：仅深度，该模式用于对象不被裁剪。

八十九：如何让已经存在的GameObject在LoadLevel后不被卸载掉？

void Awake()
{
    DontDestroyOnLoad(transform.gameObject);
}

九十：在编辑场景时将GameObject设置为Static有何作用？
答：设置游戏对象为Static将会剔除（或禁用）网格对象当这些部分被静态物体挡住而不可见时。因此，在你的场景中的所有不会动的物体都应该标记为Static。

九十一：有A和B两组物体，有什么办法能够保证A组物体永远比B组物体先渲染？
答：把A组物体的渲染对列大于B物体的渲染队列

九十二：将图片的TextureType选项分别选为Texture和Sprite有什么区别
答：Sprite作为UI精灵使用，Texture作用模型贴图使用。

九十三：问一个Terrain，分别贴3张，4张，5张地表贴图，渲染速度有什么区别？为什么？
答：没有区别，因为不管几张贴图只渲染一次。

九十四：什么是DrawCall？DrawCall高了又什么影响？如何降低DrawCall？
答：Unity中，每次引擎准备数据并通知GPU的过程称为一次Draw Call。DrawCall越高对显卡的消耗就越大。降低DrawCall的方法：
Dynamic Batching
Static Batching
高级特性Shader降级为统一的低级特性的Shader。

九十五：实时点光源的优缺点是什么？
答：可以有cookies – 带有 alpha通道的立方图(Cubemap )纹理。点光源是最耗费资源的。

九十六：Unity的Shader中，Blend SrcAlpha OneMinusSrcAlpha这句话是什么意思？
答：作用就是Alpha混合。公式：最终颜色 = 源颜色 源透明值 + 目标颜色（1 - 源透明值）

九十七：简述水面倒影的渲染原理
答: 原理就是对水面的贴图纹理进行扰动，以产生波光玲玲的效果。用shader可以通过GPU在像素级别作扰动，效果细腻，需要的顶点少，速度快

九十八：简述NGUI中Grid和Table的作用？
答：对Grid和Table下的子物体进行排序和定位

九十九：请简述NGUI中Panel和Anchor的作用
答：只要提供一个half-pixel偏移量，它可以让一个控件的位置在Windows系统上精确的显示出来（只有这个Anchor的子控件会受到影响）
如果挂载到一个对象上，那么他可以将这个对象依附到屏幕的角落或者边缘
3.UIPanel用来收集和管理它下面所有widget的组件。通过widget的geometry创建实际的draw call。没有panel所有东西都不能够被渲染出来,你可以把UIPanel当做Renderer
一百：能用foreach遍历访问的对象需要实现__接口或声明_方法的类型
答：IEnumerable；GetEnumerator