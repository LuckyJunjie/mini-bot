# Python 3

## 一、背景

Python 是由 Guido van Rossum 于 1991 年创建的高级解释型编程语言，以其简洁的语法和强大的生态系统著称。在机器人领域，Python 主要用于快速原型开发、ROS2 节点编写、数据处理和机器学习。

ROS2 的 Python 支持通过 `rclpy` 包实现，虽然不如 C++ 官方，但足以完成大多数机器人开发任务。

## 二、目的

- **ROS2 节点开发**：使用 rclpy 编写机器人控制节点
- **算法原型**：快速验证运动规划、SLAM 算法
- **数据处理**：传感器数据解析、日志分析
- **机器学习**：OpenCV、TensorFlow Lite 图像处理

## 三、官方地址

- 官网: https://www.python.org/
- 文档: https://docs.python.org/3/
- 下载: https://www.python.org/downloads/
- PyPI (包仓库): https://pypi.org/

## 四、实用教程

### 4.1 安装 Python 3 (Ubuntu)

```bash
# Ubuntu 22.04 自带 Python 3.10
python3 --version

# 安装 pip
sudo apt install -y python3-pip

# 设置虚拟环境
sudo apt install -y python3-venv python3-venv
mkdir -p ~/mini_bot_ws/venv
python3 -m venv ~/mini_bot_ws/venv
source ~/mini_bot_ws/venv/bin/activate

# 安装常用包
pip install numpy opencv-python pyserial rclpy
```

### 4.2 ROS2 Python 节点示例

```python
#!/usr/bin/env python3
# mini_bot_control/mini_bot_control/chassis_node.py

import rclpy
from rclpy.node import Node
from geometry_msgs.msg import Twist
from sensor_msgs.msg import Imu
import serial

class ChassisNode(Node):
    def __init__(self):
        super().__init__('chassis_node')
        
        # 订阅键盘控制
        self.cmd_sub = self.create_subscription(
            Twist, '/cmd_vel', self.cmd_callback, 10
        )
        
        # 发布里程计
        self.odom_pub = self.create_publisher(Twist, '/odom', 10)
        
        # ESP32 串口通信
        self.serial_port = serial.Serial('/dev/ttyUSB0', 115200, timeout=0.1)
        
        self.get_logger().info('Chassis node started')
        
    def cmd_callback(self, msg: Twist):
        """处理速度指令，发送到 ESP32"""
        linear = msg.linear.x
        angular = msg.angular.z
        
        # 简单的差速转换
        left_speed = int((linear - angular * 0.1) * 100)
        right_speed = int((linear + angular * 0.1) * 100)
        
        # 发送命令到 ESP32
        cmd = f"L{left_speed}R{right_speed}\n"
        self.serial_port.write(cmd.encode())
        
    def timer_callback(self):
        """定时发布里程计"""
        # 读取编码器并计算里程计
        odom = Twist()
        self.odom_pub.publish(odom)
        
def main(args=None):
    rclpy.init(args=args)
    node = ChassisNode()
    
    try:
        rclpy.spin(node)
    except KeyboardInterrupt:
        pass
    finally:
        node.destroy_node()
        rclpy.shutdown()

if __name__ == '__main__':
    main()
```

### 4.3 Python 常用工具链

```bash
# 包管理
pip install <package>
pip install -r requirements.txt
pip freeze > requirements.txt

# 格式化 & 检查
pip install black flake8 mypy
black .
flake8 .
mypy .

# 测试
pip install pytest pytest-cov
pytest tests/
```

## 五、Python vs C++ in ROS2

| 方面 | Python (rclpy) | C++ (rclcpp) |
|------|----------------|--------------|
| 开发速度 | ⭐⭐⭐ 快速原型 | ⭐⭐ |
| 执行效率 | ⭐⭐ 够用 | ⭐⭐⭐ 高性能 |
| 实时性 | ⭐ 不适合 | ⭐⭐⭐ 适合 |
| 库支持 | ⭐⭐⭐ 丰富 | ⭐⭐ |
| ROS2 支持 | ⭐⭐ 完整但社区小 | ⭐⭐⭐ 官方默认 |

**建议**：
- 算法原型、数据处理 → Python
- 实时控制、底层驱动 → C++

## 六、替代品

| 替代品 | 特点 |
|--------|------|
| MicroPython | 嵌入式设备，ESP32 运行 |
| PyPy | 更快解释器 |
| Cython | Python 编译加速 |

---

*此文档最后更新于 2026-04-09*
