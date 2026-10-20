# Day 8｜指针②：数组和指针为什么关系这么近？

上一课你已经掌握了最核心的模型：

```
变量 → & → 地址
地址 → * → 数据
```

今天继续解决 Day 6 留下的问题：为什么下面这个函数能够直接修改机械臂的原数组？

```
void read_joints(double joints[])
```

以及为什么：

```
joints[i]
```

可以和指针联系起来。

今天只学**一维数组 + 一级指针**，不碰二级指针、`malloc`。约 40 分钟。

## 0～5 分钟｜复习：先把 `&` 和 `*` 焊死

先建立目录：

```
mkdir -p ~/c-learning/day08
cd ~/c-learning/day08
code .
```

看这段：

```
int battery = 80;
int *p = &battery;
```

脑中必须能画出：

```
假设 battery 位于 0x1000

battery
┌─────────────┐
│     80      │
└─────────────┘
地址 0x1000
      ↑
      │
      │ p 保存 0x1000
      │
┌─────────────┐
│   0x1000    │
└─────────────┘
      p
```

所以：

```
battery   → 80
&battery  → 0x1000
p         → 0x1000
*p        → 80
```

如果：

```
*p = 20;
```

相当于：

```
沿着 p
↓
找到 0x1000
↓
把里面的数据改成 20
```

因此：

```
printf("%d\n", battery);
```

输出：

```
20
```

今天所有知识都建立在这张图上。

---

# 5～20 分钟｜核心：数组在内存里是什么？

昨天以前我们一直写：

```
double joints[6] = {
    10, 20, 30, 40, 50, 60
};
```

不要只把它理解成 Python：

```
joints = [10, 20, 30, 40, 50, 60]
```

C 更底层。

它在内存里是一段**连续空间**。

假设一个 `double` 占 8 字节：

```
低地址                                  高地址 →

joints[0] joints[1] joints[2] joints[3] joints[4] joints[5]

   10        20        30        40        50        60
┌────────┬────────┬────────┬────────┬────────┬────────┐
│ 8 byte │ 8 byte │ 8 byte │ 8 byte │ 8 byte │ 8 byte │
└────────┴────────┴────────┴────────┴────────┴────────┘
0x1000   0x1008   0x1010   0x1018   0x1020   0x1028
```

地址只是示意，但**连续排列**这个思想非常重要。

所以：

```
&joints[0]
```

是第一个元素的地址。

```
&joints[1]
```

是第二个元素的地址。

我们来亲眼看看。

创建：

```
touch array_pointer.c
```

写：

```
#include <stdio.h>

int main(void)
{
    double joints[3] = {10.0, 20.0, 30.0};

    printf("joints[0] = %.1f\n", joints[0]);
    printf("joints[1] = %.1f\n", joints[1]);
    printf("joints[2] = %.1f\n", joints[2]);

    printf("\nAddresses:\n");

    printf("&joints[0] = %p\n", (void *)&joints[0]);
    printf("&joints[1] = %p\n", (void *)&joints[1]);
    printf("&joints[2] = %p\n", (void *)&joints[2]);

    return 0;
}
```

编译：

```
gcc -Wall -Wextra -g array_pointer.c -o array_pointer
./array_pointer
```

你可能看到：

```
&joints[0] = 0x7ffd...20
&joints[1] = 0x7ffd...28
&joints[2] = 0x7ffd...30
```

如果你的机器上 `sizeof(double) == 8`，通常会发现相邻地址差 **8 字节**。

因为：

```
double
≈ 8 bytes
```

---

## 数组名 `joints` 是什么？

现在进入今天最关键的一步。

加：

```
printf("%p\n", (void *)joints);
printf("%p\n", (void *)&joints[0]);
```

你通常会看到相同的地址值。

也就是说，在**大多数表达式中**：

```
joints
```

会转换成：

```
&joints[0]
```

也就是：

> 指向第一个元素的指针。

注意我的措辞：

**不要背“数组就是指针”。**

这是不严谨的。

应该记：

> **数组不是指针变量，但数组名在很多表达式中会转换为指向首元素的指针。**

这个区别以后非常重要。

---

# 那么 `*joints` 呢？

既然很多表达式里：

```
joints
↓
第一个元素地址
```

那么：

```
*joints
```

就是：

```
第一个元素地址
↓
*
↓
取地址里的数据
```

因此：

```
printf("%.1f\n", *joints);
```

得到：

```
10.0
```

所以：

```
joints[0]
```

和：

```
*joints
```

都能取得第一个元素。

---

# 指针移动

现在：

```
joints
```

指向：

```
joints[0]
```

那么：

```
joints + 1
```

呢？

不是简单地把内存地址加 1 字节。

因为它是：

```
double *
```

所以：

```
joints + 1
```

表示：

> 向后移动 **1 个 double**。

如果一个 double 是 8 字节：

```
joints
↓
0x1000

joints + 1
↓
0x1008

joints + 2
↓
0x1010
```

于是：

```
*(joints + 0)
```

得到：

```
10
```

```
*(joints + 1)
```

得到：

```
20
```

```
*(joints + 2)
```

得到：

```
30
```

这就出现了今天最重要的等价关系：

```
joints[i]

      ⇅

*(joints + i)
```

例如：

```
joints[2]
```

可以理解成：

```
*(joints + 2)
```

---

## 为什么不是 `*joints + 2`？

括号非常重要。

```
*(joints + 2)
```

表示：

```
先移动到第三个元素地址
↓
再取值
```

结果：

```
30
```

而：

```
*joints + 2
```

表示：

```
先取得第一个元素
↓
10
↓
+2
↓
12
```

完全不同。

这也是 C 里运算符优先级开始变重要的地方。

---

# Python 和 C 在这里真正分叉了

Python：

```
joints = [10, 20, 30]print(joints[1])
```

你通常只需要考虑：

```
我要第几个元素？
```

C：

```
double joints[3] = {10, 20, 30};
```

既可以：

```
joints[1]
```

也可以从底层理解成：

```
数组首地址
   ↓
移动 1 个 double
   ↓
解引用
   ↓
20
```

即：

```
*(joints + 1)
```

这就是为什么 C 特别适合嵌入式、驱动、机器人底层：程序员可以非常明确地处理内存中的数据布局。

---

# 20～35 分钟｜机器人项目：Joint Calibration v0.8

今天做一个很实际的机器人概念：

**关节零点校准。**

假设机械臂传感器读出的角度：

```
Joint 1 = 12°
Joint 2 = -18°
Joint 3 = 32°
```

但是每个编码器存在零点偏差：

```
Joint 1 offset = +2°
Joint 2 offset = -3°
Joint 3 offset = +1°
```

校准：

```
真实角度 = 原始角度 - offset
```

我们用函数 + 数组 + 指针完成。

创建：

```
touch joint_calibration.c
```

完整代码：

```
#include <stdio.h>

#define JOINT_COUNT 6

void print_joints(const double *joints)
{
    for (int i = 0; i < JOINT_COUNT; i++)
    {
        printf("Joint %d: %7.2f deg\n",
               i + 1,
               joints[i]);
    }
}


void calibrate_joints(double *joints,
                      const double *offsets)
{
    for (int i = 0; i < JOINT_COUNT; i++)
    {
        joints[i] = joints[i] - offsets[i];
    }
}


int main(void)
{
    double joints[JOINT_COUNT] =
    {
        12.0,
        -18.0,
        32.0,
        46.0,
        -25.0,
        91.0
    };

    double offsets[JOINT_COUNT] =
    {
        2.0,
        -3.0,
        1.0,
        1.5,
        -2.0,
        1.0
    };

    printf("=== Raw Joint Angles ===\n");

    print_joints(joints);

    calibrate_joints(joints, offsets);

    printf("\n=== Calibrated Joint Angles ===\n");

    print_joints(joints);

    return 0;
}
```

编译：

```
gcc -Wall -Wextra -g joint_calibration.c -o joint_calibration
```

运行：

```
./joint_calibration
```

你应该看到大致：

```
=== Raw Joint Angles ===
Joint 1:   12.00 deg
Joint 2:  -18.00 deg
Joint 3:   32.00 deg
Joint 4:   46.00 deg
Joint 5:  -25.00 deg
Joint 6:   91.00 deg

=== Calibrated Joint Angles ===
Joint 1:   10.00 deg
Joint 2:  -15.00 deg
Joint 3:   31.00 deg
Joint 4:   44.50 deg
Joint 5:  -23.00 deg
Joint 6:   90.00 deg
```

---

## 数据是怎么流的？

这是今天最值得理解的地方：

```
main()

joints[6]
┌────┬────┬────┬────┬────┬────┐
│12  │-18 │32  │46  │-25 │91  │
└────┴────┴────┴────┴────┴────┘
  ↑
  │ 首元素地址
  │
  └──────────────┐
                 ↓
       calibrate_joints()
                 │
          double *joints
                 │
                 ↓
             joints[i]
                 │
                 ↓
       修改原数组中的数据
```

函数：

```
void calibrate_joints(double *joints,
                      const double *offsets)
```

实际上收到的是数组元素所在内存区域的地址。

所以：

```
joints[i] = joints[i] - offsets[i];
```

能够直接修改 `main()` 中原来的数组。

这就是 Day 6 那个问题的答案。

---

# 新出现的 `const` 是什么？

你看到：

```
const double *offsets
```

暂时读成：

> 我只准备通过这个指针**读取数据，不修改它指向的数据**。

因为 offset 是校准参数：

```
读取 offset
↓
计算

而不是：

修改 offset
```

所以写：

```
const double *offsets
```

是一种很好的工程习惯。

同样：

```
void print_joints(const double *joints)
```

`print_joints()` 只负责打印，不应该修改关节数据。

于是使用：

```
const
```

相当于提前告诉编译器：

> 这个函数只有“读取权限”。

这和机器人项目里的职责分离很重要。

---

# 今日 GDB｜真正观察指针移动

用：

```
gdb ./joint_calibration
```

进入后：

```
break calibrate_joints
run
```

程序停到函数附近后，可以尝试：

```
print joints
```

你会看到地址。

然后：

```
print joints[0]
```

再：

```
print *joints
```

两者都应该对应第一个关节：

```
12
```

继续：

```
print joints[1]
```

和：

```
print *(joints + 1)
```

两者都应该得到：

```
-18
```

这次 GDB 实验非常重要，因为你是在**亲眼验证**：

```
joints[i]
=
*(joints + i)
```

而不是背教材。

---

# 一个必须避免的错误

千万不要这样：

```
double joints[6];

for (int i = 0; i <= 6; i++)
{
    joints[i] = 0;
}
```

最后一次：

```
i = 6
```

意味着：

```
joints[6]
```

但合法只有：

```
joints[0]
...
joints[5]
```

指针视角下，你实际上在：

```
数组首地址
↓
向后移动 6 个 double
↓
写入数据
```

而那里已经不属于这个数组。

这就是：

**out-of-bounds access（越界访问）**。

C 通常不会像 Python 一样友好地直接给你 `IndexError`，所以这种 Bug 特别危险。

---

# 35～40 分钟｜自测

**1.**

```
double joints[6];
```

在很多表达式中：

```
joints
```

会转换为什么？

答案：

```
&joints[0]
```

也就是指向第一个元素的指针。

**2.**

```
joints[2]
```

可以写成什么指针形式？

答案：

```
*(joints + 2)
```

**3. 如果 `joints` 是 `double *`，`joints + 1` 是地址简单增加 1 字节吗？**

不是。

它移动：

```
1 个 double
```

如果当前平台一个 `double` 是 8 字节，那么通常就是向后移动 8 字节。

**4. 为什么下面函数能修改原数组？**

```
void reset(double *data)
{
    data[0] = 0;
}
```

因为 `data` 指向原数组的数据区域，`data[0]` 操作的是那块原始内存。

**5. `const double *data` 暂时怎么理解？**

答案：

> 可以通过 `data` 读取它指向的 `double` 数据，但不应该通过这个指针修改那些数据。

**6. 判断：数组就是指针。**

答案：

**错。**

更准确：

> 数组和指针是不同概念，但数组名在许多表达式中会转换为指向首元素的指针。

这是以后面试、C/C++ 学习都很重要的区别。

---

今天把这条链真正打通即可：

```
double joints[6]
        │
        ↓
连续内存
        │
        ↓
joints
        │
多数表达式中转换
        ↓
&joints[0]
        │
        ↓
double *
        │
        ├──── joints + 0 ──→ Joint 1
        ├──── joints + 1 ──→ Joint 2
        ├──── joints + 2 ──→ Joint 3
        │
        ↓
      *(...)
        │
        ↓
      数据值
```

我们现在已经从“会写 C 语法”逐渐进入 C 真正的核心：

```
变量
↓
数组
↓
函数
↓
地址
↓
指针
↓
数组 + 指针
```

**下一课不继续堆复杂指针。** 下一步更适合进入 `struct` 结构体：目前一个关节只有一个 `double angle`，但真正机器人关节至少可能需要 `angle / velocity / torque / id / enabled` 等状态。我们会把它们组合成：

```
typedef struct
{
    int id;
    double angle;
    double velocity;
    double torque;
} Joint;
```

然后从：

```
double joints[6];
```

升级成：

```
Joint joints[6];
```

到这里，我们的数据模型才会第一次真正像一个机械臂。