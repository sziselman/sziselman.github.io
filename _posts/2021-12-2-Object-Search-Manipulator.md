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
This project implements Greedy Search, perception, and manipulation in order to retrieve objects that may not be visible or within reach due to the presence of other objects in the scene. The goal is to search for an order of objects such that when they are removed, they expose the largest amount of occluded volume in the smallest amount of time.

## Building the Scene
The volume of the scene can be described by a [frustum](https://en.wikipedia.org/wiki/Viewing_frustum#/media/File:ViewFrustum.svg),
<center>
    <img src="assets/images/posts/obj_search/frustum.png" alt="frustum" style="width:400px;"/>
</center>
Within each scene, there exists objects. For the sake of simplicity, this project utilizes 0.1 x 0.1 x 0.1 m cubes as objects. The volume of occluded space must be calculated for each object within the scene. This is done by calculating volume of the frustum created by the front plane of the object and the shadow created by the object on the rear plane.

Each scene also has a target object with unknown pose and known geometry. The dimensions of the target object will also be a 0.1 x 0.1 x 0.1 m cube.

## Motion Planning
For each object, a trajectory must be generated such that the object is picked up and removed from the scene in order to expose more unexplored space. In order to simplify the trajectory and ensure that the spot at which the object is placed does not interfere with another object, the object is placed at a location rotated by 90 degrees from its original location. Since the starting location of each object is unique, it's placement location will also be unique. Once a trajectory is generated, the execution time is saved and then used to solve for the utility of the object.

## Greedy Search
The utility of an object <img src="https://latex.codecogs.com/gif.latex? A" /> is defined as the volume of occluded space due to that object <img src="https://latex.codecogs.com/gif.latex? V_A" /> divided by the time that it takes to execute a trajectory to remove that object <img src="https://latex.codecogs.com/gif.latex? T_A" />. The utility can be described as,
<center>
    <img src="https://latex.codecogs.com/gif.latex? U(A)=\frac{V_A}{T_A}" /> 
</center>

Greedy is an search algorithm that utilizes a heuristic in order to find a locally optimal solution at each stage in the search process. The heuristic of this search is the utility defined above. Once the utility of each object is calculated, Greedy Search is implemented to find an arrangement. An arrangement is defined as the sequence of objects to be removed from the scene.

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

## Future Development
There are several aspects to this project that can be further developed for future work. First, incorporating a sensor to autonomously determine the shape, dimensions, and pose of an object. Second, developing a better method of calculating the occluded volume due to an object within the scene. This can be done by using the [Computational Geometry Algorithms Library](https://www.cgal.org/).