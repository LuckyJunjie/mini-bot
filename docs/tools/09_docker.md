# Docker Desktop

## 一、背景

Docker 是 dotcloud 公司开发的开源容器化平台，允许开发者将应用程序及其依赖打包成容器，实现"构建一次，到处运行"。在机器人开发中，Docker 可用于在 macOS/Windows 上运行 ROS2 开发环境。

## 二、目的

- **跨平台开发**：在 macOS 上运行 Ubuntu + ROS2 开发环境
- **环境隔离**：避免依赖冲突
- **CI/CD**：自动化构建和测试
- **部署**：将开发环境打包部署到树莓派

## 三、官方地址

- 官网: https://www.docker.com/
- Docker Desktop: https://www.docker.com/products/docker-desktop/
- 镜像加速: https://dockerhub.azk8s.cn (国内加速)

## 四、实用教程

### 4.1 macOS 安装 Docker Desktop

```bash
# 方法1: Homebrew (如果可用)
brew install --cask docker

# 方法2: 手动下载
# 访问 https://www.docker.com/products/docker-desktop/
# 下载 Docker.dmg 并安装
```

### 4.2 配置 Docker 镜像加速 (国内)

```bash
# 创建 daemon.json
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json << 'EOF'
{
  "registry-mirrors": [
    "https://docker.mirrors.ustc.edu.cn",
    "https://hub-mirror.c.163.com"
  ]
}
EOF

# 重启 Docker
sudo systemctl restart docker
```

### 4.3 运行 ROS2 Humble 容器

```bash
# 拉取 ROS2 Humble 镜像 (带桌面环境)
docker pull osrf/ros:humble-desktop

# 运行容器
docker run -it \
  --name ros2_dev \
  --network host \
  -e DISPLAY=$DISPLAY \
  -v /tmp/.X11-unix:/tmp/.X11-unix \
  -v ~/mini_bot_ws:/home/ubuntu/workspace \
  osrf/ros:humble-desktop

# 退出容器后重新进入
docker exec -it ros2_dev bash
```

### 4.4 Docker + VSCode 开发

```bash
# 安装 VSCode Docker 扩展
# Extensions: ms-azuretools.vscode-docker

# 在容器中开发
docker exec -it ros2_dev bash
cd ~/workspace
code .
```

### 4.5 常用 Docker 命令

```bash
# 容器管理
docker ps -a              # 查看所有容器
docker start ros2_dev    # 启动容器
docker stop ros2_dev     # 停止容器
docker rm ros2_dev       # 删除容器

# 镜像管理
docker images            # 查看镜像
docker rmi <image>       # 删除镜像

# 进入容器
docker exec -it ros2_dev bash
```

## 五、替代品

| 替代品 | 特点 |
|--------|------|
| Podman | 无守护进程，更安全 |
| VirtualBox | 完整虚拟机 |
| VMware Fusion | macOS 原生虚拟机 |
| UTM | 免费 ARM 虚拟机 |

---

*此文档最后更新于 2026-04-09*
