# Project Report: TurtleBot3 Navigation with ROS 2 on Raspberry Pi

### 1. Introduction
This project explores the implementation of ROS 2 Foxy for autonomous navigation on the TurtleBot3 robot, using both simulation and a real TurtleBot. The main objectives are to set up and configure ROS 2 on a Raspberry Pi, develop a simulated navigation environment, and deploy real-world navigation. This setup enables the TurtleBot to autonomously navigate, map, and avoid obstacles, leveraging ROS 2's capabilities for robotic systems.

### 2. Objective
The key goals for this project phase are:
- Installing ROS 2 on the Raspberry Pi to support TurtleBot3.
- Setting up a simulated environment for testing navigation.
- Configuring SLAM (Simultaneous Localization and Mapping) and the Navigation 2 Stack for autonomous path planning.
- Preparing the setup for implementation on a physical TurtleBot.

### 3. Required Components
   - **Hardware**: 
     - Raspberry Pi 4 (or compatible version)
     - TurtleBot3 (model: Burger)
   - **Software**: 
     - Ubuntu 20.04 for ARM (Raspberry Pi OS)
     - ROS 2 Foxy
   - **Packages**: `turtlebot3`, `navigation2`, `slam-toolbox`, and additional ROS dependencies.

### 4. System Setup and ROS 2 Installation on Raspberry Pi
The setup begins with installing ROS 2 Foxy on a Raspberry Pi running Ubuntu 20.04. This configuration supports ARM architecture, making it compatible with the Raspberry Pi 4 hardware.

1. **Configuring the Raspberry Pi**: To prepare, the Raspberry Pi is updated, and the locale settings are configured to support ROS 2’s requirements.
   ```bash
   sudo locale-gen en_US en_US.UTF-8
   sudo update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8
   ```
   
2. **Installing ROS 2**: ROS 2 is installed by adding the ROS 2 repository, updating package lists, and installing the `ros-foxy-desktop` package.
   ```bash
   sudo apt update && sudo apt install curl gnupg2 -y
   curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.asc | sudo apt-key add -
   sudo sh -c 'echo "deb [arch=arm64] http://packages.ros.org/ros2/ubuntu $(lsb_release -cs) main" > /etc/apt/sources.list.d/ros2-latest.list'
   sudo apt update
   sudo apt install ros-foxy-desktop -y
   ```
   
3. **Environment Configuration**: ROS 2 environment variables are sourced and configured to load automatically.
   ```bash
   echo "source /opt/ros/foxy/setup.bash" >> ~/.bashrc
   source ~/.bashrc
   ```

### 5. Simulation Setup in Gazebo
To begin development in a safe, controlled environment, the TurtleBot3 is set up in the Gazebo simulation environment.

1. **TurtleBot3 Model Configuration**: The model is defined as `burger` (TurtleBot3 model) and saved to `~/.bashrc`:
   ```bash
   export TURTLEBOT3_MODEL=burger
   ```
2. **Simulation Launch**: Gazebo is launched with a predefined world file for TurtleBot3, creating a virtual environment where navigation and SLAM can be tested.
   ```bash
   ros2 launch turtlebot3_gazebo turtlebot3_world.launch.py
   ```

### 6. Navigation and Mapping with SLAM and Navigation 2
The TurtleBot3’s navigation is achieved through two main processes: SLAM for environment mapping and the Navigation 2 Stack for path planning and goal-setting.

1. **SLAM**: The SLAM process allows the TurtleBot3 to autonomously build a map of its environment. This functionality is implemented with the `slam-toolbox` package.
   ```bash
   ros2 launch turtlebot3_cartographer cartographer.launch.py use_sim_time:=true
   ```
   
2. **Navigation Stack**: The Navigation 2 Stack is responsible for autonomous navigation. It leverages pre-generated maps and path-planning algorithms to help the TurtleBot3 avoid obstacles and reach designated goals.
   ```bash
   ros2 launch turtlebot3_navigation2 navigation2.launch.py use_sim_time:=true
   ```

3. **Visualization in RViz**: RViz is used for visualization, allowing users to set goals and monitor navigation status. It opens in a separate terminal and connects to the TurtleBot3 for remote control.
   ```bash
   rviz2
   ```

### 7. Deployment on the Real TurtleBot
The final deployment step involves transferring the setup from the simulation to a real TurtleBot3. 

1. **Bringing Up TurtleBot3 Hardware**: The TurtleBot3 is brought online using the `bringup` launch file, which initializes hardware communication.
   ```bash
   ros2 launch turtlebot3_bringup robot.launch.py
   ```
2. **SLAM and Navigation in Real Environment**: SLAM and Navigation are activated to enable the TurtleBot3 to explore and navigate its physical surroundings. These processes are launched similarly to the simulation but interact directly with the TurtleBot3 hardware.

### 8. Summary of Terminal Use
The processes are distributed across multiple terminals for easier management and monitoring. For each critical step, a new terminal is opened as follows:

| Step | Command | Terminal |
|------|---------|----------|
| Simulation Launch | `ros2 launch turtlebot3_gazebo turtlebot3_world.launch.py` | Terminal 1 |
| SLAM | `ros2 launch turtlebot3_cartographer cartographer.launch.py use_sim_time:=true` | Terminal 2 |
| Navigation 2 | `ros2 launch turtlebot3_navigation2 navigation2.launch.py use_sim_time:=true` | Terminal 3 |
| Visualization (RViz) | `rviz2` | Terminal 4 |

### 9. Conclusion
The initial setup stages involve configuring ROS 2 on a Raspberry Pi and running TurtleBot3 in simulation. This approach provides a scalable development and testing environment for autonomous navigation. The setup will be further refined as the project progresses, with a focus on enhancing real-world deployment on the physical TurtleBot3.

--- 
