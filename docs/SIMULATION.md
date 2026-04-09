# Mini-Bot 仿真平台指南

## 一、主流仿真平台对比

| 平台 | 开发者 | 开源 | 优势 | 劣势 | 推荐场景 |
|------|--------|------|------|------|----------|
| **Gazebo** | Open Robotics | ✅ | 深度整合ROS2；模型丰富；传感器支持完善 | 渲染较弱；学习曲线陡峭 | ROS2项目首选 |
| **Webots** | Cyberbotics | ✅ | 上手极快；模型丰富；文档完善；跨平台 | 动力学精度略低 | 教学、快速原型验证 |
| **CoppeliaSim** | Coppelia Robotics | ✅/商用混合 | 多物理引擎可选；脚本灵活 | 学习成本稍高 | 工业自动化仿真 |
| **MuJoCo** | DeepMind/Google | ✅ | 高精度接触动力学；轻量快速；RL标准基准 | 缺场景编辑器；渲染简易 | 强化学习、控制算法研究 |
| **Isaac Sim** | NVIDIA | 部分开源 | RTX高保真渲染；数字孪生 | GPU要求高（需RTX显卡） | 视觉感知、工业级仿真 |

## 二、针对性推荐

| 项目阶段 | 推荐平台 | 理由 |
|----------|----------|------|
| 初期算法验证 | Webots | 上手最快，15分钟搭建轮式机器人模型 |
| ROS2深度开发 | Gazebo | 与ROS2无缝集成，社区支持最广 |
| 机械臂抓取/强化学习 | MuJoCo | 接触动力学精度高，Python API友好 |
| 视觉+AI感知开发 | Isaac Sim（如有GPU） | 合成数据生成，高保真渲染 |

## 三、仿真开发工作流

```
1. 建模：使用 URDF/Xacro 描述机器人结构（轮式底盘+机械臂+传感器）
2. 仿真启动：在 Gazebo/Webots 中加载模型，添加物理环境
3. 控制测试：通过 ROS2 Topic 发送 /cmd_vel 控制轮式移动
4. 数据可视化：Rviz2 实时显示传感器数据、路径规划、地图构建
5. 代码迁移：仿真验证通过的代码，稍作硬件接口适配即可部署
```

## 四、Webots 快速入门

### 4.1 安装

```bash
# Ubuntu 22.04
sudo apt install webots
```

### 4.2 创建轮式机器人模型

1. 新建项目：`File → New Project`
2. 添加机器人节点
3. 添加轮子（2个差速或4个麦克纳姆轮）
4. 添加距离传感器、IMU

### 4.3 ROS2 接口

```bash
# 安装 webots_ros2
sudo apt install ros-humble-webots-ros2

# 启动示例
ros2 launch webots_ros2_turtlebot robot_launch.py
```

## 五、Gazebo 快速入门

### 5.1 安装 ROS2 + Gazebo

```bash
# 安装 ROS2 Humble
sudo apt install ros-humble-ros-gz

# 启动 Gazebo
gz sim
```

### 5.2 创建 URDF 模型

```xml
<!-- robot.urdf -->
<robot name="mini_bot">
  <link name="base_link">
    <visual>
      <geometry>
        <cylinder length="0.1" radius="0.15"/>
      </geometry>
    </visual>
  </link>
  
  <link name="left_wheel">
    <visual>
      <geometry>
        <cylinder length="0.02" radius="0.05"/>
      </geometry>
    </visual>
  </link>
  
  <joint name="left_wheel_joint" type="continuous">
    <parent link="base_link"/>
    <child link="left_wheel"/>
    <axis xyz="0 0 1"/>
  </joint>
</robot>
```

## 六、开源项目参考

| 项目 | 特点 | 参考价值 |
|------|------|----------|
| Linorobot2 | ROS2轮式机器人框架，支持2WD/4WD/麦克纳姆轮 | 硬件配置、ROS2集成 |
| Reachy Mini | Hugging Face开源桌面机器人，Python SDK+仿真器 | 软件架构、AI集成 |
| ElectronBot | 开源桌面机器人，STM32主控 | 机械结构、显示屏UI设计 |
| ArmPi Pro | 树莓派+ROS+4WD麦克纳姆轮+机械臂套件 | 全功能集成方案 |
