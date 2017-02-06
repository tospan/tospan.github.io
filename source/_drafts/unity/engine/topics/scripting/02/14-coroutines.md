# Coroutines

How to create coroutines and use them to achieve complex behaviors.

https://unity3d.com/learn/tutorials/topics/scripting/coroutines?playlist=17117

https://youtu.be/bM3CXzj5xM4

4:15

```cs
using UnityEngine;
using System.Collections;

public class CoroutinesExample : MonoBehaviour
{
    public float smoothing = 1f;
    public Transform target;


    void Start ()
    {
        StartCoroutine(MyCoroutine(target));
    }


    IEnumerator MyCoroutine (Transform target)
    {
        while(Vector3.Distance(transform.position, target.position) > 0.05f)
        {
            transform.position = Vector3.Lerp(transform.position, target.position, smoothing * Time.deltaTime);

            yield return null;
        }

        print("Reached the target.");

        yield return new WaitForSeconds(3f);

        print("MyCoroutine is now finished.");
    }
}
```

```cs
using UnityEngine;
using System.Collections;

public class PropertiesAndCoroutines : MonoBehaviour
{
    public float smoothing = 7f;
    public Vector3 Target
    {
        get { return target; }
        set
        {
            target = value;

            StopCoroutine("Movement");
            StartCoroutine("Movement", target);
        }
    }


    private Vector3 target;


    IEnumerator Movement (Vector3 target)
    {
        while(Vector3.Distance(transform.position, target) > 0.05f)
        {
            transform.position = Vector3.Lerp(transform.position, target, smoothing * Time.deltaTime);

            yield return null;
        }
    }
}
```

```cs
using UnityEngine;
using System.Collections;

public class ClickSetPosition : MonoBehaviour
{
    public PropertiesAndCoroutines coroutineScript;


    void OnMouseDown ()
    {
        Ray ray = Camera.main.ScreenPointToRay(Input.mousePosition);
        RaycastHit hit;

        Physics.Raycast(ray, out hit);

        if(hit.collider.gameObject == gameObject)
        {
            Vector3 newTarget = hit.point + new Vector3(0, 0.5f, 0);
            coroutineScript.Target = newTarget;
        }
    }
}
```