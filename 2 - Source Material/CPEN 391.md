**Type:** [[course]]  
**Topics:** #computer-engineering   
**Date:** 2024-08-30  
**Source/Author:** {{Source/Author}} 
- **Tags**
	- 

---

# Lecture 2 (In-Class)

## Orientation, Frame Transformation, and Dead Reckoning


Strategy:

1. process out inf's and any "bad" points from range
2. 


---

# Final Project Ideas


![[Screenshot 2024-10-25 at 7.27.48 AM.png]]

RRT (rapidly exploring rapid tree)

Predictive Path Planning with Dynamic Obstacles (i.e. racing)
![[Screenshot 2024-10-25 at 7.29.55 AM.png]]

### **Predictive Path Planning with Dynamic Obstacles**

- **Goal**: Improve the car’s ability to avoid obstacles by predicting their future positions based on current sensor data (such as other cars or dynamic objects like pedestrians). The car would use this information to adjust its path in real-time.
- **Key Skills Involved**:
    - **Sensor Data Analysis**: Use lidar data (and potentially camera or other sensors) to detect and track moving obstacles.
    - **Kalman Filters/Extended Kalman Filters**: You could implement these to predict the future position of dynamic obstacles.
    - **Path Replanning**: Integrate predictive information into your car’s existing path planning system (like the Pure Pursuit or Follow-the-Gap algorithms) to replan routes dynamically.
- **Challenges**: Ensuring low-latency prediction and replanning is crucial to keep the car moving smoothly. You also need to consider how far ahead you predict and how you handle prediction uncertainty.

![[Screenshot 2024-10-25 at 7.30.12 AM.png]]
Hardware breakdown

NVIDIA Jetson Orin nano GPU Compute Platform (6 ARM Cortex-A78AE CPU cores) – Multi-threading

Setting up multiagent map. In f1tenth_gym_ros repo, set the sim.yaml in config to 2 agents. colcon build, then add a robotmodel with topic to opp_robot_desription 


Final project: Predictive path planning

https://github.com/zhm-real/PathPlanning

https://www.thinkautonomous.ai/blog/motion-planning/



### Sat Dec 14

10:30 AM – Working on making a raceline optimization script. 

Goal: Given a png and yaml, obtain the optimal feasible raceline. 

Understanding the map.

A map consists of a png, or visual representation of the map. white means free space, black is an obstacle and grey is somewhere in between.

yaml fields:
- `image` the PNG image of the map
- `resolution`: the scale of the map in meters per pixel
- `origin`: the coordinates of the bottom-left corner of the map in the map frame `[x,y,theta]` theta is rotation
1. **`occupied_thresh`**: The threshold to classify a pixel as an obstacle.
    - Pixel values above this threshold (e.g., `0.65`) are considered obstacles.
2. **`free_thresh`**: The threshold to classify a pixel as free space.
    - Pixel values below this threshold (e.g., `0.196`) are considered free space.

```yaml
slam_toolbox:
  ros__parameters:

    # Plugin params
    solver_plugin: solver_plugins::CeresSolver
    ceres_linear_solver: SPARSE_NORMAL_CHOLESKY
    ceres_preconditioner: SCHUR_JACOBI
    ceres_trust_strategy: LEVENBERG_MARQUARDT
    ceres_dogleg_type: TRADITIONAL_DOGLEG
    ceres_loss_function: None

    # ROS Parameters
    odom_frame: odom
    map_frame: map
#    base_frame: laser
    base_frame: base_link
    scan_topic: /notscan
    mode: mapping #localization

    # if you'd like to immediately start continuing a map at a given pose
    # or at the dock, but they are mutually exclusive, if pose is given
    # will use pose
    #map_file_name: test_steve
    # map_start_pose: [0.0, 0.0, 0.0]
    #map_start_at_dock: true

    debug_logging: false
    throttle_scans: 1
    transform_publish_period: 0.02 #if 0 never publishes odometry
    map_update_interval: 5.0
    resolution: 0.05
    max_laser_range: 20.0 #for rastering images
    minimum_time_interval: 0.5
    transform_timeout: 1.0
    tf_buffer_duration: 30.
    stack_size_to_use: 40000000 #// program needs a larger stack size to serialize large maps
    enable_interactive_mode: true

    # General Parameters
    use_scan_matching: true
    use_scan_barycenter: true
    minimum_travel_distance: 0.5
    minimum_travel_heading: 0.5
    scan_buffer_size: 10
    scan_buffer_maximum_scan_distance: 10.0
    link_match_minimum_response_fine: 0.1
    link_scan_maximum_distance: 1.5
    loop_search_maximum_distance: 3.0
    do_loop_closing: true
    loop_match_minimum_chain_size: 10
    loop_match_maximum_variance_coarse: 3.0
    loop_match_minimum_response_coarse: 0.35
    loop_match_minimum_response_fine: 0.45

    # Correlation Parameters - Correlation Parameters
    correlation_search_space_dimension: 0.5
    correlation_search_space_resolution: 0.01
    correlation_search_space_smear_deviation: 0.1

    # Correlation Parameters - Loop Closure Parameters
    loop_search_space_dimension: 8.0
    loop_search_space_resolution: 0.05
    loop_search_space_smear_deviation: 0.03

    # Scan Matcher Parameters
    distance_variance_penalty: 0.5
    angle_variance_penalty: 1.0

    fine_search_angle_offset: 0.00349
    coarse_search_angle_offset: 0.349
    coarse_angle_resolution: 0.0349
    minimum_angle_penalty: 0.9
    minimum_distance_penalty: 0.5
    use_response_expansion: true
```


Steps:
1. First we will obtain a centerline


```py
#!/usr/bin/env python3

import rclpy

from rclpy.node import Node

from visualization_msgs.msg import Marker, MarkerArray

from geometry_msgs.msg import Point

from nav_msgs.msg import Odometry

import numpy as np

from tf_transformations import euler_from_quaternion

  

class ReactiveRacer(Node):

def __init__(self):

super().__init__('reactive_racer_node')

# Change to MarkerArray publisher

self.marker_pub = self.create_publisher(MarkerArray, '/visualization_marker', 10)

self.pose_sub = self.create_subscription(

Odometry,

'/ego_racecar/odom',

self.pose_callback,

10)

self.LOOKAHEAD_DISTANCE = 2.0

self.NUM_TRAJECTORIES = 21

self.ANGLE_RANGE = np.pi/4

self.best_trajectory_idx = self.NUM_TRAJECTORIES // 2

self.current_x = 0.0

self.current_y = 0.0

self.current_yaw = 0.0

self.timer = self.create_timer(0.05, self.update_visualization)

  

def generate_trajectories(self, start_x, start_y, yaw):

"""Generate straight-line trajectories"""

angles = np.linspace(-self.ANGLE_RANGE, self.ANGLE_RANGE, self.NUM_TRAJECTORIES)

trajectories = []

for i, angle in enumerate(angles):

total_angle = yaw + angle

end_x = start_x + self.LOOKAHEAD_DISTANCE * np.cos(total_angle)

end_y = start_y + self.LOOKAHEAD_DISTANCE * np.sin(total_angle)

trajectories.append({

'points': [[start_x, start_y], [end_x, end_y]],

'index': i

})

return trajectories

  

def get_trajectory_color(self, trajectory_idx):

"""Get color for trajectory based on its index"""

if trajectory_idx == self.best_trajectory_idx:

return (0.0, 1.0, 0.0, 1.0) # Green

elif trajectory_idx < self.best_trajectory_idx:

return (0.0, 0.0, 1.0, 0.8) # Blue

else:

return (1.0, 0.0, 0.0, 0.8) # Red

  

def create_trajectory_marker(self, points, color, id):

"""Create a visualization marker for a trajectory"""

marker = Marker()

marker.header.frame_id = "map"

marker.header.stamp = self.get_clock().now().to_msg()

marker.id = id

marker.type = Marker.LINE_STRIP

marker.action = Marker.ADD

marker.pose.orientation.w = 1.0

marker.scale.x = 0.03 # Line width

marker.color.r = color[0]

marker.color.g = color[1]

marker.color.b = color[2]

marker.color.a = color[3]

# Set a very short lifetime

marker.lifetime = rclpy.duration.Duration(seconds=0.1).to_msg()

for point in points:

p = Point()

p.x, p.y, p.z = float(point[0]), float(point[1]), 0.0

marker.points.append(p)

return marker

  

def pose_callback(self, pose_msg):

self.current_x = pose_msg.pose.pose.position.x

self.current_y = pose_msg.pose.pose.position.y

quaternion = [

pose_msg.pose.pose.orientation.x,

pose_msg.pose.pose.orientation.y,

pose_msg.pose.pose.orientation.z,

pose_msg.pose.pose.orientation.w

]

_, _, self.current_yaw = euler_from_quaternion(quaternion)

  

def update_visualization(self):

"""Update the visualization of trajectories"""

# Generate all trajectories

trajectories = self.generate_trajectories(

self.current_x,

self.current_y,

self.current_yaw

)

# Create marker array

marker_array = MarkerArray()

# Add all trajectory markers to the array

for traj in trajectories:

color = self.get_trajectory_color(traj['index'])

marker = self.create_trajectory_marker(

traj['points'],

color,

traj['index']

)

marker_array.markers.append(marker)

# Publish all markers at once

self.marker_pub.publish(marker_array)

  

def main(args=None):

rclpy.init(args=args)

print("Reactive Racer Initialized")

reactive_racer_node = ReactiveRacer()

rclpy.spin(reactive_racer_node)

  

reactive_racer_node.destroy_node()

rclpy.shutdown()

  

if __name__ == '__main__':

main()
```

Curved trajectories

```python
#!/usr/bin/env python3

import rclpy

from rclpy.node import Node

from visualization_msgs.msg import Marker, MarkerArray

from geometry_msgs.msg import Point

from nav_msgs.msg import Odometry

import numpy as np

from tf_transformations import euler_from_quaternion

  

class ReactiveRacer(Node):

def __init__(self):

super().__init__('reactive_racer_node')

# Change to MarkerArray publisher

self.marker_pub = self.create_publisher(MarkerArray, '/visualization_marker', 10)

self.pose_sub = self.create_subscription(

Odometry,

'/ego_racecar/odom',

self.pose_callback,

10)

self.LOOKAHEAD_DISTANCE = 2.0

self.NUM_TRAJECTORIES = 21

self.ANGLE_RANGE = np.pi/4

self.best_trajectory_idx = self.NUM_TRAJECTORIES // 2

self.current_x = 0.0

self.current_y = 0.0

self.current_yaw = 0.0

self.timer = self.create_timer(0.05, self.update_visualization)

  

def generate_trajectories(self, start_x, start_y, yaw):

"""Generate arc trajectories with constant curvature"""

angles = np.linspace(-self.ANGLE_RANGE, self.ANGLE_RANGE, self.NUM_TRAJECTORIES)

trajectories = []

for i, angle in enumerate(angles):

points = []

if abs(angle) < 1e-5: # Straight line case

end_x = start_x + self.LOOKAHEAD_DISTANCE * np.cos(yaw)

end_y = start_y + self.LOOKAHEAD_DISTANCE * np.sin(yaw)

t = np.linspace(0, 1, 20)

points = [[start_x + s * (end_x - start_x),

start_y + s * (end_y - start_y)] for s in t]

else:

# Calculate turn radius based on the angle

turn_radius = self.LOOKAHEAD_DISTANCE / angle

  

# Calculate center of turning circle

center_x = start_x - turn_radius * np.sin(yaw)

center_y = start_y + turn_radius * np.cos(yaw)

# Calculate start angle

start_angle = yaw - np.pi/2

# Generate points along the arc

t = np.linspace(0, angle, 20)

for theta in t:

arc_angle = start_angle + theta

x = center_x + turn_radius * np.cos(arc_angle)

y = center_y + turn_radius * np.sin(arc_angle)

points.append([x, y])

trajectories.append({

'points': points,

'index': i

})

return trajectories

  

def get_trajectory_color(self, trajectory_idx):

"""Get color for trajectory based on its index"""

if trajectory_idx == self.best_trajectory_idx:

return (0.0, 1.0, 0.0, 1.0) # Green

elif trajectory_idx < self.best_trajectory_idx:

return (0.0, 0.0, 1.0, 0.8) # Blue

else:

return (1.0, 0.0, 0.0, 0.8) # Red

  

def create_trajectory_marker(self, points, color, id):

"""Create a visualization marker for a trajectory"""

marker = Marker()

marker.header.frame_id = "map"

marker.header.stamp = self.get_clock().now().to_msg()

marker.id = id

marker.type = Marker.LINE_STRIP

marker.action = Marker.ADD

marker.pose.orientation.w = 1.0

marker.scale.x = 0.03 # Line width

marker.color.r = color[0]

marker.color.g = color[1]

marker.color.b = color[2]

marker.color.a = color[3]

# Set a very short lifetime

marker.lifetime = rclpy.duration.Duration(seconds=0.1).to_msg()

for point in points:

p = Point()

p.x, p.y, p.z = float(point[0]), float(point[1]), 0.0

marker.points.append(p)

return marker

  

def pose_callback(self, pose_msg):

self.current_x = pose_msg.pose.pose.position.x

self.current_y = pose_msg.pose.pose.position.y

quaternion = [

pose_msg.pose.pose.orientation.x,

pose_msg.pose.pose.orientation.y,

pose_msg.pose.pose.orientation.z,

pose_msg.pose.pose.orientation.w

]

_, _, self.current_yaw = euler_from_quaternion(quaternion)

  

def update_visualization(self):

"""Update the visualization of trajectories"""

# Generate all trajectories

trajectories = self.generate_trajectories(

self.current_x,

self.current_y,

self.current_yaw

)

# Create marker array

marker_array = MarkerArray()

# Add all trajectory markers to the array

for traj in trajectories:

color = self.get_trajectory_color(traj['index'])

marker = self.create_trajectory_marker(

traj['points'],

color,

traj['index']

)

marker_array.markers.append(marker)

# Publish all markers at once

self.marker_pub.publish(marker_array)

  

def main(args=None):

rclpy.init(args=args)

print("Reactive Racer Initialized")

reactive_racer_node = ReactiveRacer()

rclpy.spin(reactive_racer_node)

  

reactive_racer_node.destroy_node()

rclpy.shutdown()

  

if __name__ == '__main__':

main()
```

```
#!/usr/bin/env python3

import rclpy

from rclpy.node import Node

from visualization_msgs.msg import Marker, MarkerArray

from geometry_msgs.msg import Point

from nav_msgs.msg import Odometry

from sensor_msgs.msg import LaserScan

import numpy as np

from tf_transformations import euler_from_quaternion

  

class ReactiveRacer(Node):

def __init__(self):

super().__init__('reactive_racer_node')

self.marker_pub = self.create_publisher(MarkerArray, '/visualization_marker', 10)

# Subscribers

self.pose_sub = self.create_subscription(

Odometry,

'/ego_racecar/odom',

self.pose_callback,

10)

self.scan_sub = self.create_subscription(

LaserScan,

'/scan',

self.scan_callback,

10)

# Parameters

self.LOOKAHEAD_DISTANCE = 2.0

self.NUM_TRAJECTORIES = 21

self.ANGLE_RANGE = np.pi/3 # ±60 degrees

self.POINTS_PER_CURVE = 20

self.COLLISION_THRESHOLD = 0.3

# State variables

self.current_x = 0.0

self.current_y = 0.0

self.current_yaw = 0.0

self.scan_points = []

self.best_trajectory_idx = None

self.timer = self.create_timer(0.1, self.update_visualization)

  

def create_trajectory_curve(self, start_point, end_point, angle_rad):

"""Create a trajectory using cubic Bézier curve with natural endpoint tangents"""

P0 = np.array(start_point)

P3 = np.array(end_point)

# Start direction is aligned with car's heading

start_direction = np.array([np.cos(self.current_yaw), np.sin(self.current_yaw)])

# End direction based on the angle

end_direction = np.array([np.cos(angle_rad), np.sin(angle_rad)])

# Distance factor for control points (0.5 of path length)

d = np.linalg.norm(P3 - P0) * 0.5

# Control points

P1 = P0 + start_direction * d

P2 = P3 - end_direction * d

# Generate points along the curve

t = np.linspace(0, 1, self.POINTS_PER_CURVE)

points = []

# Cubic Bézier curve formula

for t_i in t:

t_inv = 1 - t_i

# Compute point

point = (

t_inv**3 * P0 +

3 * t_inv**2 * t_i * P1 +

3 * t_inv * t_i**2 * P2 +

t_i**3 * P3

)

points.append(point.tolist())

return points

  

def generate_trajectories(self, start_x, start_y, yaw):

"""Generate a fan of Bézier curve trajectories"""

angles = np.linspace(-self.ANGLE_RANGE, self.ANGLE_RANGE, self.NUM_TRAJECTORIES)

trajectories = []

start = [start_x, start_y]

for i, angle in enumerate(angles):

# Calculate end point angle

end_angle = yaw + angle

# Calculate end point (vary distance based on angle)

distance = self.LOOKAHEAD_DISTANCE * (0.8 + 0.2 * np.cos(angle)) # Shorter paths for extreme angles

end_x = start_x + distance * np.cos(end_angle)

end_y = start_y + distance * np.sin(end_angle)

end = [end_x, end_y]

# Generate curve points

points = self.create_trajectory_curve(start, end, end_angle)

trajectories.append({

'points': points,

'index': i

})

return trajectories

  

def check_collision(self, points):

"""Check if trajectory collides with obstacles"""

if not self.scan_points:

return False

for p in points:

for scan_point in self.scan_points:

dist = np.hypot(p[0] - scan_point[0], p[1] - scan_point[1])

if dist < self.COLLISION_THRESHOLD:

return True

return False

  

def find_best_trajectory(self, trajectories):

"""Find best trajectory based on pure pursuit criteria"""

valid_trajectories = []

min_deviation = float('inf')

best_idx = None

# Filter out trajectories with collisions

for traj in trajectories:

if not self.check_collision(traj['points']):

valid_trajectories.append(traj)

if not valid_trajectories:

return None

# Find trajectory closest to center

center_idx = self.NUM_TRAJECTORIES // 2

for traj in valid_trajectories:

deviation = abs(traj['index'] - center_idx)

if deviation < min_deviation:

min_deviation = deviation

best_idx = traj['index']

return best_idx

  

def get_trajectory_color(self, trajectory_idx, has_collision):

"""Get color for trajectory based on its status"""

if trajectory_idx == self.best_trajectory_idx:

return (0.0, 1.0, 0.0, 1.0) # Green for best

elif has_collision:

return (1.0, 0.0, 0.0, 0.8) # Red for collision

else:

return (0.0, 0.0, 1.0, 0.8) # Blue for valid

  

def create_trajectory_marker(self, points, color, id):

"""Create a visualization marker for a trajectory"""

marker = Marker()

marker.header.frame_id = "map"

marker.header.stamp = self.get_clock().now().to_msg()

marker.id = id

marker.type = Marker.LINE_STRIP

marker.action = Marker.ADD

marker.pose.orientation.w = 1.0

marker.scale.x = 0.03

marker.color.r = color[0]

marker.color.g = color[1]

marker.color.b = color[2]

marker.color.a = color[3]

marker.lifetime = rclpy.duration.Duration(seconds=0.3).to_msg()

for point in points:

p = Point()

p.x, p.y, p.z = float(point[0]), float(point[1]), 0.0

marker.points.append(p)

return marker

  

def scan_callback(self, scan_msg):

"""Process incoming laser scan data"""

self.scan_points = []

angle = scan_msg.angle_min

for r in scan_msg.ranges:

if scan_msg.range_min <= r <= scan_msg.range_max:

x = r * np.cos(angle)

y = r * np.sin(angle)

x_car = self.current_x + x * np.cos(self.current_yaw) - y * np.sin(self.current_yaw)

y_car = self.current_y + x * np.sin(self.current_yaw) + y * np.cos(self.current_yaw)

self.scan_points.append([x_car, y_car])

angle += scan_msg.angle_increment

  

def pose_callback(self, pose_msg):

"""Update current pose"""

self.current_x = pose_msg.pose.pose.position.x

self.current_y = pose_msg.pose.pose.position.y

quaternion = [

pose_msg.pose.pose.orientation.x,

pose_msg.pose.pose.orientation.y,

pose_msg.pose.pose.orientation.z,

pose_msg.pose.pose.orientation.w

]

_, _, self.current_yaw = euler_from_quaternion(quaternion)

  

def update_visualization(self):

"""Update the visualization of trajectories"""

trajectories = self.generate_trajectories(

self.current_x,

self.current_y,

self.current_yaw

)

self.best_trajectory_idx = self.find_best_trajectory(trajectories)

marker_array = MarkerArray()

for traj in trajectories:

has_collision = self.check_collision(traj['points'])

color = self.get_trajectory_color(traj['index'], has_collision)

marker = self.create_trajectory_marker(

traj['points'],

color,

traj['index']

)

marker_array.markers.append(marker)

self.marker_pub.publish(marker_array)

  

def main(args=None):

rclpy.init(args=args)

print("Reactive Racer Initialized")

reactive_racer_node = ReactiveRacer()

rclpy.spin(reactive_racer_node)

  

reactive_racer_node.destroy_node()

rclpy.shutdown()
  

if __name__ == '__main__':

main()
```

![[Screenshot 2024-12-19 at 5.33.02 PM.png]]

I gave you some code for my current reactive racer, but I think I have a better idea. Now, I want my reactive racer to do the following: it will have a 2d array 100 along the x and 200 on the y. This is supposed to represent a 1m wide by 2m tall occupancy grid where the lidar is centered at the point (x,y) (0,0). Note that it extends from -50 <= x <= 50 cm (this may not be right because we only have 100 indices) and 0 <= y <= 200. Note that the lidar data uses polar coordinates with the length being r and the angle being theta. Essentially what we want to do is at a certain iteration, the car gets the lidar data then populates the cost grid. 1 means we hit an obstacle, 0 means its free space and -1 means unknown. There will be points in between 0 and 1. How it will work is we sample a certain amount of lidar coordinates from 0 to 90 degrees, say we do every 1 degree. Then once we hit a wall, we have to use some sort of decaying function that ouputs a value 1 to 0 based on the regression (while we're populating) for example the function e^(-0.2x) so if we know the pixel (i,j) is blocked then for any other pixel in a certain proximity, we apply a cost (maybe a convolution or a simple sum).

eventually what we want to do is once we have a populated cost map, we want to find the least cost path from (0,0) to (x,200) (this may be subject to change because perhaps we want to exit from the side at some value less than 200). Then we turn the path we found into a spline and use a pure pursuit style algorithm to follow it.

This should be a reactive path planner that works well in uncharted territory

Implementation order
(1) Orientation of the car -> receive lidar data and convert the lidar readings of a single line to the map. Get a ray from the car at -45 0 and 45 degrees and view it on the sim
(2) Create the cost map -> generate a cost map of all unknowns, -1, and visualize it, ensuring that it is oriented correctly.
(3) Populating the map. Populate the map with 1 for obstacle, 0 for free space and -1 for unknown. 1 is red 0 is green and -1 is grey


```python
#!/usr/bin/env python3

import rclpy

from rclpy.node import Node

from visualization_msgs.msg import Marker, MarkerArray

from geometry_msgs.msg import Point

from std_msgs.msg import ColorRGBA

from sensor_msgs.msg import LaserScan

import numpy as np

from tf_transformations import euler_from_quaternion

from scipy.ndimage import gaussian_filter

from queue import PriorityQueue

  
  

class CostMapVisualization(Node):

def __init__(self):

super().__init__('cost_map_visualization')

# Grid parameters

self.RESOLUTION = 0.05 # 1cm per cell

self.WIDTH = 20 # 1m lateral width (100 cells)

self.HEIGHT = 40 # 2m forward length (200 cells)

self.X_MIN = -0.5 # 50cm to the left

self.X_MAX = 0.5 # 50cm to the right

self.Y_MIN = 0.0 # Starting at car's position

self.Y_MAX = 2.0 # 2m forward

  

# Cost propagation parameters

self.GAUSSIAN_SIGMA = 3.0 # Controls the spread of the obstacle cost

  

# Path finding parameters

self.PATH_WEIGHT = 0.3 # Weight between cost (0) and distance (1)

self.OBSTACLE_COST_FACTOR = 5.0 # Multiplier for obstacle proximity cost

self.current_path = [] # Store the current path

  

# Initialize grid with unknown values (-1)

self.grid = np.full((self.HEIGHT, self.WIDTH), 1.0)

# Publishers

self.marker_pub = self.create_publisher(MarkerArray, '/cost_map_visualization', 10)

# Subscribers

self.scan_sub = self.create_subscription(

LaserScan,

'/scan',

self.scan_callback,

10)

# Timer for publishing visualization

self.create_timer(0.1, self.publish_visualization)

self.get_logger().info("CostMapVisualization node initialized")

  

def polar_to_grid(self, r, theta):

"""Convert polar coordinates (r, theta) to grid coordinates"""

# Convert to Cartesian (in car frame)

x = r * np.cos(theta) # Forward

y = r * np.sin(theta) # Left

# Convert to grid coordinates (allow out-of-bound indices)

y_idx = int(x / self.RESOLUTION) # Forward index

x_idx = int((y + self.X_MAX) / self.RESOLUTION) # Lateral index

return y_idx, x_idx

  

def get_line_points(self, start, end):

"""Get points along a line using Bresenham's algorithm"""

points = []

x0, y0 = start

x1, y1 = end

dx = abs(x1 - x0)

dy = abs(y1 - y0)

x, y = x0, y0

sx = 1 if x0 < x1 else -1

sy = 1 if y0 < y1 else -1

err = dx - dy

while True:

points.append((x, y))

if x == x1 and y == y1:

break

e2 = 2 * err

if e2 > -dy:

err -= dy

x += sx

if e2 < dx:

err += dx

y += sy

return points

  

def apply_gaussian_filter(self):

"""Apply Gaussian filter for continuous cost field"""

# Create obstacle grid

obstacle_grid = np.where(self.grid == 1, 1.0, 0.0)

# Apply Gaussian filter for smooth cost propagation

propagated_grid = gaussian_filter(obstacle_grid, sigma=self.GAUSSIAN_SIGMA)

# Scale the costs to maintain proper gradient

propagated_grid = propagated_grid * self.OBSTACLE_COST_FACTOR

# Combine with original grid - keep obstacles as obstacles

self.grid = np.where(self.grid == 1, 1.0, propagated_grid)

  

def manhattan_distance(self, p1, p2):

"""Manhattan distance heuristic for A*"""

return abs(p1[0] - p2[0]) + abs(p1[1] - p2[1])

  

def find_least_cost_path(self):

"""Find least cost path using A* algorithm"""

start = (0, self.WIDTH // 2)

best_end = None

best_path = None

min_total_cost = float('inf')

# Try all possible end points at max height

for end_x in range(self.WIDTH):

end = (self.HEIGHT-1, end_x)

path, total_cost = self._a_star_search(start, end)

if path and total_cost < min_total_cost:

min_total_cost = total_cost

best_path = path

best_end = end

self.current_path = best_path if best_path else []

return self.current_path

  

def _a_star_search(self, start, goal):

"""A* search with continuous costs"""

frontier = PriorityQueue()

frontier.put((0, start))

came_from = {start: None}

cost_so_far = {start: 0}

while not frontier.empty():

current = frontier.get()[1]

if current == goal:

break

# Check 8-connected neighbors

for dx, dy in [(0,1), (1,0), (0,-1), (-1,0), (1,1), (1,-1), (-1,1), (-1,-1)]:

next_pos = (current[0] + dx, current[1] + dy)

# Check bounds

if not (0 <= next_pos[0] < self.HEIGHT and 0 <= next_pos[1] < self.WIDTH):

continue

# Get cell cost

cell_cost = float(self.grid[next_pos])

# Only true obstacles are forbidden

if cell_cost >= 1.0: # Skip only actual obstacles

continue

# Calculate movement cost

movement_cost = 1.4 if abs(dx) + abs(dy) == 2 else 1.0

# Combine distance and safety costs

# Higher cell_cost (closer to obstacles) makes the path more expensive

# but never impossible

new_cost = cost_so_far[current] + (

self.PATH_WEIGHT * movement_cost +

(1 - self.PATH_WEIGHT) * cell_cost

)

if next_pos not in cost_so_far or new_cost < cost_so_far[next_pos]:

cost_so_far[next_pos] = new_cost

priority = new_cost + self.manhattan_distance(goal, next_pos)

frontier.put((priority, next_pos))

came_from[next_pos] = current

# Reconstruct path

if goal not in came_from:

return None, float('inf')

path = []

current = goal

total_cost = cost_so_far[goal]

while current is not None:

path.append(current)

current = came_from.get(current)

path.reverse()

return path, total_cost

  

def scan_callback(self, msg):

"""Process incoming laser scan data"""

# Reset grid to unknown

self.grid.fill(1.0)

angle = msg.angle_min

for r in msg.ranges:

if msg.range_min <= r <= msg.range_max:

# Get end point (obstacle)

end_y_idx, end_x_idx = self.polar_to_grid(r, angle)

# Get points along the beam

start = (0, self.WIDTH // 2) # Start at car position

end = (end_y_idx, end_x_idx)

points = self.get_line_points(start, end)

# Mark free space along the beam

for y_idx, x_idx in points:

if 0 <= y_idx < self.HEIGHT and 0 <= x_idx < self.WIDTH:

self.grid[y_idx, x_idx] = 0 # Free space

else:

# Stop marking if we go out of bounds

break

# Mark the endpoint as obstacle if it's within bounds

if 0 <= end_y_idx < self.HEIGHT and 0 <= end_x_idx < self.WIDTH:

self.grid[end_y_idx, end_x_idx] = 1 # Obstacle

angle += msg.angle_increment

# Apply Gaussian filter to propagate costs

self.apply_gaussian_filter()

  

self.find_least_cost_path()

  

def create_visualization_marker(self):

"""Create marker array for visualization"""

marker_array = MarkerArray()

# Create marker for grid cells

grid_marker = Marker()

grid_marker.header.frame_id = "ego_racecar/base_link"

grid_marker.header.stamp = self.get_clock().now().to_msg()

grid_marker.id = 0

grid_marker.type = Marker.POINTS

grid_marker.action = Marker.ADD

grid_marker.scale.x = self.RESOLUTION

grid_marker.scale.y = self.RESOLUTION

grid_marker.scale.z = 0.001

grid_marker.pose.orientation.w = 1.0

  

path_marker = Marker()

path_marker.header.frame_id = "ego_racecar/base_link"

path_marker.header.stamp = self.get_clock().now().to_msg()

path_marker.id = 1

path_marker.type = Marker.LINE_STRIP

path_marker.action = Marker.ADD

path_marker.scale.x = 0.03 # Line width

path_marker.color.b = 1.0 # Blue

path_marker.color.a = 1.0

path_marker.pose.orientation.w = 1.0

# Create points and colors for each cell

for y_idx in range(self.HEIGHT):

for x_idx in range(self.WIDTH):

# Convert grid coordinates to car frame coordinates

forward_dist = y_idx * self.RESOLUTION

lateral_pos = self.X_MIN + x_idx * self.RESOLUTION

point = Point()

point.x = forward_dist # Forward is X in car frame

point.y = lateral_pos # Lateral is Y in car frame

point.z = 0.0

grid_marker.points.append(point)

# Set color based on cell state

color = ColorRGBA()

cell_value = self.grid[y_idx, x_idx]

if cell_value == 1: # Obstacle

color.r = 1.0

color.g = 0.0

color.b = 0.0

color.a = 1.0

elif cell_value == -1: # Unknown

color.r = 0.5

color.g = 0.5

color.b = 0.5

color.a = 1.0

else: # Free space with propagated cost

color.r = float(min(1.0, cell_value)) # Red based on cost

color.g = float(max(0.0, 1.0 - cell_value)) # Green decreases as cost increases

color.b = 0.0

color.a = 1.0

grid_marker.colors.append(color)

for y_idx, x_idx in self.current_path:

point = Point()

point.x = y_idx * self.RESOLUTION

point.y = self.X_MIN + x_idx * self.RESOLUTION

point.z = 0.02 # Slightly above the grid

path_marker.points.append(point)

marker_array.markers.append(grid_marker)

marker_array.markers.append(path_marker)

  

return marker_array

  

def publish_visualization(self):

"""Publish visualization markers"""

marker_array = self.create_visualization_marker()

self.marker_pub.publish(marker_array)

  

def main(args=None):

rclpy.init(args=args)

node = CostMapVisualization()

rclpy.spin(node)

node.destroy_node()

rclpy.shutdown()

  

if __name__ == '__main__':

main()
```

```
#!/usr/bin/env python3

import rclpy

from rclpy.node import Node

from visualization_msgs.msg import Marker, MarkerArray

from geometry_msgs.msg import Point

from std_msgs.msg import ColorRGBA

from sensor_msgs.msg import LaserScan

import numpy as np

from tf_transformations import euler_from_quaternion

from scipy.ndimage import gaussian_filter

from queue import PriorityQueue

from ackermann_msgs.msg import AckermannDriveStamped

  
  

class CostMapVisualization(Node):

def __init__(self):

super().__init__('cost_map_visualization')

# Grid parameters

self.RESOLUTION = 0.05 # 1cm per cell

self.WIDTH = 20 # 1m lateral width (100 cells)

self.HEIGHT = 40 # 2m forward length (200 cells)

self.X_MIN = -0.5 # 50cm to the left

self.X_MAX = 0.5 # 50cm to the right

self.Y_MIN = 0.0 # Starting at car's position

self.Y_MAX = 2.0 # 2m forward

  

# Cost propagation parameters

self.GAUSSIAN_SIGMA = 3.0 # Controls the spread of the obstacle cost

self.MIN_CLEARANCE = 4 # Minimum clearance in cells

  

# Path finding parameters

self.PATH_WEIGHT = 0.3 # Weight between cost (0) and distance (1)

self.OBSTACLE_COST_FACTOR = 10.0 # Multiplier for obstacle proximity cost

self.current_path = [] # Store the current path

  

# Initialize grid with unknown values (-1)

self.grid = np.full((self.HEIGHT, self.WIDTH), 1.0)

# Publishers

self.marker_pub = self.create_publisher(MarkerArray, '/cost_map_visualization', 10)

self.drive_pub = self.create_publisher(AckermannDriveStamped, '/drive', 10)

# Subscribers

self.scan_sub = self.create_subscription(

LaserScan,

'/scan',

self.scan_callback,

10)

# Timer for publishing visualization

self.create_timer(0.1, self.publish_visualization)

self.get_logger().info("CostMapVisualization node initialized")

  

def polar_to_grid(self, r, theta):

"""Convert polar coordinates (r, theta) to grid coordinates"""

# Convert to Cartesian (in car frame)

x = r * np.cos(theta) # Forward

y = r * np.sin(theta) # Left

# Convert to grid coordinates (allow out-of-bound indices)

y_idx = int(x / self.RESOLUTION) # Forward index

x_idx = int((y + self.X_MAX) / self.RESOLUTION) # Lateral index

return y_idx, x_idx

  

def get_line_points(self, start, end):

"""Get points along a line using Bresenham's algorithm"""

points = []

x0, y0 = start

x1, y1 = end

dx = abs(x1 - x0)

dy = abs(y1 - y0)

x, y = x0, y0

sx = 1 if x0 < x1 else -1

sy = 1 if y0 < y1 else -1

err = dx - dy

while True:

points.append((x, y))

if x == x1 and y == y1:

break

e2 = 2 * err

if e2 > -dy:

err -= dy

x += sx

if e2 < dx:

err += dx

y += sy

return points

  

def apply_gaussian_filter(self):

"""Apply Gaussian filter with stronger obstacle avoidance"""

# Create obstacle grid

obstacle_grid = np.where(self.grid == 1, 1.0, 0.0)

# Apply Gaussian filter for cost propagation

propagated_grid = gaussian_filter(obstacle_grid, sigma=self.GAUSSIAN_SIGMA)

# Enhance costs near obstacles

propagated_grid = propagated_grid * self.OBSTACLE_COST_FACTOR

# Apply minimum clearance

distance_to_obstacles = np.zeros_like(self.grid)

obstacle_positions = np.where(self.grid == 1)

for i in range(self.HEIGHT):

for j in range(self.WIDTH):

if self.grid[i, j] != 1: # Not an obstacle

distances = np.sqrt((i - obstacle_positions[0])**2 +

(j - obstacle_positions[1])**2)

distance_to_obstacles[i, j] = np.min(distances) if len(distances) > 0 else np.inf

# Mark areas too close to obstacles as high cost

close_to_obstacle = distance_to_obstacles < self.MIN_CLEARANCE

propagated_grid[close_to_obstacle] = 1.0

# Combine with original grid

self.grid = np.where(self.grid == 1, 1.0, propagated_grid)

  

def manhattan_distance(self, p1, p2):

"""Manhattan distance heuristic for A*"""

return abs(p1[0] - p2[0]) + abs(p1[1] - p2[1])

  

def find_least_cost_path(self):

"""Find least cost path using A* algorithm"""

start = (0, self.WIDTH // 2)

best_end = None

best_path = None

min_total_cost = float('inf')

# Try all possible end points at max height

for end_x in range(self.WIDTH):

end = (self.HEIGHT-1, end_x)

path, total_cost = self._a_star_search(start, end)

if path and total_cost < min_total_cost:

min_total_cost = total_cost

best_path = path

best_end = end

self.current_path = best_path if best_path else []

return self.current_path

  

def _a_star_search(self, start, goal):

"""Modified A* search with enhanced safety"""

frontier = PriorityQueue()

frontier.put((0, start))

came_from = {start: None}

cost_so_far = {start: 0}

while not frontier.empty():

current = frontier.get()[1]

if current == goal:

break

# Check 8-connected neighbors

for dx, dy in [(0,1), (1,0), (0,-1), (-1,0), (1,1), (1,-1), (-1,1), (-1,-1)]:

next_pos = (current[0] + dx, current[1] + dy)

# Check bounds

if not (0 <= next_pos[0] < self.HEIGHT and 0 <= next_pos[1] < self.WIDTH):

continue

# Get cell cost

cell_cost = float(self.grid[next_pos])

# Skip high-cost areas (obstacles and their immediate vicinity)

if cell_cost > 0.7: # Lowered threshold for greater safety

continue

# Calculate movement cost

movement_cost = 1.4 if abs(dx) + abs(dy) == 2 else 1.0

# Enhanced cost calculation

proximity_cost = cell_cost * self.OBSTACLE_COST_FACTOR

new_cost = cost_so_far[current] + (

self.PATH_WEIGHT * movement_cost +

(1 - self.PATH_WEIGHT) * proximity_cost

)

if next_pos not in cost_so_far or new_cost < cost_so_far[next_pos]:

cost_so_far[next_pos] = new_cost

# Modified heuristic to prefer paths away from obstacles

heuristic = self.manhattan_distance(goal, next_pos)

priority = new_cost + heuristic

frontier.put((priority, next_pos))

came_from[next_pos] = current

# Reconstruct path

if goal not in came_from:

return None, float('inf')

path = []

current = goal

total_cost = cost_so_far[goal]

while current is not None:

path.append(current)

current = came_from.get(current)

path.reverse()

return path, total_cost

  

def scan_callback(self, msg):

"""Process incoming laser scan data"""

# Reset grid to unknown

self.grid.fill(1.0)

angle = msg.angle_min

for r in msg.ranges:

if msg.range_min <= r <= msg.range_max:

# Get end point (obstacle)

end_y_idx, end_x_idx = self.polar_to_grid(r, angle)

# Get points along the beam

start = (0, self.WIDTH // 2) # Start at car position

end = (end_y_idx, end_x_idx)

points = self.get_line_points(start, end)

# Mark free space along the beam

for y_idx, x_idx in points:

if 0 <= y_idx < self.HEIGHT and 0 <= x_idx < self.WIDTH:

self.grid[y_idx, x_idx] = 0 # Free space

else:

# Stop marking if we go out of bounds

break

# Mark the endpoint as obstacle if it's within bounds

if 0 <= end_y_idx < self.HEIGHT and 0 <= end_x_idx < self.WIDTH:

self.grid[end_y_idx, end_x_idx] = 1 # Obstacle

angle += msg.angle_increment

# Apply Gaussian filter to propagate costs

self.apply_gaussian_filter()

  

self.find_least_cost_path()

  

last_point = self.find_last_point()

steering_angle, speed = self.compute_drive_command(last_point)

if steering_angle is not None and speed is not None:

self.publish_drive_command(steering_angle, speed)

  
  

def create_visualization_marker(self):

"""Create marker array for visualization"""

marker_array = MarkerArray()

# Create marker for grid cells

grid_marker = Marker()

grid_marker.header.frame_id = "ego_racecar/base_link"

grid_marker.header.stamp = self.get_clock().now().to_msg()

grid_marker.id = 0

grid_marker.type = Marker.POINTS

grid_marker.action = Marker.ADD

grid_marker.scale.x = self.RESOLUTION

grid_marker.scale.y = self.RESOLUTION

grid_marker.scale.z = 0.001

grid_marker.pose.orientation.w = 1.0

  

path_marker = Marker()

path_marker.header.frame_id = "ego_racecar/base_link"

path_marker.header.stamp = self.get_clock().now().to_msg()

path_marker.id = 1

path_marker.type = Marker.LINE_STRIP

path_marker.action = Marker.ADD

path_marker.scale.x = 0.03 # Line width

path_marker.color.b = 1.0 # Blue

path_marker.color.a = 1.0

path_marker.pose.orientation.w = 1.0

# Create points and colors for each cell

for y_idx in range(self.HEIGHT):

for x_idx in range(self.WIDTH):

# Convert grid coordinates to car frame coordinates

forward_dist = y_idx * self.RESOLUTION

lateral_pos = self.X_MIN + x_idx * self.RESOLUTION

point = Point()

point.x = forward_dist # Forward is X in car frame

point.y = lateral_pos # Lateral is Y in car frame

point.z = 0.0

grid_marker.points.append(point)

# Set color based on cell state

color = ColorRGBA()

cell_value = self.grid[y_idx, x_idx]

if cell_value == 1: # Obstacle

color.r = 1.0

color.g = 0.0

color.b = 0.0

color.a = 1.0

elif cell_value == -1: # Unknown

color.r = 0.5

color.g = 0.5

color.b = 0.5

color.a = 1.0

else: # Free space with propagated cost

color.r = float(min(1.0, cell_value)) # Red based on cost

color.g = float(max(0.0, 1.0 - cell_value)) # Green decreases as cost increases

color.b = 0.0

color.a = 1.0

grid_marker.colors.append(color)

for y_idx, x_idx in self.current_path:

point = Point()

point.x = y_idx * self.RESOLUTION

point.y = self.X_MIN + x_idx * self.RESOLUTION

point.z = 0.02 # Slightly above the grid

path_marker.points.append(point)

marker_array.markers.append(grid_marker)

marker_array.markers.append(path_marker)

  

return marker_array

  

def publish_visualization(self):

"""Publish visualization markers"""

marker_array = self.create_visualization_marker()

self.marker_pub.publish(marker_array)

  

def find_last_point(self):

"""Find the last point in the path."""

if not self.current_path:

return None

return self.current_path[-1] # Last point in the path

  

def compute_drive_command(self, last_point):

"""Compute a simple drive command toward the last point."""

if last_point is None:

return None, None

  

# Convert last point to vehicle frame coordinates

forward_dist = last_point[0] * self.RESOLUTION

lateral_pos = self.X_MIN + last_point[1] * self.RESOLUTION

  

# Calculate desired steering angle

steering_angle = np.arctan2(lateral_pos, forward_dist)

speed = 1.0 # Constant speed for simplicity

  

return steering_angle, speed

  

def publish_drive_command(self, steering_angle, speed):

"""Publish the drive command."""

from ackermann_msgs.msg import AckermannDriveStamped

  

drive_msg = AckermannDriveStamped()

drive_msg.drive.steering_angle = steering_angle

drive_msg.drive.speed = speed

self.drive_pub.publish(drive_msg)

  

def main(args=None):

rclpy.init(args=args)

node = CostMapVisualization()

rclpy.spin(node)

node.destroy_node()

rclpy.shutdown()

  

if __name__ == '__main__':

main()
```


KM's Ran:

Dec 19: 3.69 km
Dec 22: 3.05 km
