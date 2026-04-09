# Mini-Bot 开发工具链

> 本目录包含 Mini-Bot 项目开发所需的所有软件工具指南

## 安装状态总览

### ✅ macOS 已安装 (开发工作站)

| 软件 | 版本 | 命令 | 状态 |
|------|------|------|------|
| Python 3 | 3.9.6 | `python3 --version` | ✅ |
| Git | 2.50.1 | `git --version` | ✅ |
| PlatformIO | 6.1.19 | `pio --version` | ✅ |

**PlatformIO PATH 配置** (如果 `pio` 命令找不到):
```bash
export PATH="$HOME/Library/Python/3.9/bin:$PATH"
# 永久生效:
echo 'export PATH="$HOME/Library/Python/3.9/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc
```

### ⏳ macOS 需手动安装

| 软件 | 下载地址 | 说明 |
|------|----------|------|
| VSCode | https://code.visualstudio.com/Download | 代码编辑器 |
| Webots | https://github.com/cyberbotics/webots/releases | 机器人仿真 |

### ⏳ 树莓派待安装 (主控)

| 软件 | 说明 |
|------|------|
| Ubuntu Server 22.04 | 需烧录到 SD 卡 |
| ROS2 Humble | 仅 Linux 支持 |

## 目录结构

| 文件 | 工具 | 用途 |
|------|------|------|
| [01_ubuntu.md](01_ubuntu.md) | Ubuntu / Raspberry Pi OS | 操作系统 |
| [02_ros2.md](02_ros2.md) | ROS2 Humble | 机器人框架 |
| [03_webots.md](03_webots.md) | Webots | 仿真环境（推荐） |
| [04_gazebo.md](04_gazebo.md) | Gazebo | 仿真环境（备选） |
| [05_python.md](05_python.md) | Python 3 | 编程语言 |
| [06_git.md](06_git.md) | Git | 版本控制 |
| [07_esp32.md](07_esp32.md) | ESP32-S3 Toolchain | 嵌入式开发 |
| [08_vscode.md](08_vscode.md) | VSCode | 代码编辑器 |

## 快速开始

### 开发工作站（你的 Mac/PC）

1. **安装 VSCode**: [08_vscode.md](08_vscode.md)
2. **安装 Git**: [06_git.md](06_git.md)
3. **安装仿真工具**: [03_webots.md](03_webots.md) 或 [04_gazebo.md](04_gazebo.md)

### 树莓派 5（主控）

1. **安装操作系统**: [01_ubuntu.md](01_ubuntu.md)
2. **安装 ROS2**: [02_ros2.md](02_ros2.md)
3. **配置开发工具**: [08_vscode.md](08_vscode.md) (Remote SSH)

### ESP32-S3（底层控制）

1. **安装工具链**: [07_esp32.md](07_esp32.md)
2. **使用 PlatformIO**: [08_vscode.md](08_vscode.md)

## 环境架构图

```
┌─────────────────────────────────────────────────────────┐
│                    开发工作站 (Mac/PC)                    │
│  ┌──────────┐  ┌──────────┐  ┌──────────────────────┐  │
│  │  VSCode  │  │  GitHub  │  │   Webots / Gazebo    │  │
│  │  (SSH)   │  │          │  │      仿真环境         │  │
│  └──────────┘  └──────────┘  └──────────────────────┘  │
└────────────────────────┬────────────────────────────────┘
                         │  SSH / 部署
                         ▼
┌─────────────────────────────────────────────────────────┐
│                     树莓派 5 (主控)                      │
│  ┌────────────────────────────────────────────────────┐  │
│  │              Ubuntu 22.04 + ROS2 Humble            │  │
│  │  ┌──────────┐  ┌──────────┐  ┌──────────────────┐  │  │
│  │  │  控制节点 │  │  导航堆栈 │  │    屏幕 UI       │  │  │
│  │  │ Python/C++│  │ SLAM/Path │  │   LVGL/PyQt     │  │  │
│  │  └──────────┘  └──────────┘  └──────────────────┘  │  │
│  └────────────────────────────────────────────────────┘  │
│                           │  UART / I2C                 │
└───────────────────────────┼─────────────────────────────┘
                            │
                            ▼
┌─────────────────────────────────────────────────────────┐
│                    ESP32-S3 (底层)                       │
│  ┌──────────┐  ┌──────────┐  ┌──────────────────────┐  │
│  │  电机PWM │  │   IMU    │  │      舵机控制        │  │
│  │  TB6612  │  │ MPU6050  │  │      PCA9685         │  │
│  └──────────┘  └──────────┘  └──────────────────────┘  │
└─────────────────────────────────────────────────────────┘
```

## 安装检查清单

### macOS (开发工作站) ✅

| 软件 | 状态 | 安装方式 | 备注 |
|------|------|----------|------|
| Python 3 | ✅ 已安装 | 系统自带 | Version 3.9.6 |
| Git | ✅ 已安装 | 系统自带 | Version 2.50.1 |
| PlatformIO | ✅ 已安装 | pip3 | Version 6.1.19 |
| VSCode | ⏳ 需手动安装 | 官网下载 | https://code.visualstudio.com/ |
| Webots | ⏳ 需手动安装 | 官网下载 | https://cyberbotics.com/ |
| Gazebo | ⏳ 需手动安装 | macOS 不推荐 | 仅 Linux 支持 |

### 树莓派 5 (主控计算机) ⏳

| 软件 | 状态 | 安装方式 |
|------|------|----------|
| Ubuntu Server 22.04 | ⏳ 需烧录 | Raspberry Pi Imager |
| ROS2 Humble | ⏳ 待安装 | apt install |
| Git | ⏳ 待安装 | apt install |
| Python 3 | ⏳ 待安装 | 系统自带 |

### ESP32-S3 (底层控制) ⏳

| 软件 | 状态 | 安装方式 |
|------|------|----------|
| PlatformIO | ✅ 可用 | Mac 已安装，可在任意平台开发 |
| ESP32 驱动 | ⏳ 待安装 | USB 连接后自动识别 |

---

*此目录最后更新于 2026-04-09*
