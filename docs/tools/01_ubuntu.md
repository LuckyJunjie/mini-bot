# Ubuntu Server 22.04 LTS

## 一、背景

Ubuntu 是由 Canonical 公司赞助的基于 Debian 的开源 Linux 发行版。Ubuntu Server 是其服务器版本，专为服务器、云计算和物联网设备优化，不包含图形桌面环境。

Raspberry Pi OS (原 Raspbian) 是专为树莓派优化的基于 Debian 的 Linux 发行版，提供 32-bit 和 64-bit 两个版本。

## 二、目的

- 作为 Mini-Bot 项目的主控计算机（树莓派 5）的操作系统
- 运行 ROS2、Python 程序、ROS2 导航堆栈等
- 提供稳定的开发运行环境

## 三、官方地址

- Ubuntu Server: https://ubuntu.com/download/server
- Raspberry Pi OS: https://www.raspberrypi.com/software/operating-systems/
- Ubuntu for Raspberry Pi: https://ubuntu.com/download/raspberry-pi

## 四、实用教程

### 4.1 烧录 Ubuntu Server 到 SD 卡

```bash
# 1. 下载树莓派镜像
wget https://cdimage.ubuntu.com/releases/22.04.5/release/ubuntu-22.04.5-preinstalled-server-arm64+raspi.img.xz

# 2. 使用树莓派官方 Imager 烧录（推荐）
# 下载 Raspberry Pi Imager: https://www.raspberrypi.com/software/
# 选择: Choose OS > Other general purpose OS > Ubuntu > Ubuntu Server 22.04 LTS (64-bit)
# 选择 SD 卡，点击烧录

# 3. 烧录完成后，编辑 user-data.txt 配置 WiFi 和 SSH
```

### 4.2 基础配置

```bash
# 更新系统
sudo apt update && sudo apt upgrade -y

# 安装常用工具
sudo apt install -y build-essential vim git curl wget htop net-tools

# 配置 WiFi（桌面环境外）
sudo nmcli device wifi list
sudo nmcli device wifi connect <SSID> password <PASSWORD>

# 启用 SSH
sudo systemctl enable ssh
sudo systemctl start ssh
```

### 4.3 连接树莓派

```bash
# 从 Mac/Linux 连接
ssh ubuntu@<树莓派IP>

# 从 Windows 使用 PowerShell
ssh ubuntu@<树莓派IP>
```

## 五、替代品

| 替代品 | 优点 | 缺点 |
|--------|------|------|
| Raspberry Pi OS | 官方支持，优化更好 | 软件包较旧 |
| Debian | 稳定，资源占用低 | 需要手动配置 |
| Fedora IoT | 新特性，Atomic 更新 | 社区较小 |
| Arch Linux ARM | 最新软件，高度定制 | 学习曲线陡峭 |

## 六、版本选择建议

| 设备 | 推荐系统 | 架构 |
|------|----------|------|
| 树莓派 5 | Ubuntu Server 22.04 LTS | arm64 (64-bit) |
| 树莓派 4/400 | Raspberry Pi OS | arm64 |
| PC / 虚拟机 | Ubuntu Server 22.04 LTS | x86_64 |

---

*此文档最后更新于 2026-04-09*
