# Ex.No: 2  Welcome Script in Unity
### DATE: 04-03-2025                                                                        
### REGISTER NUMBER : 212223040215
### AIM: 
 To learn the basic scripting in Unity and print welcome message in Console window. 
### Procedure:
1. Start the program
2. Open the Unity hub and Create a new 3D project
3. In Assets window, create the new folder and name it as Scripts
4. Create a new script with file name as FirstScript
5. Open the Script and print message "Welcome to Unity" inside the start function
6. Save the script
7. Create a new 3D game object in Hierarchy window and name it as 3DObject.
8. Add the component Firstscript in inspector window of 3Dobject.
9. Run the program
10. Stop the program.
### Program 
```
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
public class FirstScript : MonoBehaviour
{
    // Start is called before the first frame update
    void Start()
    {
        print("Welcome to Unity");
    }

    // Update is called once per frame
    void Update()
    {
        
    }
}
```
### Output:
![415540410-1621abf8-4fb5-4322-b882-058aa0356b8b](https://github.com/user-attachments/assets/e3438a55-6cdc-41c4-ac33-0495ed74df32)




### Result:
Thus the welcome script was printed and consoled window sucessfully.

