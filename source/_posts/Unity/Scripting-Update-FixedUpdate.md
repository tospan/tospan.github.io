## Update方法和FixedUpdate方法

https://unity3d.com/learn/tutorials/topics/scripting/update-and-fixedupdate?playlist=17117

https://youtu.be/ZukcUv3pyXQ

### Update

Update 方法也许是Unity中最常使用的方法：

* 只要脚本中有该方法，则每一帧都会调用
* 主要用于一些常规的Update：
    - 移动非物理属性的对象
    - 简单计时器
    - 监听用户输入
* Update调用的间隔时间不规律，有可能一帧比另一帧处理所消耗的时间更长（短）

### FixedUpdate

和Update方法比较相似，但稍微有些区别：

* 在每个物理步长(Physics Step)调用
* FixedUpdate调用间隔时间是固定
* 主要用于调整物理属性（Rigidbody）的游戏对象

Immediately after FixedUpdate is called, any necessary physics calculations are made.

### 示例代码

```cs
using UnityEngine;
using System.Collections;

public class UpdateAndFixedUpdate : MonoBehaviour
{
    void FixedUpdate ()
    {
        Debug.Log("FixedUpdate time :" + Time.deltaTime);
    }


    void Update ()
    {
        Debug.Log("Update time :" + Time.deltaTime);
    }
}
```