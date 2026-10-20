# Task 002 — ESP32 + VL53L0X：第一次让机器人“感知距离”

今天开始真正接传感器。

目标不是单纯“读出一个数字”，而是第一次打通：

```
真实世界
   ↓
距离
   ↓
VL53L0X
   ↓ I²C
ESP32
   ↓
C/C++ 程序
   ↓ Serial
电脑
```

完成后，你拿手靠近传感器，串口里的距离应该跟着变化：

```
distance_mm=812
distance_mm=603
distance_mm=421
distance_mm=207
...
```

这就是你的 Rover 第一次真正获得 **Perception（感知）**。

---

## 0～10 分钟：先理解 VL53L0X

VL53L0X 是一个 **ToF（Time of Flight，飞行时间）测距传感器**。

思路非常直观：

```
VL53L0X
   │
   │ 发出红外光
   ▼
  物体
   │
   │ 光反射回来
   ▼
VL53L0X
```

它测：

```
光出去
↓
碰到物体
↓
反射回来
↓
用了多长时间
↓
计算距离
```

抽象成：

```
距离 ≈ 光速 × 时间 / 2
```

除以 2，因为光走了：

```
传感器 → 物体 → 传感器
```

实际测量和内部补偿当然复杂得多，但底层思想就是这个。

以后真正的 LiDAR，本质上也和这个思想有关。

---

# 10～20 分钟：第一次学 I²C

VL53L0X 和 ESP32 不是通过“一个 GPIO 一个数据”通信，而是通过：

> **I²C 总线**

最基本只有两根通信线：

```
SDA = Serial Data
数据线

SCL = Serial Clock
时钟线
```

再加供电：

```
VCC
GND
```

于是一个传感器最基础就是：

```
ESP32                  VL53L0X

3.3V  ───────────────→ VCC
GND   ───────────────→ GND

SDA   ←──────────────→ SDA
SCL   ───────────────→ SCL
```

这里最重要的是理解：

### SDA 是“说什么”

例如：

```
读取距离
↓
返回 423 mm
```

这些数据走 SDA。

### SCL 是“什么时候说”

ESP32不断提供时钟：

```
tick tick tick tick...
```

双方按照节拍传数据。

可以类比：

```
SDA = 两个人说的话
SCL = 老师打拍子

1、2、3、4
按节拍传输每一位数据
```

---

# 20～30 分钟：接线

**先拔掉 ESP32 USB。**

不要带电插线。

你这块 ESP32EXP1 已经有专门引出的：

```
SDA
SCL
```

所以优先使用板子上明确标着 **SDA / SCL** 的接口，不用我们现在自己找 GPIO。

通常经典 ESP32 默认是：

```
SDA = GPIO21
SCL = GPIO22
```

但是既然你的扩展板已经标出来了，我们直接使用：

```
VL53L0X       ESP32EXP1

VCC      →    3.3V
GND      →    GND
SDA      →    SDA
SCL      →    SCL
```

### 为什么我现在让你用 3.3V

ESP32 本身是：

```
3.3V logic
```

很多常见 VL53L0X 模块板带稳压/电平转换，也能吃 5V，但我们还没有确认你具体那块 VL53L0X 模块。

所以第一阶段：

> **优先用 3.3V。**

更稳妥。

如果你的 VL53L0X 上还有：

```
XSHUT
GPIO1 / INT
```

今天都不接。

Task 002 只需要：

```
VCC
GND
SDA
SCL
```

---

# 一个重要概念：I²C Address

假设以后：

```
ESP32
 │
 ├── OLED
 ├── VL53L0X
 └── MPU6050
```

它们竟然可以共用：

```
SDA
SCL
```

ESP32 怎么知道自己在和谁说话？

靠：

> **I²C 地址**

例如通常：

```
VL53L0X  → 0x29
MPU6050  → 0x68
OLED     → 0x3C
```

就像一栋楼：

```
SDA/SCL = 一条走廊

0x29 = VL53L0X 房间
0x68 = MPU6050 房间
0x3C = OLED 房间
```

ESP32：

```
“0x29，你把距离告诉我。”
```

只有 VL53L0X 回答。

这就是为什么两根线可以挂很多设备。

---

# 30～40 分钟：Arduino 库

为了第一阶段快速入门，我们继续 Arduino。

Arduino IDE：

```
工具
→ 管理库
```

搜索：

```
VL53L0X
```

我建议先装：

> **VL53L0X by Pololu**

它的 API 很直接，非常适合我们理解传感器读取流程。

---

# 我们今天的程序应该长什么样？

核心思想：

```
setup()
{
    初始化串口;
    初始化I2C;
    初始化VL53L0X;
    启动连续测距;
}

loop()
{
    读取距离;
    打印距离;
    等待一小会;
}
```

机器人语言翻译：

```
开机
↓
准备传感器
↓
不停测距离
↓
不停把距离发给电脑
```

---

# Task 002 Codex 提示词

把下面整段直接交给 Codex：

```
继续开发项目：

ROS2 AI Vision Rover

本地目录：
D:\ros2-ai-vision-rover

当前状态：
Task 001 ESP32 Bring-up 已完成。
ESP32-WROOM-32E 已能成功编译、烧录，并通过 115200 串口持续输出 uptime。

现在开始：

==============================
Task 002
VL53L0X ToF Sensor Bring-up
==============================

目标：

使用 ESP32 + VL53L0X，
通过 I2C 读取真实距离数据，并通过 Serial 输出。

当前阶段：
Windows
Arduino framework

不要实现：
- OLED
- SG90
- Motor
- Encoder
- PID
- ROS2
- Wi-Fi

硬件连接预期：

VL53L0X VCC -> ESP32 3.3V
VL53L0X GND -> ESP32 GND
VL53L0X SDA -> ESP32 SDA
VL53L0X SCL -> ESP32 SCL

优先使用 Pololu VL53L0X Arduino library。

请执行：

1. 创建：

firmware/esp32/arduino/vl53l0x_002/vl53l0x_002.ino

2. 程序要求：

- Serial.begin(115200)
- Wire.begin()
- 初始化 VL53L0X
- 设置合理 timeout
- 使用连续测距模式
- 每约 200 ms 输出一次距离
- 输出单位为 mm
- 如果 timeout，明确输出错误
- 不控制其他 GPIO
- 不使用 delay(1000) 这种很长的阻塞
- 保持代码简单，适合初学者阅读

期望输出类似：

ROS2 AI Vision Rover
Task 002 VL53L0X Bring-up
VL53L0X initialized

distance_mm=523
distance_mm=519
distance_mm=517

如果超时：

VL53L0X_TIMEOUT

3. 更新：

docs/tasks/TASK_002.md

内容包括：

Goal
Hardware
Wiring
Concepts
Build steps
Acceptance criteria
Test results

Acceptance criteria：

- ESP32 code compiles
- Firmware flashes successfully
- VL53L0X initializes
- Serial reports distance in millimeters
- Moving an object toward/away from sensor changes readings
- Sensor timeout is handled
- No unrelated hardware is used

4. 更新 README 当前进度：

Task 001 PASS
Task 002 IN PROGRESS

5. 不要假装完成硬件测试。
实体传感器验证必须由用户完成。

6. 如果当前 Arduino CLI / 环境能够编译，就执行编译。
如果缺少 VL53L0X library，只报告需要安装什么，不要伪造成功。

完成后输出：

A. 修改的文件
B. 代码工作流程
C. 所需 Arduino library
D. 编译结果
E. 用户下一步实物操作
F. git diff/status
```

---

# 40～50 分钟：烧录

接线完成以后：

```
ESP32 + VL53L0X
       │
       └─ USB → PC
```

这次有一个变化。

Task 001 烧录时，我们手动：

```
GPIO0 → GND
```

才进入下载模式。

所以如果这次又报：

```
Wrong boot mode
```

不用慌，还是老办法：

```
GPIO0 → GND
↓
RESET
↓
上传
↓
出现 Writing...
↓
拔掉 GPIO0-GND
↓
烧录完成
↓
RESET
```

注意：

> **GPIO0 临时线和 VL53L0X 的 GND 不是一回事。**

VL53L0X 的 GND 正常情况下始终连接。

GPIO0-GND 只是烧录的时候临时短接。

---

# 50～60 分钟：实验

程序跑起来以后，我们不要只看它有没有数字。

做三个实验。

### 实验 A：手靠近

例如：

```
距离约 80cm

distance_mm=796
distance_mm=801
distance_mm=793
```

手慢慢靠近：

```
600
↓
400
↓
250
↓
120
```

如果数值跟着变化：

> 感知链 PASS。

### 实验 B：手远离

```
150
↓
300
↓
500
↓
800
```

验证不是随机数字。

### 实验 C：快速移动

你会发现：

```
测量
并不是瞬时的
```

传感器有：

```
Sampling Rate
采样频率
```

例如我们程序 200ms 读一次：

```
1秒 / 0.2秒 = 5 Hz
```

也就是：

> 每秒约 5 次读数。

以后机器人运动起来，就会出现一个很重要的问题：

```
机器人速度太快
+
传感器采样太慢
=
机器人来不及发现障碍物
```

这就是为什么机器人里：

> **采样频率非常重要。**

---

# 今天你真正要理解的数据流

不要只记代码。

记住：

```
            真实物体
               │
               ▼
           VL53L0X
               │
          distance_mm
               │
             I²C
        SDA + SCL
               │
               ▼
             ESP32
               │
          C/C++变量
               │
               ▼
        Serial.println
               │
             USB
               │
               ▼
             PC
```

以后 ROS2 只是再往下面接：

```
VL53L0X
↓
ESP32
↓
Serial
↓
ROS2 Node
↓
/distance
```

再以后：

```
/distance
↓
Obstacle Detection
↓
Planner
↓
/cmd_vel
↓
ESP32
↓
Motor
```

所以今天测出来的一个：

```
523 mm
```

最终真的会影响机器人怎么运动。

---

## 自测

**Q1：为什么 VL53L0X 不需要一个 GPIO 一根数据线？**

答案：

> 因为它使用 I²C 总线，通过 SDA/SCL 通信，并通过设备地址区分设备。

**Q2：SDA 和 SCL 分别是什么？**

答案：

```
SDA = 数据
SCL = 时钟
```

**Q3：为什么 VL53L0X 和 OLED 以后可以同时接 ESP32？**

答案：

> 它们可以共享 I²C 总线，只要设备地址不冲突。

**Q4：为什么现在先用 3.3V？**

答案：

> ESP32 是 3.3V 逻辑，而且还没有确认你的具体 VL53L0X 模块板是否具有完整的 5V 电平转换，因此先采用更保守的供电方式。

---

你现在先做两件事：

> **① 把 VL53L0X 实物拍一张正面清晰照片给我。**  
> **② 同时让 Codex 执行上面的 Task 002。**

我看完传感器实物后，再给你确认 **VCC/GND/SDA/SCL 到你 ESP32EXP1 上具体插哪四个孔**，这一段不要凭感觉乱接。