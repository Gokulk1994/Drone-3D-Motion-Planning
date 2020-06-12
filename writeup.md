## Project: 3D Motion Planning

### Writeup / README

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

#### 2. Set your current local position
 - The function global_to_local is used to convert the global position into local position.
 - global_to_local takes global_position and global_home as input arguments.
 - This function converts global coordinates in lat, lon format to local coordinates in NED coordinate systems .

#### 3. Set grid start position from local position
 - Create a grid for the input data along with the target altitude and safety margin from the obstacles.
 - The **create_grid()** from the planning_utils.py is used to create the grid.
 - This function returns the grid along with the north and east offset.
 - These north and east offset is considered as the grid start position ( negated values). This is the grid center.
 - Now the start position is changed to the current position instead of map center by reducing the offset from the local North and east positions.

#### 4. Set grid goal position from geodetic coords
 - Set any valid (should not contain obstacle) Lat, Lon position as goal.
 - Convert lat and lon to local NED coordinate systems using global_to_local() function 
 - Translate the points by reducing it from the North and east offsets.
 - Implement ceil operation and convert to integer type so that the grid positions can be accessed.
 - The obtained grid position is the Goal position.
  
#### 5. Modify A* to include diagonal motion (or replace A* altogether)
 - To obtain diagonal motions also in the A* planning, add all diagonal motions such as North-East, North-West, South-East and South-West to the Actions class.
 - The third element of each action in the Acion class represent the cost of that action. So add np.sqrt(2) as the cost of the diagonal actions.
 - Execute checks to ensure the actions does not leads to leavig out of the grid or does not reach any obstacles.
 - Only actions which satisfy above conditions are considered during the A* path planning.
 
#### 6. Cull waypoints 
 - Remove the points waypoints in the obtained path ( from A*) which lies in the same line.
 - This is done by executing a colinearity test on every 3 neighboring points.
 - If the determinant of the matrix formed by this 3 points is 0 or less than a small value epsilon, then it indicates the points lie exactly or almost in a straight line 
 - Remove such points from the waypoints to reduce the number of way points to be fed to the simulator.
 - This process is done by prune_path() and collinearity_check() functions in planning_utils.py file.

### Execute the flight

It works as expected. The drone reaches from start to goal position.
As there is a lot of grid cells and around 8 actions for each grid cells, It takes a while to calcualte the path using A* algorithm.
