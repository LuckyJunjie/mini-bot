# Webots

## 一、背景

Webots 是由 Cyberbotics 公司开发的开源机器人仿真软件，使用 ODE (Open Dynamics Engine) 物理引擎。它是一个跨平台的机器人仿真环境，支持多种机器人模型、传感器和控制器。

Webots 采用世界文件 (.wbt) 定义仿真环境，使用 PROTO (PROTOtype) 实现可复用的机器人组件。相比 Gazebo，Webots 开箱即用，无需复杂的配置即可开始仿真。

## 二、目的

- **运动仿真**：验证底盘控制算法（差速、麦克纳姆轮）
- **传感器仿真**：测试 IMU、编码器、距离传感器
- **无硬件开发**：在实际机器人搭建前验证核心功能
- **CI/CD 集成**：自动化测试机器人代码

## 三、官方地址

- 官网: https://www.cyberbotics.com/
- GitHub: https://github.com/cyberbotics/webots
- 文档: https://www.cyberbotics.com/doc/guide/index
- 下载: https://www.cyberbotics.com/linux/tarball.html

## 四、实用教程

### 4.1 安装 Webots

```bash
# 方法1: Snap 安装（推荐 for Ubuntu）
sudo snap install webots --classic

# 方法2: APT 安装
sudo apt install webots

# 方法3: 手动安装（最新版本）
cd /tmp
wget https://github.com/cyberbotics/webots/releases/download/v24.0.0/webots-24.0-x86_64.tar.gz
sudo tar -xzf webots-24.0-x86_64.tar.gz -C /opt/
sudo ln -s /opt/webots/webots /usr/local/bin/webots

# 方法4: macOS
brew install --cask webots

# 启动
webots
```

### 4.2 创建 Mini-Bot 仿真世界

```bash
# 创建项目目录
mkdir -p ~/mini_bot_sim/worlds
mkdir -p ~/mini_bot_sim/controllers
mkdir -p ~/mini_bot_sim/protos

# 创建控制器 (Python)
cat > ~/mini_bot_sim/controllers/diff_drive.py << 'EOF'
from controller import Robot, Motor, Gyro, DistanceSensor

class DiffDriveRobot(Robot):
    def __init__(self):
        super().__init__()
        self.time_step = int(self.getBasicTimeStep())
        
        # 获取电机
        self.left_motor = self.getMotor('left_motor')
        self.right_motor = self.getMotor('right_motor')
        self.left_motor.setPosition(float('inf'))
        self.right_motor.setPosition(float('inf'))
        
        # 初始化速度
        self.left_motor.setVelocity(0.0)
        self.right_motor.setVelocity(0.0)
        
    def run(self):
        while True:
            # 设置电机速度 (rad/s)
            self.left_motor.setVelocity(2.0)
            self.right_motor.setVelocity(2.0)
            
            # 仿真一步
            if self.step(self.time_step) == -1:
                break

robot = DiffDriveRobot()
robot.run()
```

### 4.3 Webots + ROS2 集成

```bash
# 安装 webots_ros2 包
sudo apt install ros-humble-webots-ros2Descriptions
sudo apt install ros-humble-webots-ros2-driver
sudo apt install ros-humble-webots-ros2-dsr

# 启动示例
ros2 launch webots_ros2_universal_robot universal_robot.launch.py
```

### 4.4 常用操作

```bash
# 命令行启动指定世界
webots --mode=fast --batch ~/mini_bot_sim/worlds/my_robot.wbt

# 导出视频
webots --video --no-sound /path/to/robot.wbt
```

## 五、替代品

| 替代品 | 优点 | 缺点 |
|--------|------|------|
| Gazebo | ROS 官方仿真，生态丰富 | 配置复杂 |
| NVIDIA Isaac Sim | 强大物理引擎，RTX 加速 | 商业软件 |
| CoppeliaSim (V-REP) | 多物理引擎，学术免费 | 界面较老旧 |
| Unity Robotics | 游戏引擎，视觉效果好 | 与 ROS 集成较新 |
| PyBullet | 轻量，易于 Python 集成 | 功能有限 |

## 六、Webots vs Gazebo 选择

| 特性 | Webots | Gazebo |
|------|--------|--------|
| 易用性 | ⭐⭐⭐ 开箱即用 | ⭐ 需要配置 |
| ROS2 集成 | ⭐⭐⭐ 官方支持包 | ⭐⭐⭐ 官方支持 |
| 物理精度 | ⭐⭐⭐ ODE/Bullet | ⭐⭐⭐ ODE/PhysX/DART |
| 学习资源 | ⭐⭐ 较少 | ⭐⭐⭐ 丰富 |
| 机器人模型库 | ⭐⭐⭐ 丰富 | ⭐⭐ 普通 |

**推荐**：对于 Mini-Bot 项目，**Webots 是更好的选择**，配置简单，ROS2 集成成熟。

---

*此文档最后更新于 2026-04-09*
