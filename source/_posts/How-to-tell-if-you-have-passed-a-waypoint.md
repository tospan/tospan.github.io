---
title: How to tell if you have passed a waypoint
date: 2017-02-05 09:59:08
updated:
categories: 
  - Game Development
tags:
  - Unity
  - Mathematics
  - Linear Algebra
---


It might first seem like it's easy to determine if you have passed a waypoint you are heading towards. Can't you just use the distance between you and the waypoint and if that distance is small, then change waypoint. This will work if your character has a small turning radius. But if that turning radius is large, I promise you that you will see the character turning around the waypoint and never reach it - it will just spin around in a circle. But there may be a better way. This is the basic scene

![Passed waypoint basic scene](https://raw.githubusercontent.com/tospan/tospan.github.io/source/source/_images/unity/passed-waypoint.png)

<!--more-->

To solve this problem you need at least 2 waypoints: the first is where you are going from and the second is the waypoint you are want to reach. The basic idea is that we are going to find the point on the line between the waypoints that's the closest to the character with the help of Vector Projection. Then we can tell how far between the waypoints the character is, and if it's above 1, then we know the character has passed the waypoint. If so, we change to the next waypoint and repeat the calculations. This solution will not always work, but it's much better than determining the distance between the character and the waypoint. This is the code:

```cs
using UnityEngine;
using System.Collections;

namespace LinearAlgebra
{
    //Figure out if you have passed a waypoint
    public class PassedWaypoint : MonoBehaviour
    {
        public Transform youTrans;
        public Transform waypointFromTrans;
        public Transform waypointToTrans;
        public Transform debugTrans;

        void Update()
        {
            //The vector between the character and the waypoint we are going from
            Vector3 a = youTrans.position - waypointFromTrans.position;

            //The vector between the waypoints
            Vector3 b = waypointToTrans.position - waypointFromTrans.position;

            //Vector projection from https://en.wikipedia.org/wiki/Vector_projection
            //To know if we have passed the upcoming waypoint we need to find out how much of b is a1
            //a1 = (a.b / |b|^2) * b
            //a1 = progress * b -> progress = a1 / b -> progress = (a.b / |b|^2)
            float progress = (a.x * b.x + a.y * b.y + a.z * b.z) / (b.x * b.x + b.y * b.y + b.z * b.z);

            //If progress is above 1 we know we have passed the waypoint
            if (progress > 1.0f)
            {
                Debug.Log("Change waypoint");
            }
            else
            {
                Debug.Log("Move on");
            }

            //Debug by adding a cube to where the closest point on the line between the waypoints from the character is
            debugTrans.position = progress * b + waypointFromTrans.position;
        }
    }
}
```

## 参考

* [How to tell if you have passed a waypoint?](http://www.habrador.com/tutorials/linear-algebra/2-passed-waypoint/)