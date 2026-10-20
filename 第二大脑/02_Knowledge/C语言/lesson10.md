# Day 10｜多文件 C 工程：`main.c + joint.c + joint.h`

前 9 天我们已经完成：

```
变量 → if → 循环 → 数组 → 函数
→ 指针 → 数组与指针 → struct
→ Joint joints[6]
```

今天先不增加新的复杂语法，而是做一次非常重要的**工程化升级**。之前所有代码都塞在一个 `.c` 文件里；今天第一次把机器人程序拆成多个文件，并真正理解 Day 1 提到的：

```
源代码
 ↓
编译 Compile
 ↓
目标文件 .o
 ↓
链接 Link
 ↓
可执行程序
```

这也是以后读 C/C++、ROS 2、ESP32 工程必须具备的能力。

---

## 0～5 分钟｜复习：我们为什么需要拆文件？

Day 9 有：

```
typedef struct
{
    int id;
    double angle;
    double velocity;
    double torque;
    int enabled;
} Joint;
```

以及：

```
int is_joint_safe(const Joint *joint);
void print_joint(const Joint *joint);
```

再加上：

```
int main(void)
{
    ...
}
```

现在还不长。

但未来会出现：

```
机械臂程序
│
├─ 关节状态
├─ 传感器
├─ 正运动学
├─ 逆运动学
├─ 控制器
├─ 矩阵运算
├─ 相机
└─ main
```

如果全部塞进：

```
robot.c
```

最终可能几千行。

更合理的是：

```
robot_project/
│
├── main.c
│
├── joint.c
└── joint.h
```

今天先理解三者职责：

```
joint.h
“这个模块提供什么？”

joint.c
“这些功能具体怎么实现？”

main.c
“我要怎么使用这些功能？”
```

---

# 5～20 分钟｜核心：`.h`、`.c`、编译和链接

先建立项目：

```
mkdir -p ~/c-learning/day10
cd ~/c-learning/day10
code .
```

创建三个文件：

```
touch main.c joint.c joint.h
```

执行：

```
ls
```

应该看到：

```
joint.c
joint.h
main.c
```

---

## ① `joint.h`：模块的“说明书”

写：

```
#ifndef JOINT_H
#define JOINT_H

#define MIN_ANGLE -180.0
#define MAX_ANGLE 180.0
#define MAX_VELOCITY 5.0
#define MAX_TORQUE 10.0

typedef struct
{
    int id;

    double angle;
    double velocity;
    double torque;

    int enabled;

} Joint;

int is_joint_safe(const Joint *joint);

void print_joint(const Joint *joint);

#endif
```

这里主要有两类东西：

```
数据类型
↓
Joint

函数声明
↓
is_joint_safe()
print_joint()
```

---

# 什么叫函数声明？

之前写完整函数：

```
int is_joint_safe(const Joint *joint)
{
    ...
}
```

这叫函数的：

**definition（定义）**

因为我们真的告诉编译器：

> 这个函数内部具体做什么。

但头文件中：

```
int is_joint_safe(const Joint *joint);
```

只有一行，没有 `{}`。

这是：

**declaration（声明）**

相当于告诉其他代码：

> 有这么一个函数。  
> 输入 `const Joint *`，返回 `int`。

可以类比餐厅：

```
joint.h
↓
菜单

“我们提供 is_joint_safe()”


joint.c
↓
厨房

“具体怎么做在这里”
```

---

## Python 对比

Python 常见：

```
from joint import is_joint_safe
```

然后解释器加载对应模块。

C 的组织方式不同：

```
#include "joint.h"
```

让当前源文件获得相应的**声明/类型等接口信息**；而真正的函数实现通常在另一个 `.c` 文件中，最后通过**链接**组合起来。

所以不要把：

```
#include "joint.h"
```

简单理解成 Python 的：

```
import joint
```

它们解决的问题有相似之处，但机制不同。

---

# `#include <stdio.h>` 和 `#include "joint.h"`

你已经写了很多次：

```
#include <stdio.h>
```

现在：

```
#include "joint.h"
```

初学阶段可以这样区分：

```
<stdio.h>
↓
系统/标准库头文件

"joint.h"
↓
我们自己的项目头文件
```

---

# 那三个奇怪的宏是什么？

```
#ifndef JOINT_H
#define JOINT_H

...

#endif
```

这叫：

**include guard（头文件保护）**

暂时理解为：

> 防止同一个头文件在一次预处理过程中被重复包含，导致重复定义等问题。

固定模板：

```
#ifndef XXX_H
#define XXX_H

// 内容

#endif
```

现阶段会用即可。

---

## ② `joint.c`：真正实现功能

写：

```
#include <stdio.h>

#include "joint.h"


int is_joint_safe(const Joint *joint)
{
    if (joint->angle < MIN_ANGLE ||
        joint->angle > MAX_ANGLE)
    {
        return 0;
    }

    if (joint->velocity < -MAX_VELOCITY ||
        joint->velocity > MAX_VELOCITY)
    {
        return 0;
    }

    if (joint->torque < -MAX_TORQUE ||
        joint->torque > MAX_TORQUE)
    {
        return 0;
    }

    if (!joint->enabled)
    {
        return 0;
    }

    return 1;
}


void print_joint(const Joint *joint)
{
    printf(
        "Joint %d | "
        "angle=%7.2f | "
        "velocity=%6.2f | "
        "torque=%6.2f | ",
        joint->id,
        joint->angle,
        joint->velocity,
        joint->torque
    );

    if (is_joint_safe(joint))
    {
        printf("OK\n");
    }
    else
    {
        printf("UNSAFE\n");
    }
}
```

注意：

```
#include "joint.h"
```

所以 `joint.c` 知道：

```
Joint 是什么
MAX_TORQUE 是什么
函数接口是什么
```

---

## ③ `main.c`：负责程序流程

写：

```
#include <stdio.h>

#include "joint.h"

#define JOINT_COUNT 6


int main(void)
{
    Joint joints[JOINT_COUNT] =
    {
        {1, 10.0, 1.0, 2.0, 1},
        {2, 20.0, 1.5, 3.0, 1},
        {3, 30.0, 2.0, 4.0, 1},
        {4, 40.0, 2.5, 5.0, 1},
        {5, 50.0, 3.0, 6.0, 1},
        {6, 60.0, 3.5, 7.0, 1}
    };

    int unsafe_count = 0;

    printf("=== Robot Arm Controller v1.0 ===\n\n");

    for (int i = 0; i < JOINT_COUNT; i++)
    {
        print_joint(&joints[i]);

        if (!is_joint_safe(&joints[i]))
        {
            unsafe_count++;
        }
    }

    printf("\n=== Decision ===\n");

    if (unsafe_count == 0)
    {
        printf("ARM CAN MOVE\n");
    }
    else
    {
        printf("EMERGENCY STOP\n");
        printf("Unsafe joints: %d\n", unsafe_count);
    }

    return 0;
}
```

现在结构已经明显漂亮：

```
main.c
│
│  “我要检查六个关节”
│
├────→ print_joint()
│
└────→ is_joint_safe()


joint.c
│
│  “具体怎么检查？”
│
└────→ 实现


joint.h
│
│  “Joint 和这些函数是什么？”
│
└────→ 对外接口
```

---

# 现在真正理解 GCC：编译 + 链接

以前：

```
gcc main.c -o robot
```

现在有两个 `.c`：

```
gcc -Wall -Wextra -g main.c joint.c -o robot_arm
```

运行：

```
./robot_arm
```

应该得到类似：

```
=== Robot Arm Controller v1.0 ===

Joint 1 ... OK
Joint 2 ... OK
Joint 3 ... OK
Joint 4 ... OK
Joint 5 ... OK
Joint 6 ... OK

=== Decision ===
ARM CAN MOVE
```

但是今天不要停在这里。

我们把 GCC 的过程拆开。

---

## 第一步：只编译 `main.c`

```
gcc -Wall -Wextra -g -c main.c
```

执行：

```
ls
```

会多出：

```
main.o
```

这里：

```
-c
```

意思可以先理解为：

> 编译，但先不要完成最终链接。

再：

```
gcc -Wall -Wextra -g -c joint.c
```

得到：

```
joint.o
```

现在：

```
ls
```

大概：

```
main.c
main.o

joint.c
joint.o

joint.h
```

---

# `.o` 是什么？

它叫：

**object file（目标文件）**

现在整个流程变成：

```
main.c
  │
  │ gcc -c
  ↓
main.o


joint.c
  │
  │ gcc -c
  ↓
joint.o
```

注意：

```
main.o
```

还不是我们最终要直接运行的完整程序。

---

## 第二步：链接

执行：

```
gcc main.o joint.o -o robot_arm
```

于是：

```
main.o ───┐
          │
          ├── Linker ──→ robot_arm
          │
joint.o ──┘
```

运行：

```
./robot_arm
```

这就是 Day 1 那句：

```
编译
+
链接
```

今天终于真正看见了。

---

# 为什么需要链接？

`main.c` 中有：

```
print_joint(&joints[i]);
```

编译 `main.c` 时，编译器通过 `joint.h` 知道：

> 有一个 `print_joint()`，它应该怎么被调用。

但是函数真正的机器代码来自：

```
joint.c
↓
joint.o
```

最终链接器负责把它们组合：

```
main.o

“我要 print_joint”
       │
       ↓
      Link
       ↑
       │
joint.o

“print_joint 在我这里”
```

最后得到：

```
robot_arm
```

---

# 20～35 分钟｜机器人微项目：模块化机械臂安全系统

现在故意制造一个危险关节。

在 `main.c`：

```
{3, 30.0, 2.0, 4.0, 1},
```

改成：

```
{3, 220.0, 2.0, 4.0, 1},
```

只修改了：

```
main.c
```

而：

```
joint.c
```

没有改。

那么我们可以：

```
gcc -Wall -Wextra -g -c main.c
```

重新生成：

```
main.o
```

而原来的：

```
joint.o
```

理论上仍然可以使用。

然后：

```
gcc main.o joint.o -o robot_arm
```

运行：

```
./robot_arm
```

应该看到 Joint 3：

```
UNSAFE
```

最后：

```
EMERGENCY STOP
Unsafe joints: 1
```

这里已经出现大型 C/C++ 工程的核心思想：

> **哪个源文件变了，就重新编译相关部分，然后重新链接。**

以后几十、几百个 `.cpp` 文件时，这件事情非常重要。

也正因为如此，我们后面需要：

```
Makefile
```

自动管理这些编译关系。

---

# 今天再做一个故障实验：什么叫链接错误？

执行：

```
gcc -Wall -Wextra -g main.c -o broken_robot
```

故意**不提供 `joint.c`**。

你大概率会看到类似：

```
undefined reference to `print_joint'
undefined reference to `is_joint_safe'
```

为什么？

不是 `main.c` 看不懂函数。

因为：

```
#include "joint.h"
```

已经告诉编译器这些函数的声明。

真正的问题是：

```
编译：
“我知道 print_joint() 长什么样。”
             ↓
成功

链接：
“print_joint() 的实现到底在哪里？”
             ↓
找不到
             ↓
undefined reference
```

这叫：

**linker error（链接错误）**

这是今天必须学会区分的。

---

# 三类错误开始分家

以后看到报错，第一反应不要只是：

> “C 又报错了。”

先判断属于哪层。

```
① 编译错误

main.c
 ↓
Compiler
 ❌

例如：
漏分号
变量不存在
类型错误


② 链接错误

main.o + joint.o
      ↓
    Linker
      ❌

例如：
undefined reference


③ 运行时错误

robot_arm
   ↓
运行
   ↓
   ❌

例如：
非法内存访问
数组越界导致崩溃
除零等
```

这个分类非常重要。

---

# 今日 Linux：查看文件类型

现在执行：

```
file main.c
```

再：

```
file main.o
```

最后：

```
file robot_arm
```

你会发现三者不是一回事：

```
main.c
↓
文本形式的 C 源代码


main.o
↓
目标文件


robot_arm
↓
ELF executable
Linux 可执行程序
```

所以完整数据流：

```
              Preprocessor
                   ↓
main.c ─────→ Compiler ─────→ main.o
                                │
                                │
joint.c ────→ Compiler ─────→ joint.o
                                │
                                ↓
                              Linker
                                │
                                ↓
                            robot_arm
                                │
                                ↓
                              Linux
                                │
                                ↓
                             Process
```

你现在已经能把我们前面零散学过的：

```
gcc
.o
ELF
进程
```

串起来了。

---

# 用 `nm` 偷看目标文件

再认识一个很有用的 Linux 工具：

```
nm joint.o
```

输出中可能看到：

```
is_joint_safe
print_joint
```

再：

```
nm main.o
```

可能会看到这些函数处于“未在当前目标文件定义”的状态。

这正好验证：

```
main.o
需要 print_joint
       ↓
Linker
       ↑
joint.o
提供 print_joint
```

不用背 `nm` 的所有输出符号。

今天只知道：

> `nm` 可以帮助查看目标文件里的符号。

以后排查链接问题会很有用。

---

# 35～40 分钟｜自测 + 直接答案

**1. `.h` 主要负责什么？**

答案：通常放模块对外需要共享的接口信息，例如：

```
typedef struct {...} Joint;

int is_joint_safe(const Joint *joint);
```

可以理解成模块的**接口/说明书**。

---

**2. `.c` 主要负责什么？**

例如：

```
int is_joint_safe(const Joint *joint)
{
    ...
}
```

它提供函数的具体实现。

---

**3. 下面是什么？**

```
int is_joint_safe(const Joint *joint);
```

答案：

**函数声明。**

而：

```
int is_joint_safe(const Joint *joint)
{
    return 1;
}
```

是：

**函数定义。**

---

**4. `gcc -c main.c` 做什么？**

答案：

```
main.c
↓
编译
↓
main.o
```

暂时不完成最终链接。

---

**5.**

```
gcc main.o joint.o -o robot_arm
```

做什么？

答案：把目标文件**链接**成最终的：

```
robot_arm
```

可执行程序。

---

**6. `undefined reference to print_joint` 更可能是哪类错误？**

答案：

```
链接错误
Linker Error
```

说明代码可能知道这个函数的声明，但链接器找不到对应实现。

---

**7. 为什么大型项目不能全写一个 `main.c`？**

核心不是“C 不允许”，而是工程维护问题。

我们希望：

```
main.c
↓
程序流程

joint.c / joint.h
↓
关节

sensor.c / sensor.h
↓
传感器

kinematics.c / kinematics.h
↓
运动学

controller.c / controller.h
↓
控制器
```

这样职责清晰、可以分别编译，也更容易测试和维护。

---

今天真正需要留下的是这一张图：

```
                 Robot Project

                   main.c
                     │
                     │ #include
                     ↓
                   joint.h
                 “接口是什么”
                     │
                     ↓
                   joint.c
                 “怎么实现”
                     │


构建过程：

main.c  ──gcc -c──→ main.o ──┐
                              │
                              ├── Link ─→ robot_arm
                              │
joint.c ──gcc -c──→ joint.o ──┘
```

我们的机器人程序也完成了一个明显升级：

```
v0.1  printf
 ↓
v0.2  输入
 ↓
v0.3  if 安全判断
 ↓
v0.4  Control Loop
 ↓
v0.5  六轴数组
 ↓
v0.6  函数
 ↓
v0.7  指针
 ↓
v0.8  数组 + 指针
 ↓
v0.9  Joint struct
 ↓
v1.0  多文件 Robot Arm 工程
```

下一课最自然的是 **Makefile**。因为今天每次都手动执行：

```
gcc -Wall -Wextra -g -c main.c
gcc -Wall -Wextra -g -c joint.c
gcc main.o joint.o -o robot_arm
```

三个文件还行，但未来：

```
main.c
joint.c
sensor.c
matrix.c
kinematics.c
controller.c
```

手敲 GCC 就开始失控了。下一步会让 `make` 自动判断**哪些文件需要重新编译**，并把当前项目真正变成一个小型、可维护的 C 机器人工程。