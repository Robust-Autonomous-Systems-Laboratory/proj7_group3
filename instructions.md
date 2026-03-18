# Project 7: Introduction to the ROS2 Navigation Stack

**EE5531 Intro to Robotics**

---

## Introduction

In this project you will get hands-on experience with the ROS2 Navigation Stack (Nav2) — the standard framework for autonomous mobile robot navigation. You will work through three progressively more realistic scenarios: simulated navigation with the TurtleBot3, real-world simultaneous localization and mapping (SLAM) in EERC 722, and simulated navigation with the Clearpath Jackal. Along the way you will build intuition for how a robot can localize itself in a known map, plan paths, and avoid obstacles.

---

## Learning Objectives

Upon completion of this project you will be able to:

1. Launch and operate the Nav2 navigation stack in simulation and on hardware
2. Teleoperate a robot to build a 2D occupancy grid map using SLAM
3. Use RViz2 to send navigation goals and visualize the robot's state
4. Save and interpret ROS2 map files (`.yaml` + `.pgm`)
5. Adapt a Nav2 workflow from one robot platform (TurtleBot3) to another (Jackal)

---

## Team Structure

This project is completed in **teams of two**. Both team members are expected to contribute to every part. Contributions are assessed by **individual commits** to your GitHub repository — each partner must have meaningful commits before the submission deadline.

---

## AI Use Policy

Any use of AI tools (ChatGPT, Claude, Copilot, etc.) **must be thoroughly documented** in your README. For each section where AI was used, note:
- Which tool was used
- What prompt or query was given
- How you verified or modified the output

Undisclosed AI use is an academic integrity violation.

---

## Background

### The Nav2 Stack

Nav2 is the ROS2 successor to the ROS1 `move_base` package. At a high level it provides:

- **Localization** — AMCL (particle filter) for position estimation in a known map, or SLAM Toolbox for simultaneous mapping and localization
- **Global planner** — computes a collision-free path from current pose to goal
- **Local planner / controller** — executes the path while reacting to dynamic obstacles
- **Behavior trees** — orchestrate the above components and handle recovery behaviors

### SLAM Toolbox

SLAM (Simultaneous Localization and Mapping) lets the robot build a map of an unknown environment while simultaneously tracking its own position within it. You will use `slam_toolbox` in *online synchronous* mode, driving the robot by hand while the map grows in RViz2.

### Map Files

When you save a map with `ros2 run nav2_map_server map_saver_cli`, you get two files:

| File | Contents |
|------|----------|
| `map.pgm` | Grayscale image — white = free, black = occupied, gray = unknown |
| `map.yaml` | Metadata: resolution (m/pixel), origin pose, occupancy thresholds |

Both files must be committed to your repository.

---

## Instructions

### Part 1 — TurtleBot3 Simulation with Nav2

Follow the official Nav2 **Getting Started** tutorial to drive the TurtleBot3 through a simulated environment using the full navigation stack.

**Tutorial:** https://docs.nav2.org/getting_started/index.html

**Steps:**

1. Launch the TurtleBot3 Gazebo world and Nav2:
   ```bash
   export TURTLEBOT3_MODEL=burger
   ros2 launch turtlebot3_gazebo turtlebot3_world.launch.py
   # In a second terminal:
   ros2 launch turtlebot3_navigation2 navigation2.launch.py use_sim_time:=True map:=<path_to_map>
   ```

2. Set the **2D Pose Estimate** in RViz2 to initialize AMCL.

3. Send at least **three navigation goals** using the **Nav2 Goal** tool in RViz2. Observe the global path, local costmap, and recovery behaviors.

**Required evidence:**
- Screenshot of RViz2 with the robot successfully navigating to a goal (costmaps and path visible)
- Screenshot of the terminal showing Nav2 reaching the goal (`Reached the goal!`)

---

### Part 2 — Real-World SLAM Mapping of EERC 722

Using the physical TurtleBot3 Burger in **EERC 722**, build a 2D occupancy grid map of the room by teleoperating the robot.

**Tutorial reference:** https://docs.nav2.org/tutorials/docs/navigation2_with_slam.html

**Steps:**

1. SSH into the TurtleBot3 and start the robot bringup:
   ```bash
   ros2 launch turtlebot3_bringup robot.launch.py
   ```

2. On your workstation, launch SLAM Toolbox and RViz2:
   ```bash
   export TURTLEBOT3_MODEL=burger
   ros2 launch turtlebot3_cartographer cartographer.launch.py
   ```

3. In a third terminal, start teleoperation:
   ```bash
   ros2 run turtlebot3_teleop teleop_keyboard
   ```

4. Drive the robot around the perimeter of EERC 722 and through any interior features until the map looks complete in RViz2. Drive slowly around walls to reduce noise.

5. Save the finished map:
   ```bash
   ros2 run nav2_map_server map_saver_cli -f ~/map_eerc722
   ```

6. Once you have a map, launch Nav2 with that map and demonstrate point-to-point navigation in the room by sending at least two goals from RViz2.

**Required evidence:**
- Screenshot of RViz2 showing the completed map of EERC 722 while still in SLAM mode
- The saved `map_eerc722.yaml` and `map_eerc722.pgm` files committed to your repository
- Screenshot of RViz2 showing the robot navigating to a goal in the real room

---

### Part 3 — Clearpath Jackal Simulation

Repeat the mapping and navigation workflow using the **Clearpath Jackal** in Gazebo. This introduces you to a different robot platform and its Nav2 integration.

**Tutorial:** https://docs.clearpathrobotics.com/docs/ros/tutorials/navigation_demos/overview

**Steps:**

1. Follow the Clearpath tutorial to launch the Jackal simulation and its Nav2 stack.

2. Use the teleoperation or waypoint-following demo to drive the Jackal and build a map of the simulated environment using SLAM.

3. Save the finished map:
   ```bash
   ros2 run nav2_map_server map_saver_cli -f ~/map_jackal_sim
   ```

4. Send at least two navigation goals in RViz2 and observe the Jackal navigate autonomously.

**Required evidence:**
- Screenshot of the Jackal simulation in Gazebo
- Screenshot of RViz2 showing the Jackal navigating to a goal with the map visible
- The saved `map_jackal_sim.yaml` and `map_jackal_sim.pgm` files committed to your repository

---

## Deliverables

Submit by pushing to your team's **class GitHub repository** before the deadline. Your repository README serves as the project report.

### Repository Structure

```
proj7-navigation/
├── README.md                    # Project report (see below)
├── maps/
│   ├── map_eerc722.yaml         # Real-world map metadata
│   ├── map_eerc722.pgm          # Real-world map image
│   ├── map_jackal_sim.yaml      # Jackal simulation map metadata
│   └── map_jackal_sim.pgm       # Jackal simulation map image
└── figures/                     # Screenshots embedded in README
    ├── part1_nav2_goal.png
    ├── part1_terminal.png
    ├── part2_slam_map.png
    ├── part2_nav_goal.png
    ├── part3_gazebo.png
    └── part3_nav_goal.png
```

### README Requirements

Your README must include all of the following sections. Graders will read the README from top to bottom — treat it as a complete lab report.

#### 1. Introduction and Setup (2 pts)
- Which ROS2 distribution and OS you used
- Any setup challenges and how you resolved them

#### 2. Part 1 — TurtleBot3 Simulation (4 pts)
- Embedded screenshots (RViz2 navigation view, terminal goal confirmation)
- Brief description of what you observed (path planning, costmaps, recovery behaviors)

#### 3. Part 2 — Real-World Mapping of EERC 722 (8 pts)
- Embedded screenshot of the completed SLAM map
- Description of your driving strategy and any mapping artifacts or challenges
- Embedded screenshot of RViz2 showing point-to-point navigation in the room
- The map YAML and PGM files must be present in `maps/`

#### 4. Part 3 — Jackal Simulation (5 pts)
- Embedded screenshots (Gazebo view, RViz2 navigation view)
- Brief comparison: what was different setting up Nav2 for the Jackal vs. TurtleBot3?
- The map YAML and PGM files must be present in `maps/`

#### 5. AI Use Documentation (included in overall score)
- List every section where AI tools were used and how (see AI Use Policy above)
- If no AI tools were used, state that explicitly

---

## Grading Rubric

| Component | Points |
|-----------|--------|
| **Part 1 — TurtleBot3 Simulation:** RViz2 screenshot with navigation goal and costmaps visible; terminal screenshot confirming goal reached | 4 |
| **Part 2 — Real-World SLAM:** Completed map of EERC 722 (screenshot + YAML/PGM files committed); screenshot of autonomous navigation in the real room | 8 |
| **Part 3 — Jackal Simulation:** Gazebo and RViz2 screenshots; map files committed; brief platform comparison | 5 |
| **Documentation:** README clearly structured, all screenshots labeled, setup/challenges described, AI use disclosed | 2 |
| **Repository hygiene:** Both team members have commits; commit messages are meaningful | 1 |
| **Total** | **20** |

---

## Tips

- **Drive slowly.** Fast teleoperation produces noisy, gappy maps. Make a slow pass around the perimeter first, then fill in interior features.
- **Overlap your passes.** SLAM closure works best when the robot revisits previously seen features — this lets it correct accumulated drift.
- **Check `map.yaml` resolution.** The default is 0.05 m/pixel. A larger room may need multiple passes to fill in at that resolution.
- **ROS_DOMAIN_ID.** In the lab, set `export ROS_DOMAIN_ID=<your team number>` on all machines to avoid picking up other teams' topics.
- **Save early, save often.** Run `map_saver_cli` periodically while mapping — if SLAM drifts badly you can restart from a recent save rather than from scratch.

---

## References

1. Nav2 Getting Started: https://docs.nav2.org/getting_started/index.html
2. Nav2 with SLAM Toolbox: https://docs.nav2.org/tutorials/docs/navigation2_with_slam.html
3. Clearpath Jackal Navigation Demo: https://docs.clearpathrobotics.com/docs/ros/tutorials/navigation_demos/overview
4. TurtleBot3 Manual: https://emanual.robotis.com/docs/en/platform/turtlebot3/
5. Macenski, S. et al. (2020). *The Marathon 2: A Navigation System*. IROS 2020. [[arXiv](https://arxiv.org/abs/2003.00368)]
