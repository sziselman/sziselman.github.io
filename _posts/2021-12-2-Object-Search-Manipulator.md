---
layout: post
title:  'Object Search Manipulator'
tags: [SearchAlgorithms, Manipulation]
featured_image_thumbnail:
featured_image: assets/images/posts/obj_search/search.gif
featured: true
hidden: true
author: Sarah Ziselman
---

This graduate-level independent project follows the [Object search by manipulation](https://personalrobotics.cs.washington.edu/publications/dogar2013objsearch.pdf) paper written by Mehmet R. Dogar, Michael C. Koval, Abhijeet Tallavajhula, and Siddhartha S. Srinivasa. This project utilizes ROS Noetic, C++ programming, MoveIt! open-source motion planner, and the HDT Adroit Manipulator Arm. The code for this project can be found on Github [here](https://github.com/sziselman/Object-Search-Manipulation-Robot).

## Objective
This project implements Greedy Search, perception, and manipulation in order to retrieve objects that may not be visible or within reach due to the presence of other objects in the scene. The goal is to search for an order of objects such that when they are removed, they expose the largest amount of occluded volume in the smallest amount of time. The general frame-work of the project was built from scratch except for the motion-planner, which utilizes the MoveIt! open-source framework.

## Hardware
This project utilizes the [HDT Adroit Manipulator Arm](http://www.hdtglobal.com/series/adroit-manipulator-arm/), which is a 7 degree of freedom robot manipulator.

<center>
    <img src="assets/images/posts/obj_search/robot.jpg" alt="robot" style="width:400px;"/>
</center>

## Building the Scene
A scene is defined as the the known static world that contains a movable objects with known geometry and poses. The volume of the scene can be described by a [frustum](https://en.wikipedia.org/wiki/Viewing_frustum#/media/File:ViewFrustum.svg),
<center>
    <img src="assets/images/posts/obj_search/frustum.png" alt="frustum" style="width:400px;"/>
</center>

For the sake of simplicity, every object in the scene is a rectangular prism with uniform orientation such that the front-facing plane is parallel with the nearest plane of the scene. For each object within the scene, the volume of occluded space must be calculated for each object within the scene. This is done by calculating volume of the frustum created by the front-facing plane of the object and the shadow created by the object on the farthest plane of the scene.

Each object is defined by creating a custom `Block.msg`, which contains information about the block such as the pose, dimensions, id, utility (to be defined later), and target or not.

Each scene also has a target object with unknown pose and known geometry.

## Motion Planning
The `manipulator_control` node is responsible for implementing the MoveIt! motion planner on the Adroit Manipulator Arm. Trajectories are generated using a series of pose goals, cartesian paths, and multi-joint control. This node is responsible for returning the execution time of removing an object from the scene and actually executing the controls to remove an object from the scene.

For each object, a trajectory must be generated such that the object is picked up and removed from the scene in order to expose more unexplored space. In order to simplify the trajectory and ensure that the spot at which the object is placed does not interfere with another object, the object is placed at the same "drop-off" location and removed from the scene.

Once a trajectory is generated, the execution time is saved and then used to solve for the utility of the object.

## Greedy Search
The utility of an object <img src="https://latex.codecogs.com/gif.latex? A" /> is defined as the volume of occluded space due to that object <img src="https://latex.codecogs.com/gif.latex? V_A" /> divided by the time that it takes to execute a trajectory to remove that object <img src="https://latex.codecogs.com/gif.latex? T_A" />. The utility can be described as,
<center>
    <img src="https://latex.codecogs.com/gif.latex? U(A)=\frac{V_A}{T_A}" /> 
</center>

Greedy is an search algorithm that utilizes a heuristic in order to find a locally optimal solution at each stage in the search process. The heuristic of this search is the utility defined above. Once the utility of each object is calculated, Greedy Search is implemented to find an arrangement. An arrangement is defined as the sequence of objects to be removed from the scene.

In order to sort blocks in the software, the `greedy` node utilizes a `Block` class that overloads its `>` and `<` operators to compare utility values. This allows the `sort` function to be used on a `std::vector<Block>` that stores all the visible blocks and their utilities.

## Implementation
The following four ROS nodes were created that were responsible for carrying out several functionalities. Their publishers, subscribers, and services are described below,
![nodes](assets/images/posts/obj_search/nodes.png)

The following command launches the project:
```
roslaunch greedy_search fake_search.launch
```
Upon launching the project, the `rviz` window will display the Adroit Manipulator Arm, the objects within the scene highlighted in pink, and the target object highlighted in green.

The following command launches the greedy search and removes the object with the highest utility (highlighted in yellow) from the scene:
```
rosservice call /start_search "{}"
```
Upon starting the search, the `object_handler` node will initialize all objects seen within the scene and keep track of them using a `std::map` to map the id of the block to the actual block. For each object within the scene, the `greedy_search` node will use the `scene_setup` node to get the occluded volume and the `manipulator_control` node to get the trajectory time. Once the utility is found, the `greedy_search` node sorts the blocks and returns an arrangement. It will then take the first block in the arrangement and remove it from the scene using the `manipulator_control` node. Once the object is removed from the scene, the `object_handler` will update the objects within the scene by erasing that block from the map. 

Once a cycle has been completed, the object that occludes the largest volume with the smallest removal time will have been removed. The greedy search can be reinitialized to remove the next object with the highest utility until the goal object has been found.

<center>
    <img src="assets/images/posts/obj_search/adroit.gif" alt="adroit"/>
</center>

## Future Development
There are several aspects to this project that can be further developed for future work. First, incorporating a sensor to autonomously determine the shape, dimensions, and pose of an object. Second, developing a better method of calculating the occluded volume due to an object within the scene. This can be done by using the [Computational Geometry Algorithms Library](https://www.cgal.org/).