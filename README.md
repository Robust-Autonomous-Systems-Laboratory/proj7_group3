## Assignment Implementation: EE5531 Project 7
**Authors:** Anders Smitterberg, Jackson Newell


### Part 3 - Jackal Simulation

Below are the screenshots obtained for the Jackal simulation.

**Gazebo Simulation Screenshot of the Jackal in the Warehouse Environment**
![Gazebo Simulation of Jackal in the Warehouse Environment](figures/jackal_gazebo_sim.png)

**Jackal Navigating to the first set Waypoint**
![Jackal Navigating to the first Waypoint](figures/jackal_rviz_navwaypoint1.png)

**Jackal Navigating to the second set Waypoint**
![Jackal Navigating to the second Waypoint](figures/jackal_rviz_navwaypoint2.png)

**Image of the Entire Generated Map in Rviz2**
![The Entire Map of the Warehouse Environment](figures/jackal_rviz_map.png)

The differences between the setup of the Jackal and Turtlebot3 were how to configure the simulations. The turtlebot3 has a preset configuration that needs to be declared for the 'burger' model. This is a parameter that needs to be set in the terminal. The Jackal on the other hand has a 'robot.yaml' that describes the links, sensors, and other features on the Jackal robot. The model 'J100' is a preset model which is the Jackal model, but the Jackals can have plenty of different sensors mounted onto it. Therefore, this 'robot.yaml' file declares and describes how and where the sensors are mounted. Another difference was needing to set a parameter while saving the map. The parameter was just setting the topic to save the map from. That command is in the Clearpath documentation and it is below. The documentation is for the A300 model so the command is slightly altered for the J100 namespace. In addition, the save location is also changed.

```
ros2 run nav2_map_server map_saver_cli -f "map_jackal_sim/map_jackal" --ros-args -p map_subscribe_transient_local:=true -r __ns:=/j100_0000
```


