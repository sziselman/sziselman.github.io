---
layout: post
title: 'Extended Kalman Filter SLAM'
tags: [Navigation, EKF, SLAM]
featured_image_thumbnail:
featured_image: assets/images/posts/ekf_slam/ekf_slam.gif
# featured: true
# hidden: true
author: Sarah Ziselman

---

This project is from a graduate-level Sensing, Navigating, and Machine Learning for Robotics course taught at Northwestern University. It utilizes ROS Noetic, C++ programming, and the Turtlebot3 differential drive robot. The code for this project can be found on Github [here](https://github.com/sziselman/Shermbot-Navigation).

## Objective
Simultaneous Localization and Mapping is a method of building a map of an unknown world. This project implements **Extended Kalman Filter SLAM** on a differential drive robot made by ROBOTIS known as the Turtlebot. Built entirely from the bottom-up, this project covers a broad range of areas in robotic software development. The objective of this project is to build a simulated world and allow the Turtlebot to navigate freely (controlled by the user) in order to build a map of its surroundings.

## The Simulation
This project makes use of ROS's 3D visualizer, Rviz. Several tools were implemented to visualize landmarks, sensor data, robot trajectories, and more. In order to effectively represent the motion of the robot, C++ libraries were written to derive the forward 2D kinematics and calculate the odometry of the robot.

## Feature Detection
Feature detection is used to transform lidar scanner data into landmark measurements. This process consists of three parts: clustering points, circle classification, and circle-fitting regression algorithm. 

First, we solve an unsupervised learning problem of **clustering points**. Upon recieving range-bearing measurements, a distance threshold is used to group points into clusters.

Next, we classify whether a cluster of points is considered a **circle or not**. This approach comes from the paper [Fast line, arc/circle and leg detection from laser scan data in a player driver](https://ieeexplore.ieee.org/document/1570721) by J. Xavier et. al.

Lastly, a **circle-fitting regression algorithm** was used on each cluster of points classified as a circle. This algorithm was adapted from [Error Analysis for Circle Fitting Algorithms](https://projecteuclid.org/journals/electronic-journal-of-statistics/volume-3/issue-none/Error-analysis-for-circle-fitting-algorithms/10.1214/09-EJS419.full) by A. Al-Sharadqah and N. Chernov. Once implemented, the radius and coordinates of the center of the circle are returned.

## Extended Kalman Filter
**Extended Kalman Filter** is a powerful tool in localization and mapping since it handles non-linearities, which is most representative of the real world. This filter uses Taylor expansion to linearize about an estimate of the current state and covariance. The implementation is executed in three steps: initialization, prediction, and update.

Upon **initialization**, the system assumes that the robot is certain of its initial position and that it knows nothing of its surroundings. This means the state vector, which consists of the robot's configuration and the state of the map, is initialized to zero.

At each time step when a velocity twist message is received, the system makes a **prediction** about its state by using the robot's odometry. At this step, the uncertainty matrix is also propagated.

When a sensor measurement is received, it goes through a **data association** step by either initializing a new landmark or associating that sensor measurement with a previously seen landmark. This allows the system to compute an estimate from the measurement. Once the Kalman gain is computed, it is used to **update** the state vector by weigh the importance of the estimate from the model versus the estimate from the measurement.

## Future Development
There still lives a lot of room for further development in this project. Currently, the robot is controlled using the teleop_twist_keyboard, which is a ROS package that allows the user to input twist commands via computer keyboard. It would be interesting to develop methods to automate the robot's movement such that it can explore the maximal amount of its environment in the least amount of time.
