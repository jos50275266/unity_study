# Project Boost

Player Experience:

- Precision, Skillful

Core Mechanic:

- Skillful fly spaceship and avoid environment hazards

Core Game Loop

- Get from A to B to complete the level, then progress to the next level

Game Flow And Screens

Game Theme (ie. Story & Visuals)

- Experimental early generation spacecraft.
- On an unknown planet, trying to escape

## What's Your Theme?

- Think of a central theme for your game.
- Decide on a starting name for your game.
- Share with the community!

## Onion Design

Common Development Questions

- What features should I include in my game?
- Where should I start development?
- What are my priorities?
- What if I run out of time?
- When should I stop?

![img](https://cdn-images-1.medium.com/max/1200/1*XpM855EICSYvysyGTbDP1A.png)

![img](https://cdn-images-1.medium.com/max/1200/1*CJMA1QbTVEz_efgkt4Irxw.png)

## Sketch Our Onion Design

- What do you believe is the single most important feature of our game?
- What is next most important?

![img](https://cdn-images-1.medium.com/max/1200/1*uYgC7_SabRyigvtt_Rxdzw.png)

# Unity Units

![img](https://cdn-images-1.medium.com/max/1200/1*pwn_Wli4g4-Hp7eY9qYsgw.png)

+X = right

+Y =up

+Z=forward

## Build Our Starting Pieces

**Position**: (0, -15, 0)

**Scale**: (100, 30, 20)

- Create the items from just 1 primitive each (later we'll make them more complex if need be):
  - Ground
  - Launch Pad
  - Landing Pad
  - Rocket

SampleScene --> Sandbox

## Introducing Classes

- Classes are used to organize our code
- Classes are "containers" for variables and methods that allow us to group similar things together
- Usually we aim for a class to do one main thing and not multiple things
  - Easier to read our code
  - Easier to fix issues
  - Easier to have multiple people work on a project

- Classes we create
  - We tend to create a new class each time we create a new script and have that script responsible for one thing. Eg:
    - Movement
    - CollisionHandler
    - Shooting
    - Score
    - EnemyAI

![img](https://cdn-images-1.medium.com/max/1200/1*UE0wZZLlITiG7jgKd7Hhvw.png)

- Cluttered code is like a cluttered desktop

- Also risky because everything can access everything else

## Encapsulating Our Code

- Where possible we encapsulate our code;
  - En-capsule-ating - putting in a capsule
- Think of the different parts of your code as having a **need to know basis level of access**
  - le. Don't let everything access everything else.
- For example, only the Methods in our Movement Class can alter our player's movement speed.

## Classes Already Built For Us

- Unity has many Classes (containing useful methods and variables) already created for us.
  - By accessing the class, we access its content.

- In our code we write the class name then use the `dot` operator to access things in the class:
  - **ClassName.MethodName()**

## Basic Input Binding

- Replace the "somethings" with code
- When we push "A" then print "rotate left"
- When we push "D" then print "rotate right"
- Identify the bug / issue with our logic

```c#
// movement.cs
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Movement : MonoBehaviour
{
    // Start is called before the first frame update
    void Start()
    {
            
    }

    // Update is called once per frame
    void Update()
    {
        ProcessThrust();
        ProcessRotation();
    }

    void ProcessThrust()
    {
        if (Input.GetKey(KeyCode.Space))
        {
            Debug.Log("Pressed Space - Thrusting");
        }
    }

    void ProcessRotation()
    {
        // if ?????? ?????? ?????? else if??? ?????????
        // A, B??? ????????? ???????????? ????????? ???????????????
        if (Input.GetKey(KeyCode.A))
        {
            Debug.Log("Rotating Left");
        }

        else if (Input.GetKey(KeyCode.B))
        {
            Debug.Log("Rotating Right");
        }
    }
}
```

## Using RelativeForce()

- Rocket??? rigidbody ??????

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Movement : MonoBehaviour
{
    Rigidbody rb;
    // Start is called before the first frame update
    void Start()
    {
        // Caching
        rb = GetComponent<Rigidbody>();
    }

    // Update is called once per frame
    void Update()
    {
        ProcessThrust();
        ProcessRotation();
    }

    void ProcessThrust()
    {
        if (Input.GetKey(KeyCode.Space))
        {
            // Debug.Log("Pressed Space - Thrusting");
            // Vector3.up = (0, 1, 0) ??? ??????
            rb.AddRelativeForce(0, 4, 0);
            // rb.AddRelativeForce(Vector3.up);
        }
    }

    void ProcessRotation()
    {
        // if ?????? ?????? ?????? else if??? ?????????
        // A, B??? ????????? ???????????? ????????? ???????????????
        if (Input.GetKey(KeyCode.A))
        {
            Debug.Log("Rotating Left");
        }

        else if (Input.GetKey(KeyCode.B))
        {
            Debug.Log("Rotating Right");
        }
    }
}
```

## Variable For Thrusting

- Add a variable so we can tune the main rocket thrust
- Make the force applied frame rate independent

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Movement : MonoBehaviour
{
    [SerializeField] float mainThrust = 100f;
    Rigidbody rb;
    // Start is called before the first frame update
    void Start()
    {
        // Caching
        rb = GetComponent<Rigidbody>();
    }

    // Update is called once per frame
    void Update()
    {
        ProcessThrust();
        ProcessRotation();
    }

    void ProcessThrust()
    {
        if (Input.GetKey(KeyCode.Space))
        {
            // Debug.Log("Pressed Space - Thrusting");
            // Vector3.up = (0, 1, 0) ??? ??????
            rb.AddRelativeForce(Vector3.up * mainThrust * Time.deltaTime);
        }
    }

    void ProcessRotation()
    {
        // if ?????? ?????? ?????? else if??? ?????????
        // A, B??? ????????? ???????????? ????????? ???????????????
        if (Input.GetKey(KeyCode.A))
        {
            Debug.Log("Rotating Left");
        }

        else if (Input.GetKey(KeyCode.B))
        {
            Debug.Log("Rotating Right");
        }
    }
}
```

## Transform.Rotate() Our Rocket

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Movement : MonoBehaviour
{
    [SerializeField] float mainThrust = 100f;
    Rigidbody rb;
    // Start is called before the first frame update
    void Start()
    {
        // Caching
        rb = GetComponent<Rigidbody>();
    }

    // Update is called once per frame
    void Update()
    {
        ProcessThrust();
        ProcessRotation();
    }

    void ProcessThrust()
    {
        if (Input.GetKey(KeyCode.Space))
        {
            // Debug.Log("Pressed Space - Thrusting");
            // Vector3.up = (0, 1, 0) ??? ??????
            rb.AddRelativeForce(Vector3.up * mainThrust * Time.deltaTime);
        }
    }

    void ProcessRotation()
    {
        // if ?????? ?????? ?????? else if??? ?????????
        // A, B??? ????????? ???????????? ????????? ???????????????
        if (Input.GetKey(KeyCode.A))
        {
            // (0, 0, 1)
            transform.Rotate(Vector3.forward);
        }

        else if (Input.GetKey(KeyCode.D))
        {
            transform.Rotate(-Vector3.forward);
        }
    }
}
```

Tuning Our Rocket's Rotation

- Add a variable so we can tune the rotation speed
- Make the rotation speed frame rate independent

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Movement : MonoBehaviour
{
    // ????????? ??????
    [SerializeField] float mainThrust = 100f;
    [SerializeField] float rotationThrust = 1f;
    Rigidbody rb;
    // Start is called before the first frame update
    void Start()
    {
        // Caching
        rb = GetComponent<Rigidbody>();
    }

    // Update is called once per frame
    void Update()
    {
        ProcessThrust();
        ProcessRotation();
    }

    void ProcessThrust()
    {
        if (Input.GetKey(KeyCode.Space))
        {
            // Debug.Log("Pressed Space - Thrusting");
            // Vector3.up = (0, 1, 0) ??? ??????
            rb.AddRelativeForce(Vector3.up * mainThrust * Time.deltaTime);
        }
    }

    void ProcessRotation()
    {
        // if ?????? ?????? ?????? else if??? ?????????
        // A, B??? ????????? ???????????? ????????? ???????????????
        if (Input.GetKey(KeyCode.A))
        {
            ApplyRotation(rotationThrust);
        }

        else if (Input.GetKey(KeyCode.D))
        {
            ApplyRotation(-rotationThrust);
        }
    }

    void ApplyRotation(float rotationThisFrame)
    {
        transform.Rotate(Vector3.forward * rotationThisFrame * Time.deltaTime);
    }
}
```

# Rigidbody Constraints

Tidy Up Our Movement

- Apply changes to prefab
- Add an obstacle
- Freeze constraints for our rocket
- Fix our obstacle-hitting bug

- Set our drag value

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Movement : MonoBehaviour
{
    // ????????? ??????
    [SerializeField] float mainThrust = 100f;
    [SerializeField] float rotationThrust = 1f;
    Rigidbody rb;
    // Start is called before the first frame update
    void Start()
    {
        // Caching
        rb = GetComponent<Rigidbody>();
    }

    // Update is called once per frame
    void Update()
    {
        ProcessThrust();
        ProcessRotation();
    }

    void ProcessThrust()
    {
        if (Input.GetKey(KeyCode.Space))
        {
            // Debug.Log("Pressed Space - Thrusting");
            // Vector3.up = (0, 1, 0) ??? ??????
            rb.AddRelativeForce(Vector3.up * mainThrust * Time.deltaTime);
        }
    }

    void ProcessRotation()
    {
        // if ?????? ?????? ?????? else if??? ?????????
        // A, B??? ????????? ???????????? ????????? ???????????????
        if (Input.GetKey(KeyCode.A))
        {
            ApplyRotation(rotationThrust);
        }

        else if (Input.GetKey(KeyCode.D))
        {
            ApplyRotation(-rotationThrust);
        }
    }

    void ApplyRotation(float rotationThisFrame)
    {
        rb.freezeRotation = true; // freezing rotation so we can manually rotate
        transform.Rotate(Vector3.forward * rotationThisFrame * Time.deltaTime);
        rb.freezeRotation = false; // unfreezing rotation so the physics system can take over
    }
}
```

?????? ?????? **Overrides**??? ???????????? Apply All ????????? ????????? ????????? ????????? Rigidbody ?????? ?????? ???????????? ?????? ????????? ??? ??????.

**Rocket**??? **Rigidbody ==> Constraints ==> Freeze Position (Z) + Rotation (X, Y)**

**Drag: ?????????. ???????????? ?????? ?????????=????????? ????????? ?????? ??? ???.**

**(0?????? ??????????????? 0?????? ?????????(INF)??? ???????????? ?????? ?????? ????????? ?????? ???????????? ?????? ??? ???)**

**Drag**: 0.25

**Assets --> Project Settings --> Physics --> Gravity (y: -4)**??? ???????????? ?????? ??? ?????? ???????????? ????????? ?????? ??? ??????.

## Unity Audio Introduction

https://docs.unity3d.com/ScriptReference/AudioSource.html

https://freesound.org/

https://www.audacityteam.org/download/

![img](https://cdn-images-1.medium.com/max/1200/1*hTIaErAMr8atwsGe8ylIVA.png)

1. **Audio Source**??????

- https://freesound.org/
  - Search For Rocket Boost
- https://www.audacityteam.org/download/
  - Make your own sound - Export Audio (????????? ????????? ??????????????? ????????????)

2. **Audio** ????????? ??? ????????? **Project Settings**??? ?????? **Audio** ?????? **System Sample Rate**??? 1??? ????????? ??????.

## Play AudioSource SFX

Play SFX When Thrusting

- Cache a reference to AudioSource called **audioSource**
- Use `audioSource.play()` to play when we are thrusting
- Use `!audioSource.isPlaying` to make sure we only play if we aren't already playing 
- Use an `else` condition and `audioSource.Stop()` to stop our SFX when we aren't thrusting

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Movement : MonoBehaviour
{
    // ????????? ??????
    [SerializeField] float mainThrust = 100f;
    [SerializeField] float rotationThrust = 1f;
    Rigidbody rb;
    AudioSource audioSource;
    // Start is called before the first frame update
    void Start()
    {
        // Caching
        rb = GetComponent<Rigidbody>();
        audioSource = GetComponent<AudioSource>();
    }

    // Update is called once per frame
    void Update()
    {
        ProcessThrust();
        ProcessRotation();
    }

    void ProcessThrust()
    {
        if (Input.GetKey(KeyCode.Space))
        {
            rb.AddRelativeForce(Vector3.up * mainThrust * Time.deltaTime);

            if (!audioSource.isPlaying)
            {
                audioSource.Play();
            }
        }

        else
        {
            audioSource.Stop();
        }
    }

    void ProcessRotation()
    {

        if (Input.GetKey(KeyCode.A))
        {
            ApplyRotation(rotationThrust);
        }

        else if (Input.GetKey(KeyCode.D))
        {
            ApplyRotation(-rotationThrust);
        }
    }

    void ApplyRotation(float rotationThisFrame)
    {
        // freezing rotation so we can manually rotate
        rb.freezeRotation = true;
        transform.Rotate(Vector3.forward * rotationThisFrame * Time.deltaTime);
        // unfreezing rotation so the physics system can take over
        rb.freezeRotation = false; 
    }
}
```

## Switch Statements

```c#
switch (variableToCompare)
{
    case valueA:
        ActionToTake();
        break;
    case valueB:
        ActionToTake();
        break;
    default:
        YetAnotherAction();
        break;
}
```

**Print Different Collision Situations**

- Using a Switch Statement, print to the console different messages based on what we collide into
- Remember that our collision tag returns a `string`

- Our variable will be: `other.gameObject.tag`

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class CollisionHandler : MonoBehaviour
{
    void OnCollisionEnter(Collision other)
    {
        switch (other.gameObject.tag)
        {
            case "Land":
                Debug.Log("This thing is landing area");
                break;
            case "Finish":
                Debug.Log("Congrats, you finished");
                break;
            case "Fuel":
                Debug.Log("You Picked up fuel");
                break;
            default:
                Debug.Log("Soory, you blew up!");
                break;
        }
        
    }
}
```

## Respawn Using SceneManager

https://docs.unity3d.com/ScriptReference/SceneManagement.SceneManager.html

**File** ==> **Build Settings** ==> **Add Open Scenes**??? ????????? ??????.

??? ?????? ????????? ????????? ?????????????????? ?????? ?????? ??????????????? ???, ????????? ????????? ???????????? ?????? ????????????.

**Reload Current Scene**

- Use `SceneManager.LoadScene()` to load the current scene and therefore respawn our rocket ship when it collides with the ground
- You'll need to use the `UnityEngine.SceneManagement` namespace

```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine.SceneManagement;
using UnityEngine;

public class CollisionHandler : MonoBehaviour
{
    void OnCollisionEnter(Collision other)
    {
        switch (other.gameObject.tag)
        {
            case "Land":
                Debug.Log("This thing is landing area");
                break;
            case "Finish":
                Debug.Log("Congrats, you finished");
                break;
            case "Fuel":
                Debug.Log("You Picked up fuel");
                break;
            default:
                // Debug.Log("Soory, you blew up!");
                ReloadLevel();
                break;
        }
    }

    void ReloadLevel()
    {
        // SceneManager.LoadScene(0);
        // ?????? ????????? ?????? ?????????
        int currentSceneIndex = SceneManager.GetActiveScene().buildIndex;
        SceneManager.LoadScene(currentSceneIndex);
    }
}
```





































