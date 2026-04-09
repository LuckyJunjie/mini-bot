# Mini-Bot 软件参考项目

> 本目录用于存放开源参考项目，供开发时学习和借鉴。

## 下载脚本

由于网络原因，建议在**树莓派或网络较好的环境**中执行以下命令：

```bash
cd ~/Projects/mini-bot/sw-reference

# ROS2 Webots 集成
git clone --depth 1 https://github.com/cyberbotics/webots_ros2.git

# 底盘控制参考
git clone --depth 1 https://github.com/husarion/rosbot_ros2.git

# 差速驱动开源实现
git clone --depth 1 https://github.com/Robotech-UTT/rosbot-xl.git

# IMU 驱动
git clone --depth 1 https://github.com/EAIBOT/ydlidar_ros2.git

# 机器人 UI 参考
git clone --depth 1 https://github.com/ros-teleop/teleop_tools.git

# ESP32 机器人控制
git clone --depth 1 https://github.com/howmanygiant/esp32_robot.git
```

## 参考项目列表

### 1. webots_ros2 ⭐⭐⭐ (推荐)
- **地址**: https://github.com/cyberbotics/webots_ros2
- **大小**: ~50MB
- **用途**: Webots 仿真器与 ROS2 的官方集成包
- **参考内容**: 机器人模型、传感器接口、控制器示例

### 2. rosbot_ros2 (Husarion)
- **地址**: https://github.com/husarion/rosbot_ros2
- **大小**: ~30MB
- **用途**: ROS2 机器人完整示例（包含底盘、导航、UI）
- **参考内容**:
  - 差速驱动控制
  - 里程计计算
  - 键盘控制
  - 仿真与真实机器人切换

### 3. rosbot-xl
- **地址**: https://github.com/Robotech-UTT/rosbot-xl
- **用途**: 麦克纳姆轮机器人 ROS2 实现
- **参考内容**: 全向移动控制、IMU 融合

### 4. ESP32 机器人
- **地址**: https://github.com/howmanygiant/esp32_robot
- **用途**: ESP32 差速机器人完整代码
- **参考内容**:
  - 电机 PID 控制
  - 串口通信协议
  - IMU 数据采集

### 5. teleop_tools
- **地址**: https://github.com/ros-teleop/teleop_tools
- **用途**: ROS 键盘/手柄控制工具
- **参考内容**: teleop_twist_keyboard, teleop_twist_joy

## 快速下载 (单行命令)

```bash
cd ~/Projects/mini-bot/sw-reference
for repo in "cyberbotics/webots_ros2" "husarion/rosbot_ros2" "Robotech-UTT/rosbot-xl" "EAIBOT/ydlidar_ros2" "ros-teleop/teleop_tools"; do
    git clone --depth 1 "https://github.com/$repo.git"
done
```

## 网络优化建议

如果 git clone 速度慢，可尝试：

1. **使用 SSH 替代 HTTPS**
   ```bash
   git clone git@github.com:cyberbotics/webots_ros2.git
   ```

2. **设置 Git 代理** (如果有代理)
   ```bash
   git config --global http.proxy http://127.0.0.1:7890
   git config --global https.proxy http://127.0.0.1:7890
   ```

3. **使用 Gitee 镜像** (仅部分仓库)
   - 在 https://gitee.com/ 搜索对应的开源项目

4. **分批次克隆**
   - 先 clone 所有仓库的 `--depth 1` 版本
   - 需要详细代码时再 `git fetch --unshallow`

---

*此文档最后更新于 2026-04-09*
