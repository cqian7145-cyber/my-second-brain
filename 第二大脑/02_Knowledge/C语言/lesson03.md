# Day 3｜`if / else`：让机器人程序开始“做决策”

前两天我们已经走到：

```
Day 1：C源码 → GCC → 可执行程序 → printf
Day 2：变量 → int/double → scanf → 简单安全判断
Day 3：if / else → 比较运算 → 逻辑运算 → 机器人安全控制器
```

今天约 **40 分钟**。重点不是背语法，而是理解机器人控制程序最基本的模式：

```
读取状态 → 判断条件 → 做出决策 → 输出控制结果
```

这也是以后机械臂控制、传感器处理乃至 EI-Beginner Task 1 都会反复出现的结构。

## 0～5 分钟：快速复习

先建立今天的目录：

```
mkdir -p ~/c-learning/day03
cd ~/c-learning/day03
code .
```

昨天最重要的四句话：

```
int battery;
double temperature;

scanf("%d", &battery);
scanf("%lf", &temperature);
```

对应 Python：

```
battery = int(input())
temperature = float(input())
```

再复习编译：

```
gcc -Wall -Wextra robot.c -o robot
./robot
```

现在养成一个习惯：**不要只写 `gcc robot.c`。**

我们现阶段统一：

```
gcc -Wall -Wextra 文件.c -o 程序名
```

让 GCC 尽可能早点发现问题。

---

# 5～20 分钟：核心知识——条件判断

假设机器人电池：

```
int battery = 15;
```

我们的要求是：

> 电量低于 20%，停止机器人。

Python 写法：

```
if battery < 20:
    print("STOP")
```

C 写法：

```
if (battery < 20)
{
    printf("STOP\n");
}
```

你可以这样读：

```
if
↓
如果

(battery < 20)
↓
条件

{ ... }
↓
条件成立以后执行的代码
```

### `if / else`

现在要求：

> 低于 20% 停止，否则继续运行。

Python：

```
if battery < 20:
    print("STOP")
else:
    print("RUN")
```

C：

```
if (battery < 20)
{
    printf("STOP\n");
}
else
{
    printf("RUN\n");
}
```

两者思想几乎完全相同。

主要区别是 Python 靠缩进表示代码块，而 C 主要靠：

```
{
}
```

表示代码块。

---

## 比较运算符

今天掌握这 6 个：

|C|含义|
|---|---|
|`<`|小于|
|`>`|大于|
|`<=`|小于等于|
|`>=`|大于等于|
|`==`|等于|
|`!=`|不等于|

最容易犯的错误是：

```
battery = 20
```

和：

```
battery == 20
```

完全不是一回事。

`=` 是：

> 把 20 赋值给 battery。

`==` 是：

> battery 是否等于 20？

因此：

```
battery = 20;
```

是赋值。

而：

```
if (battery == 20)
```

才是判断。

这个坑以后一定会遇到。

---

# 多条件判断

真实机器人不会只看电池。

假设机器人必须同时满足：

```
电量 ≥ 20%
并且
电机温度 ≤ 70℃
```

才能运动。

C：

```
if (battery >= 20 && temperature <= 70.0)
{
    printf("Robot can move.\n");
}
```

这里：

```
&&
```

就是：

> AND / 并且

Python：

```
if battery >= 20 and temperature <= 70:
```

C：

```
if (battery >= 20 && temperature <= 70.0)
```

---

另一个符号：

```
||
```

表示：

> OR / 或者

例如：

```
if (angle > 180.0 || angle < -180.0)
```

意思：

> 角度大于 180° **或者** 小于 -180°。

---

还有：

```
!
```

表示 NOT。

例如以后可能出现：

```
if (!motor_enabled)
```

大致就是：

> 如果电机没有启用。

所以 Python → C：

```
Python      C

and         &&
or          ||
not         !
```

这三个以后非常常用。

---

# `else if`

机器人电池状态不一定只有“正常”和“异常”。

我们可以分三级：

```
80～100 → HIGH
20～79  → NORMAL
0～19   → CRITICAL
```

C：

```
if (battery >= 80)
{
    printf("Battery: HIGH\n");
}
else if (battery >= 20)
{
    printf("Battery: NORMAL\n");
}
else
{
    printf("Battery: CRITICAL\n");
}
```

注意判断顺序。

假设：

```
battery = 90
```

第一个：

```
battery >= 80
```

成立。

程序执行：

```
printf("Battery: HIGH\n");
```

后面的 `else if` 就不再执行。

所以：

```
if
↓
else if
↓
else
```

是一条**互斥决策链**。

---

# 20～35 分钟：Robot Safety Controller v0.3

今天把前面的 Monitor 升级成：

> **机器人安全控制器**

创建：

```
touch robot_safety.c
```

VS Code 打开它。

完整代码：

```
#include <stdio.h>

int main(void)
{
    int battery;
    double temperature;
    double joint_angle;

    printf("=== Robot Safety Controller v0.3 ===\n\n");

    printf("Battery (0-100): ");
    scanf("%d", &battery);

    printf("Motor temperature (C): ");
    scanf("%lf", &temperature);

    printf("Joint angle (-180 to 180): ");
    scanf("%lf", &joint_angle);

    printf("\n=== Status ===\n");

    if (battery < 0 || battery > 100)
    {
        printf("ERROR: Invalid battery value.\n");
        return 1;
    }

    if (joint_angle < -180.0 || joint_angle > 180.0)
    {
        printf("ERROR: Joint angle out of range.\n");
        return 1;
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

    printf("\n=== Control Decision ===\n");

    if (battery >= 20 &&
        temperature <= 70.0 &&
        joint_angle >= -180.0 &&
        joint_angle <= 180.0)
    {
        printf("SAFE: Robot can move.\n");
    }
    else
    {
        printf("UNSAFE: Robot must stop.\n");
    }

    return 0;
}
```

编译：

```
gcc -Wall -Wextra robot_safety.c -o robot_safety
```

运行：

```
./robot_safety
```

测试第一组：

```
Battery: 85
Temperature: 42
Joint angle: 30
```

应该得到核心结果：

```
Battery: NORMAL
Motor: NORMAL

SAFE: Robot can move.
```

第二组：

```
Battery: 12
Temperature: 45
Joint angle: 60
```

得到：

```
Battery: CRITICAL
Motor: NORMAL

UNSAFE: Robot must stop.
```

第三组：

```
Battery: 90
Temperature: 86
Joint angle: 20
```

虽然电池很好，但是：

```
Motor: OVERHEATING
UNSAFE: Robot must stop.
```

这就是：

```
battery >= 20 &&
temperature <= 70.0
```

中的 `&&` 发挥作用。

**所有安全条件必须同时成立。**

---

## 一个机器人控制思想

今天代码看起来非常简单，但已经出现了真正控制系统里的重要思想：

```
Sensor / State
      ↓
battery
temperature
joint_angle
      ↓
Safety Check
      ↓
Decision
   ↙       ↘
 SAFE     UNSAFE
  ↓          ↓
 MOVE       STOP
```

以后真正机械臂可能变成：

```
编码器
摄像头
力传感器
IMU
   ↓
状态估计
   ↓
安全条件
   ↓
控制算法
   ↓
电机
```

所以你现在的：

```
if (temperature > 70.0)
```

虽然简单，但程序结构并不是“玩具知识”。

---

# 今天再认识一个 C 概念：`return 1`

代码里出现：

```
return 1;
```

之前一直是：

```
return 0;
```

一般约定：

```
return 0
→ 程序正常结束

return 非0
→ 程序出现异常/错误
```

Linux 能看到这个返回值。

运行：

```
./robot_safety
```

故意输入：

```
Battery: 200
```

程序：

```
return 1;
```

结束以后立即输入：

```
echo $?
```

你应该看到：

```
1
```

再正常运行一次。

然后：

```
echo $?
```

应该看到：

```
0
```

这就是今天的 Linux 知识。

你第一次看到了：

> **C 程序和 Linux 操作系统之间的信息传递。**

以后 Shell、自动化脚本、ROS 节点、CI/CD 都大量依赖退出状态。

---

# 再做一个非常有价值的小实验

把：

```
if (battery >= 20 &&
    temperature <= 70.0)
```

故意改成：

```
if (battery >= 20 ||
    temperature <= 70.0)
```

然后输入：

```
Battery = 10
Temperature = 40
```

机器人居然可能被判断为：

```
SAFE
```

为什么？

因为：

```
10 >= 20
FALSE

40 <= 70
TRUE
```

而：

```
FALSE || TRUE
```

结果：

```
TRUE
```

这说明机器人安全逻辑里：

```
&&
```

和：

```
||
```

写错一个字符，结果可能完全不同。

这也是为什么 C 项目需要测试。

实验完记得改回来。

---

# 35～40 分钟：今日自测

**1. `=` 和 `==` 有什么区别？**

答案：

```
=   赋值
==  判断是否相等
```

例如：

```
battery = 80;
```

表示设置电量。

```
battery == 80
```

表示询问电量是否为 80。

**2. Python 的 `and` 在 C 中是什么？**

```
&&
```

**3. Python 的 `or` 呢？**

```
||
```

**4. 机器人只有“电量正常并且温度正常”才能移动，应该用哪个？**

答案：

```
&&
```

因为两个条件必须同时满足。

**5. Linux 中下面是什么意思？**

```
echo $?
```

答案：查看**上一条程序的退出状态码**。

通常：

```
0 = 成功
非0 = 出现错误/异常情况
```

**6. 下面最终输出什么？**

```
int battery = 15;

if (battery < 20)
{
    printf("STOP\n");
}
else
{
    printf("MOVE\n");
}
```

答案：

```
STOP
```

---

今天真正需要留下来的知识只有这一张脑图：

```
                条件判断
                   │
          ┌────────┼────────┐
          ↓        ↓        ↓
         if     else if    else
          │
      比较运算符
 < > <= >= == !=
          │
      逻辑运算符
       && || !
          │
          ↓
     Robot State
          ↓
     Safety Check
          ↓
     MOVE / STOP
```

现在我们的机器人小项目已经连续演化：

```
v0.1
Robot Status Monitor
固定状态
      ↓
v0.2
读取 battery / temperature / angle
      ↓
v0.3
Robot Safety Controller
状态 + 安全决策
```

下一阶段最值得学的是 **`while` / `for` 循环**。机器人显然不能“检测一次状态 → 程序退出”，真正的控制程序需要：

```
读取传感器
    ↓
判断
    ↓
控制
    ↓
再次读取
    ↓
判断
    ↓
控制
    ↓
   ...
```

也就是一个不断运行的 **control loop（控制循环）**。等数组加入后，我们就能从一个 `joint_angle` 正式升级到多个机械臂关节，这时会开始明显接近你最终要做的 EI-Beginner 机械臂任务。