---
title: Unity编程要点
categories:
  - Unity
tags:
  - Unity
  - C#
date: 2017-02-06 14:08:41
updated: 2017-02-06 14:08:41
---


## Scripts as Behaviour Components


## 游戏构成

游戏包含多个场景，每个场景含有多个GameObject，每个GameObject有多个Component。

### Instantiate

GameObject可以直接在场景中，或者动态创建。如果直接在游戏场景中，我们没有必有管它，动态创建，我们使用 `Instantiate`来实例化一个。

而游戏管理器或场景管理等对GameObject的操作往往如下：

### 激活或禁用GameObject

### 启用或禁用Component

要执行这个操作首先需要能获取Component。

## GameObject的生命周期

### Awake and Start

### Update

`Update`方法的访问修饰符可不可以是`public` ？

> Update and a collection of other methods are considered special by Unity. It will find them and invoke them when appropriate, no matter how we declare them.
> 
> We shouldn't declare Update public, because it's not intended to be invoked by anything other than the Unity engine. The philosophy here is to make everything private that doesn't need to be accessed from outside the class. Private stuff is to not be messed with from – or directly relied upon by – the outside world. This reduces the potential for bugs.
> 
> We could omit the private statement, because methods and fields are private by default. I prefer to be explicit, so it is obviously intentional instead of a potential oversight.


### FixedUpdate

### LateUpdate

### Destroy


## GameObject的分类及行为特征

### Translate

### Rotate

### LookAt



## 游戏交互

### GetButton and GetKey

### GetAxis

### OnMouseDown

## 其他

### Vector Maths

### Linear Interpolation

### Delta Time

### Invoke