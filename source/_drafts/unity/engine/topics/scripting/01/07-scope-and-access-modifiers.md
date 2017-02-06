# 访问域与访问修饰符

Understanding variable & function scope and accessibility.


https://unity3d.com/learn/tutorials/topics/scripting/scope-and-access-modifiers?playlist=17117

https://youtu.be/XKGT_4mR1C4

## ScopeAndAccessModifiers

```cs
using UnityEngine;
using System.Collections;

public class ScopeAndAccessModifiers : MonoBehaviour
{
    public int alpha = 5;


    private int beta = 0;
    private int gamma = 5;


    private AnotherClass myOtherClass;


    void Start ()
    {
        alpha = 29;

        myOtherClass = new AnotherClass();
        myOtherClass.FruitMachine(alpha, myOtherClass.apples);
    }


    void Example (int pens, int crayons)
    {
        int answer;
        answer = pens * crayons * alpha;
        Debug.Log(answer);
    }


    void Update ()
    {
        Debug.Log("Alpha is set to: " + alpha);
    }
}
```

## AnotherClass

```cs
using UnityEngine;
using System.Collections;

public class AnotherClass
{
    public int apples;
    public int bananas;


    private int stapler;
    private int sellotape;


    public void FruitMachine (int a, int b)
    {
        int answer;
        answer = a + b;
        Debug.Log("Fruit total: " + answer);
    }


    private void OfficeSort (int a, int b)
    {
        int answer;
        answer = a + b;
        Debug.Log("Office Supplies total: " + answer);
    }
}
```