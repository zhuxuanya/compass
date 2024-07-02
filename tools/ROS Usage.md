# ROS Usage

## Initialization

Start by setting up the environment and running the ros services:
```bash
source /opt/ros/noetic/setup.bash
roscore
```

**Note:** Open another terminal for the following tasks after starting `roscore`.

## Visualization with rviz
To start rviz, use:
```bash
rviz
```
To load a specific rviz configuration, use the `-d` option:
```bash
rviz -d /path/to/config/file.rviz
```

## Work with rosbag
### Play rosbag files
To continuously play a bag file:
```bash
rosbag play --loop /path/to/bag/file.bag
```
### Inspect rosbag files
To check information about a bag file:
```bash
rosbag info /path/to/bag/file.bag 
```
### Record rosbag files
To record all topics with specified buffer size and duration:
```bash
rosbag record -a -b 9086 --duration 30
```
- `-a`: Record all topics
- `-b 9086`: Set the buffer size
- `--duration 30`: Record for 30 seconds

## Topic Management
### List topics
To list all active topics:
```bash
rostopic list
```
### Display topic information
To print messages from a specific topic:
```bash
rostopic echo /topic_name
```
### Monitor topic frequency
To display the publishing rate of a topic:
```bash
rostopic hz /topic_name
```

## Diagnostics
To check the running environment and configuration for potential issues:
```bash
roswtf
```
