继续开发 ROS2 AI Vision Rover。

Repository:
D:\ros2-ai-vision-rover

已完成基础能力：

Task 001:
ESP32 Bring-up

Task 002:
VL53L0X I2C / distance sensing

Task 004:
SG90 PWM control

现在开始：

\==============================
Task 005
SG90 + VL53L0X Active ToF Scanner
=================================

目标：

将 SG90 180-degree servo 和 VL53L0X ToF sensor 组合，
形成一个低成本主动二维环境扫描器。

Hardware:

ESP32-WROOM-32E

VL53L0X:
SDA GPIO21
SCL GPIO22

SG90 signal:
GPIO25

Arduino-ESP32 Core:
3.x

要求：

1. 创建：

firmware/esp32/arduino/tof\_scanner\_005/
tof\_scanner\_005.ino

2. 功能：

- Serial 115200
- Wire.begin(21, 22)
- 初始化 VL53L0X
- Continuous ranging
- 初始化 ESP32 LEDC servo PWM
- 使用 Arduino-ESP32 3.x API
- 不使用已经移除的 ledcSetup/ledcAttachPin

扫描范围第一版：

30 degrees to 150 degrees

step:
5 degrees

扫描方向：

30 -> 150
150 -> 30

每个角度：

1. Command servo position
2. Wait approximately 80-120 ms for settling
3. Read VL53L0X
4. Print angle and distance

Serial format必须方便以后机器解析，例如：

SCAN\_BEGIN
angle\_deg=30,distance\_mm=826
angle\_deg=35,distance\_mm=810
...
SCAN\_END

Timeout明确输出：

angle\_deg=90,distance\_mm=TIMEOUT

3. 代码拆成清晰函数：

pulseUsToDuty()
setServoAngle()
scanPoint()

避免把所有逻辑堆在 loop()。

4. 增加一轮扫描中的最近障碍物统计：

输出例如：

nearest\_angle\_deg=85
nearest\_distance\_mm=312

忽略 timeout。

5. 创建：

docs/tasks/TASK\_005.md

包含：

Goal
Architecture
Hardware
Wiring
Active scanning principle
Polar coordinates
Scan cycle
Power limitations
Build steps
Acceptance criteria
Hardware results

6. README更新当前进度。

7. 不实现：

Motor
Encoder
PID
ROS2
SLAM
Nav2
YOLO

Task 005只专注：

Perception + Actuation

8. 不伪造硬件测试。

9. 如果环境能够编译，则实际编译并报告结果。

Acceptance criteria:

- VL53L0X initializes
- SG90 initializes
- Servo repeatedly scans 30-150 degrees
- Distance changes with environment
- Serial outputs angle + distance pairs
- nearest obstacle angle/distance is calculated
- ESP32 does not unexpectedly reset
- No severe servo jitter or mechanical binding

最后输出：

A. 修改文件
B. 数据流
C. 编译结果
D. 实物安装与接线要求
E. 验收方法
F. git diff/status