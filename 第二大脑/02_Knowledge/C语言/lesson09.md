# Day 9｜`struct` 结构体：从“6 个数字”升级成真正的机械臂关节

前面已经走完：

```
变量 → 条件 → 循环 → 数组 → 函数 → 指针 → 数组与指针
```

今天先不继续增加指针难度，而是学习 C 里对机器人非常重要的 **`struct`（结构体）**。

之前：

```
double joints[6];
```

只能表达：

> 机械臂有 6 个角度。

但真实关节明显不只有角度，还可能有：

```
关节编号 id
角度 angle
速度 velocity
力矩 torque
是否启用 enabled
```

今天的目标就是把这些数据组织起来：

```
Joint
├── id
├── angle
├── velocity
├── torque
└── enabled
```

最终实现：

```
Joint joints[6];
```

这一步会让我们的代码第一次拥有比较正式的**机器人数据模型**。

---

## 0～5 分钟｜复习：为什么数组已经不够用了

先创建今天目录：

```
mkdir -p ~/c-learning/day09
cd ~/c-learning/day09
code .
```

Day 8：

```
double joints[6] =
{
    10,
    20,
    30,
    40,
    50,
    60
};
```

它非常适合保存：

```
同一种数据
↓
6 个关节角度
```

例如：

```
joints[0] = 10°
joints[1] = 20°
...
```

但假设 Joint 1 现在有：

```
angle    = 30°
velocity = 1.2 rad/s
torque   = 4.7 N·m
enabled  = 1
```

一种笨办法是：

```
int ids[6];
double angles[6];
double velocities[6];
double torques[6];
int enabled[6];
```

于是同一个 Joint 3 的信息散落在：

```
ids[2]
angles[2]
velocities[2]
torques[2]
enabled[2]
```

数据越来越难管理。

我们真正希望：

```
Joint 3
┌─────────────────────┐
│ id       = 3        │
│ angle    = 30       │
│ velocity = 1.2      │
│ torque   = 4.7      │
│ enabled  = 1        │
└─────────────────────┘
```

这就是 `struct` 的用途。

---

# 5～20 分钟｜核心：什么是 `struct`

最基础写法：

```
struct Joint
{
    int id;
    double angle;
    double velocity;
    double torque;
};
```

这不是创建了一个具体关节。

而是在定义：

> **Joint 这种数据应该长什么样。**

可以类比设计图：

```
struct Joint
      ↓
关节的数据模板

┌─────────────────┐
│ int id          │
│ double angle    │
│ double velocity │
│ double torque   │
└─────────────────┘
```

然后：

```
struct Joint joint1;
```

才是真正创建一个变量。

---

## Python 对比

如果用 Python，可能写：

```
class Joint:    def __init__(self, id, angle):        self.id = id        self.angle = angle
```

然后：

```
joint1 = Joint(1, 30.0)
```

C 的 `struct`：

```
struct Joint
{
    int id;
    double angle;
};

struct Joint joint1;
```

但要注意：

> `struct` **不是 C++/Python 那种完整意义上的 class**。

现阶段可以把它理解成：

> **把多个相关变量打包成一种新的数据类型。**

---

# 给结构体成员赋值

```
struct Joint joint1;

joint1.id = 1;
joint1.angle = 30.0;
joint1.velocity = 1.5;
joint1.torque = 4.2;
```

这里第一次出现：

```
.
```

称为成员访问运算符。

所以：

```
joint1.angle
```

读：

> joint1 的 angle。

Python 其实非常像：

```
joint1.angle
```

---

# 初始化结构体

也可以：

```
struct Joint joint1 =
{
    1,
    30.0,
    1.5,
    4.2
};
```

但是工程中我更希望你逐渐认识**指定初始化器**：

```
struct Joint joint1 =
{
    .id = 1,
    .angle = 30.0,
    .velocity = 1.5,
    .torque = 4.2
};
```

这样一眼就知道：

```
1 是什么？
30 是什么？
1.5 是什么？
```

可读性更强。

---

# `typedef`：把名字变漂亮

如果每次都写：

```
struct Joint joint1;
struct Joint joint2;
struct Joint joint3;
```

比较啰嗦。

C 中经常写：

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

以后：

```
Joint joint1;
```

就可以了。

现阶段把：

```
typedef
```

理解为：

> 给一种类型起一个更方便使用的名字。

于是我们创造了一个自己的类型：

```
C 原生类型：

int
double
char

我们自己的类型：

Joint
```

这就是今天一个非常重要的思维升级。

以前：

```
int battery;
double angle;
```

现在：

```
Joint joint;
```

---

# 结构体数组

现在真正进入机械臂。

既然：

```
Joint joint;
```

表示一个关节。

那么：

```
Joint joints[6];
```

自然就是：

> 6 个 Joint。

内存概念：

```
joints

        Joint 1
┌────────────────────┐
│ id                 │
│ angle              │
│ velocity           │
│ torque             │
│ enabled            │
├────────────────────┤
│       Joint 2      │
│ id                 │
│ angle              │
│ velocity           │
│ torque             │
│ enabled            │
├────────────────────┤
│        ...         │
├────────────────────┤
│       Joint 6      │
└────────────────────┘
```

访问第三个关节：

```
joints[2]
```

第三个关节角度：

```
joints[2].angle
```

所以这里其实是两个旧知识组合：

```
joints[2]
   ↑
数组

.angle
   ↑
结构体成员
```

最终：

```
joints[i].angle
```

就成为非常典型的机器人代码。

---

# 20～35 分钟｜Robot Arm State Model v0.9

今天正式把：

```
double joints[6];
```

升级成：

```
Joint joints[6];
```

创建：

```
touch robot_struct.c
```

完整程序：

```
#include <stdio.h>

#define JOINT_COUNT 6
#define MAX_TORQUE 10.0
#define MAX_VELOCITY 5.0

typedef struct
{
    int id;

    double angle;
    double velocity;
    double torque;

    int enabled;

} Joint;


int is_joint_safe(const Joint *joint)
{
    if (joint->angle < -180.0 ||
        joint->angle > 180.0)
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

    return 1;
}


void print_joint(const Joint *joint)
{
    printf(
        "Joint %d | "
        "angle: %7.2f | "
        "velocity: %6.2f | "
        "torque: %6.2f | ",
        joint->id,
        joint->angle,
        joint->velocity,
        joint->torque
    );

    if (!joint->enabled)
    {
        printf("DISABLED\n");
    }
    else if (is_joint_safe(joint))
    {
        printf("OK\n");
    }
    else
    {
        printf("UNSAFE\n");
    }
}


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

    printf("=== Robot Arm State v0.9 ===\n\n");

    for (int i = 0; i < JOINT_COUNT; i++)
    {
        print_joint(&joints[i]);

        if (!is_joint_safe(&joints[i]))
        {
            unsafe_count++;
        }
    }

    printf("\n=== Summary ===\n");

    if (unsafe_count == 0)
    {
        printf("All joints safe.\n");
        printf("Decision: ARM CAN MOVE\n");
    }
    else
    {
        printf("Unsafe joints: %d\n",
               unsafe_count);

        printf("Decision: EMERGENCY STOP\n");
    }

    return 0;
}
```

编译：

```
gcc -Wall -Wextra -g robot_struct.c -o robot_struct
```

运行：

```
./robot_struct
```

正常情况下：

```
Joint 1 ... OK
Joint 2 ... OK
...
Joint 6 ... OK

All joints safe.
Decision: ARM CAN MOVE
```

现在故意修改：

```
{3, 30.0, 2.0, 15.0, 1},
```

Joint 3 的 torque：

```
15 N·m
```

超过：

```
MAX_TORQUE = 10
```

重新编译运行：

```
gcc -Wall -Wextra -g robot_struct.c -o robot_struct
./robot_struct
```

应该出现：

```
Joint 3 ... UNSAFE

Unsafe joints: 1
Decision: EMERGENCY STOP
```

---

# 今天最重要的新东西：`->`

你应该发现我没有写：

```
joint.angle
```

而写：

```
joint->angle
```

为什么？

函数：

```
int is_joint_safe(const Joint *joint)
```

这里的：

```
joint
```

不是一个 `Joint` 本身。

它是：

```
Joint *
```

即：

> 指向 Joint 的指针。

假设：

```
joint
   │
   │ 地址
   ↓
┌─────────────────┐
│ Joint           │
│                 │
│ angle = 30      │
│ velocity = 2    │
│ torque = 4      │
└─────────────────┘
```

如果有真正的结构体变量：

```
Joint j;
```

访问成员：

```
j.angle
```

如果有结构体指针：

```
Joint *p = &j;
```

访问成员：

```
p->angle
```

所以记：

```
有东西本身
↓
.

有东西的地址/指针
↓
->
```

例如：

```
Joint j;

j.angle = 30;
```

而：

```
Joint *p = &j;

p->angle = 30;
```

---

# `->` 底层其实是什么？

上一课你已经会：

```
*
地址 → 数据
```

所以：

```
p->angle
```

本质上等价于：

```
(*p).angle
```

拆开：

```
p
↓
Joint 的地址

*p
↓
那个 Joint 本身

(*p).angle
↓
Joint 的 angle
```

所以 C 给了一个方便写法：

```
p->angle
```

这就是为什么我们先学指针再学结构体非常合适。

你不是在背一个新符号，而是在组合旧知识：

```
*
+
.
=
->
```

---

# 为什么函数传 `Joint *`？

我们也可以：

```
int is_joint_safe(Joint joint)
```

但这通常意味着按值传递整个结构体。

而：

```
int is_joint_safe(const Joint *joint)
```

传递的是地址。

可以粗略理解：

```
按值：

整个 Joint
┌─────────────────┐
│ id              │
│ angle           │
│ velocity        │
│ torque          │
│ enabled         │
└─────────────────┘
       ↓
     复制
       ↓
函数


指针：

Joint
┌─────────────────┐
│ ...             │
└─────────────────┘
       ↑
       │
    一个地址
       │
       ↓
     函数
```

结构体以后可能非常大，所以机器人/C 工程里经常传指针。

而我们写：

```
const Joint *joint
```

又表示：

> 函数只负责查看这个 Joint，不应该通过这个指针修改它。

---

# 今日 Linux + GDB｜查看结构体

现在：

```
gdb ./robot_struct
```

输入：

```
break main
run
next
```

执行到数组初始化完成之后，可以：

```
print joints[0]
```

GDB 可能显示类似：

```
{
  id = 1,
  angle = 10,
  velocity = 1,
  torque = 2,
  enabled = 1
}
```

再：

```
print joints[0].angle
```

得到：

```
10
```

查看地址：

```
print &joints[0]
```

然后：

```
print sizeof(Joint)
```

看看一个 `Joint` 到底占多少字节。

你可能会发现：

```
4 + 8 + 8 + 8 + 4
```

简单相加和 `sizeof(Joint)` 不一定完全一样。

这是因为 C 编译器可能进行：

**memory alignment / padding（内存对齐/填充）**。

今天只知道它存在即可，暂时不要深入。

---

# 一个值得你自己动手改的实验

把：

```
Joint joints[JOINT_COUNT] =
{
    {1, 10.0, 1.0, 2.0, 1},
    ...
};
```

改成指定初始化：

```
Joint joints[JOINT_COUNT] =
{
    {
        .id = 1,
        .angle = 10.0,
        .velocity = 1.0,
        .torque = 2.0,
        .enabled = 1
    },

    {
        .id = 2,
        .angle = 20.0,
        .velocity = 1.5,
        .torque = 3.0,
        .enabled = 1
    },

    {
        .id = 3,
        .angle = 30.0,
        .velocity = 2.0,
        .torque = 4.0,
        .enabled = 1
    },

    {
        .id = 4,
        .angle = 40.0,
        .velocity = 2.5,
        .torque = 5.0,
        .enabled = 1
    },

    {
        .id = 5,
        .angle = 50.0,
        .velocity = 3.0,
        .torque = 6.0,
        .enabled = 1
    },

    {
        .id = 6,
        .angle = 60.0,
        .velocity = 3.5,
        .torque = 7.0,
        .enabled = 1
    }
};
```

代码长了一些，但工程可读性更好。

---

## 35～40 分钟｜自测

**1. `struct` 主要解决什么问题？**

答案：把多个彼此相关、甚至不同类型的数据组合成一个整体。

例如：

```
typedef struct
{
    double angle;
    double velocity;
    double torque;
} Joint;
```

---

**2.**

```
Joint joint;
```

如何访问角度？

答案：

```
joint.angle
```

---

**3.**

```
Joint *p = &joint;
```

如何访问角度？

答案：

```
p->angle
```

---

**4. `p->angle` 等价于什么？**

答案：

```
(*p).angle
```

理解过程：

```
p
↓
地址

*p
↓
Joint

(*p).angle
↓
Joint 的 angle
```

---

**5. 为什么不是所有机器人关节数据都继续用几个独立数组？**

因为：

```
angles[]
velocities[]
torques[]
enabled[]
```

会把同一个关节的数据拆散。

结构体可以变成：

```
Joint 1
├─ angle
├─ velocity
├─ torque
└─ enabled
```

数据组织更符合真实对象。

---

**6. 六轴机械臂如何表示？**

答案：

```
Joint joints[6];
```

所以这里其实组合了三个知识：

```
Joint
↓
struct

joints[6]
↓
array

&joints[i]
↓
pointer
```

---

今天真正需要掌握的是：

```
以前

double joints[6]

每个关节
只有一个数字


             ↓ struct


现在

Joint joints[6]

joints[0]
┌─────────────────┐
│ id              │
│ angle            │
│ velocity         │
│ torque           │
│ enabled          │
└─────────────────┘

joints[1]
┌─────────────────┐
│ ...             │
└─────────────────┘

...

joints[5]
┌─────────────────┐
│ ...             │
└─────────────────┘
```

到 Day 9 为止，你已经完成第一轮非常重要的 C 拼图：

```
变量
 ↓
条件
 ↓
循环
 ↓
数组
 ↓
函数
 ↓
指针
 ↓
数组 + 指针
 ↓
struct
 ↓
Joint joints[6]
```

下一课适合做一次**工程化升级**：把现在所有东西都塞在 `robot_struct.c` 的程序拆成 `main.c + joint.c + joint.h`，第一次理解**头文件、源文件、函数声明、分别编译、链接**，然后用 GCC 把多个 `.c` 文件组成一个机器人程序。等这一层掌握，再进入 Makefile、文件 I/O 和机器人配置文件，会比继续堆语法更有价值。