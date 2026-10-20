继续开发：

ROS2 AI Vision Rover

Repository:
D:\ros2-ai-vision-rover

开始：

\==============================
Task 004
SG90 Servo + PWM Bring-up
=========================

目标：

使用 ESP32 原生 LEDC PWM 控制 SG90 180-degree servo，
理解 PWM pulse width 与 servo position 的关系。

环境：

ESP32-WROOM-32E
Arduino-ESP32 Core 3.x
Arduino framework

Servo signal pin:
GPIO25

重要：

Arduino-ESP32 3.x 不要使用已经移除的：
ledcSetup()
ledcAttachPin()

使用当前 3.x API，例如：
ledcAttach()
ledcWrite()

不要使用 ESP32Servo library。
这个 Task 的目的之一就是直接理解 PWM。

创建：

firmware/esp32/arduino/servo\_004/servo\_004.ino

要求：

- Serial 115200
- PWM frequency 50 Hz
- 使用合理分辨率，例如 16 bit
- 实现 pulseUsToDuty()
- 实现 setServoAngle()
- 初始脉宽范围先保守使用约 1000-2000 us
- 开机先移动至90度
- 测试30、90、150度
- Serial 输出 angle\_deg 和 pulse\_us
- 不控制电机
- 不实现 ROS2
- 不实现 PID
- 不修改 Task 002/003 的功能

然后创建第二个示例：

firmware/esp32/arduino/servo\_serial\_004/
servo\_serial\_004.ino

功能：

用户从 Serial Monitor 输入 0-180 的整数角度。
ESP32解析输入。
合法则移动舵机。
非法则明确打印错误。
启动时默认90度。

创建：

docs/tasks/TASK\_004.md

包含：

Goal
PWM principle
Servo pulse-width principle
Wiring
Power considerations
Common ground
Build steps
Acceptance criteria
Hardware test results

Acceptance criteria:

1. Firmware compiles
2. Servo initializes at approximately 90 degrees
3. Servo moves between 30/90/150 degrees
4. ESP32 does not reset during motion
5. Serial reports commanded angle and pulse width
6. Serial interactive control accepts 0-180
7. No unrelated hardware behavior is introduced

不要伪造实体硬件测试结果。

最后输出：

A. 修改文件 B. PWM数据流 C. 编译结果 D. 实物接线要求 E. 验收步骤 F. git diff/status  全部文件push到github仓库