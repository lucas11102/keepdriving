# keepdriving (ROS 2 Humble)

Local simulation workspace for A* + DWA autonomous driving.

## Scenario implemented
- Robot model: rectangular chassis approximated by radius (size target 0.15 m x 0.25 m)
- Obstacles: preset cones with diameter 0.20 m and height 0.30 m
- Localization: publishes ground-truth directly to `/odom_combined`
- Obstacle sensing: publishes only ground-truth cones inside configurable camera-like frustum (FOV + range)
- Mapping: occupancy grid + inflated costmap
- Planning: A* global path on costmap + DWA local trajectory tracking

## Topics
- `/sim/ground_truth_pose` (PoseStamped)
- `/sim/ground_truth_obstacles` (PoseArray)
- `/sim/visible_obstacles` (PoseArray)
- `/odom_combined` (Odometry)
- `/keepdriving/occupancy` (OccupancyGrid)
- `/keepdriving/costmap` (OccupancyGrid)
- `/keepdriving/global_path` (Path)
- `/keepdriving/local_trajectory` (Path)
- `/cmd_vel` (Twist)

## Build
```bash
cd /home/x/projects/SCR/keepdriving
source /opt/ros/humble/setup.bash
colcon build
```

## Run (recommended)
```bash
cd /home/x/projects/SCR/keepdriving
./run_keepdriving.sh
```

## Run (manual)
```bash
cd /home/x/projects/SCR/keepdriving
source /opt/ros/humble/setup.bash
source install/setup.bash
ros2 launch keepdriving sim_a_star_dwa.launch.py
```

## Optional quick checks
```bash
ros2 topic echo /odom_combined --once
ros2 topic echo /sim/visible_obstacles --once
ros2 topic echo /cmd_vel --once
```

## Mission behavior (this map)
After start at `P`, robot should:
1. Move to within 1.0 m of task release point (`TASK_POINT`).
2. Go to upper white ring and follow loop waypoints for one clockwise lap.
3. Return to `P` and stop.

In RViz2, use fixed frame `map` and add:
- Map: `/keepdriving/costmap`
- Path: `/keepdriving/global_path`
- Path: `/keepdriving/local_trajectory`
- Odometry: `/odom_combined`
