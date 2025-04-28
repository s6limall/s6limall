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

### Project Goal
This is a ROS2 project, completed for the USI 2025 robotics lecture. The assignment was to make a ROS2 turtlesim turtle draw the letters "USI" while simultaneously attacking other turtles that are messing up the "painting." I amp up the challenge by remaking the USI logo.

### Implementation
Implementing the logo was actually quite familiar, as it required the Canny edge detection algorithm, which had been covered in many of my lectures (although I ended up just using the cv2.Canny() method). In hindsight, I could have used my marching square implementation from my computational fabrication course.

In terms of ROS2, I set up four nodes: one for the turtlesim node, one for the provided movement node, a state handler node, and an enemy turtle node. First, I extended the movement node to no longer rotate and move at the same time to achieve precise lines. The state handler node used the points extracted from the USI photo to draw each letter in the WRITING state. Additionally, the node subscribed to both the drawing and enemy turtle positions. Once the Euclidean distance passed a specific threshold, the turtle would change into a ANGRY state and aim at the enemy's predicted position (simple sine-cosine calculations). Once the enemy turtle was close enough, the kill service was called (I attached a callback function that would spawn a new turtle once the kill service was completed). Then, the turtle would enter a RETURN state until it reached the last reached checkpoint and continue WRITING.

Here is are my results:

![Angry Turtle](/assets/gifs/Angry_Turtle.GIF)

The code is available on [GitHub](https://github.com/s6limall/usi_angry_turtle_project)