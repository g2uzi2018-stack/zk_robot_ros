# zk_robot_ros

面向 PAL TIAGo++ 的 ROS 2 仿真、感知和棋子抓取工作区。

本仓库当前只做 TIAGo++：在 Gazebo 中使用 ROS 2 组织相机、TF、棋子感知、运动规划、
机械臂和夹爪控制，完成棋子的识别、定位、抓取和放置。其他机型和其他末端执行器协议
不属于本次仿真范围。

## 当前范围

- 使用 ROS 2 组织 TIAGo++ 的相机、TF、感知、规划和执行节点。
- 在 Gazebo 中验证 TIAGo++ 棋盘和棋子的抓取流程。
- 使用 `gazebo_bot` 已有的 TIAGo++ 虚拟电机和 SocketCAN/vCAN 闭环。
- 为头部相机、腕部相机、双臂和双夹爪建立稳定的 ROS 2 接口。

当前仓库还没有可直接运行的 ROS 2 package；TIAGo++ 仿真和底层控制代码仍在相关
仓库中。本仓库先把单一机型的接口、环境和抓取流程写清楚，避免把 Gazebo 默认参数
误当成真实 TIAGo 的标定值。

## 文档

- [开发环境与棋子抓取](docs/开发环境与棋子抓取.md)：Jetson/ROS 2/Gazebo 基线、TIAGo++ 棋盘坐标、相机话题和抓取流程。
- [接口协议与安全边界](docs/接口协议与安全边界.md)：TIAGo++ 仿真 CAN、ROS 2、Gazebo bridge、夹爪动作和安全约束。

## 相关项目

- `gazebo_bot`：PAL TIAGo++ Gazebo 场景、模型、RGB-D 相机、棋盘棋子和仿真 vCAN。
- `zk_robot`：已有的 TIAGo++ C++ SocketCAN、关节、机械臂和夹爪控制实现；本仓库只复用 TIAGo++ 相关部分。
- `robot_station`：可选的 Web 控制台和机器人网关；当前 TIAGo++ ROS 2 仿真不依赖它。

这些项目可以独立构建；`zk_robot_ros` 负责把 TIAGo++ 仿真数据和执行器接入 ROS 2，
不复制整套底层实现。

## 环境快速检查

目标开发环境为 Ubuntu 22.04 / aarch64 Jetson，具体版本和检查方式见环境文档。典型
ROS 2 命令如下：

```bash
source /opt/ros/humble/setup.bash
ros2 doctor --report
gz sim --versions
```

仿真阶段只使用 `vcan*`，不要把验证脚本、演示程序或故障测试指向真实 `can0`。
仓库不保存 SSH 地址、端口、密钥路径、令牌或其他机器私密信息；这些信息应通过
本机私有配置或密码管理器提供。

## 后续实现顺序

1. 建立 TIAGo++ ROS 2 package、消息/动作接口和 `ros_gz_bridge` 传感器桥接。
2. 接入棋盘/棋子检测，输出带时间戳和坐标系的 `PoseStamped`。
3. 用 TF2 将目标转换到 TIAGo++ 基座坐标，并加入碰撞、限位和可达性检查。
4. 接入 TIAGo++ MoveIt/控制器和双夹爪动作，完成接近、闭合、抬升、移动、放置和释放。
5. 在 vCAN 和 Gazebo 中完成看门狗、停止、反馈新鲜度和抓取失败恢复验收。
