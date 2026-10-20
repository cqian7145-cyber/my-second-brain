我建议最终 Demo 做 5 个能力：

1. **手机遥控**
2. **自动避障**
3. **YOLO目标识别**
4. **人体/目标跟随**
5. **自然语言控制**好，信息已经够了。路线就按你选的方案定：

> **Windows + Arduino 快速入门 1～2 天 → 再转 ESP-IDF → U盘到后装 Ubuntu 24.04 → ROS2 Jazzy。**  
> **ChatGPT 负责教学/架构/任务拆解，Codex 负责本地代码、配置、编译与仓库维护。**

我也检查了你的 GitHub 仓库：`cqian7145-cyber/ros2-ai-vision-rover` 已经存在，目前是一个空仓库，默认分支是 `main`，正适合从零按规范搭。

另外你这次发的电机参数非常关键。你的是 **MG310 + 霍尔编码器**，额定 **7.4V**、减速比标称 1:20（实际约 1:20.409）、空载约 500 RPM、额定约 400 RPM、堵转电流可到 2A；霍尔编码器标的是 **13 PPR**。这套可以用于我们的 PID + Odometry 项目。

要注意一个以后会专门讲的坑：

```text
电机编码器：13 PPR
        ↓
减速箱：约 20.409 : 1
        ↓
输出轮轴一圈

理论基础脉冲约：
13 × 20.409 ≈ 265 pulses/rev
```

但如果我们采用 AB 相四倍频：

```text
265 × 4 ≈ 1061 counts/rev
```

实际到底按 265、530 还是 1061 计，后面不会靠猜，我们会**实测标定**。

---

# ROS2 AI Vision Rover
## Task 001：建立工程 + 让 ESP32 第一次受我们控制

今天先**不接电机、不接18650、不接TB6612**。

第一课只做：

```text
电脑
 ↓ USB
ESP32
 ↓
程序运行
 ↓
串口输出
```

目标不是“Hello World 很简单”。

真正目标是先建立这条链：

```text
源代码
  ↓
编译
  ↓
机器代码
  ↓
USB烧录
  ↓
ESP32 Flash
  ↓
CPU执行
  ↓
Serial返回电脑
```

这条链跑不通，后面：

```text
PWM
Encoder
PID
I2C
Wi-Fi
ROS2
```

全部没意义。

---

## 今天 40～60 分钟怎么分

```text
0～10 min
建立项目仓库

10～25 min
Arduino ESP32环境

25～40 min
烧录第一个程序

40～50 min
理解 setup / loop / Serial

50～60 min
Git提交 + 验收
```

---

# 第一步：让 Codex 建项目骨架

你本地已有：

```text
ros2-ai-vision-rover/
```

我们最终希望它长这样：

```text
ros2-ai-vision-rover/
│
├── firmware/
│   └── esp32/
│       ├── arduino/
│       └── esp-idf/
│
├── ros2_ws/
│   └── src/
│
├── vision/
│
├── hardware/
│   ├── wiring/
│   └── datasheets/
│
├── docs/
│   ├── tasks/
│   └── notes/
│
├── scripts/
│
├── .gitignore
└── README.md
```

注意这里的思想：

```text
ESP32底层代码
    ≠
ROS2代码
    ≠
AI视觉代码
```

所以分开。

以后：

```text
firmware/
   ↓
“小脑”

ros2_ws/
   ↓
“神经系统”

vision/
   ↓
“视觉大脑”

hardware/
   ↓
“身体结构”
```

---

# 现在把下面整段丢给 Codex

```text
你正在协助我开发一个真实机器人项目：

项目名：
ROS2 AI Vision Rover

GitHub：
https://github.com/cqian7145-cyber/ros2-ai-vision-rover

本地目录：
ros2-ai-vision-rover

项目目标：
使用 ESP32 + 编码器差速底盘 + IMU + VL53L0X + SG90 + OLED
+ Ubuntu 24.04 + ROS2 Jazzy + OpenCV + YOLO，
逐步完成一个具有闭环电机控制、传感器融合、ROS2通信、
AI视觉识别和目标跟随能力的机器人小车。

你的职责主要是：
1. 创建和修改本地文件
2. 编写代码
3. 配置构建环境
4. 执行编译/测试
5. 根据终端报错修复问题
6. 维护合理的项目目录结构

不要一次实现完整机器人。
严格按 Task 顺序工作。

==============================
Task 001
Repository Bootstrap + ESP32 Bring-up
==============================

当前阶段：
Windows
Arduino 框架快速入门
Ubuntu / ROS2 尚未开始。

请完成：

1. 检查当前 ros2-ai-vision-rover 仓库状态。

2. 如果目录不存在，则创建：

firmware/
firmware/esp32/
firmware/esp32/arduino/
firmware/esp32/esp-idf/

ros2_ws/
ros2_ws/src/

vision/

hardware/
hardware/wiring/
hardware/datasheets/

docs/
docs/tasks/
docs/notes/

scripts/

3. 创建或完善 README.md。

README 至少写清楚：
- 项目名称
- 项目目标
- 当前硬件
- 总体架构
- 目录说明
- 当前进度
- Roadmap

当前硬件写：

- ESP32EA MOC / ESP32-WROOM-32E
- ESP32 EXP1 Expansion Board
- MG310 Hall Encoder Motor x2
- TB6612FNG
- MPU6050
- VL53L0X
- SG90 180-degree servo
- SSD1306 OLED
- USB UVC Camera
- 2WD differential chassis
- 2x18650 battery system
- DC-DC regulator

4. 创建：

docs/tasks/TASK_001.md

写清楚：

Goal:
Verify PC -> compile -> flash -> ESP32 -> serial monitor chain.

Acceptance criteria:
- Source code exists in repository
- Code compiles
- Firmware can be flashed
- Serial monitor receives periodic output
- No motor or external power is used in this task

1. 创建：

firmware/esp32/arduino/bringup_001/bringup_001.ino

内容保持极简。

要求：
- Serial.begin(115200)
- 启动时打印项目名
- 每秒打印一次 uptime
- 不控制未知 GPIO
- 不假设开发板自带 LED 对应哪个 GPIO
- 不接触电机
- 不使用外部库

期望串口输出类似：

ROS2 AI Vision Rover
Task 001 ESP32 Bring-up
uptime_ms=1000
uptime_ms=2000
uptime_ms=3000

6. 创建合理的 .gitignore。

7. 不要安装 ROS2。
8. 不要实现 PID。
9. 不要实现电机控制。
10. 不要修改与 Task 001 无关的内容。

完成后输出：

A. 创建/修改了哪些文件
B. 每个文件作用
C. 当前 git status
D. 我下一步在 Arduino IDE 中具体需要做什么
E. 如果当前环境允许，尝试做静态检查；不要假装已经烧录硬件
```

这里有一句特别重要：

> **“不要假装已经烧录硬件。”**

Codex能写代码，但最终：

```text
USB线
ESP32
COM口
实体硬件
```

这一段要在你电脑上真正验证。

---

# 第二步：你自己做 Arduino 环境

Windows 下载并安装 **Arduino IDE 2.x**。

打开 Arduino IDE 后：

```text
File
 ↓
Preferences
 ↓
Additional Boards Manager URLs
```

然后我们安装 ESP32 board package。

不过这里我暂时不让你随便复制网上某个 URL，因为 Espressif 的安装地址和当前稳定版本可能变化。

你安装 Arduino IDE 后，下一条把：

> **Arduino IDE 首页/版本号截图**

给我。

我再带你一步一步配置 ESP32，不让你一次装十堆东西。

---

# 为什么我们不用 Codex 一口气把全部代码写完？

因为这次目的不是“让 AI 帮你造完小车”。

你要真正学机器人。

所以我们的协作结构固定成：

```text
              ChatGPT
                 │
        为什么 / 原理 / 架构
                 │
                 ▼
               Task
                 │
                 ▼
               Codex
                 │
        代码 / 文件 / 编译
                 │
                 ▼
               你
                 │
        接线 / 烧录 / 测试
                 │
                 ▼
              真实硬件
                 │
           输出实验结果
                 │
                 ▼
             ChatGPT分析
```

以后每一个 Task 都这样。

这会比：

```text
“Codex 给我造个机器人”
```

学到的东西多得多。

---

# 顺便先认识今天最重要的两行 C/C++

之后 Codex 会生成类似：

```cpp
void setup() {
    Serial.begin(115200);
}

void loop() {
}
```

第一次看到很正常会觉得奇怪。

Arduino程序实际上不是：

```text
从第一行
↓
一直运行到最后一行
↓
结束
```

而是：

```text
ESP32启动
   ↓
setup()
只运行一次
   ↓
┌────────────┐
│   loop()   │
│      ↓     │
│   loop()   │
│      ↓     │
│   loop()   │
│      ↓     │
│    ...     │
└────────────┘
```

因此：

```cpp
setup()
```

相当于：

> 开机初始化。

以后这里会初始化：

```text
Serial
I2C
PWM
Encoder
IMU
Wi-Fi
```

而：

```cpp
loop()
```

相当于：

> 机器人不停活着的时候要干什么。

---

# 为什么这和机器人有关？

未来我们可能会有：

```cpp
void loop() {

    readEncoder();

    readIMU();

    calculateVelocity();

    pidUpdate();

    setMotorPWM();
}
```

也就是：

```text
读传感器
   ↓
估计状态
   ↓
控制算法
   ↓
控制电机
   ↓
再次读取
```

你之前一直看到的：

```text
Sense
 ↓
Think
 ↓
Act
 ↓
Sense again
```

第一次在代码里出现了。

---

## 今天唯一验收目标

最后你的 Arduino Serial Monitor 能稳定出现：

```text
ROS2 AI Vision Rover
Task 001 ESP32 Bring-up

uptime_ms=1000
uptime_ms=2000
uptime_ms=3000
uptime_ms=4000
...
```

我们就宣布：

> **Task 001 Bring-up PASS**

然后 Task 002 我们才会真正接一个外设，开始：

> **ESP32 + OLED：第一次学习 I²C**

之后：

```text
Task 003：VL53L0X距离传感器
Task 004：SG90 + PWM
Task 005：扫描雷达
Task 006：TB6612 单电机
Task 007：Encoder
Task 008：RPM
Task 009：PID
...
```

先让 Codex 执行我上面那段 **Task 001 提示词**。执行完把它最后的输出和 `git status` 发给我，我们马上继续验收。