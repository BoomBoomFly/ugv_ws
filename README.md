# ugv_ws

> 本仓库为 [BoomBoomFly](https://github.com/BoomBoomFly/BoomBoomFly) 主仓库中 UGV 子系统的独立镜像/发布工作区，日常主线研发请以主仓库为准。

## 目录结构

- `src/ugv_interfaces`：UGV 共享服务接口（`MissionStart.srv`）
- `src/ugv_bridge`：BX72 FPGA 遥测、命令、视频与 HTTP 网关
- `src/car/`：`car_bringup`、`car_navigation`、`car_vision`（内含自包含 ByteTrack 子集）
- `src/legged/`：`legged_bridge`、`legged_bringup`（ACG720 接入与雷达建图）

## 构建与启动

```bash
source /opt/ros/humble/setup.bash

# Jetson 小车完整构建
colcon build --symlink-install --executor sequential --packages-up-to car_bringup

# 无 TensorRT 的开发机（跳过 vision_node C++ 编译）
colcon build --symlink-install --packages-up-to car_bringup --cmake-args -DBUILD_TENSORRT_VISION=OFF

# 树莓派（ACG720 六足桥接端）
colcon build --symlink-install --packages-up-to legged_bridge

source install/local_setup.bash
```