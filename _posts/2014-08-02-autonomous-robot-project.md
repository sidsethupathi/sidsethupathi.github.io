---
layout: post
title: A* search in an autonomous ball-seeking robot
description: "An robot using A* to navigate a course and pick up balls for GE 423 at UIUC"
modified: 
tags: [robotics, ai, uiuc, astar]
comments: true
published: false
---

The final project in UIUC's Mechatronics course ([GE4 423](#)), our class was tasked with programming a robotic car to navigate a course with randomly placed obstacles, pick up randomly placed balls of different colors, and then deposit them in the final goal area. There were five known way points that the robot car also had to hit in a certain order.
\\
\\
The mechanism used to pick up balls was similar across the class, with pretty much ever group doing a mechanical arms that swung open when the robot was close enough to the car. For navigating the course, most groups chose to do unintelligent "bug obstacle avoidance" algorithms, where the robot would avoid obstacles in a preprogrammed way. Our group, however, attempted to make the car more intelligent and implemented an A* search on the microcontroller. 

### Learning the layout

The first challenge we faced was that the robot had no knowledge of what the course layout looked like. A* (or any path planning algorithm) relies on knowing the layout in order to "plan a path". With out this knowledge base, the path would always be a direct line between the robot and the goal. We were not allowed to program the layout of the course into the robot's memory, so we had to rely on the LADAR sensors on the car. 
\\
\\
First, we started with a empty knowledge base. Since the challenge gave us 5 known waypoints that we had to hit, we used those intial points to make the car start to move. If the LADAR sensors detected walls, we would update our knowledge base on the fly and then run the search again. Since the size of our search space was not that large, we were able to run the algorithm without noticeable slowdowns. If the size or resolution of our search space increased, I'm not sure that this solution would have worked as well as it did.

### A* Search

A* search is considered an **informed** search algorithm. It is informed because as it expands nodes in the search space, it uses the knowledge of where the goal is to make its path. This knowledge is in the form of the heuristic function, or a cost of expanding the next node. Searches like BFS and DFS are **uninformed**, because as the expand the frontier, they do not use the knowledge of where the goal is when picking which node to expand.
\\
\\
The basic structure for the A* algorithm is this:
{% raw %}
    push the starting node onto a priority queue
    while the priority queue is not empty
		pop the top element off the priority queue

		if you aren't at the goal node
	    		for each neighboring node
	    			calculate the cost of traveling to each neighbor
	    			push neighbor onto the priority queue
	    	else
	    		you've reached the goal node and can stop searcing

{% endraw %}