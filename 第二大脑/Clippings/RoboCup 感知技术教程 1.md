---
title: RoboCup 感知技术教程
source: https://chatgpt.com/c/6a4a14be-0bcc-83eb-8607-9a208ac072b5?mweb_fallback=1
author:
published:
created: 2026-07-07
description: ChatGPT conversation with 4 messages
tags:
  - clippings
---
robocup 机器人比赛感知方向技术栈 网上像GitHub有没有相关的教程 源代码 robocup 机器人比赛感知方向技术栈 网上像GitHub有没有相关的教程 源代码

---

如果你的目标是参加 **RoboCup（尤其是机器人足球、人形机器人、@Home 等）** ，感知方向的技术栈基本都是下面这一套。

## RoboCup 感知方向技术栈

```
Linux (Ubuntu)
        ↓
ROS2
        ↓
OpenCV
        ↓
YOLO (目标检测)
        ↓
深度相机 / 双目相机
        ↓
点云(Point Cloud)
        ↓
SLAM / 定位
        ↓
TensorRT部署优化
```

其中最重要的是：

---

## GitHub 开源项目（非常值得学习）

### ① yolo\_ros（目前非常推荐）

官方把 YOLO 和 ROS2 封装好了。

GitHub：

- [^1]

支持：

- YOLOv8
- YOLOv9
- YOLOv10
- YOLOv11
- ROS2 Humble/Jazzy

还能直接发布 ROS Topic。 [^1]

---

### ② darknet\_ros（经典项目）

很多 RoboCup 老队伍都参考过。

GitHub：

- [^2]

内容包括：

- ROS
- YOLO
- 图像订阅
- Bounding Box 发布

虽然偏旧，但架构很值得学习。 [^2]

---

### ③ RoboCup 官方 Demo

如果是机器人足球，这个很有参考价值。

GitHub：

- [^3]

里面直接拆成：

- vision（视觉）
- brain（决策）
- game\_controller（裁判通信）

vision 就是：

```
Camera
↓

YOLO

↓

坐标计算

↓

ROS Topic

↓

Brain
```

这是 RoboCup 常见的软件架构。 [^3]

---

## 推荐学习教程

### OpenCV

学会：

- HSV颜色识别
- 边缘检测
- 轮廓
- 相机标定
- PnP
- 图像坐标→世界坐标

---

### ROS2 + YOLO

推荐：

- [^4]

以及

- [^5]

里面收集了大量 ROS2 学习资源。 [^4]

---

## 如果你要走 RoboCup 感知方向，我建议按这个路线学（2~3个月）

1. Linux（Ubuntu）
2. Git + GitHub
3. C++
4. Python
5. ROS2
6. OpenCV
7. YOLO
8. 相机标定
9. AprilTag
10. TensorRT
11. CUDA
12. 点云（PCL）
13. 深度相机（Intel RealSense）

---

结合我们之前聊过你的情况（机器人工程专业，希望走机器人感知，同时长期目标是 AI 和创业），我建议你 **优先投入 ROS2 + OpenCV + YOLO + TensorRT** 。这是目前 RoboCup、机器人公司和自动驾驶感知岗位都非常通用的核心技能，之后再深入点云、SLAM 和多传感器融合，会形成更完整的感知能力。

---

https://github.com/BoosterRobotics/robocup\_demo?utm\_source=chatgpt.com 怎么学

---

这个仓库（robocup\_demo）本质上不是“教程”，而是 **一套已经能跑 RoboCup 比赛的完整工程模板（production-level demo）** ，结构比较工程化，所以正确学习方式不是“看一遍”，而是 **拆模块 + 跑起来 + 改参数 + 重写一部分逻辑** 。

我给你一个非常实用的学习路径（按 RoboCup 真实队伍方式来学）。

---

## 一、先搞清楚它的结构（最关键）

这个 repo = 3个系统：

```
vision（感知）
    ↓
brain（决策）
    ↓
game_controller（裁判/比赛状态）
```

你可以理解为：

| 模块 | 作用 | 你要学什么 |
| --- | --- | --- |
| vision | 看见球/机器人 | OpenCV + YOLO + 坐标变换 |
| brain | 做决策 | 状态机 + 行为树 |
| game\_controller | 比赛规则 | ROS2 topic + UDP广播 |

---

## 二、正确学习顺序（非常重要）

## Step 0：先不要看代码（90%人错在这里）

先做一件事：

👉 把 RoboCup demo 跑起来（哪怕是仿真）

你目标是：

- 能看到摄像头画面
- 能看到 detection box
- ROS topic 在发布数据

---

## Step 1：只学 vision（感知层）

路径：

```
src/vision
```

你重点看3件事：

### ① YOLO推理

关键词：

- best.engine / onnx
- TensorRT
- detection output

你要理解：

```
image → YOLO → bbox → class（ball / robot）
```

---

### ② 坐标转换（RoboCup核心）

这是最关键知识点：

```
像素坐标 → 相机坐标 → 世界坐标
```

涉及：

- 相机标定（intrinsics）
- PnP
- 透视变换

👉 RoboCup本质不是识别，而是“定位”

---

### ③ ROS2发布

你会看到：

- /vision/ball
- /vision/robot
- /camera/image

你要搞懂：

```
vision = ROS publisher
brain = ROS subscriber
```

---

## Step 2：学 brain（决策系统）

路径：

```
src/brain
```

这里本质是：

### 👉 状态机（FSM）

典型逻辑：

```
SEARCH_BALL
→ GOTO_BALL
→ ALIGN_GOAL
→ KICK
→ BACK_TO_DEFENSE
```

你要重点理解：

- state machine
- behavior tree（有些队用）
- control loop（50Hz）

---

## Step 3：game\_controller（比赛规则输入）

这个比较简单：

它只是把：

```
RoboCup referee UDP packet
→ ROS topic
```

你只需要知道：

- 比赛状态（READY / PLAYING / STOP）
- 红黄牌
- 分数

---

## 三、你真正要“抄作业”的地方

这个仓库最有价值的不是算法，而是：

## ✔ 工程结构

你要重点看：

```
configs/
scripts/
src/vision/
src/brain/
third_party/
```

---

## ✔ ROS2通信设计

重点：

- topic设计
- message结构
- node拆分方式

---

## ✔ YOLO + robotics pipeline

真正工业结构：

```
Camera
→ Preprocess
→ YOLO TensorRT
→ postprocess
→ coordinate transform
→ ROS topic
```

---

## 四、建议你这样“拆代码学”（非常关键）

不要从 main 开始读。

按这个顺序：

## ① 先找入口

```bash
scripts/start.sh
```

看它启动了哪些 node

---

## ② 找 vision node

找：

```
vision.cpp / vision_node.cpp
```

---

## ③ 找 YOLO inference

关键词搜索：

```bash
grep -r "engine"
grep -r "TensorRT"
grep -r "detect"
```

---

## ④ 找坐标转换

关键词：

- camera\_matrix
- homography
- PnP
- transform

---

## 五、你要达到的目标（学习成果定义）

如果你学懂这个 repo，你应该能做到：

### ✔ 能自己写一个 RoboCup vision module

输入：

- camera image

输出：

- ball position (x,y,z)
- robot position
- goal position

---

### ✔ 能改 brain 战术

例如：

- 改成“优先防守”
- 改成“强制射门”
- 改成“跟球策略”

---

### ✔ 能替换 YOLO模型

比如：

---

## 六、如果你想走“比赛/就业路线”，重点升级技能

优先级排序：

### 🔥 必学（核心）

- ROS2
- OpenCV
- YOLO
- 相机标定
- PnP

### ⚙️ 加分

- TensorRT
- CUDA
- C++
- Linux系统开发

### 🚀 高阶

- SLAM（ORB-SLAM3）
- 多传感器融合
- Behavior Tree

---

## 七、如果你要我帮你进一步拆

我可以帮你做更具体的东西，比如：

- ✔ 把 vision 模块画成流程图
- ✔ 帮你逐文件解释 robocup\_demo
- ✔ 帮你改成“最简单可跑版本”
- ✔ 或者帮你做“RoboCup入门最短路径（2周版）”

直接说你现在卡在哪一层（代码 / ROS / YOLO / 坐标变换），我可以帮你拆到“可以动手改代码”的级别。

[^1]: [github.com](https://github.com/mgonzs13/yolo_ros?utm_source=chatgpt.com)

[^2]: [github.com](https://github.com/leggedrobotics/darknet_ros?utm_source=chatgpt.com)

[^3]: [github.com](https://github.com/BoosterRobotics/robocup_demo?utm_source=chatgpt.com)

[^4]: [enginksz.github.io](https://enginksz.github.io/projects/YOLOv8-with-ROS2/?utm_source=chatgpt.com)

[^5]: [fkromer.github.io](https://fkromer.github.io/awesome-ros2/?utm_source=chatgpt.com)