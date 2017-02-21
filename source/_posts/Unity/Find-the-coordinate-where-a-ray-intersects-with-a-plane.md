---
title: Find the coordinate where a ray intersects with a plane
date: 2017-02-05 10:08:33
updated:
categories:
  - Game Development
tags:
  - Unity
  - Math
  - Linear Algebra
---


Let's say you want to make some randomly generated clouds. To do that you fire a ray from the camera to the cloud layer where you figure out the density of the clouds at that point. It will look like this:

![Volume rendered clouds](https://raw.githubusercontent.com/tospan/tospan.github.io/source/source/_images/unity/clouds.png)

<!--more-->

But how do you know where the line intersects with the plane? The answer is that you once again need to use the Dot Product. In the scene, the planeTrans is just the simple ground plane. This plane is not endless, but the plane in the code below is. The lineRenderer is just an empty game object to which I attached a line renderer.


```cs
using UnityEngine;
using System.Collections;

namespace LinearAlgebra
{
    //Figure out at which coordinate a ray intersects with a plane
    public class PlaneRayIntersection : MonoBehaviour
    {
        public LineRenderer lineRenderer;
        public Transform youTrans;
        public Transform planeTrans;

        void Update()
        {
            //Math from http://www.scratchapixel.com/lessons/3d-basic-rendering/minimal-ray-tracer-rendering-simple-shapes/ray-plane-and-ray-disk-intersection

            //A plane can be defined as:
            //a point representing how far the plane is from the world origin
            Vector3 p_0 = planeTrans.position;
            //a normal (defining the orientation of the plane), should be negative if we are firing the ray from above
            Vector3 n = -planeTrans.up;
            //We are intrerested in calculating a point in this plane called p
            //The vector between p and p0 and the normal is always perpendicular: (p - p_0) . n = 0

            //A ray to point p can be defined as: l_0 + l * t = p, where:
            //the origin of the ray
            Vector3 l_0 = youTrans.position;
            //l is the direction of the ray
            Vector3 l = youTrans.forward;
            //t is the length of the ray, which we can get by combining the above equations:
            //t = ((p_0 - l_0) . n) / (l . n)

            //But there's a chance that the line doesn't intersect with the plane, and we can check this by first
            //calculating the denominator and see if it's not small. 
            //We are also checking that the denominator is positive or we are looking in the opposite direction
            float denominator = Vector3.Dot(l, n);

            if (denominator > 0.00001f)
            {
                //The distance to the plane
                float t = Vector3.Dot(p_0 - l_0, n) / denominator;

                //Where the ray intersects with a plane
                Vector3 p = l_0 + l * t;

                //Display the ray with a line renderer
                lineRenderer.SetPosition(0, p);
                lineRenderer.SetPosition(1, l_0);
            }
            else
            {
                Debug.Log("No intersection");
            }
        }
    }
}
```

If everything is working it should look like this:

![Plane ray intersection final scene](https://raw.githubusercontent.com/tospan/tospan.github.io/source/source/_images/unity/plane-ray-intersection.png)

## 参考

* [Find the coordinate where a ray intersects with a plane](http://www.habrador.com/tutorials/linear-algebra/4-plane-ray-intersection/)