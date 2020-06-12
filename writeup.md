## Project: 3D Motion Planning

### Writeup / README

#### 1. Provide a Writeup / README that includes all the rubric points and how you addressed each one.  You can submit your writeup as markdown or pdf.  

You're reading it! Below I describe how I addressed each rubric point and where in my code each point is handled.

### Explain the Starter Code

**States in backyard_flyer_solution.py:**
1. Manual
2. Arming
3. Takeoff
4. Waypoint
5. Landing
6. Disarming

**States in motion_planning.py:**
1. Manual
2. Arming
3. **Planning**
4. Takeoff
5. Waypoint
6. Landing
7. Disarming

- As we can from the states in both the applications ( backyard flying and path planning), the state Planning is newly introduced in the Motion planning.
- In Backyerd flyer, the path of the Drone is constant (A square path)
- But Motion plannig is similar to a real world problem, where the drone find the feasible path between the start and the goal.
- This operation is done in the function plan_path() in  motion_planning.py.

#### Key steps in motion_planning.py

1. Read the input map ( colliders.csv in this case)
2. Obtain home (lat, lon) and obstacles position from the map
3. Get the start and goal position
4. Find the path using A* algorithm with a cost function.
5. Prune the obtained path
6. Provide waypoints in the path to the simulation.

#### Key steps in planning_utils.py
This file consists of the support function for the motion planning.

1. Create the grid from the obtained data
2. Define all available actions that the drone can take to reach the target. 
3. This includes both straight and diagonal actions.
4. Extract only valid actions based on the boundaries and obstacle positions.
5. Function to find the path between start and goal using A* algorithm
6. Estimate the heuristics value for the given position and goal position.
7. Finally prune the path.



### Implementing Your Path Planning Algorithm

#### 1. Set your global home position
The first line of the colliders.csv file contains the latitude and longitude. "lat0 37.792480, lon0 -122.397450"
 - Read the first line of the input file using **fp.readlines(1)** - fp: input file pointer
 - Split the data with ',' and get lat and lon
 - Convert string to float
 - set the home position with the **self.set_home_position(lon0, lat0, 0)** command

And here is a lovely picture of our downtown San Francisco environment from above!
![Map of SF](./misc/map.png)




Meanwhile, here's a picture of me flying through the trees!
![Forest Flying](./misc/in_the_trees.png)

#### 3. Set grid start position from local position
This is another step in adding flexibility to the start location. As long as it works you're good to go!

#### 4. Set grid goal position from geodetic coords
This step is to add flexibility to the desired goal location. Should be able to choose any (lat, lon) within the map and have it rendered to a goal location on the grid.

#### 5. Modify A* to include diagonal motion (or replace A* altogether)
Minimal requirement here is to modify the code in planning_utils() to update the A* implementation to include diagonal motions on the grid that have a cost of sqrt(2), but more creative solutions are welcome. Explain the code you used to accomplish this step.

#### 6. Cull waypoints 
For this step you can use a collinearity test or ray tracing method like Bresenham. The idea is simply to prune your path of unnecessary waypoints. Explain the code you used to accomplish this step.


