# Development-journals
Simplified template 

# Platformer

<br>Keisha Byrne </br>
<br>2401317</br>
<br>Fundamentals Of Games Development</br>

# Research
<br>For this game, I wanted to add my own twist onto the prompts that were already given to us. I knew I wanted to make it more fun and personal to me so that I would enjoy the process a lot more when making the game. I decided on making a game where you play as a lizard and collect eggs whilst going around the world and trying to svoid the obstacles which are trying to kill you. In order for this game to come alive I had to do a lot of research into more complicated scripts/code.</br>

### What research did you do and how did it help? 
<br> I wanted to add a slight delay to when the player dies and the level restarts back to the beginning. For this to happen I had to research coroutines and I done this by referring to unitys help guide online and also using ChatGPT to create the code needed and explain it in smaller chunks for me. I now have a greater understanding of co routines which I can now apply to my other game projects in the furture. I also researched a youtube video on how to create a main menu scene and to exit the game so that it is easy for users to navigate around my game. I feel like it adds an extra level to my game and polishes it. It was quite simple to follow through and I can now apply this to my future projects as well. I also researched how to make text appear when the player dies, saying you have died and when they hit the check point there is text that appears which says you have won and the scene restarts to the begining.</br>

### Links 
https://docs.unity3d.com/ScriptReference/WaitForSeconds.html
https://chatgpt.com/c/671cfb80-586c-8000-a516-cbe9180e86d8 (Co routine, text appearing)
https://www.youtube.com/watch?v=DX7HyN7oJjE&t=188s (main menu tutorial)



# How Did I Make It?

## Assets
<br>To make this game come to life after I had done some research I started with implementing assets which I found on unity. I wated to start with the assets as I felt like that if I found them it would help give me an idea of where I wanted this game to go. I imported these assets and then I placed them into my world. Once I had the assets in I then knew what scripts I needed to make in order to make the props work in the world. </br>

## Spikes and moving platforms
<br>I made a script which makes the spiky balls move left or right and forward in correlation to how they are positioned in the world. I had to make two seperate scripts because some spikes were placed differently so the script didnt work correctlty. I also attached a collider to them and the death script to them so that it killed the player when they touched it. I also attached this script to a platform to make it move and to make the blades spin whislt also moving left to right later on in the game.This script was also used on the objects at the end to make them connect together as they are broken blocks which open and close.  </br>

## Obstacles

<br>I wanted the player to die when they touched the obstacles and then restart from the start after the words "You died" appeared. I done this by using `OnCollisionEnter` , `IEnumerator` and TMPro text. I also had to add box colliders to all of these objects in order for the script to actually work when the player came in contact with them. </br>

## Rotating object 

<br> I made a rotating script to make the barrels and the blades spin. I feel like this makes the game feel more animated and adds a special touch to the game rather than all of the objects just staying still. This is also how a player would expect it to move if they saw these objects in other games.</br>

## Jump pad 
<br> I made a script for the jump pad to add a boost to the player to be able to get to the otherside of the map. I also had to animate the pad moving up and down to add a bit of life to this prop rather than it just give the player a boost and then not move.I used animator to do this and I had to add it to the prop and move the prop up and down during some timeframes. This was quite simple and then I had to reference it in the script. </br>

## Hammer 

<br> The hammer swings left and right, creating a dangerous obstacle for the player. The player must avoid it carefully, as touching the hammer means instant death. </br>
# Outcome (The Final Product)

## Spike scripts

```csharp 
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Leftorright : MonoBehaviour
{
    public float moveDistance = 5f; // the distance the object moves at 
    public float moveSpeed = 2; // how fast it moves

    // moving back and forth between positions 
    private Vector3 startPosition;
    private bool movingRight = true;

    private void Start() //get the initial position of the object 
    {
        startPosition = transform.position;
    }

    private void Update()
    {
        //target position
        Vector3 targetPosition = movingRight ? startPosition + Vector3.right * moveDistance : startPosition - Vector3.right * moveDistance;
        //move object towards target position 
        transform.position = Vector3.MoveTowards(transform.position, targetPosition, moveSpeed * Time.deltaTime);

        //check if the object has reached the target and then reverse the direction 
        if (transform.position == targetPosition)
        {
            movingRight = !movingRight;
        }
    }
}
``` 
<br>This code snippet explains how I used vectors and if statements to check the positions of the spikes and allow them to move accordingly.I used the same code for the other spikes as well, changing only the direction.</br>

```csharp 
public class Forward : MonoBehaviour
{

    public float moveDistance = 5f; // the distance the object moves at 
    public float moveSpeed = 2; // how fast it moves

    // moving back and forth between positions 
    private Vector3 startPosition;
    private bool movingForward = true;

    private void Start() //get the initial position of the object 
    {
        startPosition = transform.position;
    }

    private void Update()
    {
        //target position
        Vector3 targetPosition = movingForward ? startPosition + Vector3.forward * moveDistance : startPosition - Vector3.forward * moveDistance;
        //move object towards target position 
        transform.position = Vector3.MoveTowards(transform.position, targetPosition, moveSpeed * Time.deltaTime);

        //check if the object has reached the target and then reverse the direction 
        if (transform.position == targetPosition)
        {
            movingForward = !movingForward;
        }
    }
}
``` 

## Obstacle 

```csharp
using System.Collections;
using System.Collections.Generic;
using TMPro;
using UnityEngine;
using UnityEngine.SceneManagement;
// SceneManagement is needed to be able to restart the level 

namespace ObstacleNS
{
    public class Obstacle : MonoBehaviour
    {
        public bool playerDead;
        private TMP_Text deathText;
       
        private void Start()
        {
            deathText = GameObject.Find("DeathText").GetComponent<TMP_Text>();
        }

        private void OnCollisionEnter(Collision collision)
        {
            if (collision.collider)
            { 
                playerDead = true; 
                deathText.text = "You Died!";
                //when the player touches the obstacle You died will be shown 

                Destroy(collision.gameObject);
                //destroys the character

                StartCoroutine(RestartLevel());
                // this will restart the level after 
            }
        }

        private IEnumerator RestartLevel()
        {
            yield return new WaitForSeconds(2f);
            SceneManager.LoadScene(SceneManager.GetActiveScene().name);
            // this controls how long it is before the level is restarted 
        } 
    }
}
``` 
please see comments for explanation of this code. 

## Rotating object 

```csharp 
using System.Collections;
using System.Collections.Generic;
using UnityEngine;


public class RotatingObject : MonoBehaviour
{
    public Vector3 rotationDirection = Vector3.up;
    public float rotationSpeed = 50f;

// this controls how fast the object will rotate 

    private void Update()
    {
        transform.Rotate(rotationDirection.normalized, rotationSpeed * Time.deltaTime);
    }
    // this updates the objects position during the rotation
}
``` 
This code snippet shows how I used Vector3 again to control the position of the object and allow it to rotate around a point. It also shows how I can adjust and control the speed of the rotation.

## Jump pad 
```csharp 
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Jump : MonoBehaviour
{
    public float boostForce = 500f; //how high it will be boosted
    [SerializeField] private Animator springAnim; //referencing animation

    private void OnCollisionEnter(Collision collision)
    { 
        if (collision.gameObject.CompareTag("Player"))
        {
            Rigidbody rb = collision.gameObject.GetComponent<Rigidbody>();
            if (rb != null)
            { // checking if the player collides with the pad
                rb.AddForce(Vector3.up * boostForce);
                //the animation is triggered
                StartCoroutine(SpringAnimation());
            }
        }
    }

    IEnumerator SpringAnimation()
    {
        springAnim.SetBool("Spring", true);
        yield return new WaitForSeconds(2f);
        springAnim.SetBool("Spring", false);
    }

``` 
see comments 

## Hammer

```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Hammer : MonoBehaviour
{
    public float swingSpeed = 2.0f; // Speed of the hammer swing
    public float swingAngle = 45.0f; // Maximum angle to swing to each side

    private float startAngle;
   

    void Start()
    {
        // Initialize the starting rotation of the hammer
        startAngle = transform.rotation.eulerAngles.z;
    }

    void Update()
    {
        SwingHammer();
    }

    void SwingHammer()
    {
        // Calculate the current rotation angle based on the sine function to create a smooth back and forth motion
        float angle = swingAngle * Mathf.Sin(Time.time * swingSpeed);

        // Apply the rotation to the hammer using the calculated angle
        transform.rotation = Quaternion.Euler(0, 0, startAngle + angle);
    }
}
``` 
see comments 

## Egg 

```csharp 
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Egg : MonoBehaviour

{
    // Variable to specify how much score to add when the egg is collected
    [SerializeField] private int scoreAmount = 10;

    [SerializeField] private AudioClip audioClip; 
    [SerializeField] private AudioSource audioSource;

    // Detects when another collider enters the trigger collider
    private void OnTriggerEnter(Collider other)
    {
        // Try to get the Score component from the other object
        Score scoreComponent = other.GetComponent<Score>();

        // If a Score component is found, add to the score
        if (scoreComponent != null)
        {
            scoreComponent.AddScore(scoreAmount);
            audioSource.clip = audioClip;
            audioSource.Play();
            Debug.Log("EGG");
            Destroy(gameObject);
        }
    }
void Start() {
    //To give a value to audioSource we do two things
    //First, we find the Main Camera, by finding the gameobject with the MainCamera tag
    //We then use GetComponent in Children to reference the child object with the Audio Source
    audioSource = GameObject.FindGameObjectWithTag("MainCamera").GetComponentInChildren<AudioSource>(); 
} 
}
``` 
This is a code snippet for the score system when an egg is collected and the audio snippet is being played. 

# Reflection

In my opinion, I feel like this project went a lot better than the first one. I was very motivated to complete this project this time around and I managed to get it to a good enough level to be able to upload it onto Itch.io and get play testing feedback. I feel very proud of this project this time and I learned a lot more about C# during this project as I needed to know a lot of difficult code to make my ideas come alive. 


# Bibliography

https://www.youtube.com/watch?v=DX7HyN7oJjE&t=188s 
https://docs.unity3d.com/ScriptReference/WaitForSeconds.html 
https://chatgpt.com/c/671cfb80-586c-8000-a516-cbe9180e86d8 

# Declared Assets 
### Audios 
https://assetstore.unity.com/packages/audio/music/casual-game-bgm-5-135943 (noise effect)

https://assetstore.unity.com/packages/audio/sound-fx/8-bits-elements-16848   (background music)

### Props 
https://assetstore.unity.com/packages/3d/props/food/3d-props-adorable-foods-31249 (egg) 

https://assetstore.unity.com/packages/3d/environments/poly-style-platformer-starter-pack-284167 (world)

https://assetstore.unity.com/packages/3d/characters/animals/quirky-series-free-animals-pack-178235 (lizard)
