# 任务

## Bouncing Ball

In this assignment, you'll learn how to make an infinitely bouncing ball using Colliders, Rigidbodies, and Physic Materials.


## Brick Shooter

Create a wall of bricks and knock them down. In this Assignment you'll learn about Rigidbodies, Colliders, the Instantiate function and using Prefabs.




## Creating a Hoverr Car with Physics


In this session we'll look at using physics to create a simple hover car prototype. This session will cover some basic physics functions like using AddForce to move our vehicle and Raycast to cause it to maintain it's distance from the ground. We'll also add some dynamic audio to create sound for our engine. Tutor - Matthew Schell

### 示例代码


HoverMotor

```cs
using UnityEngine;
using System.Collections;

public class HoverMotor : MonoBehaviour {

    public float speed = 90f;
    public float turnSpeed = 5f;
    public float hoverForce = 65f;
    public float hoverHeight = 3.5f;
    private float powerInput;
    private float turnInput;
    private Rigidbody carRigidbody;


    void Awake () 
    {
        carRigidbody = GetComponent <Rigidbody>();
    }

    void Update () 
    {
        powerInput = Input.GetAxis ("Vertical");
        turnInput = Input.GetAxis ("Horizontal");
    }

    void FixedUpdate()
    {
        Ray ray = new Ray (transform.position, -transform.up);
        RaycastHit hit;

        if (Physics.Raycast(ray, out hit, hoverHeight))
        {
            float proportionalHeight = (hoverHeight - hit.distance) / hoverHeight;
            Vector3 appliedHoverForce = Vector3.up * proportionalHeight * hoverForce;
            carRigidbody.AddForce(appliedHoverForce, ForceMode.Acceleration);
        }

        carRigidbody.AddRelativeForce(0f, 0f, powerInput * speed);
        carRigidbody.AddRelativeTorque(0f, turnInput * turnSpeed, 0f);

    }
}
```

HoverAudio

```cs
using UnityEngine;
using System.Collections;

public class HoverAudio : MonoBehaviour {

    public AudioSource jetSound;
    private float jetPitch;
    private const float LowPitch = .1f;
    private const float HighPitch = 2.0f;
    private const float SpeedToRevs = .01f;
    Vector3 myVelocity;
    Rigidbody carRigidbody;
    
    void Awake () 
    {
        carRigidbody = GetComponent<Rigidbody>();
    }

    private void FixedUpdate()
    {
        myVelocity = carRigidbody.velocity;
        float forwardSpeed = transform.InverseTransformDirection(carRigidbody.velocity).z;
        float engineRevs = Mathf.Abs (forwardSpeed) * SpeedToRevs;
        jetSound.pitch = Mathf.Clamp (engineRevs, LowPitch, HighPitch);
    }

}
```



## 2D Physics: Fun with Effectors

In this live training session we're going to show you how to use 2D Physics Effectors to create areas of your scene which can add physics forces to objects or change their physics behavior. The Effectors we'll look at include Platform Effector 2D, Surface Effector 2D, Area Effector 2D and Point Effector 2D. We'll do this in the context of a platformer style game.


```cs
using UnityEngine;
using System.Collections;

public class CoinSprayer : MonoBehaviour {

    public int numCoins = 10;
    public GameObject coinPrefab;
    public float offSetRange = 1.5f;

    // Use this for initialization
    void Start () {
        SpawnCoins();
    
    }
    
    void SpawnCoins()
    {
        for (int i = 0; i < numCoins; i++)
        {
            Vector2 spawnOffset = new Vector2 (Random.Range(-offSetRange, offSetRange), Random.Range(-offSetRange, offSetRange));
            Instantiate(coinPrefab, (Vector2)transform.position + spawnOffset, Quaternion.identity);
        }
    }
}
```

```cs
using UnityEngine;
using System.Collections;

public class DropCoins : MonoBehaviour {

    public GameObject coinSprayerPf;
    bool gotHit;

    void OnCollisionEnter2D (Collision2D col)
    {
        if (col.gameObject.CompareTag("Enemy") && !gotHit)
        {
            gotHit = true;
            Instantiate(coinSprayerPf, transform.position, Quaternion.identity);
        }
    }
}
```