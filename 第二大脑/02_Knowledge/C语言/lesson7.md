可以把整个学习计划压缩成一条主线：

> **C 基础语法 → Linux/C 开发工具 → C 底层能力 → 机器人数学与数据结构 → EI-Beginner 机械臂 Task 1**

《C Primer Plus》第 6 版会作为主教材，重点覆盖数据类型、控制语句、函数、数组与指针、内存管理、文件 I/O、结构体、位操作等内容。 C Primer Plus 第6版 中文版

### 第一阶段：C 基础，约 2～3 周

我们现在就在这里，而且已经学到 Day 6。

```
✅ printf / GCC / Linux基本命令
✅ int / double / scanf
✅ if / else
✅ while / for
✅ 数组
✅ 函数
↓
下一步：指针
↓
字符串
↓
多文件程序
```

目标不是刷语法，而是持续维护一个机器人项目：

```
Robot Monitor
→ Safety Controller
→ Control Loop
→ 6轴关节数组
→ 模块化机械臂程序
```

### 第二阶段：C 核心能力，约 3～4 周

这是最重要的一阶段：

```
指针
↓
地址 / 内存
↓
数组与指针
↓
struct 结构体
↓
动态内存 malloc/free
↓
文件读写
↓
头文件 .h
↓
多源文件编译
↓
Makefile
```

机器人项目同步升级成：

```
Joint
RobotArm
Vector3
Matrix
SensorData
```

同时学习 Linux 开发工具：

```
gcc
gdb
make
文件权限
进程
基础系统概念
```

### 第三阶段：C × 机器人，约 3～5 周

开始减少“纯语法课”，增加机器人工程。

重点做：

```
向量
↓
二维数组 / 矩阵
↓
矩阵乘法
↓
三角函数
↓
旋转矩阵
↓
坐标系
↓
齐次变换矩阵
↓
2自由度机械臂
↓
6自由度机械臂基础
```

比如自己用 C 写：

```
Vector3
Matrix3x3
Matrix4x4
```

以及：

```
forward_kinematics();
```

也就是**正运动学**。

### 第四阶段：进入 EI-Beginner Task 1

最终从：

```
C / Linux
```

过渡到：

```
机器人学基础
+
Python/C++
+
PyBullet 或 MuJoCo
```

完成 EI-Beginner Task 1 的路线：

```
坐标变换
↓
正运动学
↓
逆运动学
↓
控制基础
↓
机械臂仿真
↓
目标物体
↓
机械臂移动
↓
抓取
```

所以我们并不是准备“用纯 C 完成 EI-Beginner”。

**C 的任务是把你的底层编程能力、内存思维和机器人计算基础打牢；最终机器人仿真会逐渐回到 Python/C++。**

每天仍然保持约 **40 分钟**：

```
5分钟   昨日复习
15分钟  C Primer Plus核心知识
15分钟  机器人微项目
5分钟   Linux/GCC/GDB + 自测
```

你现在的位置大概是：

```
基础语法 ██████░░░░
C核心能力 ██░░░░░░░░
机器人编程 ██░░░░░░░░
EI-Beginner ░░░░░░░░░░
```

**下一课就是一个关键节点：指针 `pointer`。** 我会从你已经会的 `scanf("%d", &x)` 和 `joint_angles[]` 出发讲，不会直接让你背一堆 `*`、`&`。

# Day 7｜指针①：终于搞懂 `&` 和 `*`

今天进入 C 最关键的知识之一：**pointer（指针）**。但今天只解决一个核心问题：
<mark style="background: #FFB8EBA6;">
> **变量有“值”，也有“地址”；指针就是专门保存地址的变量。</mark>**

你前面已经见过 `scanf("%d", &battery)`、数组传函数，所以现在学指针正合适。今天不碰二级指针、复杂指针和动态内存。

## 0～5 分钟：从你已经会的代码复习

你写过：

```
int battery = 80;

printf("%d\n", battery);
```

可以先想象内存：

```
内存地址              内容
0x7ffd1000            80
     ↑                 ↑
  battery在哪里      battery的值
```

所以一个变量其实有两个值得关注的东西：

```
battery
→ 值：80

&battery
→ 地址：例如 0x7ffd1000
```

你之前写：

```
scanf("%d", &battery);
```

现在终于可以开始真正解释为什么有 `&`。

---

## 5～20 分钟：指针的核心

先创建今天目录：

```
mkdir -p ~/c-learning/day07
cd ~/c-learning/day07
code .
```

创建：

```
pointer_demo.c
```

写：

```
#include <stdio.h>

int main(void)
{
    int battery = 80;

    printf("battery value   = %d\n", battery);
    printf("battery address = %p\n", (void *)&battery);

    return 0;
}
```

编译运行：

```
gcc -Wall -Wextra -g pointer_demo.c -o pointer_demo
./pointer_demo
```
<mark style="background: #FFB86CA6;">参数

含义

-c

编译到目标文件，不链接

-o

指定输出文件名称

-g

生成调试信息，方便 GDB 调试

-Wall

启用一组常见编译警告

-Wextra

启用额外的编译警告

-E

只预处理

-S

生成汇编代码，不汇编

-O2

开启常用的较高级别优化

记忆技巧：

-E：停在预处理阶段。

-S：停在汇编代码阶段。

-c：停在目标文件阶段。

不加这些停止参数：通常继续链接，生成可执行程序。</mark>
可能看到：

```
battery value   = 80
battery address = 0x7ffda38c9234
```

地址每次运行可能变化，不需要和示例一样。

这里：

```
&battery
```

就是：

> **取 battery 的地址。**

所以今天第一个符号：

```
&
↓
address of
↓
取地址
```

---

### 指针变量

普通整数变量：

```
int battery = 80;
```

保存：

```
80
```

现在：

```
int *p = &battery;
```

`p` 保存的不是 80，而是：

```
battery 的地址
```

脑图：

```
battery

地址 0x1000
┌─────────────┐
│     80      │
└─────────────┘
      ↑
      │
      │ 指向
      │
┌─────────────┐
│   0x1000    │
└─────────────┘
      p
```

所以：

```
int *p;
```

读成：

> `p` 是一个指向 `int` 的指针。

而：

```
p = &battery;
```

表示：

> 把 battery 的地址保存进 p。

完整代码：

```
#include <stdio.h>

int main(void)
{
    int battery = 80;

    int *p = &battery;

    printf("battery       = %d\n", battery);
    printf("&battery      = %p\n", (void *)&battery);

    printf("p             = %p\n", (void *)p);
    printf("*p            = %d\n", *p);

    return 0;
}
```

运行后重点观察：

```
&battery
```

和：

```
p
```

应该是同一个地址。

因为：

```
p = &battery;
```

---

## 第二个关键符号：`*`

现在最容易混乱的地方来了。

```
int *p = &battery;
```

这里的 `*` 表示：

> 声明 p 是指针。

但后面：

```
printf("%d\n", *p);
```

这里：

```
*p
```

表示：

> 去 p 保存的那个地址，取出里面的数据。

这叫：

**dereference（解引用）**。

所以：

```
battery
↓
80

&battery
↓
battery 的地址

p
↓
battery 的地址

*p
↓
顺着地址找到 battery
↓
80
```

这是今天最重要的四行：

```
battery   → 值

&battery  → 地址

p         → 地址

*p        → 地址里的值
```

---

## 为什么指针这么重要？

现在做一个实验。

Python：

```
def set_battery(x):
    x = 100

battery = 20
set_battery(battery)

print(battery)
```

这里涉及 Python 自己的对象/参数模型，我们暂时不展开。

看看 C：

```
void set_battery(int battery)
{
    battery = 100;
}
```

然后：

```
int battery = 20;

set_battery(battery);

printf("%d\n", battery);
```

结果仍然：

```
20
```

为什么？

因为函数得到的是一个值副本，可以先粗略理解成：

```
main 的 battery = 20

       ↓ 复制

函数 battery = 20

函数修改：
battery = 100

main 的 battery
仍然 = 20
```

---

### 指针解决这个问题

写：

```
void set_battery(int *battery)
{
    *battery = 100;
}
```

调用：

```
set_battery(&battery);
```

现在：

```
main
battery = 20
地址 = 0x1000

       │
       │ &battery
       ↓

set_battery
收到 0x1000

       │
       │ *battery
       ↓

访问 0x1000
       ↓
修改为 100
```

因此 main 中的变量真的变成：

```
100
```

这就是指针真正强大的地方之一：

> **函数可以通过地址访问和修改外部数据。**

---

# 20～35 分钟：机器人微项目

今天做：

> **Robot Emergency Battery Controller v0.7**

创建：

```
touch robot_pointer.c
```

完整代码：

```
#include <stdio.h>

void charge_battery(int *battery)
{
    *battery = 100;
}

void consume_battery(int *battery, int amount)
{
    *battery = *battery - amount;

    if (*battery < 0)
    {
        *battery = 0;
    }
}

void emergency_stop(int *motor_enabled)
{
    *motor_enabled = 0;
}

int main(void)
{
    int battery = 80;
    int motor_enabled = 1;

    printf("=== Robot Controller v0.7 ===\n");

    printf("\nInitial state:\n");
    printf("Battery: %d%%\n", battery);
    printf("Motor: %d\n", motor_enabled);

    printf("\nRobot moving...\n");

    consume_battery(&battery, 30);

    printf("Battery: %d%%\n", battery);

    consume_battery(&battery, 40);

    printf("Battery: %d%%\n", battery);

    if (battery < 20)
    {
        printf("\nWARNING: Low battery!\n");

        emergency_stop(&motor_enabled);
    }

    printf("\nFinal state:\n");
    printf("Battery: %d%%\n", battery);
    printf("Motor: %d\n", motor_enabled);

    printf("\nCharging robot...\n");

    charge_battery(&battery);

    printf("Battery: %d%%\n", battery);

    return 0;
}
```

编译：

```
gcc -Wall -Wextra -g robot_pointer.c -o robot_pointer
```

运行：

```
./robot_pointer
```

你应该看到类似：

```
Initial state:
Battery: 80%
Motor: 1

Robot moving...
Battery: 50%
Battery: 10%

WARNING: Low battery!

Final state:
Battery: 10%
Motor: 0

Charging robot...
Battery: 100%
```

现在重点看：

```
consume_battery(&battery, 30);
```

为什么传：

```
&battery
```

？

因为函数定义：

```
void consume_battery(int *battery, int amount)
```

要求：

```
第一个参数
↓
int 的地址
```

所以：

```
battery
→ 80

&battery
→ battery 的地址
```

函数拿到地址以后：

```
*battery = *battery - amount;
```

假设：

```
*battery = 80
amount = 30
```

那么：

```
80 - 30
↓
50
```

而且修改的是原变量。

---

## 现在回头看 `scanf`

你 Day 2 就写过：

```
scanf("%d", &battery);
```

现在终于可以真正理解：

```
scanf
需要修改 battery
      ↓
必须知道 battery 在哪里
      ↓
&battery
      ↓
获得地址
      ↓
scanf 根据地址
找到那块内存
      ↓
写入用户输入
```

所以：

```
scanf("%d", battery);
```

错在哪里？

`scanf` 要：

```
地址
```

你却给了：

```
battery 中保存的整数值
```

如果 battery 恰好是：

```
80
```

就相当于让程序：

> 去内存地址 80 写数据。

这显然非常危险，可能直接导致崩溃。

---

## 再回头看昨天的数组

昨天：

```
void read_joints(double joints[])
```

调用：

```
read_joints(joint_angles);
```

你会发现没有：

```
&joint_angles
```

这是因为数组传给函数时有特殊规则：在这种函数参数场景下，数组表达式会与**首元素地址**紧密关联。

例如：

```
joint_angles
```

在很多表达式中可以得到指向：

```
joint_angles[0]
```

的指针。

所以：

```
void read_joints(double joints[])
```

在函数参数中，与：

```
void read_joints(double *joints)
```

有非常密切的关系。

后面我们专门讲：

> **数组和指针的关系。**

现在不要简单记成“数组就是指针”，这句话并不严谨。

---

# 今日 Linux + GDB：亲眼看地址

今天 GDB 特别适合学习指针。

编译：

```
gcc -Wall -Wextra -g pointer_demo.c -o pointer_demo
```

进入：

```
gdb ./pointer_demo
```

输入：

```
break main
run
```

然后：

```
next
```

执行到：

```
int *p = &battery;
```

之后尝试：

```
print battery
```

可能：

```
$1 = 80
```

再：

```
print &battery
```

会看到一个地址。

再：

```
print p
```

也会看到地址。

最后：

```
print *p
```

得到：

```
80
```

你应该看到：

```
print battery
→ 80

print &battery
→ 0x......

print p
→ 0x......

print *p
→ 80
```

这比死背指针定义有效得多。

退出：

```
quit
```

---

# 35～40 分钟：自测

第一题：

```
int battery = 80;
```

`battery` 是什么？

答案：

```
80
```

`&battery` 是什么？

答案：

> battery 的内存地址。

第二题：

```
int *p = &battery;
```

`p` 保存什么？

答案：

> battery 的地址。

`*p` 是什么？

答案：

> p 指向地址中保存的数据，也就是 battery 的值。

因此：

```
battery = *p
```

在当前例子里都是：

```
80
```

而：

```
&battery = p
```

表示的是同一个地址。

第三题，如果：

```
int x = 10;
int *p = &x;

*p = 50;
```

最后：

```
x = ?
```

答案：

```
50
```

因为：

```
p
↓
指向 x

*p
↓
访问 x

*p = 50
↓
修改 x
```

第四题，为什么：

```
scanf("%d", &x);
```

需要 `&`？

答案：

> `scanf()` 需要变量地址，才能找到并修改那块内存。

第五题，下面函数：

```
void stop_motor(int *motor)
{
    *motor = 0;
}
```

应该怎么调用？

答案：

```
int motor = 1;

stop_motor(&motor);
```

调用后：

```
motor = 0
```

---

今天只需要牢牢记住这一张图：

```
          int battery = 80
                 │
        ┌────────┴────────┐
        ↓                 ↓
    battery            &battery
       80             0x1000
                         │
                         ↓
                  int *p = &battery
                         │
                         ↓
                         p
                      0x1000
                         │
                         ↓
                        *p
                         │
                         ↓
                        80
```

不要把 `*` 和 `&` 当成奇怪符号背。

理解成两个动作就行：

```
&
变量 → 地址

*
地址 → 地址里的数据
```

我们的路线现在来到：

```
Day 1  编译 / printf
  ↓
Day 2  类型 / scanf
  ↓
Day 3  if / else
  ↓
Day 4  while / for
  ↓
Day 5  数组
  ↓
Day 6  函数
  ↓
Day 7  指针①：地址、&、*
  ↓
下一课
指针②：数组与指针
```

下一课会直接研究你机械臂程序里的 `double joint_angles[6]` 在内存中究竟长什么样，以及为什么 `joint_angles[i]` 和指针运算存在对应关系。等这部分打通，再进入 `struct Joint`，我们就能从“6 个角度数字”升级成真正包含**角度、速度、力矩**的机器人关节模型。