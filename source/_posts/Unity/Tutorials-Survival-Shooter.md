---
title: Unity教程:Survival Shooter游戏
categories: 
  - Unity Tutorials
tags:
  - Unity
  - Patterns
date: 2017-02-03 15:29:58
updated: 2017-02-05 15:29:58
---

## Environment setup

This is part 1 of 10 of the Survival shooter tutorial, in which you will setup a pre-designed environment with lighting and background music.

<!--more-->

## Player Character

This is part 2 of 10 of the Survival Shooter tutorial, in which you will setup and code the player character, as well as create an animator state machine.

```cs
using UnityEngine;

public class PlayerMovement : MonoBehaviour
{
    public float speed = 6f;            // The speed that the player will move at.

    Vector3 movement;                   // The vector to store the direction of the player's movement.
    Animator anim;                      // Reference to the animator component.
    Rigidbody playerRigidbody;          // Reference to the player's rigidbody.
    int floorMask;                      // A layer mask so that a ray can be cast just at gameobjects on the floor layer.
    float camRayLength = 100f;          // The length of the ray from the camera into the scene.

    void Awake ()
    {
        // Create a layer mask for the floor layer.
        floorMask = LayerMask.GetMask ("Floor");

        // Set up references.
        anim = GetComponent <Animator> ();
        playerRigidbody = GetComponent <Rigidbody> ();
    }


    void FixedUpdate ()
    {
        // Store the input axes.
        float h = Input.GetAxisRaw ("Horizontal");
        float v = Input.GetAxisRaw ("Vertical");

        // Move the player around the scene.
        Move (h, v);

        // Turn the player to face the mouse cursor.
        Turning ();

        // Animate the player.
        Animating (h, v);
    }

    void Move (float h, float v)
    {
        // Set the movement vector based on the axis input.
        movement.Set (h, 0f, v);
        
        // Normalise the movement vector and make it proportional to the speed per second.
        movement = movement.normalized * speed * Time.deltaTime;

        // Move the player to it's current position plus the movement.
        playerRigidbody.MovePosition (transform.position + movement);
    }

    void Turning ()
    {
        // Create a ray from the mouse cursor on screen in the direction of the camera.
        Ray camRay = Camera.main.ScreenPointToRay (Input.mousePosition);

        // Create a RaycastHit variable to store information about what was hit by the ray.
        RaycastHit floorHit;

        // Perform the raycast and if it hits something on the floor layer...
        if(Physics.Raycast (camRay, out floorHit, camRayLength, floorMask))
        {
            // Create a vector from the player to the point on the floor the raycast from the mouse hit.
            Vector3 playerToMouse = floorHit.point - transform.position;

            // Ensure the vector is entirely along the floor plane.
            playerToMouse.y = 0f;

            // Create a quaternion (rotation) based on looking down the vector from the player to the mouse.
            Quaternion newRotation = Quaternion.LookRotation (playerToMouse);

            // Set the player's rotation to this new rotation.
            playerRigidbody.MoveRotation (newRotation);
        }
    }

    void Animating (float h, float v)
    {
        // Create a boolean that is true if either of the input axes is non-zero.
        bool walking = h != 0f || v != 0f;

        // Tell the animator whether or not the player is walking.
        anim.SetBool ("IsWalking", walking);
    }
}
```


## Camera  setup

This is part 3 of 10 of the Survival Shooter tutorial, in which you will design the camera view and behaviour.

```cs
using UnityEngine;
using System.Collections;

public class CameraFollow : MonoBehaviour
{
    public Transform target;            // The position that that camera will be following.
    public float smoothing = 5f;        // The speed with which the camera will be following.

    Vector3 offset;                     // The initial offset from the target.

    void Start ()
    {
        // Calculate the initial offset.
        offset = transform.position - target.position;
    }

    void FixedUpdate ()
    {
        // Create a postion the camera is aiming for based on the offset from the target.
        Vector3 targetCamPos = target.position + offset;

        // Smoothly interpolate between the camera's current position and it's target position.
        transform.position = Vector3.Lerp (transform.position, targetCamPos, smoothing * Time.deltaTime);
    }
}
```

## Creating Enemy #1

This is part 4 of 10 of the Survival Shooter tutorial, in which you will create the enemy behaviour, including animation, state machines, nav-mesh and code.

```cs
using UnityEngine;
using System.Collections;

public class EnemyMovement : MonoBehaviour
{
    Transform player;               // Reference to the player's position.
    PlayerHealth playerHealth;      // Reference to the player's health.
    EnemyHealth enemyHealth;        // Reference to this enemy's health.
    NavMeshAgent nav;               // Reference to the nav mesh agent.


    void Awake ()
    {
        // Set up the references.
        player = GameObject.FindGameObjectWithTag ("Player").transform;
        playerHealth = player.GetComponent <PlayerHealth> ();
        enemyHealth = GetComponent <EnemyHealth> ();
        nav = GetComponent <NavMeshAgent> ();
    }


    void Update ()
    {
        // If the enemy and the player have health left...
        if(enemyHealth.currentHealth > 0 && playerHealth.currentHealth > 0)
        {
            // ... set the destination of the nav mesh agent to the player.
            nav.SetDestination (player.position);
        }
        // Otherwise...
        else
        {
            // ... disable the nav mesh agent.
            nav.enabled = false;
        }
    } 
}
```

## Health HUD

This is part 5 of 10 of the Survival Shooter tutorial, in which you will create a Heads Up Display for Health using the new UI tools in Unity 4.6.

## Player Health

This is part 6 of 10 of the Survival Shooter tutorial, in which you will program the player's health, which is used in reference by other elements of the game.

```cs
using UnityEngine;
using UnityEngine.UI;
using System.Collections;

public class PlayerHealth : MonoBehaviour
{
    public int startingHealth = 100;                            // The amount of health the player starts the game with.
    public int currentHealth;                                   // The current health the player has.
    public Slider healthSlider;                                 // Reference to the UI's health bar.
    public Image damageImage;                                   // Reference to an image to flash on the screen on being hurt.
    public AudioClip deathClip;                                 // The audio clip to play when the player dies.
    public float flashSpeed = 5f;                               // The speed the damageImage will fade at.
    public Color flashColour = new Color(1f, 0f, 0f, 0.1f);     // The colour the damageImage is set to, to flash.


    Animator anim;                                              // Reference to the Animator component.
    AudioSource playerAudio;                                    // Reference to the AudioSource component.
    PlayerMovement playerMovement;                              // Reference to the player's movement.
    PlayerShooting playerShooting;                              // Reference to the PlayerShooting script.
    bool isDead;                                                // Whether the player is dead.
    bool damaged;                                               // True when the player gets damaged.


    void Awake ()
    {
        // Setting up the references.
        anim = GetComponent <Animator> ();
        playerAudio = GetComponent <AudioSource> ();
        playerMovement = GetComponent <PlayerMovement> ();
        playerShooting = GetComponentInChildren <PlayerShooting> ();

        // Set the initial health of the player.
        currentHealth = startingHealth;
    }


    void Update ()
    {
        // If the player has just been damaged...
        if(damaged)
        {
            // ... set the colour of the damageImage to the flash colour.
            damageImage.color = flashColour;
        }
        // Otherwise...
        else
        {
            // ... transition the colour back to clear.
            damageImage.color = Color.Lerp (damageImage.color, Color.clear, flashSpeed * Time.deltaTime);
        }

        // Reset the damaged flag.
        damaged = false;
    }


    public void TakeDamage (int amount)
    {
        // Set the damaged flag so the screen will flash.
        damaged = true;

        // Reduce the current health by the damage amount.
        currentHealth -= amount;

        // Set the health bar's value to the current health.
        healthSlider.value = currentHealth;

        // Play the hurt sound effect.
        playerAudio.Play ();

        // If the player has lost all it's health and the death flag hasn't been set yet...
        if(currentHealth <= 0 && !isDead)
        {
            // ... it should die.
            Death ();
        }
    }


    void Death ()
    {
        // Set the death flag so this function won't be called again.
        isDead = true;

        // Turn off any remaining shooting effects.
        playerShooting.DisableEffects ();

        // Tell the animator that the player is dead.
        anim.SetTrigger ("Die");

        // Set the audiosource to play the death clip and play it (this will stop the hurt sound from playing).
        playerAudio.clip = deathClip;
        playerAudio.Play ();

        // Turn off the movement and shooting scripts.
        playerMovement.enabled = false;
        playerShooting.enabled = false;
    }       
}
```

```cs
using UnityEngine;
using System.Collections;


public class EnemyAttack : MonoBehaviour
{
    public float timeBetweenAttacks = 0.5f;     // The time in seconds between each attack.
    public int attackDamage = 10;               // The amount of health taken away per attack.


    Animator anim;                              // Reference to the animator component.
    GameObject player;                          // Reference to the player GameObject.
    PlayerHealth playerHealth;                  // Reference to the player's health.
    EnemyHealth enemyHealth;                    // Reference to this enemy's health.
    bool playerInRange;                         // Whether player is within the trigger collider and can be attacked.
    float timer;                                // Timer for counting up to the next attack.


    void Awake ()
    {
        // Setting up the references.
        player = GameObject.FindGameObjectWithTag ("Player");
        playerHealth = player.GetComponent <PlayerHealth> ();
        enemyHealth = GetComponent<EnemyHealth>();
        anim = GetComponent <Animator> ();
    }


    void OnTriggerEnter (Collider other)
    {
        // If the entering collider is the player...
        if(other.gameObject == player)
        {
            // ... the player is in range.
            playerInRange = true;
        }
    }


    void OnTriggerExit (Collider other)
    {
        // If the exiting collider is the player...
        if(other.gameObject == player)
        {
            // ... the player is no longer in range.
            playerInRange = false;
        }
    }


    void Update ()
    {
        // Add the time since Update was last called to the timer.
        timer += Time.deltaTime;

        // If the timer exceeds the time between attacks, the player is in range and this enemy is alive...
        if(timer >= timeBetweenAttacks && playerInRange && enemyHealth.currentHealth > 0)
        {
            // ... attack.
            Attack ();
        }

        // If the player has zero or less health...
        if(playerHealth.currentHealth <= 0)
        {
            // ... tell the animator the player is dead.
            anim.SetTrigger ("PlayerDead");
        }
    }


    void Attack ()
    {
        // Reset the timer.
        timer = 0f;

        // If the player has health to lose...
        if(playerHealth.currentHealth > 0)
        {
            // ... damage the player.
            playerHealth.TakeDamage (attackDamage);
        }
    }
}

```

## Harming Enemies

This is part 7 of 10 of the Survival Shooter tutorial, in which you will give create the player shooting mechanic using Raycasting, Line Renderers and more.

```cs
using UnityEngine;

public class EnemyHealth : MonoBehaviour
{
    public int startingHealth = 100;            // The amount of health the enemy starts the game with.
    public int currentHealth;                   // The current health the enemy has.
    public float sinkSpeed = 2.5f;              // The speed at which the enemy sinks through the floor when dead.
    public int scoreValue = 10;                 // The amount added to the player's score when the enemy dies.
    public AudioClip deathClip;                 // The sound to play when the enemy dies.


    Animator anim;                              // Reference to the animator.
    AudioSource enemyAudio;                     // Reference to the audio source.
    ParticleSystem hitParticles;                // Reference to the particle system that plays when the enemy is damaged.
    CapsuleCollider capsuleCollider;            // Reference to the capsule collider.
    bool isDead;                                // Whether the enemy is dead.
    bool isSinking;                             // Whether the enemy has started sinking through the floor.


    void Awake ()
    {
        // Setting up the references.
        anim = GetComponent <Animator> ();
        enemyAudio = GetComponent <AudioSource> ();
        hitParticles = GetComponentInChildren <ParticleSystem> ();
        capsuleCollider = GetComponent <CapsuleCollider> ();

        // Setting the current health when the enemy first spawns.
        currentHealth = startingHealth;
    }

    void Update ()
    {
        // If the enemy should be sinking...
        if(isSinking)
        {
            // ... move the enemy down by the sinkSpeed per second.
            transform.Translate (-Vector3.up * sinkSpeed * Time.deltaTime);
        }
    }


    public void TakeDamage (int amount, Vector3 hitPoint)
    {
        // If the enemy is dead...
        if(isDead)
            // ... no need to take damage so exit the function.
            return;

        // Play the hurt sound effect.
        enemyAudio.Play ();

        // Reduce the current health by the amount of damage sustained.
        currentHealth -= amount;
            
        // Set the position of the particle system to where the hit was sustained.
        hitParticles.transform.position = hitPoint;

        // And play the particles.
        hitParticles.Play();

        // If the current health is less than or equal to zero...
        if(currentHealth <= 0)
        {
            // ... the enemy is dead.
            Death ();
        }
    }


    void Death ()
    {
        // The enemy is dead.
        isDead = true;

        // Turn the collider into a trigger so shots can pass through it.
        capsuleCollider.isTrigger = true;

        // Tell the animator that the enemy is dead.
        anim.SetTrigger ("Dead");

        // Change the audio clip of the audio source to the death clip and play it (this will stop the hurt clip playing).
        enemyAudio.clip = deathClip;
        enemyAudio.Play ();
    }


    public void StartSinking ()
    {
        // Find and disable the Nav Mesh Agent.
        GetComponent <NavMeshAgent> ().enabled = false;

        // Find the rigidbody component and make it kinematic (since we use Translate to sink the enemy).
        GetComponent <Rigidbody> ().isKinematic = true;

        // The enemy should no sink.
        isSinking = true;

        // Increase the score by the enemy's score value.
        ScoreManager.score += scoreValue;

        // After 2 seconds destory the enemy.
        Destroy (gameObject, 2f);
    }
}
```

```cs
using UnityEngine;

public class PlayerShooting : MonoBehaviour
{
    public int damagePerShot = 20;                  // The damage inflicted by each bullet.
    public float timeBetweenBullets = 0.15f;        // The time between each shot.
    public float range = 100f;                      // The distance the gun can fire.

    float timer;                                    // A timer to determine when to fire.
    Ray shootRay;                                   // A ray from the gun end forwards.
    RaycastHit shootHit;                            // A raycast hit to get information about what was hit.
    int shootableMask;                              // A layer mask so the raycast only hits things on the shootable layer.
    ParticleSystem gunParticles;                    // Reference to the particle system.
    LineRenderer gunLine;                           // Reference to the line renderer.
    AudioSource gunAudio;                           // Reference to the audio source.
    Light gunLight;                                 // Reference to the light component.
    float effectsDisplayTime = 0.2f;                // The proportion of the timeBetweenBullets that the effects will display for.

    void Awake ()
    {
        // Create a layer mask for the Shootable layer.
        shootableMask = LayerMask.GetMask ("Shootable");

        // Set up the references.
        gunParticles = GetComponent<ParticleSystem> ();
        gunLine = GetComponent <LineRenderer> ();
        gunAudio = GetComponent<AudioSource> ();
        gunLight = GetComponent<Light> ();
    }

    void Update ()
    {
        // Add the time since Update was last called to the timer.
        timer += Time.deltaTime;

        // If the Fire1 button is being press and it's time to fire...
        if(Input.GetButton ("Fire1") && timer >= timeBetweenBullets)
        {
            // ... shoot the gun.
            Shoot ();
        }

        // If the timer has exceeded the proportion of timeBetweenBullets that the effects should be displayed for...
        if(timer >= timeBetweenBullets * effectsDisplayTime)
        {
            // ... disable the effects.
            DisableEffects ();
        }
    }

    public void DisableEffects ()
    {
        // Disable the line renderer and the light.
        gunLine.enabled = false;
        gunLight.enabled = false;
    }

    void Shoot ()
    {
        // Reset the timer.
        timer = 0f;

        // Play the gun shot audioclip.
        gunAudio.Play ();

        // Enable the light.
        gunLight.enabled = true;

        // Stop the particles from playing if they were, then start the particles.
        gunParticles.Stop ();
        gunParticles.Play ();

        // Enable the line renderer and set it's first position to be the end of the gun.
        gunLine.enabled = true;
        gunLine.SetPosition (0, transform.position);

        // Set the shootRay so that it starts at the end of the gun and points forward from the barrel.
        shootRay.origin = transform.position;
        shootRay.direction = transform.forward;

        // Perform the raycast against gameobjects on the shootable layer and if it hits something...
        if(Physics.Raycast (shootRay, out shootHit, range, shootableMask))
        {
            // Try and find an EnemyHealth script on the gameobject hit.
            EnemyHealth enemyHealth = shootHit.collider.GetComponent <EnemyHealth> ();

            // If the EnemyHealth component exist...
            if(enemyHealth != null)
            {
                // ... the enemy should take damage.
                enemyHealth.TakeDamage (damagePerShot, shootHit.point);
            }

            // Set the second position of the line renderer to the point the raycast hit.
            gunLine.SetPosition (1, shootHit.point);
        }
        // If the raycast didn't hit anything on the shootable layer...
        else
        {
            // ... set the second position of the line renderer to the fullest extent of the gun's range.
            gunLine.SetPosition (1, shootRay.origin + shootRay.direction * range);
        }
    }
}
```

## Scoring points

This is part 8 of 10 of the Survival Shooter tutorial, in which you will program the player's score, updating it with each kill.

```cs
using UnityEngine;
using UnityEngine.UI;
using System.Collections;

public class ScoreManager : MonoBehaviour
{
    public static int score;        // The player's score.


    Text text;                      // Reference to the Text component.


    void Awake ()
    {
        // Set up the reference.
        text = GetComponent <Text> ();

        // Reset the score.
        score = 0;
    }


    void Update ()
    {
        // Set the displayed text to be the word "Score" followed by the score value.
        text.text = "Score: " + score;
    }
}
```

```cs
using UnityEngine;

public class EnemyHealth : MonoBehaviour
{
    public int startingHealth = 100;          // The amount of health the enemy starts the game with.
    public int currentHealth;                    // The current health the enemy has.
    public float sinkSpeed = 2.5f;           // The speed at which the enemy sinks through the floor when dead.
    public int scoreValue = 10;               // The amount added to the player's score when the enemy dies.
    public AudioClip deathClip;              // The sound to play when the enemy dies.

    Animator anim;                              // Reference to the animator.
    AudioSource enemyAudio;              // Reference to the audio source.
    ParticleSystem hitParticles;             // Reference to the particle system that plays when the enemy is damaged.
    CapsuleCollider capsuleCollider;     // Reference to the capsule collider.
    bool isDead;                                  // Whether the enemy is dead.
    bool isSinking;                               // Whether the enemy has started sinking through the floor.

    void Awake ()
    {
        // Setting up the references.
        anim = GetComponent <Animator> ();
        enemyAudio = GetComponent <AudioSource> ();
        hitParticles = GetComponentInChildren <ParticleSystem> ();
        capsuleCollider = GetComponent <CapsuleCollider> ();

        // Setting the current health when the enemy first spawns.
        currentHealth = startingHealth;
    }

    void Update ()
    {
        // If the enemy should be sinking...
        if(isSinking)
        {
            // ... move the enemy down by the sinkSpeed per second.
            transform.Translate (-Vector3.up * sinkSpeed * Time.deltaTime);
        }
    }

    public void TakeDamage (int amount, Vector3 hitPoint)
    {
        // If the enemy is dead...
        if(isDead)
            // ... no need to take damage so exit the function.
            return;

        // Play the hurt sound effect.
        enemyAudio.Play ();

        // Reduce the current health by the amount of damage sustained.
        currentHealth -= amount;
        
        // Set the position of the particle system to where the hit was sustained.
        hitParticles.transform.position = hitPoint;

        // And play the particles.
        hitParticles.Play();

        // If the current health is less than or equal to zero...
        if(currentHealth <= 0)
        {
            // ... the enemy is dead.
            Death ();
        }
    }

    void Death ()
    {
        // The enemy is dead.
        isDead = true;

        // Turn the collider into a trigger so shots can pass through it.
        capsuleCollider.isTrigger = true;

        // Tell the animator that the enemy is dead.
        anim.SetTrigger ("Dead");

        // Change the audio clip of the audio source to the death clip and play it (this will stop the hurt clip playing).
        enemyAudio.clip = deathClip;
        enemyAudio.Play ();
    }

    public void StartSinking ()
    {
        // Find and disable the Nav Mesh Agent.
        GetComponent <NavMeshAgent> ().enabled = false;

        // Find the rigidbody component and make it kinematic (since we use Translate to sink the enemy).
        GetComponent <Rigidbody> ().isKinematic = true;

        // The enemy should no sink.
        isSinking = true;

        // Increase the score by the enemy's score value.
        ScoreManager.score += scoreValue;

        // After 2 seconds destory the enemy.
        Destroy (gameObject, 2f);
    }
}
```

## Spawning Enemies

This is part 9 of 10, in which you will add two other enemy types to the game, as well as a spawner manager to create them during the game.

```cs
using UnityEngine;

public class EnemyManager : MonoBehaviour
{
    public PlayerHealth playerHealth;       // Reference to the player's heatlh.
    public GameObject enemy;                // The enemy prefab to be spawned.
    public float spawnTime = 3f;            // How long between each spawn.
    public Transform[] spawnPoints;         // An array of the spawn points this enemy can spawn from.


    void Start ()
    {
        // Call the Spawn function after a delay of the spawnTime and then continue to call after the same amount of time.
        InvokeRepeating ("Spawn", spawnTime, spawnTime);
    }


    void Spawn ()
    {
        // If the player has no health left...
        if(playerHealth.currentHealth <= 0f)
        {
            // ... exit the function.
            return;
        }

        // Find a random index between zero and one less than the number of spawn points.
        int spawnPointIndex = Random.Range (0, spawnPoints.Length);

        // Create an instance of the enemy prefab at the randomly selected spawn point's position and rotation.
        Instantiate (enemy, spawnPoints[spawnPointIndex].position, spawnPoints[spawnPointIndex].rotation);
    }
}
```

## Game Over

This is part 10 of 10 of the Survival Shooter tutorial, in which you will create the Game Over screen with UI, animation and code.

```cs
using UnityEngine;

public class GameOverManager : MonoBehaviour
{
    public PlayerHealth playerHealth;       // Reference to the player's health.
    public float restartDelay = 5f;         // Time to wait before restarting the level


    Animator anim;                          // Reference to the animator component.
    float restartTimer;                     // Timer to count up to restarting the level


    void Awake ()
    {
        // Set up the reference.
        anim = GetComponent <Animator> ();
    }


    void Update ()
    {
        // If the player has run out of health...
        if(playerHealth.currentHealth <= 0)
        {
            // ... tell the animator the game is over.
            anim.SetTrigger ("GameOver");

            // .. increment a timer to count up to restarting.
            restartTimer += Time.deltaTime;

            // .. if it reaches the restart delay...
            if(restartTimer >= restartDelay)
            {
                // .. then reload the currently loaded level.
                Application.LoadLevel(Application.loadedLevel);
            }
        }
    }
}
```

## 相关教程

* [Prefabs - Concept & Usage](https://unity3d.com/learn/tutorials/topics/interface-essentials/prefabs-concept-usage) (Lesson)
* [Lights](https://unity3d.com/learn/tutorials/topics/graphics/lights) (Lesson)
* [Using Lights](https://unity3d.com/learn/tutorials/topics/graphics/using-lights) (Lesson)
* [Animator Scripting](https://unity3d.com/learn/tutorials/topics/animation/animator-scripting) (Lesson)
* [The Animator Component](https://unity3d.com/learn/tutorials/topics/animation/animator-component) (Lesson)
* [The Animator Controller](https://unity3d.com/learn/tutorials/topics/animation/animator-controller) (Lesson)
* [Variables and Functions](https://unity3d.com/learn/tutorials/topics/scripting/variables-and-functions) (Lesson)
* [Rigidbodies](https://unity3d.com/learn/tutorials/topics/physics/rigidbodies) (Lesson)
* [Colliders](https://unity3d.com/learn/tutorials/topics/physics/colliders) (Lesson)
* [Cameras](https://unity3d.com/learn/tutorials/topics/graphics/cameras) (Lesson)
* [Using Cameras](https://unity3d.com/learn/tutorials/topics/graphics/using-cameras) (Lesson)
* [Nav Meshes](https://unity3d.com/learn/tutorials/topics/navigation/nav-meshes) (Lesson)
* [Navigation Overview](https://unity3d.com/learn/tutorials/topics/navigation/navigation-overview) (Lesson)
* [NavMesh Baking](https://unity3d.com/learn/tutorials/topics/navigation/navmesh-baking) (Lesson)
* [NavMesh Obstacles](https://unity3d.com/learn/tutorials/topics/navigation/navmesh-obstacles) (Lesson)
* [The NavMesh Agent](https://unity3d.com/learn/tutorials/topics/navigation/navmesh-agent) (Lesson)
* [UI Canvas](https://unity3d.com/learn/tutorials/topics/user-interface-ui/ui-canvas) (Lesson)
* [The new UI](https://unity3d.com/learn/tutorials/modules/beginner/live-training-archive/the-new-ui) (Lesson)
* [UI Slider](https://unity3d.com/learn/tutorials/topics/user-interface-ui/ui-slider) (Lesson)
* [UI Image](https://unity3d.com/learn/tutorials/topics/user-interface-ui/ui-image) (Lesson)
* [UI RectTransform](https://unity3d.com/learn/tutorials/modules/beginner/ui/rect-transform) (Lesson)
* [Raycasting](https://unity3d.com/learn/tutorials/topics/physics/raycasting) (Lesson)
* [Statics](https://unity3d.com/learn/tutorials/topics/scripting/statics) (Lesson)
* [The Animation View](https://unity3d.com/learn/tutorials/topics/animation/animation-view) (Lesson)
* [UI Text](https://unity3d.com/learn/tutorials/topics/user-interface-ui/ui-text) (Lesson)