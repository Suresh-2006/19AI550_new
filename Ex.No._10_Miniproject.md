# Ex.No: 10  Implementation of 2D/3D game -3D airplane obstacle avoidance game
### DATE:  21-05-2025                                                             
### REGISTER NUMBER : 212223040215
### AIM: 
To develop a 3D airplane obstacle avoidance game in Unity using AI for game-over detection and score management.

 
### Algorithm:
```
Initialize a 3D scene in Unity

Add a plane as the player and create movement scripts

Generate obstacles using prefabs and spawn logic

Implement AI-based collision detection for game over

Add UI elements for score and Game Over panel

Use particle effects and sound on collision

Add skybox, lighting, and materials to enhance visuals

Run and test the game in Unity Editor


```  
### Program:
playermovement.cs
```
using UnityEngine;

public class PlayerMovement : MonoBehaviour
{
    public float moveSpeed = 10f;
    public float horizontalSpeed = 5f;

    void Update()
    {
        transform.Translate(Vector3.forward * moveSpeed * Time.deltaTime);

        float h = Input.GetAxis("Horizontal");
        transform.Translate(Vector3.right * h * horizontalSpeed * Time.deltaTime);
    }

    private void OnCollisionEnter(Collision collision)
    {
        if (collision.gameObject.CompareTag("Obstacle"))
        {
            FindObjectOfType<GameOverHandler>().TriggerGameOver();
        }
    }
}

```
scoremanager.cs
```
using UnityEngine;
using TMPro;

public class ScoreManager : MonoBehaviour
{
    public TextMeshProUGUI scoreText;
    private int score;
    private bool isGameOver = false;

    void Start()
    {
        score = 0;
        InvokeRepeating("IncreaseScore", 1f, 1f);
    }

    void IncreaseScore()
    {
        if (!isGameOver)
        {
            score++;
            scoreText.text = "Score: " + score.ToString();
        }
    }

    public void StopScoring()
    {
        isGameOver = true;
    }
}
```
Game overhandler.cs
```
using UnityEngine;

public class GameOverHandler : MonoBehaviour
{
    public GameObject gameOverPanel;

    void Start()
    {
        gameOverPanel.SetActive(false);
    }

    public void TriggerGameOver()
    {
        gameOverPanel.SetActive(true);
        Time.timeScale = 0f;
        FindObjectOfType<ScoreManager>().StopScoring();
    }
}

```
obstaclespawner:
```
using UnityEngine;

public class ObstacleSpawner : MonoBehaviour
{
    public GameObject obstaclePrefab;
    public float spawnInterval = 2f;
    public float xRange = 8f;
    public float zSpawn = 30f;

    void Start()
    {
        InvokeRepeating("SpawnObstacle", 2f, spawnInterval);
    }

    void SpawnObstacle()
    {
        Vector3 spawnPos = new Vector3(Random.Range(-xRange, xRange), 0f, zSpawn);
        Instantiate(obstaclePrefab, spawnPos, Quaternion.identity);
    }
}

```

### Output:
The player controls an airplane flying forward. Obstacles appear randomly. When the plane hits an obstacle, the game displays a "Game Over" screen, stops gameplay, and plays a sound. Score increases over time and is shown on screen.



### Result:
Thus the game was developed using Unity and done successfully.
