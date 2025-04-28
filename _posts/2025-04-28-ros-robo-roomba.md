---
title: "ROS2 Project: Drawing USI logo with turtle"
tags:
  - project
  - ros2
  - robotics
values:
  - layout: single
  - author_profile: true
  - read_time: true
  - share: true
  - related: true
---

## Project Goal
This is a ROS2 project, completed for the USI 2025 robotics lecture. There were 4 tasks to implement:
1. Task: Move the robot in an eight figure pattern
2. Task: The robot has to move toward a wall and then turn to face it
3. Task: The robot has to move toward a wall then face away from it and move 2 meters
4. Task (Extra): Make the Robot move like a Roomba through the room and avoid obstacles

## Implementation
### Task 1: Move the robot in an eight figure pattern
Implementing the open-loop controller was quite straightforward, I made the robot turn left for a given time, then turn right for a given time. The drawback of the method is that the ROS and SIM time diverge over time, so that the figure-eights slowly moves around. In hindesight this should be possible to syncronize with a clock topic.

### Task 2: The robot has to move toward a wall and then turn to face it
Enabling the robot to face the wall was more difficult, I divided the procedure into multiple states: 
1. APPROACHING: The robot drives forward until one of the two front tof sensors detects a range below 20cm. 
2. TURNING: In order to face the wall, the two front sensors have to measure the same distance. Therefore angular velocity is based on this diffrence, which also slows down the turning when reaching the optimal rotation.
3. IDLE: The robot is finished


### Task 3: The robot has to move toward a wall then face away from it and move 2 meters
For this task I expanded on task 2.
1. APPROACHING: stays the same
2. TURNING: In order to face away from the wall, I first let the robot turn until one of the rear sensors detected the wall. Then I used the same procedure as in the previous task, just with the rear sensors.
3. MEASURING: I used basic geometry formulars to use calculate the distanc to the wall by considering the sensors measured distance and their mounting angle of 20 degrees.
4. FORWARD: Then the robot moves forward used the data from the odometry + measured distance to determine when 2 meters are reached.
3. IDLE: The robot is finished

![Wall Avoidance](/assets/gifs/Wall_Avoidance.GIF)

### Extra Task: Make the Robot move like a Roomba through the room and avoid obstacles
For making the Roomba application, I basically reduced task 3 by making some adjustments.
1. APPROACHING: since the room contains small obstacles they can fit into the blind spot of the front distance sensors. Therefore I let the turn left and right while driving forward to scan the blind spot. In addition to this alternating pattern, I let the robot move in an arc to encourage exploratory behaviour.
2. TURNING: To avoid getting the robot stuck bouncing between two parallel walls, I made it turn on the spot until one of the rear sensors detected an obstacle. In addition I introduced an alternating pattern to its rotation.
3. Back to step 1.

Before I came up with the wiggle pattern, I detected smaller objects, by letting the robot collide and then use the IMU’s acceleration sensor to detect abrupt acceleration changes that indicate collisions.

![Robo Roomba](/assets/gifs/Robo_Roomba.GIF)

The code is available on [GitHub](https://github.com/s6limall/robo_roomba)