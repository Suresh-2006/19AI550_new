# Ex.No: 7  Implementation of Path finding using A* algorithm
### DATE: 15-04-2025                                                     --
### REGISTER NUMBER : 212223040215
### AIM: 
To write a program to create graph using waypoints and use A* algorithm to find path between source and destination.
### Algorithm:
```
1. Create a New Unity Project by Open the  Unity Hub and create a new 3D Project,Name the project (e.g., Pathfinding).
2. Create Waypoints in Scene => Create empty or sphere GameObjects ( minimum 4)  and  name it as Waypoint1, Waypoint2, ..., Waypoint4
   Position them freely in the scene (not on a grid)
3. Write a waypoint script to draw the lines between neighbors.
4.Attach waypoint.cs script to each waypoint GameObject and Manually assign neighbors in the Inspector to form a graph.
5. Create a empty game object and name it as WaypointManager - to manage all waypoints
6. Attach Waypoint script to it
7.Write a Pathfinding algorithm using A*search
8. Create a Game Object for Player ( choose capsule or any others) and attach the script to move player from start to end waypoints
```  
### Program:
```
**#1.Waypoint.cs**
using UnityEngine;
using System.Collections.Generic;

public class Waypoint : MonoBehaviour {
    public List<Waypoint> neighbors = new List<Waypoint>();

    // Optional: Draw connections in the editor
    private void OnDrawGizmos() {
        Gizmos.color = Color.yellow;
        foreach (var neighbor in neighbors) {
            if (neighbor != null) {
                Gizmos.DrawLine(transform.position, neighbor.transform.position);
            }
        }
    }
}
**#2. WaypointGraph.cs**
using UnityEngine;

public class WaypointGraph : MonoBehaviour {
    public Waypoint[] allWaypoints;

    void Awake() {
        allWaypoints = FindObjectsOfType<Waypoint>();
    }
}
**#3.Pathfinding.cs**
using System.Collections.Generic;
using UnityEngine;
public class Pathfinding : MonoBehaviour {
    public static List<Waypoint> FindPath(Waypoint start, Waypoint goal) {
        var openSet = new List<Waypoint>();
        var cameFrom = new Dictionary<Waypoint, Waypoint>();
        var gScore = new Dictionary<Waypoint, float>();
        var fScore = new Dictionary<Waypoint, float>();

        foreach (var wp in FindObjectsOfType<Waypoint>()) {
            gScore[wp] = float.PositiveInfinity;
            fScore[wp] = float.PositiveInfinity;
        }

        gScore[start] = 0f;
        fScore[start] = Vector3.Distance(start.transform.position, goal.transform.position);
        openSet.Add(start);

        while (openSet.Count > 0) {
            Waypoint current = openSet[0];
            foreach (var wp in openSet) {
                if (fScore[wp] < fScore[current]) {
                    current = wp;
                }
            }

            if (current == goal) {
                return ReconstructPath(cameFrom, current);
            }

            openSet.Remove(current);

            foreach (var neighbor in current.neighbors) {
                float tentativeG = gScore[current] + Vector3.Distance(current.transform.position, neighbor.transform.position);
                if (tentativeG < gScore[neighbor]) {
                    cameFrom[neighbor] = current;
                    gScore[neighbor] = tentativeG;
                    fScore[neighbor] = tentativeG + Vector3.Distance(neighbor.transform.position, goal.transform.position);

                    if (!openSet.Contains(neighbor)) {
                        openSet.Add(neighbor);
                    }
                }
            }
        }

        return null; // No path found
    }

    private static List<Waypoint> ReconstructPath(Dictionary<Waypoint, Waypoint> cameFrom, Waypoint current) {
        var path = new List<Waypoint> { current };
        while (cameFrom.ContainsKey(current)) {
            current = cameFrom[current];
            path.Insert(0, current);
        }
        return path;
    }
}

**#4.AICharacter.cs**
using UnityEngine;
using System.Collections.Generic;

public class AICharacter : MonoBehaviour {
    public Waypoint startWaypoint;
    public Waypoint goalWaypoint;
    public float speed = 3f;

    private List<Waypoint> path;
    private int currentIndex = 0;

    void Start() {
        path = Pathfinding.FindPath(startWaypoint, goalWaypoint);
    }

    void Update() {
        if (path == null || currentIndex >= path.Count) return;

        Vector3 target = path[currentIndex].transform.position;
        transform.position = Vector3.MoveTowards(transform.position, target, speed * Time.deltaTime);

        if (Vector3.Distance(transform.position, target) < 0.1f) {
            currentIndex++;
        }
    }
}

Check the following
1. Waypoints placed in scene
2. Neighbors set manually via Inspector
3. WaypointGraph script on a manager
4. AICharacter assigned a start and goal
```
### Output:

### Initial Position:


![433913643-4dd19eb2-f294-4a79-af6d-5ba14c05b1ec](https://github.com/user-attachments/assets/de56e9d7-cb0b-44cf-b2cb-dd1800f1a5e7)

![433913884-6362dd23-2e22-46bd-ba66-ef5519c68e0d](https://github.com/user-attachments/assets/6a0dbcbc-eb34-4265-9cde-7d74f7918fc0)

![433911389-a6fca724-8f29-4962-9264-c5a443474640](https://github.com/user-attachments/assets/7827654e-b283-457e-96b4-476917acda87)

![433911435-68682ed2-127f-40f5-9344-64c99900e9ba](https://github.com/user-attachments/assets/5efe07b4-458d-4587-8054-78b39c56f759)




### Result:
Thus the pathfinding algorithm was sucessfully implemented.
