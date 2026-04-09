# Mini-Bot 桌面机器人

> 轮式行走 + 屏幕展示 + 可选机械手的桌面机器人项目

## 📋 项目概述

Mini-Bot 是一款面向桌面场景的智能机器人，支持轮式移动、屏幕表情交互，可扩展 3-6 自由度机械臂。

**技术路线：** 仿真先行（Webots/Gazebo）+ 真实硬件开发（ROS2 + ESP32-S3）

## 🛠 技术栈

| 层级 | 技术选型 |
|------|----------|
| 主控 | Raspberry Pi 5 + ESP32-S3 双主控 |
| 框架 | ROS2 Humble |
| 仿真 | Webots（初期）/ Gazebo（深度开发）|
| 屏幕 | LVGL / PyQt |
| 视觉 | OpenCV |

## 📁 目录结构

```
mini-bot/
├── docs/              # 技术文档
│   ├── ARCHITECTURE.md
│   ├── HARDWARE.md
│   ├── ROS2_GUIDE.md
│   ├── SIMULATION.md
│   └── BOM.md
├── src/               # 源代码
│   ├── pi5/           # Raspberry Pi 5 (ROS2)
│   └── esp32/          # ESP32-S3 固件
├── sim/               # 仿真模型
│   ├── webots/
│   └── gazebo/
├── hardware/          # 硬件设计
└── tests/             # 测试用例
```

## 🚀 开发阶段

1. **阶段一**：环境搭建 + 仿真验证（1-2周）
2. **阶段二**：硬件驱动 + 底层控制（2-3周）
3. **阶段三**：核心功能开发（3-4周）
4. **阶段四**：系统集成与优化（2-3周）
5. **阶段五**：扩展与迭代（持续）

## 📦 项目状态

- **状态**: 🟡 规划中
- **创建时间**: 2026-04-09
- **负责人**: Tesla 团队

## 🔗 相关文档

- [架构设计](./docs/ARCHITECTURE.md)
- [硬件方案](./docs/HARDWARE.md)
- [仿真指南](./docs/SIMULATION.md)
- [BOM 成本估算](./docs/BOM.md)

## 📝 GitHub 地址

- **Repository**: https://github.com/LuckyJunjie/mini-bot
- **状态**: 🟡 待初始化（需 GitHub CLI 认证后 `gh repo create`）

**创建命令（需先 `gh auth login`）**:
```bash
gh repo create LuckyJunjie/mini-bot --public --description "Mini-Bot 桌面机器人 - 轮式行走+屏幕展示+可选机械手"
cd mini-bot && git init && git add . && git commit -m "Initial commit" && git remote add origin https://github.com/LuckyJunjie/mini-bot.git && git push -u origin main
```
