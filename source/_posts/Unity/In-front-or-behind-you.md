---
title: 敌人在你前面还是后面？
date: 2017-02-05 09:42:09
updated:
categories: 
  - Game Development
tags:
  - Unity
  - Mathematics
  - Linear Algebra
---

If you've ever played Minecraft you will have noticed that the nasty creepers tend to sneak up on you when you face in another direction. But creepers are not smart, so have can they figure out if you are looking in another direction? The answer is math, so they are actually smarter than most people... and you until you have read this tutorial! Anyway, this is the basic scene. The green guy is the one trying to sneak up on you.

![Behind or in front basic scene](https://raw.githubusercontent.com/tospan/tospan.github.io/source/source/_images/unity/in-front-behind.png)

<!--more-->

To solve this problem we are going to use the Dot Product. The basic idea is that we have two vectors: one going from you to the enemy and we also know in which direction you are facing. Now we are just going to figure out if these vectors are pointing in the same direction or not. This is how the code looks like:


```cs
using UnityEngine;
using System.Collections;

//Figure out if an enemy is behind or in front of you
namespace LinearAlgebra
{
    public class BehindInFront : MonoBehaviour
    {
        public Transform youTrans;
        public Transform enemyTrans;
        
        void Update()
        {
            //Your forward vector
            Vector3 youForward = youTrans.forward;
            
            //The vector from you to the enemy
            Vector3 youToEnemy = enemyTrans.position - youTrans.position;

            //Now we need to figure out if these vectors are pointing in the opposite direction, if so
            //the enemy is behind you. To do that we will use the dot product
            //Unity's built in version:
            //float dotProduct = Vector3.Dot(youForward.normalized, youToEnemy.normalized);
            //Our own version, which is the same:
            float dotProduct = DotProduct(youForward, youToEnemy);

            //For normalized vectors the dot product returns: 
            // 1 if they point in exactly the same direction, 
            //-1 if they point in completely opposite directions 
            // 0 if the vectors are perpendicular
            //But normalization takes some computer time, so you can ignore it if you are just interested
            //in knowing if the enemy is behind you and your field-of-view is 90 degrees
            //Otherwise you have to normalize the vectors and define "in front" as something like > 0.5
            if (dotProduct >= 0f)
            {
                Debug.Log("In front");
            }
            else
            {
                Debug.Log("Behind");
            }
        }

        float DotProduct(Vector3 vec1, Vector3 vec2)
        {
            float dotProduct = vec1.x * vec2.x + vec1.y * vec2.y + vec1.z * vec2.z;

            return dotProduct;
        }
    }
}
```

## 参考

* [In front or behind you?](http://www.habrador.com/tutorials/linear-algebra/1-behind-or-in-front/)