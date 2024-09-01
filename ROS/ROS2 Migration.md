Tags: [[ROS2]], [[ROS]]

When transitioning a workspace from ROS Noetic to ROS2 Humble, several changes need to be made, particularly concerning launch files, node handles, message passing, and other ROS-related functionalities. Here's a detailed breakdown of the changes typically required:

### 1. Launch Files

- **XML to Python**: ROS Noetic uses XML for launch files (`.launch`), whereas ROS2 uses Python-based launch files (`.py`). You'll need to rewrite your launch files in Python.
- **Node Declaration**: In ROS2, you need to declare nodes using the `Node` class, providing parameters like `package`, `executable`, and `name`.
- **Parameter Handling**: Parameters are handled differently in ROS2. You'll need to use the `DeclareLaunchArgument` and `Parameter` classes to manage parameters.

### 2. Imports
* Instead of `#include <ros/ros.h>`, use `#include "rclcpp/rclcpp.hpp"`
* `#include <std_msgs/String.h>` becomes `#include "std_msgs/msg/string.hpp"`
* `#include <sensor_msgs/Image.h>` becomes `#include "sensor_msgs/msg/image.hpp"`
* `#include <Eigen/Eigen>` becomes `#include <eigen3/Eigen/Eigen>`
* `#include "sensor_msgs/image_encodings.h"` becomes `#include "sensor_msgs/image_encodings.hpp"`
* there's a lot more...
### 2. Node Handles

- **Node Initialization**: Instead of `ros::NodeHandle`, ROS2 uses `rclcpp::Node`. You'll need to refactor your code to use `rclcpp::Node::SharedPtr` for node management.
- **Parameter Services**: In ROS2, parameters are managed using services, so parameter-related code (`getParam`, `setParam`) will need to be adjusted.

### 3. Publishers and Subscribers

- **rclcpp::Publisher and rclcpp::Subscription**: Replace `ros::Publisher` and `ros::Subscriber` with `rclcpp::Publisher` and `rclcpp::Subscription` respectively.
- **Callback Functions**: Callback functions remain similar, but you might need to adjust them to fit ROS2's API.

### 4. Timers

- **rclcpp::TimerBase**: Replace `ros::Timer` with `rclcpp::TimerBase` for periodic callbacks.
- **Wall Timer**: In ROS2, you use `create_wall_timer` instead of `createTimer`.

### 5. Parameters

- **Parameter Declaration**: In ROS2, parameters must be declared before use. Use the `declare_parameter` method in your node's constructor.
- **Parameter Retrieval**: Use `get_parameter` to retrieve parameters instead of `getParam`.

### 6. Message Passing

- **Differences in Message API**: The API for message creation and passing is similar but might have some differences in how messages are instantiated or passed between nodes.

### 7. Services and Actions

- **Service Definitions**: Services and actions in ROS2 have a different format and initialization. You'll need to adapt your service server and client code accordingly.

### 8. CMakeLists.txt and package.xml

- **Dependencies**: Update dependencies in `CMakeLists.txt` and `package.xml` to use ROS2 equivalents. For example, replace `find_package(catkin REQUIRED COMPONENTS ...)` with `find_package(ament_cmake REQUIRED COMPONENTS ...)`.
- **Build System**: Switch from `catkin_make` or `catkin build` to `colcon build`.

### 9. File System Differences

- **Directory Structure**: The directory structure in ROS2 might differ slightly, especially in terms of where build files and logs are stored.

### 10. ROS2 Specific Features

- **QoS (Quality of Service)**: ROS2 introduces QoS settings for publishers and subscribers, allowing more fine-grained control over message passing.
- **Lifecycle Nodes**: Consider using lifecycle nodes if you need more control over node states (e.g., configuring, activating, deactivating).