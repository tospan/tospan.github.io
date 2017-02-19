---
title: Unity GPP Spatial Partition pattern
categories: 
  - Unity
tags:
  - Unity
date: 2017-02-05 15:29:58
updated: 2017-02-05 15:29:58
---

## What's the spatial partition pattern?

When making a game, you will often have a need to find enemies that are close to the player. The common way is to store all enemies in a list, search through the list of all enemies, and for each enemy calculate the distance to the player. This code in C# will find the closest enemy, and it looks like this:

<!--more-->

```cs
//Soldier is a friendly soldier
GameObject FindClosestEnemySlow(GameObject soldier)
{
	GameObject closestEnemy = null;

	float bestDistSqr = Mathf.Infinity;

	//Loop through all enemies
	for (int i = 0; i < enemySoldiers.Count; i++)
	{
		//The distance sqr between the soldier and this enemy
		float distSqr = (soldier.transform.position - enemySoldiers[i].transform.position).sqrMagnitude;

		//If this distance is better than the previous best distance, then we have found an enemy that's closer
		if (distSqr < bestDistSqr)
		{
			bestDistSqr = distSqr;

			closestEnemy = enemySoldiers[i];
		}
	}

	return closestEnemy;
}
```

This may work if you have a few enemies, but what if you have an entire battlefield with several friendly soldiers and enemies? Then for each friendly soldier you have to search through the list of all enemies. If you have 100 friendly soldiers, then you would have to repeat the above calculation 100 times.

A better way is to store all enemies in a spatial data structure that organizes the objects by their positions. You can store both moving gameobjects, but also static geometry like obstacles in such a structure. A common way is to divide the area into a 2D grid and then you associate each cell with the enemies in it. Then to find the closest enemy, you have to figure out which cell you are standing in to get the enemies in that cell, and then you just figure out which of those enemies in the cell is the closest enemy.

## The spatial partition pattern in Unity

The idea here is that we will have spheres chasing cubes. The sphere are the friendly while the cubes are called enemies. So in Unity you should add an empty gameobject called GameController, and two empty gameobjects called Enemy soldiers and Friendly soldiers to which we will parent the cubes and spheres to get a clean workspace. Also add a ground plane. The plane's corner has to begin at (0,0,0) for this work, so the position should be (25, 0, 25) and the scale (5, 1, 5). The reason is that we have to translate coordinates of the cubes and spheres to positions in the grid, and it's easier if everything begins at (0,0,0). Everything should look like this:

![Spatial partition basic scene](https://raw.githubusercontent.com/tospan/tospan.github.io/source/source/_images/unity/spatial-partition-setup-scene.png)

You also need a sphere and a cube, and they should be prefabs. You also need different materials. The reason is that we are going to change the cubes's materials to detect which of them is the closest.

![Spatial partition more stuff](https://raw.githubusercontent.com/tospan/tospan.github.io/source/source/_images/unity/spatial-partition-setup-stuff.png)

## The spatial partition scripts

Now you need to add a new script called GameController. This script will add new cubes and spheres randomly on the map, it will handle the changing of colors, and I've also included the slow version of finding-the-closest-enemy. So add the following:

```cs
using UnityEngine;
using System.Collections;
using System.Collections.Generic;

namespace SpatialPartitionPattern
{
    public class GameController : MonoBehaviour 
    {
        public GameObject friendlyObj;
        public GameObject enemyObj;
        
        //Change materials to detect which enemy is the closest
        public Material enemyMaterial;
        public Material closestEnemyMaterial;
        
        //To get a cleaner workspace, parent all soldiers to these empty gameobjects
        public Transform enemyParent;
        public Transform friendlyParent;
        
        //Store all soldiers in these lists
        List<Soldier> enemySoldiers = new List<Soldier>();
        List<Soldier> friendlySoldiers = new List<Soldier>();
        
        //Save the closest enemies to easier change back its material
        List<Soldier> closestEnemies = new List<Soldier>();

        //Grid data
        float mapWidth = 50f;
        int cellSize = 10;

        //Number of soldiers on each team
        int numberOfSoldiers = 100;

        //The Spatial Partition grid
        Grid grid;

		
        void Start() 
        {
            //Create a new grid
            grid = new Grid((int)mapWidth, cellSize);
            
            //Add random enemies and friendly and store them in a list
            for (int i = 0; i < numberOfSoldiers; i++)
            {
                //Give the enemy a random position
                Vector3 randomPos = new Vector3(Random.Range(0f, mapWidth), 0.5f, Random.Range(0f, mapWidth));

                //Create a new enemy
                GameObject newEnemy = Instantiate(enemyObj, randomPos, Quaternion.identity) as GameObject;

                //Add the enemy to a list
                enemySoldiers.Add(new Enemy(newEnemy, mapWidth, grid));

                //Parent it
                newEnemy.transform.parent = enemyParent;


                //Give the friendly a random position
                randomPos = new Vector3(Random.Range(0f, mapWidth), 0.5f, Random.Range(0f, mapWidth));

                //Create a new friendly
                GameObject newFriendly = Instantiate(friendlyObj, randomPos, Quaternion.identity) as GameObject;

                //Add the friendly to a list
                friendlySoldiers.Add(new Friendly(newFriendly, mapWidth));

                //Parent it 
                newFriendly.transform.parent = friendlyParent;
            }
        }
	
	
        void Update() 
        {
            //Move the enemies
            for (int i = 0; i < enemySoldiers.Count; i++)
            {
                enemySoldiers[i].Move();
            }

            //Reset material of the closest enemies
            for (int i = 0; i < closestEnemies.Count; i++)
            {
                closestEnemies[i].soldierMeshRenderer.material = enemyMaterial;
            }

            //Reset the list with closest enemies
            closestEnemies.Clear();

            //For each friendly, find the closest enemy and change its color and chase it
            for (int i = 0; i < friendlySoldiers.Count; i++)
            {
                //Soldier closestEnemy = FindClosestEnemySlow(friendlySoldiers[i]);

                //The fast version with spatial partition
                Soldier closestEnemy = grid.FindClosestEnemy(friendlySoldiers[i]);

                //If we found an enemy
                if (closestEnemy != null)
                {
                    //Change material
                    closestEnemy.soldierMeshRenderer.material = closestEnemyMaterial;

                    closestEnemies.Add(closestEnemy);

                    //Move the friendly in the direction of the enemy
                    friendlySoldiers[i].Move(closestEnemy);
                }
            }
        }


        //Find the closest enemy - slow version
        Soldier FindClosestEnemySlow(Soldier soldier)
        {
            Soldier closestEnemy = null;

            float bestDistSqr = Mathf.Infinity;

            //Loop thorugh all enemies
            for (int i = 0; i < enemySoldiers.Count; i++)
            {
                //The distance sqr between the soldier and this enemy
                float distSqr = (soldier.soldierTrans.position - enemySoldiers[i].soldierTrans.position).sqrMagnitude;

                //If this distance is better than the previous best distance, then we have found an enemy that's closer
                if (distSqr < bestDistSqr)
                {
                    bestDistSqr = distSqr;

                    closestEnemy = enemySoldiers[i];
                }
            }

            return closestEnemy;
        }
    }
}
```

Next up are the brave soldiers. They inherit from a soldier base class:

```cs
using UnityEngine;
using System.Collections;

namespace SpatialPartitionPattern
{
    //The soldier base class for enemies and friendly
    public class Soldier
    {
        //To change material
        public MeshRenderer soldierMeshRenderer;
        //To move the soldier
        public Transform soldierTrans;
        //The speed the soldier is walking with
        protected float walkSpeed;
        //Has to do with the grid, so we can avoid storing all soldiers in an array
        //Instead we are going to use a linked list where all soldiers in the cell 
        //Are linked to each other
        public Soldier previousSoldier;
        public Soldier nextSoldier;

        //The enemy doesnt need any outside information
        public virtual void Move()
        { }

        //The friendly has to move which soldier is the closest
        public virtual void Move(Soldier soldier)
        { }
    }
}
```

The enemy cubes:

```cs
using UnityEngine;
using System.Collections;

namespace SpatialPartitionPattern
{
    //The enemy cube being chased by the spheres
    public class Enemy : Soldier
    {
        //The position the soldier is heading for when moving
        Vector3 currentTarget;
        //The position the soldier had before it moved, so we can see if it should change cell
        Vector3 oldPos;
        //The width of the map to generate random coordinated within the map
        float mapWidth;
        //The grid
        Grid grid;


        //Init enemy
        public Enemy(GameObject soldierObj, float mapWidth, Grid grid)
        {
            //Save what we need to save
            this.soldierTrans = soldierObj.transform;

            this.soldierMeshRenderer = soldierObj.GetComponent();

            this.mapWidth = mapWidth;

            this.grid = grid;

            //Add this unit to the grid
            grid.Add(this);

            //Init the old pos
            oldPos = soldierTrans.position;

            this.walkSpeed = 5f;

            //Give it a random coordinate to move towards
            GetNewTarget();
        }


        //Move the cube randomly across the map
        public override void Move()
        {            
            //Move towards the target
            soldierTrans.Translate(Vector3.forward * Time.deltaTime * walkSpeed);

            //See if the the cube has moved to another cell
            grid.Move(this, oldPos);

            //Save the old position
            oldPos = soldierTrans.position;

            //If the soldier has reached the target, find a new target
            if ((soldierTrans.position - currentTarget).magnitude < 1f)
            {
                GetNewTarget();
            }
        }


        //Give the enemy a new target to move towards and rotate towards that target
        void GetNewTarget()
        {
            currentTarget = new Vector3(Random.Range(0f, mapWidth), 0.5f, Random.Range(0f, mapWidth));

            //Rotate towards the target
            soldierTrans.rotation = Quaternion.LookRotation(currentTarget - soldierTrans.position);
        }
    }
}
```

The friendly spheres:

```cs
using UnityEngine;
using System.Collections;

namespace SpatialPartitionPattern
{
    //The friendly sphere which is chasing the enemy cubes
    public class Friendly : Soldier
    {
        //init friendly
        public Friendly(GameObject soldierObj, float mapWidth)
        {
            this.soldierTrans = soldierObj.transform;

            this.walkSpeed = 2f;
        }


        //Move towards the closest enemy - will always move within its grid
        public override void Move(Soldier closestEnemy)
        {
            //Rotate towards the closest enemy
            soldierTrans.rotation = Quaternion.LookRotation(closestEnemy.soldierTrans.position - soldierTrans.position);
            //Move towards the closest enemy
            soldierTrans.Translate(Vector3.forward * Time.deltaTime * walkSpeed);
        }
    }
}
```

When I'm thinking about it, it is actually the friendly spheres that are chasing the enemy cubes, but since the cubes are just moving randomly, it's actually the spheres that should be the enemies. Anyway, we have arrived at the final script that will take care of the grid. The confusing part is that we are not storing the soldiers that stand in each cell in an array, but in a linked list. So each soldier refers to another soldier in the same cell, and that other soldier refers to yet another soldier, and so on. If you are confused, read the chapter in the book. The script looks like this:

```cs
using UnityEngine;
using System.Collections;

namespace SpatialPartitionPattern
{
    public class Grid
    {
        //Need this to convert from world coordinate position to cell position
        int cellSize;

        //This is the actual grid, where a soldier is in each cell
        //Each individual soldier links to other soldiers in the same cell
        Soldier[,] cells; 


        //Init the grid
        public Grid(int mapWidth, int cellSize)
        {
            this.cellSize = cellSize;

            int numberOfCells = mapWidth / cellSize;

            cells = new Soldier[numberOfCells, numberOfCells];
        }


        //Add a unity to the grid
        public void Add(Soldier soldier)
        {
            //Determine which grid cell the soldier is in
            int cellX = (int)(soldier.soldierTrans.position.x / cellSize);
            int cellZ = (int)(soldier.soldierTrans.position.z / cellSize);

            //Add the soldier to the front of the list for the cell it's in
            soldier.previousSoldier = null;
            soldier.nextSoldier = cells[cellX, cellZ];

            //Associate this cell with this soldier
            cells[cellX, cellZ] = soldier;

            if (soldier.nextSoldier != null)
            {
                //Set this soldier to be the previous soldier of the next soldier of this soldier (linked lists ftw)
                soldier.nextSoldier.previousSoldier = soldier;
            }
        }


        //Get the closest enemy from the grid
        public Soldier FindClosestEnemy(Soldier friendlySoldier)
        {
            //Determine which grid cell the friendly soldier is in
            int cellX = (int)(friendlySoldier.soldierTrans.position.x / cellSize);
            int cellZ = (int)(friendlySoldier.soldierTrans.position.z / cellSize);

            //Get the first enemy in grid
            Soldier enemy = cells[cellX, cellZ];

            //Find the closest soldier of all in the linked list
            Soldier closestSoldier = null;

            float bestDistSqr = Mathf.Infinity;

            //Loop through the linked list
            while (enemy != null)
            {
                //The distance sqr between the soldier and this enemy
                float distSqr = (enemy.soldierTrans.position - friendlySoldier.soldierTrans.position).sqrMagnitude;

                //If this distance is better than the previous best distance, then we have found an enemy that's closer
                if (distSqr < bestDistSqr)
                {
                    bestDistSqr = distSqr;

                    closestSoldier = enemy;
                }

                //Get the next enemy in the list
                enemy = enemy.nextSoldier;
            }

            return closestSoldier;
        }


        //A soldier in the grid has moved, so see if we need to update in which grid the soldier is
        public void Move(Soldier soldier, Vector3 oldPos)
        {
            //See which cell it was in 
            int oldCellX = (int)(oldPos.x / cellSize);
            int oldCellZ = (int)(oldPos.z / cellSize);

            //See which cell it is in now
            int cellX = (int)(soldier.soldierTrans.position.x / cellSize);
            int cellZ = (int)(soldier.soldierTrans.position.z / cellSize);

            //If it didn't change cell, we are done
            if (oldCellX == cellX && oldCellZ == cellZ)
            {
                return;
            }

            //Unlink it from the list of its old cell
            if (soldier.previousSoldier != null)
            {
                soldier.previousSoldier.nextSoldier = soldier.nextSoldier;
            }

            if (soldier.nextSoldier != null)
            {
                soldier.nextSoldier.previousSoldier = soldier.previousSoldier;
            }

            //If it's the head of a list, remove it
            if (cells[oldCellX, oldCellZ] == soldier)
            {
                cells[oldCellX, oldCellZ] = soldier.nextSoldier;
            }

            //Add it bacl to the grid at its new cell
            Add(soldier);
        }
    }
}
```

If you've added all scripts and all the other stuff, and you press play, you should see the following:

![Spatial partition final scene](https://raw.githubusercontent.com/tospan/tospan.github.io/source/source/_images/unity/spatial-partition-final-scene.png)

You will notice that the spheres are not always chasing cubes if there's not a cube within their cell. That's a nice side-effect, so you could maybe use the script to make patrolling gameobjects.