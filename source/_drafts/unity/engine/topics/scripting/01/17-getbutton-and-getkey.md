# 获取按钮或键盘按键事件

https://unity3d.com/learn/tutorials/topics/scripting/getbutton-and-getkey?playlist=17117

https://youtu.be/qf3Oo3BW83Y

How to get button or key for input and how these axes behave / can be modified with the Input manager

## 示例代码

KeyInput

```cs
using UnityEngine;
using System.Collections;

public class KeyInput : MonoBehaviour
{
    public GUITexture graphic;
    public Texture2D standard;
    public Texture2D downgfx;
    public Texture2D upgfx;
    public Texture2D heldgfx;

    void Start()
    {
        graphic.texture = standard;
    }

    void Update]() ()
    {
        bool down = Input.GetKeyDown(KeyCode.Space);
        bool held = Input.GetKey(KeyCode.Space);
        bool up = Input.GetKeyUp(KeyCode.Space);

        if(down)
        {
            graphic.texture = downgfx;
        }
        else if(held)
        {
            graphic.texture = heldgfx;
        }
        else if(up)
        {
            graphic.texture = upgfx;
        }
        else
        {
            graphic.texture = standard;
        }

        guiText.text = " " + down + "\n " + held + "\n " + up;
    }
}
```

ButtonInput

```cs
using UnityEngine;
using System.Collections;

public class ButtonInput : MonoBehaviour
{
    public GUITexture graphic;
    public Texture2D standard;
    public Texture2D downgfx;
    public Texture2D upgfx;
    public Texture2D heldgfx;

    void Start()
    {
        graphic.texture = standard;
    }

    void Update]() ()
    {
        bool down = Input.GetButtonDown("Jump");
        bool held = Input.GetButton("Jump");
        bool up = Input.GetButtonUp("Jump");

        if(down)
        {
            graphic.texture = downgfx;
        }
        else if(held)
        {
            graphic.texture = heldgfx;
        }
        else if(up)
        {
            graphic.texture = upgfx;
        }
        else
        {
            graphic.texture = standard;
        }

        guiText.text = " " + down + "\n " + held + "\n " + up;
    }
}
```

## 相关文档

* [GetKey](http://docs.unity3d.com/Documentation/ScriptReference/Input.GetKey.html?_ga=1.121392449.838993178.1480250241) (Script Reference)
* [GetKeyUp](http://docs.unity3d.com/Documentation/ScriptReference/Input.GetKeyUp.html?_ga=1.121392449.838993178.1480250241) (Script Reference)
* [GetKeyDown](http://docs.unity3d.com/Documentation/ScriptReference/Input.GetKeyDown.html?_ga=1.121392449.838993178.1480250241) (Script Reference)
* [GetButton](http://docs.unity3d.com/Documentation/ScriptReference/Input.GetButton.html?_ga=1.121392449.838993178.1480250241) (Script Reference)
* [GetButtonUp](http://docs.unity3d.com/Documentation/ScriptReference/Input.GetButtonUp.html?_ga=1.187903457.838993178.1480250241) (Script Reference)
* [GetButtonDown](http://docs.unity3d.com/Documentation/ScriptReference/Input.GetButtonDown.html?_ga=1.187903457.838993178.1480250241) (Script Reference)
* [The Input Manager](http://docs.unity3d.com/Documentation/Components/class-InputManager.html?_ga=1.187903457.838993178.1480250241) (Manual)

## 社区资源

* [Using the Xbox Controller in Unity](http://answers.unity3d.com/questions/8019/how-do-i-use-an-xbox-360-controller-in-unity.html?_ga=1.187903457.838993178.1480250241) (Answers)
