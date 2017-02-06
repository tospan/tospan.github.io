---
title: Unity引擎重要概念
categories:
  - Unity
tags:
  - Unity
date: 2017-02-06 14:19:29
updated: 2017-02-06 14:19:29
---

## GameObject是什么？

> Basically, everything that's placed in a scene is a GameObject. It has a name, a tag, a layer, a Transform component, and can be marked as static. By itself it doesn't do anything, it's just an empty container. You can turn it into something useful by attaching components to it and by putting other objects inside it.

## 子对象(Child Object)是什么？

> If you put an object inside another (by dragging in the Hierarchy view) then it's considered the child of the object that now contains it. The transformation of the parent is inherited by its children and is applied first. So it the child's transform's position is set to (10, 0, 0) while the parent's is (2, 1, 0), then the child will end up at (12, 1, 0). But if the parent's transform's rotation is set to (0, 0, 90) as well, the child effectively orbits its parent and ends up rotated (0, 0, 90) at position (2, 11, 0). Scale is inherited in the same fashion.


## MonoBehaviour是什么?

> MonoBehaviour is a class from the UnityEngine namespace. If you create a class that you want to use as a Unity component, then it should inherit from MonoBehaviour. It contains a lot of useful stuff and makes things like Update work.