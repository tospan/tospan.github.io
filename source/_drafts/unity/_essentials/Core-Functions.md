# 核心函数

## Update

Update函数会在每一帧渲染之前调用，我们的大部分代码都会在这里运行

## FixedUpdate

FixedUpdate函数会在进行各种物理计算之前调用，我们会在这里调用和物理相关的代码


## LateUpdate

LateUpdate和Update函数一样也会每一帧都调用，但它会确保所有元素都在Update方法里处理过了再运行，例如让相机跟随玩家的时候，在LateUpdate方法里设置相机的位置，我们可以确保在当前帧玩家已经位置已经是最新状态了。
对于跟踪相机，过程动画，获取最新的状态信息，最好是放到LateUpdate方法里
However for follow cameras, procedural animation and gathering last known states it's best to use LateUpdate.

LateUpdate runs every frame, just like Update, but it is guaranteed to run after all items have been processed in Update. So when we set the position of the camera we know absolutely that the Player has already moved for that frame.