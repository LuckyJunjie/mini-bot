# Gazebo

## 一、背景

Gazebo 是 Open Source Robotics Foundation (OSRF，现为 Intrinsic) 开发的开源机器人仿真器。它是 ROS 的官方仿真工具，提供独立的物理仿真环境和丰富的机器人模型库。

Gazebo 使用 ODE (默认)、Bullet、PhysX、DART 等多种物理引擎，支持复杂的机器人动力学仿真。它与 ROS/ROS2 深度集成，是机器人研究领域最广泛使用的仿真工具之一。

## 二、目的

- **高保真仿真**：复杂动力学、接触力、多机器人仿真
- **ROS 导航测试**：SLAM、路径规划、避障算法验证
- **传感器仿真**：激光雷达、深度相机、IMU 噪声模型
- **硬件在环**：在真实硬件连接前进行 HIL 测试

## 三、官方地址

- 官网: https://gazebosim.org/
- ROS2 Gazebo: https://gazebosim.org/docs
- GitHub: https://github.com/gazebosim/gz-sim
- 安装: https://gazebosim.org/docs/fortress/install

## 四、实用教程

### 4.1 安装 Gazebo (Ubuntu 22.04)

```bash
# 方法1: ROS2 附带安装（推荐）
sudo apt update
sudo apt install ros-humble-gazebo-ros-pkgs

# 方法2: 独立安装 Gazebo Fortress (LTS)
sudo wget https://packages.osrfoundation.org/gazebo.key -O /usr/share/keyrings/pkgs-osrf-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/pkgs-osrf-archive-keyring.gpg] http://packages.osrfoundation.org/gazebo/ubuntu $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/gazebo.list > /dev/null
sudo apt update
sudo apt install gz-fortress

# 启动 Gazebo
gazebo
```

### 4.2 创建 Mini-Bot URDF 模型

```xml
<!-- mini_bot.urdf.xacro -->
<?xml version="1.0"?>
<robot xmlns:xacro="http://www.ros.org/xacro">
  
  <!-- 基础参数 -->
  <xacro:property name="PI" value="3.1415926535897931"/>
  <xacro:property name="wheel_radius" value="0.03"/>
  <xacro:property name="base_width" value="0.15"/>
  
  <!-- 宏定义: 轮子 -->
  <xacro:macro name="wheel" params="name x y">
    <link name="${name}_wheel">
      <visual>
        <geometry><cylinder radius="${wheel_radius}" length="0.02"/></geometry>
        <material name="black"/>
      </geometry>
      <collision>
        <geometry><cylinder radius="${wheel_radius}" length="0.02"/></geometry>
      </collision>
    </link>
    
    <joint name="${name}_wheel_joint" type="continuous">
      <parent link="base_link"/>
      <child link="${name}_wheel"/>
      <origin xyz="${x} ${y} 0" rpy="${PI/2} 0 0"/>
      <axis xyz="0 0 1"/>
    </joint>
  </xacro:macro>
  
  <!-- 底盘 -->
  <link name="base_link">
    <visual>
      <geometry><box size="0.2 0.15 0.05"/></geometry>
      <material name="blue"/>
    </visual>
  </link>
  
  <!-- 轮子 -->
  <xacro:wheel name="left" x="0" y="${base_width/2}"/>
  <xacro:wheel name="right" x="0" y="${-base_width/2}"/>
  
</robot>
```

### 4.3 Gazebo + ROS2 启动文件

```python
# mini_bot_gazebo/launch/bringup.launch.py
from launch import LaunchDescription
from launch.actions import ExecuteProcess
from launch_ros.actions import Node

def generate_launch_description():
    return LaunchDescription([
        # 启动 Gazebo
        ExecuteProcess(
            cmd=['gazebo', '--verbose', '-s', 'libgazebo_ros_init.so'],
            output='screen'
        ),
        
        # 加载机器人模型
        Node(
            package='gazebo_ros',
            executable='spawn_entity.py',
            arguments=['-entity', 'mini_bot', '-file', 'mini_bot.urdf'],
            output='screen'
        ),
        
        # 启动控制器
        Node(
            package='mini_bot_control',
            executable='chassis_node',
            output='screen'
        ),
    ])
```

### 4.4 常用命令

```bash
# 启动空世界
gazebo --verbose empty.world

# 加载世界文件
gazebo my_world.world

# ROS2 工具
ros2 launch gazebo_ros gazebo.launch.py

# 查看模型
ros2 run gazebo_ros model list

# Spawn 模型
ros2 run gazebo_ros spawn_entity.py -file robot.urdf -entity robot_name
```

## 五、Gazebo 版本选择

| 版本 | 状态 | 特点 |
|------|------|------|
| Gazebo Citadel | EOL (2025) | ROS2 Foxy/Galactic 默认 |
| **Gazebo Fortress** | **LTS (2026-2029)** | **推荐，ROS2 Humble 默认** |
| Gazebo Garden | 活跃开发 | 新特性，API 变化 |
| Gazebo Harmonic | 最新 | 商业支持 |

## 六、替代品

See [03_webots.md](03_webots.md) for a comparison with Webots.

---

*此文档最后更新于 2026-04-09*
