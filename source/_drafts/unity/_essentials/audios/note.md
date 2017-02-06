# 音频

Everything for Game Audio and Sound design in Unity


## Audio Setup

### Audio Listeners & Sources

The basics of audio playback and perception, using Audio Source and Listeners, as well as the Import settings for an audio clip.


## Audio Mixing

### 1. Audio Mixer and Audio Mixer Groups

AudioMixers allow us to control the signal flow of audio sources in a Unity project. Using an AudioMixer one can change volume levels, route signals into groups and process sounds with audio effects. In this lesson we'll cover the basics of creating and configuring AudioMixers including assigning audio sources to groups, routing groups into one another and routing AudioMixers into other audio mixers.

### 2. Audio Effects

In Unity it's possible to process audio signals in an AudioMixer Group using Audio Effects. In this Lesson we'll process our music using Low Pass Filter and Distortion audio effects and learn how the order Audio Effects are applied in changes the signal.

### 3. Send and Receive Audio Effects

In this lesson we'll look at controlling our signal flow using Send and Receive effects. Send effects allow us to route a duplicate of an audio signal to another group or effect in our mixer enabling complex signal flows. We'll look at using sends to route signal to a reverb audio effect.


### 4. Duck Volume Audio Effect

In this lesson we'll look at Unity's Duck Volume audio effect. Duck Volume allows us to cause the volume of an audio signal to duck or get quieter based on the volume of that signal, or a different one. A common use of this effect is sidechain compression. We'll look at how to set up Duck Volume including routing signals to control ducking with Send audio effects.


### 5. Audio Mixer Snapshots

In Unity it's possible to store and recall the state of an AudioMixer including volumes and effect settings using Snapshots. Snapshots can be recalled via script using the TransitionTo or the TransitionToSnapshots functions.


### 6. Exposed AudioMixer Parameters

In Unity it's possible to control the parameters of an AudioMixer directly by exposing them to script control. Parameters that have been exposed to script control can be set using the SetFloat function.


## Live Trainings on Audio

### Sound Effect & scripting

In this training session we demonstrate how to add sound effects to your game that are triggered by game events, using C# scripting. We also review the main audio components of Unity: Audio Listeners, Audio Sources and Audio Clips. Tutor - Matt Schell

```cs

using UnityEngine;
using System.Collections;

public class ThrowObject : MonoBehaviour {

    public GameObject projectile;
    public AudioClip shootSound;


    private float throwSpeed = 2000f;
    private AudioSource source;
    private float volLowRange = .5f;
    private float volHighRange = 1.0f;


    void Awake () {
    
        source = GetComponent<AudioSource>();

    }


    void Update () {

        if (Input.GetButtonDown("Fire1"))
        {
            float vol = Random.Range (volLowRange, volHighRange);
            source.PlayOneShot(shootSound,vol);
            GameObject throwThis = Instantiate (projectile, transform.position, transform.rotation) as GameObject;
            throwThis.rigidbody.AddRelativeForce (new Vector3(0,0,throwSpeed));
        }
    
    }
}

```

```cs
using UnityEngine;
using System.Collections;

public class CrashSound : MonoBehaviour {

    public AudioClip crashSoft;
    public AudioClip crashHard;


    private AudioSource source;
    private float lowPitchRange = .75F;
    private float highPitchRange = 1.5F;
    private float velToVol = .2F;
    private float velocityClipSplit = 10F;


    void Awake () {
    
        source = GetComponent<AudioSource>();
    }


    void OnCollisionEnter (Collision coll)
    {
        source.pitch = Random.Range (lowPitchRange,highPitchRange);
        float hitVol = coll.relativeVelocity.magnitude * velToVol;
        if (coll.relativeVelocity.magnitude < velocityClipSplit)
            source.PlayOneShot(crashSoft,hitVol);
        else 
            source.PlayOneShot(crashHard,hitVol);
    }

}
```

### Adding Music to Your Game

In this live training session we're going to show you how to add music to your game using the audio features in Unity 5.


```cs
using UnityEngine;
using System.Collections;
using UnityEngine.Audio;

public class CombatMusicControl : MonoBehaviour {


    public AudioMixerSnapshot outOfCombat;
    public AudioMixerSnapshot inCombat;
    public AudioClip[] stings;
    public AudioSource stingSource;
    public float bpm = 128;


    private float m_TransitionIn;
    private float m_TransitionOut;
    private float m_QuarterNote;

    // Use this for initialization
    void Start () 
    {
        m_QuarterNote = 60 / bpm;
        m_TransitionIn = m_QuarterNote;
        m_TransitionOut = m_QuarterNote * 32;

    }

    void OnTriggerEnter(Collider other)
    {
        if (other.CompareTag("CombatZone"))
        {
            inCombat.TransitionTo(m_TransitionIn);
            PlaySting();
        }
    }

    void OnTriggerExit(Collider other)
    {
        if (other.CompareTag("CombatZone"))
        {
            outOfCombat.TransitionTo(m_TransitionOut);
        }
    }

    void PlaySting()
    {
        int randClip = Random.Range (0, stings.Length);
        stingSource.clip = stings[randClip];
        stingSource.Play();
    }


}

```

### Sound  Effects in Unity 5

In this live training session we'll explore how to use Unity's mixer features to mix ambience, sound effects and apply audio effects like reverb and echo using send and receive effects.

Find the Unity Labs asset here: https://www.assetstore.unity3d.com/en/#!/content/33835

## Upgrading Audio to Unity 5

### Upgrading Unity 4 Audio Part 1: Mixeers and Effects

### Upgrading Unity 4 Audio Part 2: Snapshots and Exposed Paramters





## 相关教程

* [Audio](https://unity3d.com/learn/tutorials/projects/space-shooter-tutorial/audio-0)

## 相关文档

* [Audio Listener](http://docs.unity3d.com/Documentation/Components/class-AudioListener.html?_ga=1.209453659.838993178.1480250241) (Manual)
