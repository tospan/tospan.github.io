---
title: Unity使用过程中的小问题与答案 
date: 2012-12-21 09:49:59
updated:
categories:
    - Unity
tags:
    - Unity
---

## 将Editor(包括系统自带如Light等)添加到EditorWindow中

http://leahayes.wordpress.com/2012/07/16/possibility-of-reusing-custom-editors-in-custom-window/

----- 

更新：This functionality has since been added to Unity 4, please refer to the following for more information:

* [Editor.CreateEditor](https://docs.unity3d.com/ScriptReference/Editor.CreateEditor.html)

## 内置GUI实现禁用部分控件功能

估计这个也算是一个优雅的解决方法，这个思路是借鉴的这里的：

http://forum.unity3d.com/threads/24641-Enable-Disable-GUI-Components

说明：专门针对一个GUI区创建bool类型，用于指定是否禁用GUI比如说 `bool  isEnabled = true; `

<!--more-->

在非GUI方法中创建控件组：

```cs
void TestCombine(bool isEnabled)
{
     GUI.enabled = isForbid;
     GUI.BeginGroup(new Rect((Screen.width - 500) * .5f, (Screen.height - 500) * .5f, 500, 500), "ViewPoints", GUI.skin.window);

	if (GUI.Button(new Rect(10, 20, 100, 25), "Add"))
	{
		//  弹出保存存对话框
		if (isShowAdd)
		{
			isShowAdd = false;
		}
		else
		{
			isShowAdd = true;
		}
	}

	GUI.Box(new Rect(10, 50, 480, 440), "");

	foreach (VPs4 vp in viewpoints)
	{
		if (GUI.Button(ViewPointsButtonRect(vp.index), vp.name))
		{
			print("hello");
		}
	}
	GUI.EndGroup();
	GUI.enabled = true; //非常重要！否在这调用该方法之后的所有控件都会被禁用
}
```

看到这里你就能明白是怎么回事了，在OnGUI里调用该方法时通过传递isEnabled值来决定是否禁用该Group里控件

还有一些其他的思路可以参考：
http://answers.unity3d.com/questions/7531/if-i-have-a-button-over-my-3d-world-how-can-i-dete.html 

这是对于使用默认GUI元素来控制的，如果使用GUIText,GUITexture，可以参考以下的：
http://answers.unity3d.com/questions/16587/gui-click-through.html


## 如何根据类型查找项目中所有的Asset

在Editor文件夹下创建C#脚本，我命名为TryFindEditor，你自便啦！复制下面的脚本，刷新下项目就可以在Window菜单下找到Find All Type选项，完成！


```cs
using UnityEngine;
using System.Collections.Generic;
using UnityEditor;

public class TryFindEditor : EditorWindow
{
    // Add menu item named "My Window" to the Window menu
    [MenuItem("Window/Find All Type")]
    public static void ShowWindow()
    {
        //Show existing window instance. If one doesn't exist, make one.
        EditorWindow.GetWindow(typeof(TryFindEditor));
    }
    List<Material> allMaterials;
    void OnGUI()
    {
        if (GUILayout.Button("Load"))
        {
            allMaterials = new List<Material>();
            string[] paths = AssetDatabase.GetAllAssetPaths();
            foreach (string path in paths)
            {
                Material mat = (Material)AssetDatabase.LoadAssetAtPath(path, typeof(Material));
                if (mat != null)
                {
                    Debug.Log(mat.name);
                    allMaterials.Add(mat);
                }
            }
            Debug.Log(allMaterials.Count);
        }
    }
}
```