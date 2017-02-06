# Unity Engine架构

首先，Unity是商业软件，它并不开源，所以我们找不到关于引擎内部架构的设计文档或细节描述。

不过有一些信息是公知的。

Unity对于3D物理使用PhysX，2D物理使用Box2D，对于lightmapping，Unity 4使用Beast，Unity5 使用Enlighten，但它们的内部接口之间是如何相互工作的并不清楚。

作为开发人员，我们需要了解的是其对外公开的API就好。


[Unity3d 引擎原理详细介绍、Unity3D引擎架构设计](http://www.cnblogs.com/zhibolife/p/3620440.html)