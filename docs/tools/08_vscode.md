# Visual Studio Code

## 一、背景

Visual Studio Code (VSCode) 是微软开发的开源代码编辑器，支持 Windows、macOS、Linux。通过丰富的扩展插件，VSCode 可以成为强大的机器人开发 IDE，支持 Python、C++、ROS、ESP32 等多种开发环境。

## 二、目的

- **代码编辑**：Python、C++、CMake、XML 语法高亮
- **ROS2 开发**：ros-navigation, roslint 插件
- **ESP32 开发**：PlatformIO IDE 插件
- **Git 集成**：源代码管理
- **远程开发**：通过 SSH 连接树莓派编辑代码

## 三、官方地址

- 官网: https://code.visualstudio.com/
- 下载: https://code.visualstudio.com/Download
- 扩展市场: https://marketplace.visualstudio.com/

## 四、实用教程

### 4.1 安装与配置

```bash
# macOS
brew install --cask visual-studio-code

# Ubuntu
sudo snap install --classic code

# 或从官网下载 .deb 安装包
```

### 4.2 推荐扩展

| 扩展名 | 功能 |
|--------|------|
| Python | Python 语言支持、调试 |
| C/C++ | C/C++ IntelliSense、调试 |
| PlatformIO IDE | ESP32/Arduino 开发 |
| ROS | ROS 语法、启动文件支持 |
| Docker | 容器管理 |
| GitLens | Git 增强 |
| SSH FS | 远程编辑文件 |
| YAML | YAML 语法检查 |

### 4.3 ROS2 开发配置

```json
// .vscode/tasks.json
{
    "version": "2.0.0",
    "tasks": [
        {
            "label": "Build ROS2 Workspace",
            "type": "shell",
            "command": "cd ~/mini_bot_ws && source /opt/ros/humble/setup.bash && colcon build",
            "problemMatcher": [],
            "group": "build"
        },
        {
            "label": "Source ROS2",
            "type": "shell",
            "command": "source /opt/ros/humble/setup.bash",
            "problemMatcher": []
        }
    ]
}
```

### 4.4 SSH 远程开发树莓派

```bash
# 1. 安装 Remote - SSH 扩展
# Ctrl+Shift+P -> Remote-SSH: Connect to Host

# 2. 添加 SSH 配置
cat >> ~/.ssh/config << 'EOF'
Host pi5
    HostName 192.168.x.x
    User ubuntu
    IdentityFile ~/.ssh/id_rsa
EOF

# 3. 连接并打开文件夹
# Ctrl+Shift+P -> Remote-SSH: Connect to Host -> pi5
```

### 4.5 PlatformIO ESP32 开发

```bash
# 1. 安装 PlatformIO 扩展
# Extensions: platformio.platformio-ide

# 2. 打开 Mini-Bot 项目
# File -> Open Folder -> ~/mini_bot_firmware

# 3. PlatformIO Toolbar
# - Build: 编译
# - Upload: 烧录
# - Monitor: 串口监视器
# - Clean: 清理构建文件
```

## 五、替代品

| 替代品 | 平台 | 特点 |
|--------|------|------|
| CLion | 全平台 | CMake 集成强，付费 |
| VIM/Neovim | 全平台 | 轻量，高度可定制 |
| PyCharm | 全平台 | Python 开发首选 |
| STM32CubeIDE | 全平台 | STM32 专用 |
| Arduino IDE | 全平台 | Arduino 官方，简单 |

---

*此文档最后更新于 2026-04-09*
