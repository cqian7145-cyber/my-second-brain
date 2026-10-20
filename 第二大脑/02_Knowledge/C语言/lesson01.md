好，正式开始 **Day 1**。今天控制在约 **40 分钟**，主题是：

> **从 Python 程序员第一次真正进入 C：源代码 → GCC → 可执行程序 → 第一个机器人状态监视器**

《C Primer Plus》第 1 章专门强调：C 是**编译型语言**，基本过程是源代码经过编译、链接后生成可执行文件；Linux 下通常直接使用 GCC。第 2 章才开始真正拆解 `main()`、`printf()`、变量、赋值这些基本结构。我们今天就严格沿这条线走。 C Primer Plus 第6版 中文版

---

# Day 1｜C 程序到底是怎么跑起来的

## 0～5 分钟：先理解 Python 和 C 最大的区别

你以前写 Python：

```
print("Hello Robot")
```

然后：

```
python hello.py
```

Python 大致是：

```
hello.py
   ↓
Python 解释器
   ↓
执行
```

而 C 不是直接运行 `.c`：

```
robot.c
   ↓
GCC 编译
   ↓
目标代码
   ↓
链接
   ↓
robot 可执行文件
   ↓
运行
```

所以以后你看到：

```
gcc robot.c -o robot
./robot
```

不要把它当两条莫名其妙的 Linux 命令。

它实际表示：

```
第一条：把 C 源代码制造成程序
第二条：运行制造出来的程序
```

书中第 1 章把这个过程进一步拆成了**编译器 + 链接器**：编译器负责翻译你的源代码，链接器再把你的代码、启动代码以及像 `printf()` 这样的库代码组合成最终的可执行程序。 C Primer Plus 第6版 中文版

---

# 5～12 分钟：进入 Linux

你在 Windows 上学习，但我们主要在：

```
Windows
└── WSL2
    └── Ubuntu
        ├── GCC
        ├── Git
        ├── C
        └── 以后 ROS / Robot
```

进行开发。

先打开 **Ubuntu / WSL 终端**。

不要在 PowerShell 里执行下面的 Linux 命令。

先输入：

```
pwd
```

意思：

```
print working directory
显示：我现在在哪个目录？
```

再：

```
ls
```

相当于查看当前文件夹。

创建我们的学习目录：

```
mkdir -p ~/c-learning/day01
```

进入：

```
cd ~/c-learning/day01
```

再次：

```
pwd
```

你应该看到类似：

```
/home/你的用户名/c-learning/day01
```

然后检查 GCC：

```
gcc --version
```

如果成功出现类似：

```
gcc (Ubuntu ...) ...
```

环境基本没问题。

如果显示：

```
gcc: command not found
```

执行：

```
sudo apt update
sudo apt install build-essential
```

安装完成后：

```
gcc --version
```

---

# 12～22 分钟：第一个 C 程序

创建：

```
hello_robot.c
```

如果 VS Code 已经能进入 WSL：

```
code .
```

然后新建：

```
hello_robot.c
```

输入：

```
#include <stdio.h>

int main(void)
{
    printf("Hello, Robot!\n");

    return 0;
}
```

先不要背。

我们逐行拆。

---

## 第一行

```
#include <stdio.h>
```

暂时理解为：

> 我要使用 C 的标准输入输出工具。

`stdio`

就是：

```
standard input output
标准输入输出
```

为什么需要它？

因为下面用了：

```
printf()
```

《C Primer Plus》第 2 章就是这样解释的：`stdio.h` 为类似 `printf()` 的标准输入输出函数提供必要信息。 C Primer Plus 第6版 中文版

---

# 第二部分

```
int main(void)
```

这是 C 程序非常关键的东西。

所有普通 C 程序都从：

```
main()
```

开始执行。

暂时把：

```
int main(void)
```

理解为：

> 这里是程序入口。

Python：

```
print("hello")
```

可以直接从第一行开始。

C：

```
int main(void)
{
}
```

一般明确告诉计算机：

```
程序从这里开始。
```

---

# `{ }`

```
{
    ...
}
```

表示：

```
main 函数里面包含什么。
```

可以粗暴理解成 Python 的缩进块。

Python：

```
if condition:    do_something()
```

C：

```
if (condition)
{
    do_something();
}
```

不过 `if` 我们之后再学。

---

# `printf()`

```
printf("Hello, Robot!\n");
```

对应 Python：

```
print("Hello, Robot!")
```

其中：

```
\n
```

表示：

```
newline
换行
```

也就是说：

```
printf("Hello");
printf("Robot");
```

得到：

```
HelloRobot
```

而：

```
printf("Hello\n");
printf("Robot\n");
```

得到：

```
Hello
Robot
```

---

# 分号 `;`

这是你从 Python 转 C 后首先要形成的肌肉记忆。

C：

```
printf("Hello\n");
```

最后：

```
;
```

不能随便漏。

Python：

```
x = 10
```

C：

```
int x = 10;
```

以后你至少有几十次会因为忘记分号报错。

正常。

---

# `return 0`

```
return 0;
```

暂时理解：

> main 正常结束，告诉操作系统“程序成功执行”。

书里也特意指出，在 Linux / UNIX 环境中，这个返回值实际上是有用途的。 C Primer Plus 第6版 中文版

---

# 22～27 分钟：真正编译

在 Ubuntu 终端中确认：

```
ls
```

应该看到：

```
hello_robot.c
```

执行：

```
gcc hello_robot.c -o hello_robot
```

如果没出现任何输出：

**通常是好事。**

再：

```
ls
```

现在应该看到：

```
hello_robot
hello_robot.c
```

这里非常重要：

```
hello_robot.c
```

是：

**源代码**

而：

```
hello_robot
```

是：

**可执行程序**

运行：

```
./hello_robot
```

应该输出：

```
Hello, Robot!
```

---

# 为什么一定要 `./`？

你可能会问：

为什么不是：

```
hello_robot
```

而是：

```
./hello_robot
```

`./`

表示：

```
当前目录
```

所以整句话其实是：

```
运行当前目录下面的 hello_robot
```

这就是第一点 Linux 思维。

---

# 27～35 分钟：第一个机器人微项目

现在不写普通的：

```
Hello World
```

我们直接搞：

## Robot Status Monitor v0.1

创建：

```
robot_status.c
```

代码：

```
#include <stdio.h>

int main(void)
{
    int robot_id = 1;
    int joint_count = 6;
    int battery = 87;
    int motor_temperature = 42;

    printf("===== Robot Status =====\n");

    printf("Robot ID: %d\n", robot_id);
    printf("Joint Count: %d\n", joint_count);
    printf("Battery: %d%%\n", battery);
    printf("Motor Temperature: %d C\n", motor_temperature);

    printf("========================\n");

    return 0;
}
```

编译：

```
gcc robot_status.c -o robot_status
```

运行：

```
./robot_status
```

应该得到：

```
===== Robot Status =====
Robot ID: 1
Joint Count: 6
Battery: 87%
Motor Temperature: 42 C
========================
```

这已经是我们第一个极简机器人程序了。

---

# 今天第一个新概念：变量

看：

```
int battery = 87;
```

Python：

```
battery = 87
```

C：

```
int battery = 87;
```

最大的区别：

Python：

> 你先给我数据，我大致自己处理类型。

C：

> 你先明确告诉我，这块内存里准备存什么东西。

所以：

```
int
```

表示：

```
integer
整数
```

> [!NOTE]
> 这就是 C 和 Python 思维开始真正分叉的地方。==设置变量前先声明==

---

# `%d` 是什么？

看：

```
printf("Battery: %d%%\n", battery);
```

这里：

```
%d
```

意思：

><mark style="background: #FFB8EBA6;"> 这里放一个十进制整数</mark>。

例如：

```
int battery = 87;

printf("%d\n", battery);
```

输出：

```
87
```

可以把 `%d` 暂时理解成一个：

```
整数占位符
```

Python 类似：

```
battery = 87print(f"Battery: {battery}")
```

C：

```
printf("Battery: %d\n", battery);
```

---

# 为什么这里有 `%%`

因为单独 `%` 在 `printf()` 里面有特殊意义。

所以如果真的想打印：

```
%
```

要写：

```
%%
```

因此：

```
printf("Battery: %d%%\n", battery);
```

得到：

```
Battery: 87%
```

---

# 35～38 分钟：故意制造 Bug

这是今天非常重要的一步。

把：

```
printf("Robot ID: %d\n", robot_id);
```

最后的：

```
;
```

删掉。

变成：

```
printf("Robot ID: %d\n", robot_id)
```

重新：

```
gcc robot_status.c -o robot_status
```

看看 GCC 怎么骂你。

然后把 `;` 加回来。

《C Primer Plus》第 2 章专门区分了：

```
语法错误 syntax error
```

和：

```
语义错误 semantic error
```

今天这个：

```
漏分号
```

属于：

**语法错误。**

以后：

```
battery = 87;
```

如果你实际想表达的是：

```
battery = 50
```

代码合法但逻辑错了，这属于：

**语义错误。**

---

# 38～40 分钟：今天只记 7 个东西

不要背整章。

今天能把下面 7 个东西说清楚就够了：

<span style="color:rgb(0, 0, 0)">```</span>
1. <span style="color:rgb(0, 0, 0)">.c</span>
<span style="color:rgb(0, 0, 0)">C 源代码文件</span>

2. <span style="color:rgb(0, 0, 0)">gcc</span>
<span style="color:rgb(0, 0, 0)">C 编译器</span>

3. <span style="color:rgb(0, 0, 0)">main()</span>
<span style="color:rgb(0, 0, 0)">C 程序入口</span>

4. <span style="color:rgb(0, 0, 0)">printf()</span>
<span style="color:rgb(0, 0, 0)">输出</span>

5. <span style="color:rgb(0, 0, 0)">int</span>
<span style="color:rgb(0, 0, 0)">整数类型</span>

6. <span style="color:rgb(0, 0, 0)">;</span>
<span style="color:rgb(0, 0, 0)">结束一条语句</span>

7. <span style="color:rgb(0, 0, 0)">./robot_status</span>
<span style="color:rgb(0, 0, 0)">运行当前目录的程序</span>
<span style="color:rgb(0, 0, 0)">```</span>

<span style="color:rgb(0, 1, 0)">再记住整个链</span>：

```
robot_status.c
       ↓
      gcc
       ↓
robot_status
       ↓
./robot_status
       ↓
程序运行
```

这其实就是《C Primer Plus》第 1 章最值得你真正理解的一件事。

---

## 今天的 P<span style="color:rgb(0, 0, 0)">yth</span>on → C 对照

| Python        | C                 |     |
| ------------- | ----------------- | --- |
| `print("Hi")` | `printf("Hi\n");` |     |
| `x = 10`      | `int x = 10;`     |     |
| 不需要编译         | GCC 编译            |     |
| `.py`         | `.c`              |     |
| Python 解释器执行  | 生成原生可执行程序         |     |
| 类型比较动态        | 类型需要明确声明          |     |

---

## 今天的项目进度

我们的最终长线不是：

```
打印 Hello World
```

而是：

```
Robot Status Monitor v0.1
        ↓
用户输入
        ↓
电机状态判断
        ↓
多个关节
        ↓
数组
        ↓
Joint 结构体
        ↓
机器人状态系统
        ↓
向量 / 矩阵
        ↓
机械臂运动学
        ↓
EI-Beginner Task 1
```

今天的：

```
int joint_count = 6;
```

几周之后会变成类似：

```
Joint joints[6];
```

再之后就是机械臂真正的关节状态。

**现在先去实际运行 `robot_status.c`。**  
你下一条直接把终端输出或者报错贴给我，我们继续 **Day 1 后半段：`gcc -Wall -Wextra` + Linux 文件系统 + 让机器人程序接受用户输入**。