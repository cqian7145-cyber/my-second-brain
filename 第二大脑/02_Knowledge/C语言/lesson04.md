# Day 4｜`while` / `for`：写出第一个机器人 Control Loop

前三天我们已经完成：

```
Day 1  C → GCC → Linux → printf
Day 2  int / double → scanf → 读取机器人状态
Day 3  if / else → && / || → 安全决策
Day 4  循环 → Control Loop → 持续监控机器人
```

今天约 **40 分钟**。这是一个很关键的节点，因为机器人程序通常不是“检测一次就退出”，而是不断执行：

```
读取状态
   ↓
判断状态
   ↓
计算控制
   ↓
执行
   ↓
再次读取
   ↺
```

这就是最原始的 **控制循环（control loop）**。

## 0～5 分钟：复习

先创建今天目录：

```
mkdir -p ~/c-learning/day04
cd ~/c-learning/day04
code .
```

昨天核心逻辑：

```
if (battery >= 20 &&
    temperature <= 70.0)
{
    printf("SAFE: Robot can move.\n");
}
else
{
    printf("UNSAFE: Robot must stop.\n");
}
```

Python 对应：

```
if battery >= 20 and temperature <= 70:    print("SAFE")else:    print("UNSAFE")
```

现在有个问题。

程序运行：

```
读取状态
↓
判断
↓
结束
```

但真实机器人应该：

```
读取 → 判断 → 控制
 ↑             ↓
 └─────────────┘
```

所以我们需要 **循环**。

---

# 5～20 分钟：核心知识 `while`

Python：

```
count = 0while count < 5:    print(count)    count += 1
```

C：

```
int count = 0;

while (count < 5)
{
    printf("%d\n", count);

    count++;
}
```

输出：

```
0
1
2
3
4
```

逐步看。

初始：

```
int count = 0;
```

第一次：

```
count < 5
0 < 5
TRUE
```

执行循环。

然后：

```
count++;
```

等价于：

```
count = count + 1;
```

于是：

```
0 → 1 → 2 → 3 → 4 → 5
```

当：

```
5 < 5
```

为假，循环结束。

所以 `while` 的核心模型只有：

```
while (条件成立)
{
    重复执行;
}
```

---

## `++` 是什么？

C 经常出现：

```
count++;
```

现阶段可以直接理解为：

```
count = count + 1;
```

Python：

```
count += 1
```

C 也支持：

```
count += 1;
```

但 C 程序员经常写：

```
count++;
```

---

# 无限循环

如果写：

```
while (1)
{
    printf("Robot running...\n");
}
```

程序会一直执行。

因为：

```
1 = 真
```

所以：

```
while (1)
```

可以理解为：

> 永远循环。

机器人程序里你以后会大量遇到这种模式：

```
while (1)
{
    read_sensor();
    calculate_control();
    move_motor();
}
```

当然真实系统还要考虑频率、异常处理、实时性等。

---

# 怎么退出无限循环？

使用：

```
break;
```

例如：

```
while (1)
{
    scanf("%d", &battery);

    if (battery < 20)
    {
        printf("Battery too low!\n");
        break;
    }
}
```

当电量低于 20：

```
break
↓
跳出循环
```

程序继续执行循环后面的代码。

---

# 第二种循环：`for`

Python：

```
for i in range(6):    print(i)
```

C：

```
for (int i = 0; i < 6; i++)
{
    printf("%d\n", i);
}
```

输出：

```
0
1
2
3
4
5
```

这是今天最值得理解的一行：

```
for (int i = 0; i < 6; i++)
```

拆开：

```
int i = 0
↓
从 0 开始

i < 6
↓
只要小于 6 就继续

i++
↓
每轮 +1
```

所以：

```
i
↓
0
1
2
3
4
5
```

---

# 为什么机器人特别适合 `for`？

因为机械臂可能有：

```
Joint 0
Joint 1
Joint 2
Joint 3
Joint 4
Joint 5
```

6 个关节。

以后我们可以：

```
for (int i = 0; i < 6; i++)
{
    printf("Checking Joint %d\n", i);
}
```

输出：

```
Checking Joint 0
Checking Joint 1
...
Checking Joint 5
```

现在你应该开始看到：

```
for
↓
多个关节
↓
数组
↓
Joint joints[6]
```

这条路线了。

---

# `while` 和 `for` 怎么选？

目前记一个简单规则：

```
不知道循环多少次
↓
while

明确循环 N 次
↓
for
```

比如机器人持续运行：

```
while (1)
```

检查六个关节：

```
for (int i = 0; i < 6; i++)
```

不是绝对规则，但对初学非常好用。

---

# 20～35 分钟：Robot Control Loop v0.4

今天正式把昨天的机器人安全控制器升级为：

> **持续运行的机器人状态监控器**

创建：

```
touch robot_loop.c
```

完整代码：

```
#include <stdio.h>

int main(void)
{
    int battery;
    double temperature;
    double joint_angle;

    int cycle = 1;

    printf("=== Robot Control Loop v0.4 ===\n");
    printf("Enter -1 as battery value to stop.\n\n");

    while (1)
    {
        printf("----- Control Cycle %d -----\n", cycle);

        printf("Battery (0-100): ");
        scanf("%d", &battery);

        if (battery == -1)
        {
            printf("Shutdown command received.\n");
            break;
        }

        printf("Motor temperature (C): ");
        scanf("%lf", &temperature);

        printf("Joint angle (-180 to 180): ");
        scanf("%lf", &joint_angle);

        if (battery < 0 || battery > 100)
        {
            printf("ERROR: Invalid battery value.\n\n");
            continue;
        }

        if (joint_angle < -180.0 || joint_angle > 180.0)
        {
            printf("ERROR: Joint angle out of range.\n\n");
            continue;
        }

        if (battery < 20)
        {
            printf("Battery: CRITICAL\n");
        }
        else if (battery < 50)
        {
            printf("Battery: LOW\n");
        }
        else
        {
            printf("Battery: NORMAL\n");
        }

        if (temperature > 70.0)
        {
            printf("Motor: OVERHEATING\n");
        }
        else
        {
            printf("Motor: NORMAL\n");
        }

        if (battery >= 20 &&
            temperature <= 70.0 &&
            joint_angle >= -180.0 &&
            joint_angle <= 180.0)
        {
            printf("Decision: MOVE\n");
        }
        else
        {
            printf("Decision: STOP\n");
        }

        printf("\n");

        cycle++;
    }

    printf("\nRobot controller stopped.\n");

    return 0;
}
```

编译：

```
gcc -Wall -Wextra robot_loop.c -o robot_loop
```

运行：

```
./robot_loop
```

测试：

```
Battery: 80
Temperature: 42
Joint angle: 30
```

结果：

```
Battery: NORMAL
Motor: NORMAL
Decision: MOVE
```

程序不会结束。

它重新进入：

```
----- Control Cycle 2 -----
```

再输入：

```
15
45
20
```

得到：

```
Battery: CRITICAL
Motor: NORMAL
Decision: STOP
```

再下一轮输入：

```
-1
```

触发：

```
break;
```

最终：

```
Shutdown command received.

Robot controller stopped.
```

这已经是真正意义上的一个非常简化的：

**Robot Control Loop。**

---

## 今天又出现一个新东西：`continue`

代码里：

```
if (battery < 0 || battery > 100)
{
    printf("ERROR: Invalid battery value.\n\n");
    continue;
}
```

假设：

```
battery = 200
```

我们不希望程序继续拿错误数据计算。

于是：

```
continue
```

意思是：

> 当前这一轮剩下的代码别执行，直接开始下一轮。

所以：

```
break
```

和：

```
continue
```

一定区分。

```
break
↓
整个循环结束
```

而：

```
continue
↓
当前这一轮结束
↓
进入下一轮
```

这是 C 初学非常重要的一组概念。

---

# 今天的 Linux：停止失控程序

现在故意创建：

```
#include <stdio.h>

int main(void)
{
    while (1)
    {
        printf("Robot running...\n");
    }

    return 0;
}
```

编译：

```
gcc -Wall -Wextra infinite.c -o infinite
```

运行：

```
./infinite
```

你会发现终端疯狂：

```
Robot running...
Robot running...
Robot running...
Robot running...
...
```

怎么办？

按：

```
Ctrl + C
```

程序停止。

这是 Linux 开发非常重要的快捷操作。

以后：

```
Python
Node.js
C
ROS
开发服务器
```

程序在终端持续运行时，你都会频繁使用：

```
Ctrl + C
```

它并不是“复制”。

在终端运行程序时，它通常会向前台进程发送一个 **SIGINT（interrupt signal）**，请求程序中断。

今天不用研究 signal。

只需要形成肌肉记忆：

> 终端程序跑不完/需要停止 → `Ctrl + C`

---

# 再认识一个 Linux 命令

运行：

```
ps
```

它可以查看当前终端相关的进程。

以后我们会进一步学：

```
ps
top
htop
kill
```

然后你会逐渐理解：

```
C 程序
↓
可执行文件
↓
运行
↓
Linux Process
```

也就是：

> **磁盘上的程序运行起来以后，会成为 Linux 中的一个进程。**

这是以后学 ROS 节点、进程通信甚至操作系统的基础。

---

# 35～40 分钟：自测

**1. 下面执行几次？**

```
for (int i = 0; i < 6; i++)
{
    printf("%d\n", i);
}
```

答案：

```
6 次
```

分别：

```
0 1 2 3 4 5
```

不是：

```
1 2 3 4 5 6
```

这个区别以后数组非常重要。

**2. `while (1)` 是什么？**

答案：

> 无限循环，因为条件 `1` 始终为真。

**3. `break` 和 `continue` 区别？**

答案：

```
break
→ 退出整个循环

continue
→ 跳过本轮剩余代码
→ 进入下一轮
```

**4. Python：**

```
for i in range(6):    print(i)
```

C 是：

```
for (int i = 0; i < 6; i++)
{
    printf("%d\n", i);
}
```

**5. Linux 终端程序一直运行，常用什么停止？**

```
Ctrl + C
```

**6. 为什么机器人程序需要循环？**

因为真实机器人必须持续：

```
读取传感器
↓
状态判断
↓
计算控制
↓
执行动作
↓
再次读取
```

而不是执行一次就退出。

---

今天真正要掌握的是：

```
             C 循环
               │
       ┌───────┴───────┐
       ↓               ↓
     while            for
       │               │
不知道次数          明确次数
持续控制            遍历关节
       │               │
       └───────┬───────┘
               ↓
       break / continue
               ↓
        Robot Control Loop
```

现在项目已经从：

```
v0.1  显示机器人状态
 ↓
v0.2  scanf 读取状态
 ↓
v0.3  if/else 安全决策
 ↓
v0.4  while 持续控制循环
```

下一步会进入目前为止**最像真正机械臂的一课：数组 `array`**。

我们会把现在唯一的：

```
double joint_angle;
```

升级成：

```
double joint_angles[6];
```

然后用：

```
for (int i = 0; i < 6; i++)
```

一次处理机械臂 **6 个关节**。

届时程序结构会第一次变成：

```
joint_angles[0] ─→ Joint 1
joint_angles[1] ─→ Joint 2
joint_angles[2] ─→ Joint 3
joint_angles[3] ─→ Joint 4
joint_angles[4] ─→ Joint 5
joint_angles[5] ─→ Joint 6
                     ↓
                安全检查
                     ↓
                机械臂控制
```

这一步开始，我们就会明显从“学 C 语法”进入**用 C 表达机器人系统**。