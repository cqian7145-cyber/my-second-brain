# Task 003 — SSD1306 OLED：让机器人第一次“有仪表盘”

Task 002 让 Rover 能感知距离；Task 003 的目标是把这些内部数据直接显示出来。

最终效果：

```
┌────────────────────┐
│ ROS2 AI ROVER      │
│ Task 003           │
│                    │
│ Distance: 327 mm   │
│ Uptime:   18 s     │
└────────────────────┘
```

这次你会真正理解一个很重要的东西：

```
              同一条 I²C 总线
                     │
          ┌──────────┴──────────┐
          ↓                     ↓
      VL53L0X                 OLED
       0x29                   0x3C
```

也就是：

> **ESP32 的 SDA/SCL 不只能接一个设备。**

---

## 一、今天要学什么

大约 40～60 分钟：

```
0～10 min
理解 OLED / Pixel / Frame Buffer

10～20 min
OLED 接线 + I²C Scanner

20～35 min
显示 Hello Rover

35～50 min
VL53L0X + OLED 同时工作

50～60 min
理解数据流 + 验收
```

---

# 1. OLED 是什么

你买的是：

> **0.96 英寸 SSD1306 4 针 OLED**

通常分辨率：

```
128 × 64
```

意思是有：

```
128列 × 64行
= 8192 个像素
```

每个像素可以理解成一个很小的灯点。

比如字母：

```
H

█   █
█   █
█████
█   █
█   █
```

实际上就是程序决定哪些 Pixel 点亮。

---

# 2. 四个针分别是什么

你的 OLED 一般会写：

```
GND
VCC
SCL
SDA
```

对应：

```
OLED          ESP32

GND   ─────→  GND
VCC   ─────→  3V3
SCL   ─────→  SCL22
SDA   ─────→  SDA21
```

**以 OLED 板子上的丝印为准，不要按线的颜色猜。**

而且：

> Task 003 仍然 **不用电池**。

供电：

```
电脑 USB
   ↓
ESP32
   ├── 3.3V → OLED
   └── 3.3V → VL53L0X
```

---

# 3. 你之前问过黄色、蓝色 I²C 接哪排

现在就能体现为什么扩展板做两排了。

建议：

```
黄色 I2C → VL53L0X

蓝色 I2C → OLED
```

但是要理解：

```
黄色 SDA ─┐
          ├── GPIO21
蓝色 SDA ─┘

黄色 SCL ─┐
          ├── GPIO22
蓝色 SCL ─┘
```

它们不是两条不同 I²C。

它们是：

> **同一条总线的两个物理插口。**

很像插线板：

```
墙上的电
   ↓
插线板
├── 手机
├── 电脑
└── 台灯
```

---

# 4. 先做 Task 003A：扫描两个设备

如果 VL53L0X 还接着，不用拔。

把 OLED 接好以后，重新运行你 Task 002A 的 I²C Scanner。

正常情况下应该从：

```
I2C device found at 0x29
Total devices: 1
```

变成：

```
I2C device found at 0x29
I2C device found at 0x3C
Total devices: 2
```

其中：

```
0x29 = VL53L0X
0x3C = SSD1306 OLED
```

部分 OLED 可能是：

```
0x3D
```

所以如果出现：

```
0x29
0x3D
```

也不一定有问题。

这一步非常重要。

因为它证明：

```
             GPIO21 SDA
                 │
ESP32 ───────────┼──── OLED 0x3C
                 │
                 └──── VL53L0X 0x29


             GPIO22 SCL
                 │
ESP32 ───────────┼──── OLED
                 │
                 └──── VL53L0X
```

这就是机器人里所谓的：

> **Bus / 总线**

---

# 5. Arduino 安装 OLED 库

Arduino IDE：

```
库管理器
```

搜索并安装：

```
Adafruit SSD1306
```

它通常还会需要：

```
Adafruit GFX Library
```

如果 Arduino 提示安装 Dependencies：

> 选全部安装。

这里的关系：

```
Adafruit GFX
    ↓
负责“怎么画”

Adafruit SSD1306
    ↓
负责“怎么让 SSD1306 屏幕显示出来”
```

可以理解成：

```
GFX = 画家
SSD1306 library = OLED翻译员
```

---

# 6. Task 003B：第一次显示文字

新建：

```
firmware/esp32/arduino/oled_003/
└── oled_003.ino
```

代码：

```
#include <Wire.h>
#include <Adafruit_GFX.h>
#include <Adafruit_SSD1306.h>

#define SCREEN_WIDTH 128
#define SCREEN_HEIGHT 64

#define SDA_PIN 21
#define SCL_PIN 22

#define OLED_ADDRESS 0x3C

Adafruit_SSD1306 display(
    SCREEN_WIDTH,
    SCREEN_HEIGHT,
    &Wire,
    -1
);

void setup() {
  Serial.begin(115200);
  delay(500);

  Serial.println();
  Serial.println("ROS2 AI Vision Rover");
  Serial.println("Task 003 - OLED Bring-up");

  Wire.begin(SDA_PIN, SCL_PIN);

  if (!display.begin(
          SSD1306_SWITCHCAPVCC,
          OLED_ADDRESS)) {

    Serial.println("ERROR: OLED initialization failed!");

    while (true) {
      delay(1000);
    }
  }

  Serial.println("OLED initialized");

  display.clearDisplay();

  display.setTextSize(1);
  display.setTextColor(SSD1306_WHITE);

  display.setCursor(0, 0);
  display.println("ROS2 AI Rover");

  display.setCursor(0, 16);
  display.println("Task 003");

  display.display();
}

void loop() {

  display.clearDisplay();

  display.setCursor(0, 0);
  display.println("ROS2 AI Rover");

  display.setCursor(0, 16);
  display.println("Task 003");

  display.setCursor(0, 32);
  display.print("Uptime: ");
  display.print(millis() / 1000);
  display.println(" s");

  display.display();

  delay(200);
}
```

---

# 7. 为什么必须 `display.display()`？

这是今天一个值得理解的概念。

你执行：

```
display.print("Hello");
```

并不一定是立刻一个字一个字发送给 OLED。

通常更像：

```
C++代码
   ↓
在 ESP32 RAM 里先画
   ↓
Frame Buffer
   ↓
display.display()
   ↓
通过 I²C 整块发送
   ↓
OLED
```

可以理解成：

> 先在草稿纸上把整张画画完，再一次贴到屏幕上。

所以：

```
display.clearDisplay();
```

清空草稿纸。

```
display.print();
```

在草稿纸上画。

```
display.display();
```

真正交给 OLED。

---

# 8. Task 003C：真正有用的版本

单纯显示：

```
Hello World
```

对我们的机器人意义不大。

最终 Task 003 应该把 Task 002 和 Task 003 合并：

```
真实物体
   ↓
VL53L0X
   ↓
distance_mm
   ↓
ESP32
   ├────────→ Serial Monitor
   │
   └────────→ OLED
```

屏幕：

```
┌────────────────────┐
│ ROS2 AI Rover      │
│                    │
│ DIST: 425 mm       │
│                    │
│ Sensor: OK         │
└────────────────────┘
```

这个以后就是你机器人的最初版仪表盘。

之后可以继续增加：

```
Battery: 7.6V
WiFi: OK
ROS2: Connected
Mode: AUTO
Distance: 425mm
Left RPM: 201
Right RPM: 198
```

所以 OLED 后面会非常适合调试。

---

# 9. 给 Codex 的 Task 003

把下面直接交给 Codex：

```
继续开发：

ROS2 AI Vision Rover

Repository:
D:\ros2-ai-vision-rover

已完成：

Task 001
ESP32 Bring-up

Task 002
VL53L0X I2C detection / distance sensing

现在开始：

==============================
Task 003
SSD1306 OLED + Shared I2C Bus
==============================

目标：

让 ESP32 使用 SSD1306 OLED 显示系统状态，
并最终把 VL53L0X 的实时距离同时输出到 Serial 和 OLED。

当前平台：

ESP32-WROOM-32E
Arduino framework

I2C:
SDA = GPIO21
SCL = GPIO22

预期设备：
VL53L0X = 0x29
SSD1306 = usually 0x3C

要求：

1. 创建：

firmware/esp32/arduino/oled_003/oled_003.ino

2. 使用：

Adafruit GFX Library
Adafruit SSD1306

3. 第一阶段实现：

OLED显示：

ROS2 AI Rover
Task 003
Uptime: XX s

4. 然后创建综合版本：

firmware/esp32/arduino/display_distance_003/
display_distance_003.ino

功能：

- 初始化 Wire
- 初始化 VL53L0X
- 初始化 SSD1306
- VL53L0X 连续测距
- Serial 输出 distance_mm
- OLED 同时显示 distance
- 显示 sensor OK / timeout
- 刷新周期约 200 ms

5. 避免：

- Motor
- Encoder
- Servo
- Wi-Fi
- ROS2
- PID

6. 创建：

docs/tasks/TASK_003.md

包含：

Goal
Hardware
I2C architecture
Wiring
Libraries
Build steps
Acceptance criteria
Hardware results

7. 更新 README：

Task 001 PASS
Task 002 PASS / 根据实际状态记录
Task 003 IN PROGRESS

8. 不要伪造硬件测试结果。

Acceptance criteria：

- I2C scanner detects OLED
- OLED initializes
- Text is visible
- uptime updates
- VL53L0X and OLED coexist on same I2C bus
- distance changes on OLED as object moves
- Serial output remains functional

最后输出：

A. 文件变化
B. 架构说明
C. 编译结果
D. 所需 Arduino libraries
E. 实物验收步骤
F. git diff/status
```

---

# Task 003 最重要的知识点

你今天不要只记：

```
OLED = 显示屏
```

真正要记住：

```
             I2C BUS
                │
      ┌─────────┴─────────┐
      ↓                   ↓
VL53L0X 0x29          OLED 0x3C
      │                   │
      └─────────┬─────────┘
                ↓
              ESP32
```

**总线 + 地址** 是嵌入式非常核心的思想。

等 Task 003 完成之后，Task 004 就完全换一个方向：

> **SG90 + PWM：让机器人第一次主动改变真实世界，而不是只感知和显示。**

也就是说目前路线：

```
Task 001
ESP32运行程序
     ↓
Task 002
机器人能感知
     ↓
Task 003
机器人能显示内部状态
     ↓
Task 004
机器人能产生动作
     ↓
Task 005
感知 + 动作
= 180°扫描雷达
```