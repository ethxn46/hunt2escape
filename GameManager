using UnityEngine;
using UnityEngine.UI;
using UnityEngine.SceneManagement;

public class GameManager : MonoBehaviour
{
    public static GameManager instance;

    public float timeRemaining = 20;
    public GameObject gameStartPanel;
    public GameObject gameOverPanel;
    public GameObject gameWinPanel;

    private bool isGameOver = false;
    private int PickUpCurrentCounter = 0;
    private int PickUpTotalCounter = 0;
    private GameObject goalGameObject;
    private GameObject Controller;
    private Text PickUpCounterText;
    private Text TimerText;

    // Start is called before the first frame update
    void Start()
    {
        if (instance == null)
        {
            instance = this;
            isGameOver = true;

            // Getting all references //
            goalGameObject = GameObject.FindGameObjectWithTag("Goal");
            Controller = GameObject.FindGameObjectWithTag("Player");
            TimerText = GameObject.Find("TimerText").GetComponent<Text>();
            PickUpCounterText = GameObject.Find("PickUpCounterText").GetComponent<Text>();
            PickUpTotalCounter = GameObject.FindGameObjectsWithTag("PickUp").Length;
            Controller.GetComponent<SUPERCharacter.SUPERCharacterAIO>().enabled = false;
            goalGameObject.SetActive(false);
            
            // Update UIs //
            UpdatePickUpCounter(0);
            DisplayTime(0);
        }
    }

    // Update is called once per frame
    void Update()
    {
        if (!isGameOver)
        {
            if (timeRemaining > 0)
            {
                timeRemaining -= Time.deltaTime;
                DisplayTime(timeRemaining);
            }
            else
            {
                print("Game Over! Time has run out!");
                timeRemaining = 0;
                isGameOver = true;
                gameOverPanel.SetActive(true);

                Controller.GetComponent<SUPERCharacter.SUPERCharacterAIO>().enabled = false;
                Controller.GetComponent<Rigidbody>().velocity = Vector3.zero;
                Cursor.visible = true;
                Cursor.lockState = CursorLockMode.None;
            }
        }
    }

    // Function to format and update timer on screen //
    private void DisplayTime(float timeToDisplay)
    {
        if (timeToDisplay > 0)
        {
            float minutes = Mathf.FloorToInt(timeToDisplay / 60);
            float seconds = Mathf.FloorToInt(timeToDisplay % 60);

            string timeToDisplayStr = timeToDisplay.ToString();
            int index = timeToDisplayStr.IndexOf('.');
            string mSeconds = timeToDisplay.ToString().Substring(index + 1, 2);

            TimerText.text = "Time: " + string.Format("{0:00}:{1:00}:{2:00}", minutes, seconds, mSeconds);
        }
        else
        {
            TimerText.text = "Time: " + "00:00:00";
        }
    }

    // Function to update pick up counter on screen //
    public void UpdatePickUpCounter(int increment) 
    {
        // Increase the count and update the UI //
        PickUpCurrentCounter += increment;
        PickUpCounterText.text = "Pick Up: " + PickUpCurrentCounter + " / " + PickUpTotalCounter;

        // Win Condition // 
        if (PickUpCurrentCounter == PickUpTotalCounter)
        {
            // Enable Goal GameObject to appear //
            goalGameObject.SetActive(true);
        }
    }
    
    // Function when the Goal is touched //
    public void GoalCollected() 
    {
        // Stop the game and show the win panel //
        isGameOver = true;
        gameWinPanel.SetActive(true);

        // Disable components for character and rigidbody //
        Controller.GetComponent<SUPERCharacter.SUPERCharacterAIO>().enabled = false;
        Controller.GetComponent<Rigidbody>().velocity = Vector3.zero;

        // Show the mouse cursor and free the mouse //
        Cursor.visible = true;
        Cursor.lockState = CursorLockMode.None;
    }

    // Function for start button //
    public void OnStartButtonPressed()
    {
        // Hide the start panel //
        gameStartPanel.SetActive(false);

        // Start the game by setting isGameOver to false //
        isGameOver = false;

        // Enable the player script //
        Controller.GetComponent<SUPERCharacter.SUPERCharacterAIO>().enabled = true;
    }

    // Function for the restart button //
    public void OnRestartButtonPressed()
    {
        // Reload the whole game scene //
        SceneManager.LoadScene("GameScene");
    }
}
