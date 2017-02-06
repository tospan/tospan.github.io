# 线性插补

https://unity3d.com/learn/tutorials/topics/scripting/linear-interpolation?playlist=17117

##

When making games it can sometimes be useful to linearly interpolate between two values. This is done with a function called Lerp. Linearly interpolating is finding a value that is some percentage between two given values. For example, we could linearly interpolate between the numbers 3 and 5 by 50% to get the number 4. This is because 4 is 50% of the way between 3 and 5.

In Unity there are several Lerp functions that can be used for different types. For the example we have just used, the equivalent would be the Mathf.Lerp function and would look like this:

```cs
// In this case, result = 4
float result = Mathf.Lerp (3f, 5f, 0.5f);
```

The Mathf.Lerp function takes 3 float parameters: one representing the value to interpolate from; another representing the value to interpolate to and a final float representing how far to interpolate. In this case, the interpolation value is 0.5 which means 50%. If it was 0, the function would return the ‘from’ value and if it was 1 the function would return the ‘to’ value.

Other examples of Lerp functions include Color.Lerp and Vector3.Lerp. These work in exactly the same way as Mathf.Lerp but the ‘from’ and ‘to’ values are of type Color and Vector3 respectively. The third parameter in each case is still a float representing how much to interpolate. The result of these functions is finding a colour that is some blend of two given colours and a vector that is some percentage of the way between the two given vectors.

Let’s look at another example:

```cs
Vector3 from = new Vector3 (1f, 2f, 3f);
Vector3 to = new Vector3 (5f, 6f, 7f);

// Here result = (4, 5, 6)
Vector3 result = Vector3.Lerp (from, to, 0.75f);
```

In this case the result is (4, 5, 6) because 4 is 75% of the way between 1 and 5; 5 is 75% of the way between 2 and 6 and 6 is 75% of the way between 3 and 7.

The same principle is applied when using Color.Lerp. In the Color struct, colours are represented by 4 floats representing red, blue, green and alpha. When using Lerp, these floats are interpolated just as with Mathf.Lerp and Vector3.Lerp.

Under some circumstances Lerp functions can be used to smooth a value over time. Consider the following piece of code:

```cs
void Update ()
{
    light.intensity = Mathf.Lerp(light.intensity, 8f, 0.5f);
}
```

If the intensity of the light starts off at 0 then after the first update it will be set to 4. The next frame it will be set to 6, then to 7, then to 7.5 and so on. Thus over several frames, the lights intensity will tend towards 8 but the rate of it’s change will slow as it approaches its target. Note that this happens over the course of several frames. If we wanted this to not be frame rate dependent then we could use the following code:

```cs
void Update ()
{
    light.intensity = Mathf.Lerp(light.intensity, 8f, 0.5f * Time.deltaTime);
}
```

This would mean the change to intensity would happen per second instead of per frame.

Please note that when smoothing a value it is often best to use the SmoothDamp function. Only use Lerp for smoothing if you are sure of the effect you want.

## 相关教程

* [Vector Maths](https://unity3d.com/learn/tutorials/topics/scripting/vector-maths) (Lesson)

## 相关文档

* [Vector3 Lerp](http://docs.unity3d.com/Documentation/ScriptReference/Vector3.Lerp.html?_ga=1.209404507.838993178.1480250241)  (Script Reference)
* [Mathf Lerp](http://docs.unity3d.com/Documentation/ScriptReference/Mathf.Lerp.html?_ga=1.172630506.838993178.1480250241)  (float) (Script Reference)
* [Color Lerp](http://docs.unity3d.com/Documentation/ScriptReference/Color.Lerp.html?_ga=1.172630506.838993178.1480250241)  (Script Reference)
* [Light intensity](http://docs.unity3d.com/Documentation/ScriptReference/Light-intensity.html?_ga=1.172630506.838993178.1480250241)  (Script Reference)
* [Vector3 Smooth Damp](http://docs.unity3d.com/ScriptReference/Vector3.SmoothDamp.html?_ga=1.172630506.838993178.1480250241)  (Script Reference)