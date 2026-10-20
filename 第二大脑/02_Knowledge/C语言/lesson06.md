# Day 6｜函数 `function`：把机器人程序拆成真正的“模块”

昨天我们已经做到：

```
double joint_angles[6]
        ↓
for 遍历 6 个关节
        ↓
if 检查安全范围
```

今天进入《C Primer Plus》中非常核心的一块：**函数**。你已经学过 Python 函数，所以这一课重点不是“函数是什么”，而是搞懂 **C 函数和 Python 函数到底哪里不同**。

今天结束时，我们会把之前越来越长的 `main()`：

```
main
 ├─ 输入关节
 ├─ 检查关节
 ├─ 输出状态
 └─ 判断能否运动
```

重构成：

```
main
 ├─ read_joints()
 ├─ print_joints()
 ├─ check_joints()
 └─ 最终决策
```

这已经开始接近真正工程代码的组织方式。

---

## 0～5 分钟｜复习：数组 + for

先建立今天目录：

```
mkdir -p ~/c-learning/day06
cd ~/c-learning/day06
code .
```

昨天最重要的代码：

```
#define JOINT_COUNT 6

double joint_angles[JOINT_COUNT];

for (int i = 0; i < JOINT_COUNT; i++)
{
    scanf("%lf", &joint_angles[i]);
}
```

脑子里应该形成：

```
joint_angles
       ↓
┌────┬────┬────┬────┬────┬────┐
│ 10 │ 20 │ 30 │ 40 │ 50 │ 60 │
└────┴────┴────┴────┴────┴────┘
  [0]  [1]  [2]  [3]  [4]  [5]

          ↑
          i
          ↑
       for循环
```

但现在有一个工程问题：如果所有东西都塞进 `main()`，程序以后会变成几百甚至几千行。

所以需要函数。

---

# 5～20 分钟｜核心：C 函数

Python：

```
def stop_robot():    print("Robot stopped")
```

调用：

```
stop_robot()
```

C：

```
void stop_robot(void)
{
    printf("Robot stopped\n");
}
```

调用：

```
stop_robot();
```

两者思想完全一样：

> 给一段代码起名字，需要的时候调用。

但 C 比 Python 多了一件非常重要的事情：

> **必须明确输入类型和返回类型。**

---

## 第一种函数：没有输入，没有返回值

```
void show_title(void)
{
    printf("=== Robot Controller ===\n");
}
```

这里两个 `void` 分别理解。

前面的：

```
void show_title
```

表示：

> 这个函数不返回数据。

括号里面：

```
show_title(void)
```

表示：

> 这个函数不接收参数。

所以：

```
void show_title(void)
```

可以读成：

> `show_title` 是一个不接收参数，也不返回结果的函数。

调用：

```
show_title();
```

---

# 第二种：有输入，没有返回值

机器人报警：

```
void warning_temperature(double temperature)
{
    printf("WARNING: %.2f C\n", temperature);
}
```

调用：

```
warning_temperature(85.5);
```

这里：

```
85.5
 ↓
temperature
 ↓
函数内部
```

Python：

```
def warning_temperature(temperature):    print(temperature)
```

C：

```
void warning_temperature(double temperature)
```

区别已经非常明显：

```
Python
temperature

C
double temperature
```

C 要明确告诉编译器参数是什么类型。

---

# 第三种：有输入，有返回值

这才是今天最重要的一种。

假设判断关节是否安全：

```
int is_joint_safe(double angle)
{
    if (angle >= -180.0 && angle <= 180.0)
    {
        return 1;
    }

    return 0;
}
```

调用：

```
int result;

result = is_joint_safe(90.0);
```

如果输入：

```
90°
```

返回：

```
1
```

如果：

```
250°
```

返回：

```
0
```

于是：

```
if (is_joint_safe(angle))
{
    printf("SAFE\n");
}
```

这已经非常接近自然语言：

> 如果这个关节是安全的，就打印 SAFE。

---

# 函数的基本结构

记住：

```
返回类型 函数名(参数)
{
    函数代码;

    return 返回值;
}
```

例如：

```
double add(double a, double b)
{
    return a + b;
}
```

那么：

```
double result = add(3.0, 5.0);
```

得到：

```
8.0
```

Python：

```
def add(a, b):    return a + b
```

C：

```
double add(double a, double b)
{
    return a + b;
}
```

这里正体现 C 的特点：

```
                Python        C

参数类型          不写        double
返回类型          不写        double
语句结束          换行         ;
代码块            缩进         {}
```

---

# 一个非常重要的概念：局部变量

```
void test(void)
{
    int x = 10;
}
```

这里：

```
x
```

通常只在：

```
{
    ...
}
```

这个函数作用域里存在。

你不能在 `main()` 里随便：

```
printf("%d", x);
```

这叫：

**scope（作用域）**。

可以先理解成：

> 每个函数都有自己的“小房间”。

例如：

```
main()
┌─────────────────┐
│ battery         │
│ joint_angles    │
└─────────────────┘

check_joint()
┌─────────────────┐
│ angle           │
│ result          │
└─────────────────┘
```

两个函数里的局部变量一般互不直接干扰。

这是模块化的基础。

---

# 20～35 分钟｜机器人项目 v0.6

今天把昨天的程序重构成：

> **Modular Robot Arm Safety Controller**

创建：

```
touch robot_arm_v06.c
```

完整代码：

```
#include <stdio.h>

#define JOINT_COUNT 6
#define MIN_ANGLE -180.0
#define MAX_ANGLE 180.0


void show_title(void)
{
    printf("================================\n");
    printf("  Robot Arm Safety Controller\n");
    printf("================================\n");
}


int is_joint_safe(double angle)
{
    if (angle >= MIN_ANGLE && angle <= MAX_ANGLE)
    {
        return 1;
    }

    return 0;
}


void read_joints(double joints[])
{
    for (int i = 0; i < JOINT_COUNT; i++)
    {
        printf("Enter Joint %d angle: ", i + 1);

        scanf("%lf", &joints[i]);
    }
}


void print_joints(double joints[])
{
    printf("\n=== Joint Status ===\n");

    for (int i = 0; i < JOINT_COUNT; i++)
    {
        printf("Joint %d: %7.2f deg",
               i + 1,
               joints[i]);

        if (is_joint_safe(joints[i]))
        {
            printf("  [OK]\n");
        }
        else
        {
            printf("  [UNSAFE]\n");
        }
    }
}


int count_unsafe_joints(double joints[])
{
    int unsafe_count = 0;

    for (int i = 0; i < JOINT_COUNT; i++)
    {
        if (!is_joint_safe(joints[i]))
        {
            unsafe_count++;
        }
    }

    return unsafe_count;
}


int main(void)
{
    double joint_angles[JOINT_COUNT];

    show_title();

    read_joints(joint_angles);

    print_joints(joint_angles);

    int unsafe_count =
        count_unsafe_joints(joint_angles);

    printf("\n=== Control Decision ===\n");

    if (unsafe_count == 0)
    {
        printf("SAFE: ARM CAN MOVE\n");
    }
    else
    {
        printf("UNSAFE: EMERGENCY STOP\n");

        printf("Unsafe joints: %d\n",
               unsafe_count);
    }

    return 0;
}
```

编译：

```
gcc -Wall -Wextra robot_arm_v06.c -o robot_arm_v06
```

运行：

```
./robot_arm_v06
```

测试：

```
10
20
30
40
50
60
```

应该得到：

```
Joint 1:   10.00 deg  [OK]
Joint 2:   20.00 deg  [OK]
...
Joint 6:   60.00 deg  [OK]

SAFE: ARM CAN MOVE
```

再输入：

```
10
20
230
40
-220
60
```

应该发现：

```
Joint 3: 230.00 deg  [UNSAFE]

Joint 5: -220.00 deg [UNSAFE]

UNSAFE: EMERGENCY STOP
Unsafe joints: 2
```

---

# 重点理解：数据到底怎么走

现在我们的程序已经不是从上往下一坨代码了。

实际调用关系：

```
                main()
                  │
          ┌───────┼────────┐
          ↓       ↓        ↓
    show_title  read     print
                  │        │
                  │        ↓
                  │   is_joint_safe
                  │
                  ↓
          joint_angles[6]
                  │
                  ↓
       count_unsafe_joints
                  │
                  ↓
          is_joint_safe
                  │
                  ↓
            unsafe_count
                  │
                  ↓
             MOVE / STOP
```

这叫：

**函数调用关系。**

以后真正机器人项目不会：

```
main.c
5000 行
```

而会逐渐变成：

```
main
 ↓
sensor
 ↓
kinematics
 ↓
controller
 ↓
motor
```

每个模块负责自己的事情。

---

# 一个非常值得注意的问题

看看：

```
void read_joints(double joints[])
```

然后：

```
read_joints(joint_angles);
```

函数里面居然能够修改：

```
joint_angles
```

你可能会发现：

之前：

```
void test(double x)
```

里面修改 `x`，通常不会直接修改外面的变量。

但数组却似乎不一样。

为什么？

答案先不给复杂版本，只告诉你：

> **数组传入函数时和内存地址存在非常紧密的关系。**

也就是说今天这行：

```
void read_joints(double joints[])
```

其实已经一只脚踩进：

**pointer（指针）**

了。

后面我们会看到：

```
double *joints
```

你最终会发现：

```
void read_joints(double joints[])
```

和某些场景下的：

```
void read_joints(double *joints)
```

关系非常密切。

这就是 C 最重要的一道门。

---

# 今日 Linux｜第一次用 `gdb` 看程序内部

之前我们的调试方法基本是：

```
printf(...)
```

今天第一次认识真正的 C 调试器：

**GDB — GNU Debugger**

先检查：

```
gdb --version
```

如果没有：

```
sudo apt update
sudo apt install gdb
```

调试时编译稍微不同：

```
gcc -Wall -Wextra -g robot_arm_v06.c -o robot_arm_v06
```

这里：

```
-g
```

表示：

> 在可执行文件中加入调试信息。

进入：

```
gdb ./robot_arm_v06
```

你会进入类似：

```
(gdb)
```

输入：

```
break main
```

意思：

> 在 `main()` 开头设置断点。

然后：

```
run
```

程序会停在 `main()`。

输入：

```
next
```

执行下一行。

可以反复：

```
next
```

退出：

```
quit
```

今天不要学一大堆 GDB 命令。

只认识 4 个：

```
break main
    ↓
设置断点

run
    ↓
运行

next
    ↓
执行下一行

quit
    ↓
退出
```

以后我们遇到真正 Bug，再系统学习：

```
print
step
backtrace
watch
```

---

# 35～40 分钟｜自测

**1. Python：**

```
def add(a, b):    return a + b
```

C 如果都是整数：

```
int add(int a, int b)
{
    return a + b;
}
```

---

**2.**

```
void hello(void)
```

两个 `void` 分别什么意思？

答案：

前面的：

```
不返回值
```

后面的：

```
不接收参数
```

---

**3.**

```
int is_joint_safe(double angle)
```

说明什么？

答案：

```
函数名：
is_joint_safe

输入：
一个 double

输出：
一个 int
```

---

**4. 为什么写函数？**

不是单纯为了少写代码。

更重要的是：

```
复杂问题
   ↓
拆成小问题
   ↓
每个函数负责一个职责
```

例如：

```
读取数据
→ read_joints()

判断安全
→ is_joint_safe()

统计异常
→ count_unsafe_joints()

显示状态
→ print_joints()
```

---

**5. `-g` 是干什么的？**

```
gcc -g robot.c -o robot
```

答案：

> 加入调试信息，方便 GDB 调试程序。

---

**6. GDB 中：**

```
break main
```

设置断点。

```
run
```

运行。

```
next
```

执行下一行。

```
quit
```

退出。

---

今天最重要的不是把函数语法背下来，而是完成一次思维升级：

```
之前：

main()
 │
 ├── 输入
 ├── 判断
 ├── 循环
 ├── 输出
 ├── 安全检查
 └── 控制


现在：

                main()
                  │
       ┌──────────┼──────────┐
       ↓          ↓          ↓
 read_joints   check      print
       │          │
       └──────┬───┘
              ↓
        Robot State
              ↓
        Control Decision
```

我们目前已经完成：

```
Day 1  编译 / printf
  ↓
Day 2  数据类型 / scanf
  ↓
Day 3  if / else
  ↓
Day 4  while / for
  ↓
Day 5  array
  ↓
Day 6  function
```

下一阶段会进入 C 最有代表性的知识之一：**指针 `pointer`**。

但不会上来背：

```
int *p;
int **p;
```

这种符号。

我们会直接从今天留下的问题开始：

```
double joint_angles[6]
        ↓
read_joints(joint_angles)
        ↓
函数为什么能修改原数组？
        ↓
内存地址是什么？
        ↓
&joint_angles[0]
        ↓
double *p
        ↓
pointer
```

一旦这一关打通，你会第一次真正理解 **C 和 Python 在底层思维上的巨大差异**，也会开始理解为什么机器人、嵌入式、操作系统和高性能代码如此重视 C/C++。