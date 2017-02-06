# 脚本即行为组件


https://unity3d.com/learn/tutorials/modules/beginner/scripting/scripts-as-behaviour-components?playlist=17117

https://youtu.be/YvA1O7MYs_w

Scripts should be considered as behaviour components in Unity. As with other components in Unity they can be applied to objects and are seen in the inspector. With this particular example, this cube has a rigidbody component which gives it a physical mass.

And when you press play the cube falls to the ground as it uses gravity. We also have added an examples script. This behaviour script has code in it which changes the colour of the cube by effecting the colour value of the default material attached to that object. When we press the R key on the keyboard the colour gets changed to red. When we press G the colour gets changed to green. And when we press B it gets changed to blue. By attaching this script to the object when we refer to game object we're referring to this particular item. We then drill down to the value that we want and effect it. Here we're addressing the game object this script is attached to, we're then addressing the renderer, which is the component seen here, mesh renderer. We're then addressing the material attached to that renderer and finally the colour of that material. And we're setting it to a shortcut called red which is part of the colour class. Let's see this in action. If I press play, then use the R, G or B keys on the keyboard I can change the colour. And you can see that the material is being effected. So this material is applied to the renderer, default diffuse, you can see that listed there, and we're then effecting the main colour value and setting it to a certain value in here. The same as it would if I was actually doing it by hand in the editor.

Scripts can be created in the project panel by choosing Create and then choosing a language of your choice. For example, they can then be attached to objects either by dragging and dropping or by choosing the Add Component button at the bottom of the Component menu and then choosing from the list of scripts in your current project. Scripts can also be created using the Add Component button by choosing New Script from the bottom and naming the script and selecting a language type from the drop-down menu. This can then be created and added in one step. The final way to add a script to your object is to select the object in the hierarchy and then choose Components - Scripts and then choose from the list of scripts in your current project. Of course you can apply scripts to do all manner of other behaviours of objects. Try to think of scripts as components that you create yourself, allowing you to create behaviour for different game objects in your game, this could be characters, it could be environments or it could be scripts that manage the functionality of the game. This example script that we've looked at is written in C# but in Unity you can write in Javascript, C# and Boo. Boo is a derivative of Python, though it's not as commonly used as the other two. So you'll likely see Javascript or C# examples when you see scripting from Unity around the web. The videos that you see in this learning area will be written in C# but we'll also provide the Javascript equivalent beneath the video.

```cs
using UnityEngine;
using System.Collections;

public class ExampleBehaviourScript : MonoBehaviour
{
    void Update()
    {
        if (Input.GetKeyDown(KeyCode.R))
        {
            GetComponent<Renderer>().material.color = Color.red;
        }
        if (Input.GetKeyDown(KeyCode.G))
        {
            GetComponent<Renderer>().material.color = Color.green;
        }
        if (Input.GetKeyDown(KeyCode.B))
        {
            GetComponent<Renderer>().material.color = Color.blue;
        }
    }
}
```

## 相关文档

* [Reference Homepage](http://docs.unity3d.com/Documentation/ScriptReference/index.html?_ga=1.138685178.838993178.1480250241) (Script Reference)
* [Scripting in Unity](http://docs.unity3d.com/Manual/ScriptingSection.html?_ga=1.176885100.838993178.1480250241) (Manual)