# Day 2｜让机器人程序“读懂你的输入”

昨天已经完成了第一条链路：

```
.c 源代码 → gcc 编译 → 可执行程序 → ./程序运行
```

今天继续《C Primer Plus》前期最重要的基础：**变量、基本数据类型、`scanf()` 输入**。目标是把昨天只能显示固定数据的 `Robot Status Monitor v0.1` 升级成 **v0.2：可以输入真实机器人状态并进行安全判断**。

总时长约 **40 分钟**。

---

## ① 0～5 分钟：复习

先进入昨天目录：

```
cd ~/c-learning/day01
ls
```

你应该能看到类似：

```
hello_robot.c
hello_robot
robot_status.c
robot_status
```

重新编译一次，但今天增加两个参数：

```
gcc -Wall -Wextra robot_status.c -o robot_status
```

运行：

```
./robot_status
```

以后我们逐渐养成习惯：

```
gcc -Wall -Wextra xxx.c -o xxx
```

其中：

```
-Wall
```

开启大量常用编译警告。

```
-Wextra
```

再开启一些额外警告。

Python 很多问题可能运行到某一行才发现；C 编译器则可以在**编译阶段**帮我们发现不少危险写法。

---

# ② 5～20 分钟：今天的 C 核心——变量、类型、scanf

昨天你写过：

```
int battery = 87;
```

Python 是：

```
battery = 87
```

C 却必须：

```
int battery = 87;
```

原因之一是 C 对数据在内存里的表示更加明确。

今天先掌握 3 种：

|C 类型|用途|示例|
|---|---|---|
|`int`|整数|6、87、1500|
|`float`|小数|36.5|
|`double`|更高精度小数|3.1415926|

机器人里马上就能对应起来：

```
int joint_count = 6;

float battery = 87.5f;

double joint_angle = 45.23891;
```

---

## 为什么机器人特别需要 `double`？

以后机械臂角度可能是：

```
31.725831°
```

末端坐标可能是：

```
x = 0.427391 m
```

如果大量数学计算都只保存整数：

```
31
0
```

机器人运动学直接废掉。

因此以后我们会大量看到：

```
double
```

---

# Python 和 C 对比

Python：

```
angle = 45.5temperature = 38
```

C：

```
double angle = 45.5;
int temperature = 38;
```

Python 帮你管理大量类型细节。

C 要求程序员明确：

> 我准备存什么类型的数据？

这也是以后理解**内存、指针、嵌入式机器人程序**的基础。

---

# `printf()` 的不同占位符

整数：

```
int battery = 87;

printf("%d\n", battery);
```

小数：

```
double angle = 45.5;

printf("%f\n", angle);
```

控制显示两位：

```
printf("%.2f\n", angle);
```

输出：

```
45.50
```

以后机械臂坐标就经常这样显示：

```
printf("Joint angle: %.2f deg\n", angle);
```

---

# 今天真正的新东西：`scanf()`

昨天：

```
int battery = 87;
```

数据写死在程序里。

现在希望：

```
请输入电量：
83
```

于是：

```
int battery;

printf("Enter battery: ");
scanf("%d", &battery);
```

完整程序：

```
#include <stdio.h>

int main(void)
{
    int battery;

    printf("Enter battery: ");
    scanf("%d", &battery);

    printf("Battery: %d%%\n", battery);

    return 0;
}
```

编译：

```
gcc -Wall -Wextra input.c -o input
```

运行：

```
./input
```

例如：

```
Enter battery: 73
Battery: 73%
```

---

# 一个非常重要的符号出现了

你应该注意：

```
scanf("%d", &battery);
```

为什么不是：

```
scanf("%d", battery);
```

而是：

```
&
```

？

这是你第一次碰到 C 最重要的概念之一：

> **地址。**

现在不要学指针。

今天只建立直觉。

假设：

```
int battery;
```

计算机给 `battery` 找了一小块内存。

比如假想地址：

```
0x7ffd1234
```

那么：

```
battery
```

可以理解成：

> 这块内存里面的值。

而：

```
&battery
```

可以暂时理解成：

> battery 在哪里。

`scanf()` 要修改 battery，因此你得告诉它：

> **去这个地址，把用户输入的数据写进去。**

所以：

```
scanf("%d", &battery);
```

现在只记：

> `scanf` 输入普通数值变量时，通常要写 `&变量名`。

几周后学**指针**，你会真正理解这个 `&`。

---

# ③ 20～35 分钟：机器人微项目

创建今天目录：

```
mkdir -p ~/c-learning/day02
cd ~/c-learning/day02
code .
```

创建：

```
robot_monitor.c
```

我们做：

# Robot Safety Monitor v0.2

完整答案直接给你：

```
#include <stdio.h>

int main(void)
{
    int robot_id;
    int battery;
    double joint_angle;
    double motor_temperature;

    printf("===== Robot Safety Monitor =====\n");

    printf("Enter robot ID: ");
    scanf("%d", &robot_id);

    printf("Enter battery level (0-100): ");
    scanf("%d", &battery);

    printf("Enter joint angle (deg): ");
    scanf("%lf", &joint_angle);

    printf("Enter motor temperature (C): ");
    scanf("%lf", &motor_temperature);

    printf("\n===== Robot Status =====\n");

    printf("Robot ID: %d\n", robot_id);
    printf("Battery: %d%%\n", battery);
    printf("Joint Angle: %.2f deg\n", joint_angle);
    printf("Motor Temperature: %.2f C\n", motor_temperature);

    if (battery < 20)
    {
        printf("WARNING: Low battery!\n");
    }

    if (motor_temperature > 70.0)
    {
        printf("WARNING: Motor overheating!\n");
    }

    if (joint_angle > 180.0 || joint_angle < -180.0)
    {
        printf("WARNING: Joint angle out of range!\n");
    }

    return 0;
}
```

今天的重点不是完全掌握 `if`。

你先把下面三句读成人话：

```
if (battery < 20)
```

就是：

> 如果电量低于 20。

---

```
if (motor_temperature > 70.0)
```

就是：

> 如果电机温度高于 70℃。

---

```
joint_angle > 180.0 || joint_angle < -180.0
```

其中：

```
||
```

表示：

**OR（或者）**

所以：

> 如果关节角度大于 180°，或者小于 -180°。

后面我们会专门系统学 `if` 和逻辑运算符。

---

# 编译

```
gcc -Wall -Wextra robot_monitor.c -o robot_monitor
```

运行：

```
./robot_monitor
```

第一组测试：

```
Robot ID: 1
Battery: 85
Joint Angle: 45
Motor Temperature: 42
```

应该没有警告。

第二组故意制造危险状态：

```
Robot ID: 2
Battery: 12
Joint Angle: 220
Motor Temperature: 86
```

应该出现：

```
WARNING: Low battery!
WARNING: Motor overheating!
WARNING: Joint angle out of range!
```

现在你已经写出了一个非常原始的：

```
传感器数据
   ↓
读取
   ↓
状态判断
   ↓
安全警告
```

这就是机器人程序非常典型的逻辑雏形。

---

# 一个容易踩的坑：`%f` 和 `%lf`

注意我们读取 `double`：

```
double joint_angle;

scanf("%lf", &joint_angle);
```

这里是：

```
%lf
```

而输出：

```
printf("%.2f\n", joint_angle);
```

却使用：

```
%f
```

初学阶段直接记这个规则：

```
scanf + double → %lf

printf + double → %f
```

以后我们再解释底层原因。

---

# Linux 今日知识：文件到底在哪里？

现在执行：

```
pwd
```

可能得到：

```
/home/chen/c-learning/day02
```

执行：

```
ls
```

得到：

```
robot_monitor
robot_monitor.c
```

执行：

```
ls -l
```

你会看到类似：

```
-rwxr-xr-x ... robot_monitor
-rw-r--r-- ... robot_monitor.c
```

注意：

```
robot_monitor
```

前面存在：

```
x
```

其中 `x` 就是：

```
executable
可执行
```

这就是 Linux **文件权限**的第一次露面。

现在不用背：

```
r w x
```

今天知道：

```
r = read
w = write
x = execute
```

就够了。

以后 ROS、Shell 脚本、机器人程序经常遇到：

```
Permission denied
```

那时你就会知道它通常与权限有关。

---

# ④ 35～40 分钟：自测 + 答案

### Q1

Python：

```
temperature = 42.5
```

C 怎么写？

答案：

```
double temperature = 42.5;
```

---

### Q2

下面哪个是整数？

```
int
double
```

答案：

```
int
```

---

### Q3

为什么：

```
scanf("%d", &battery);
```

有 `&`？

答案：

> `scanf()` 需要知道 `battery` 所在的内存地址，才能把用户输入的数据写进去。

这就是未来**指针**知识的伏笔。

---

### Q4

Linux：

```
gcc robot.c -o robot
```

是什么意思？

答案：

> 使用 GCC 编译 `robot.c`，生成名为 `robot` 的可执行程序。

---

### Q5

```
./robot
```

是什么意思？

答案：

> 执行当前目录里的 `robot` 程序。

---

### Q6

机器人里为什么大量使用 `double` 而不是全部使用 `int`？

答案：

> 机器人中的角度、坐标、速度、加速度、传感器值等经常需要小数和较高精度。

---

## 今天真正需要掌握的知识

不要试图记住所有代码。

只记住这一条数据流：

```
用户 / 传感器
      ↓
    scanf
      ↓
C 变量
int / double
      ↓
     if
      ↓
安全判断
      ↓
   printf
      ↓
状态 / 警告
```

我们现在已经从：

```
Day 1
固定显示机器人状态
```

升级成：

```
Day 2
输入机器人状态
+
安全阈值判断
```

接下来会进入一个明显更有意思的阶段：**运算符 + `if/else` + 循环**。届时这个程序会升级为持续运行的机器人监控器，而不是输入一次就退出。

再往后学习**数组**以后，就不再只有：

```
double joint_angle;
```

而会第一次出现真正像机械臂的东西：

```
double joint_angles[6];
```

这会成为我们通向 **6 轴机械臂 → 坐标变换 → 正运动学 → EI-Beginner Task 1** 的第一座桥。