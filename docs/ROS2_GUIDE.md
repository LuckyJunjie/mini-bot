# Mini-Bot ROS2 开发指南

## 一、环境搭建

### 1.1 安装 Ubuntu 22.04 + ROS2 Humble

```bash
# 安装 Ubuntu Server 22.04
# 参考: https://ubuntu.com/download/raspberry-pi

# 设置 locale
sudo locale-gen en_US.UTF-8
sudo update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8
export LANG=en_US.UTF-8

# 添加 ROS2 GPG
sudo apt install curl gnupg lsb-release
sudo curl -ssl https://raw.githubusercontent.com/ros/rosdistro/master/ros.key | sudo gpg --dearmor -o /usr/share/keyrings/ros-archive-keyring.gpg

# 添加 ROS2 源
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] http://packages.ros.org/ros2/ubuntu $(source /etc/os-release && echo $UBUNTU_CODENAME) main" | sudo tee /etc/apt/sources.list.d/ros2.list > /dev/null

# 安装 ROS2 Humble
sudo apt update
sudo apt install ros-humble-ros-base
sudo apt install ros-humble-robot-localization ros-humble-robot-navigation

# 安装 Gazebo
sudo apt install ros-humble-gazebo-ros-pkgs
```

### 1.2 创建工作空间

```bash
mkdir -p ~/mini_bot_ws/src
cd ~/mini_bot_ws
source /opt/ros/humble/setup.bash
colcon build
```

## 二、创建 ROS2 包

### 2.1 包结构

```
mini_bot/
├── mini_bot_bringup/      # 启动文件
├── mini_bot_control/      # 控制器
├── mini_bot_description/  # URDF/SRDF
├── mini_bot_navigation/   # 导航
└── mini_bot_ui/           # 屏幕UI
```

### 2.2 创建底盘控制节点

```python
# mini_bot_control/mini_bot_control/chassis_node.py
import rclpy
from rclpy.node import Node
from geometry_msgs.msg import Twist
from std_msgs.msg import Float32MultiArray

class ChassisNode(Node):
    def __init__(self):
        super().__init__('chassis_driver')
        self.subscription = self.create_subscription(
            Twist,
            'cmd_vel',
            self.cmd_vel_callback,
            10)
        self.publisher = self.create_publisher(
            Float32MultiArray,
            'motor_targets',
            10)
        
    def cmd_vel_callback(self, msg):
        # 差速运动学
        left_speed = msg.linear.x - msg.angular.z * 0.1
        right_speed = msg.linear.x + msg.angular.z * 0.1
        
        # 发送到 ESP32
        motor_msg = Float32MultiArray()
        motor_msg.data = [left_speed, right_speed]
        self.publisher.publish(motor_msg)

def main(args=None):
    rclpy.init(args=args)
    node = ChassisNode()
    rclpy.spin(node)
    node.destroy_node()
    rclpy.shutdown()
```

## 三、URDF 建模

```xml
<!-- mini_bot_description/urdf/mini_bot.urdf.xacro -->
<?xml version="1.0"?>
<robot xmlns:xacro="http://www.ros.org/ontology/xacro/v1.0">
  
  <xacro:property name="PI" value="3.1415926535897931"/>
  <xacro:property name="wheel_radius" value="0.05"/>
  <xacro:property name="wheel_base" value="0.2"/>
  
  <!-- Base Link -->
  <link name="base_link">
    <visual>
      <geometry>
        <cylinder length="0.08" radius="0.15"/>
      </geometry>
      <material name="blue"/>
    </visual>
    <collision>
      <geometry>
        <cylinder length="0.08" radius="0.15"/>
      </geometry>
    </collision>
  </link>
  
  <!-- Left Wheel -->
  <link name="left_wheel">
    <visual>
      <geometry>
        <cylinder length="0.02" radius="${wheel_radius}"/>
      </geometry>
      <material name="black"/>
    </visual>
  </link>
  
  <joint name="left_wheel_joint" type="continuous">
    <parent link="base_link"/>
    <child link="left_wheel"/>
    <origin xyz="0 0.1 0" rpy="${PI/2} 0 0"/>
    <axis xyz="0 0 1"/>
  </joint>
  
  <!-- Right Wheel -->
  <link name="right_wheel">
    <visual>
      <geometry>
        <cylinder length="0.02" radius="${wheel_radius}"/>
      </geometry>
      <material name="black"/>
    </visual>
  </link>
  
  <joint name="right_wheel_joint" type="continuous">
    <parent link="base_link"/>
    <child link="right_wheel"/>
    <origin xyz="0 -0.1 0" rpy="${PI/2} 0 0"/>
    <axis xyz="0 0 1"/>
  </joint>
  
</robot>
```

## 四、导航集成

```bash
# 安装 Nav2
sudo apt install ros-humble-navigation ros-humble-slam-toolbox

# 启动导航
ros2 launch mini_bot_navigation navigation.launch.py
```

## 五、常用命令

```bash
# 查看所有 Topic
ros2 topic list

# 查看 /cmd_vel
ros2 topic echo /cmd_vel

# 手动发送控制命令
ros2 topic pub /cmd_vel geometry_msgs/msg/Twist "{linear: {x: 0.5, y: 0, z: 0}, angular: {x: 0, y: 0, z: 0}}"

# 查看节点图
rqt_graph

# 启动 Rviz2
rviz2
```
