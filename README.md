# zk_robot_ros

ROS 2 integration workspace for robot simulation, perception, and control.

## Goals

- Control the robot through ROS 2.
- Develop and validate grasping workflows in Gazebo.
- Integrate the existing CAN driver and robot control code.
- Connect the head and wrist camera streams to perception and manipulation nodes.

## Related projects

- `gazebo_bot`: Gazebo simulation, robot models, camera sensors, and virtual CAN integration.
- `zk_robot`: Existing C++ robot and CAN driver implementation.
- `robot_station`: Robot control user interface.

Machine-specific access notes belong in the local `AGENT.md` file and are intentionally excluded from Git.
