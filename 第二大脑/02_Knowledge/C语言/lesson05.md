# Day 5｜数组 `array`：第一次真正表示“6 轴机械臂”

今天约 **40 分钟**。前四天你已经有了 `printf → scanf → if/else → while/for`，今天把这些知识第一次串成一个真正像机器人程序的数据结构：

```
一个关节：double joint_angle;
             ↓
六个关节：double joint_angles[6];
             ↓
for 循环遍历
             ↓
6 轴机械臂状态检查
```

这一步很关键。以后 EI-Beginner 的机械臂运动学，本质上会不断处理一组关节角、坐标和矩阵。

## 0～5 分钟：复习 `for`

昨天有：

```
for (int i = 0; i < 6; i++)
{
    printf("Joint %d\n", i);
}
```

它实际执行：

```
i = 0
i = 1
i = 2
i = 3
i = 4
i = 5
```

注意没有 `6`。

这件事今天非常重要，因为 C 数组也是从 **0** 开始编号。

先创建目录：

```
mkdir -p ~/c-learning/day05
cd ~/c-learning/day05
code .
```

---

# 5～20 分钟：数组到底是什么？

以前如果表示六个机械臂关节，你可以写：

```
double joint1 = 10.0;
double joint2 = 20.0;
double joint3 = 30.0;
double joint4 = 40.0;
double joint5 = 50.0;
double joint6 = 60.0;
```

能运行，但非常蠢。

假如以后是：

```
30 个关节
100 个激光雷达测距点
640×480 个深度数据
```

不可能一个变量一个变量写。

所以 C 提供：

```
double joint_angles[6];
```

把它想象成连续的 6 个小格子：

```
joint_angles

下标
  0       1       2       3       4       5
┌───────┬───────┬───────┬───────┬───────┐
│ 10.0  │ 20.0  │ 30.0  │ 40.0  │ 50.0  │ 60.0  │
└───────┴───────┴───────┴───────┴───────┘
   ↑
joint_angles[0]
```

这里有一个初学者必须立刻建立的概念：

> **6 个元素的数组，下标不是 1～6，而是 0～5。**

因此：

```
joint_angles[0]
```

第一个关节。

```
joint_angles[5]
```

第六个关节。

---

## Python 对比

Python：

```
joint_angles = [10, 20, 30, 40, 50, 60]print(joint_angles[0])
```

C：

```
double joint_angles[6] = {
    10.0,
    20.0,
    30.0,
    40.0,
    50.0,
    60.0
};

printf("%.2f\n", joint_angles[0]);
```

两者表面非常像。

但是底层区别很重要。

Python 的 `list`：

```
data = [10, "robot", 3.14]
```

可以放不同类型的对象。

C：

```
double joint_angles[6];
```

明确规定：

> 这里有 **6 个 `double`**。

因此它在内存中更接近：

```
地址增加 →

┌────────┬────────┬────────┬────────┬────────┬────────┐
│ double │ double │ double │ double │ double │ double │
└────────┴────────┴────────┴────────┴────────┴────────┘
```

这是我们以后学习**指针**的重要伏笔。

---

# 数组 + `for` 才是真正的组合

如果：

```
double joint_angles[6] =
{
    10, 20, 30, 40, 50, 60
};
```

我们当然可以：

```
printf("%.2f\n", joint_angles[0]);
printf("%.2f\n", joint_angles[1]);
printf("%.2f\n", joint_angles[2]);
printf("%.2f\n", joint_angles[3]);
printf("%.2f\n", joint_angles[4]);
printf("%.2f\n", joint_angles[5]);
```

但这又回到了重复代码。

真正的 C 写法：

```
for (int i = 0; i < 6; i++)
{
    printf("Joint %d: %.2f deg\n",
           i + 1,
           joint_angles[i]);
}
```

这里第一次出现了非常重要的模式：

```
array[i]
```

以后你会见无数次。

意思就是：

> 取数组中第 `i` 个位置的数据。

---

# 为什么打印 `i + 1`？

内部：

```
joint_angles[0]
```

是第一个元素。

但是人类通常说：

```
Joint 1
```

而不是：

```
Joint 0
```

所以：

```
printf("Joint %d", i + 1);
```

只是为了让输出更符合人的习惯。

程序内部仍然：

```
0 → 第一个
1 → 第二个
...
5 → 第六个
```

---

# 从键盘输入六个关节角

现在把 `scanf()` 和数组结合：

```
double joint_angles[6];

for (int i = 0; i < 6; i++)
{
    printf("Enter Joint %d angle: ", i + 1);
    scanf("%lf", &joint_angles[i]);
}
```

注意：

```
&joint_angles[i]
```

你之前见过：

```
&joint_angle
```

现在只是变成：

```
第 i 个数组元素的地址
```

这就是旧知识开始组合。

---

# 一个必须知道的危险：越界

如果数组：

```
double joints[6];
```

合法：

```
joints[0]
joints[1]
joints[2]
joints[3]
joints[4]
joints[5]
```

但是：

```
joints[6]
```

已经越界。

这在 Python 中：

```
joints[6]
```

通常直接：

```
IndexError
```

但 C 更危险。

C 不一定立即阻止你。

你可能会读写到**不属于这个数组的内存**。

所以这段代码是错的：

```
for (int i = 0; i <= 6; i++)
```

因为最后一次：

```
i = 6
```

会访问：

```
joints[6]
```

正确：

```
for (int i = 0; i < 6; i++)
```

这也是为什么 C 程序员特别敏感：

```
<
```

还是：

```
<=
```

---

# 20～35 分钟：6-Axis Robot Joint Monitor v0.5

今天把之前的单关节程序正式升级成：

> **六轴机械臂关节安全监视器**

创建：

```
touch robot_arm.c
```

完整代码：

```
#include <stdio.h>

#define JOINT_COUNT 6

int main(void)
{
    double joint_angles[JOINT_COUNT];
    int unsafe_count = 0;

    printf("=== 6-Axis Robot Joint Monitor ===\n\n");

    /* 输入六个关节角 */
    for (int i = 0; i < JOINT_COUNT; i++)
    {
        printf("Enter Joint %d angle (-180 to 180): ", i + 1);

        scanf("%lf", &joint_angles[i]);
    }

    printf("\n=== Joint Status ===\n");

    /* 检查六个关节 */
    for (int i = 0; i < JOINT_COUNT; i++)
    {
        printf("Joint %d: %7.2f deg",
               i + 1,
               joint_angles[i]);

        if (joint_angles[i] < -180.0 ||
            joint_angles[i] > 180.0)
        {
            printf("  [UNSAFE]\n");
            unsafe_count++;
        }
        else
        {
            printf("  [OK]\n");
        }
    }

    printf("\n=== Summary ===\n");

    if (unsafe_count == 0)
    {
        printf("All joints are safe.\n");
        printf("Decision: ARM CAN MOVE\n");
    }
    else
    {
        printf("%d unsafe joint(s) detected.\n",
               unsafe_count);

        printf("Decision: EMERGENCY STOP\n");
    }

    return 0;
}
```

今天第一次出现：

```
#define JOINT_COUNT 6
```

暂时理解成：

> 给数字 `6` 起一个有意义的名字。

于是不用到处：

```
double joint_angles[6];

for (int i = 0; i < 6; i++)
```

而可以：

```
double joint_angles[JOINT_COUNT];

for (int i = 0; i < JOINT_COUNT; i++)
```

代码可读性明显更好。

以后机械臂从 6 轴换成其他配置，也更容易调整。

---

## 编译

```
gcc -Wall -Wextra robot_arm.c -o robot_arm
```

运行：

```
./robot_arm
```

第一组：

```
10
-20
35
90
-45
120
```

你应该看到六个：

```
[OK]
```

最后：

```
All joints are safe.
Decision: ARM CAN MOVE
```

再故意测试：

```
10
-20
220
90
-250
120
```

那么：

```
Joint 3: 220.00 deg  [UNSAFE]
Joint 5: -250.00 deg [UNSAFE]
```

最后：

```
2 unsafe joint(s) detected.
Decision: EMERGENCY STOP
```

注意这里：

```
unsafe_count++;
```

每发现一个危险关节：

```
0
↓
1
↓
2
...
```

所以数组、循环、`if` 已经完全串起来了：

```
joint_angles[]
      ↓
     for
      ↓
joint_angles[i]
      ↓
     if
      ↓
   unsafe?
   ↙    ↘
 yes     no
  ↓       ↓
count++   OK
```

---

# 今天的 Linux：看看数组到底占多少内存

这是一个非常值得做的小实验。

在代码中加入：

```
printf("Size of double: %zu bytes\n",
       sizeof(double));

printf("Size of joint_angles: %zu bytes\n",
       sizeof(joint_angles));
```

重新：

```
gcc -Wall -Wextra robot_arm.c -o robot_arm
./robot_arm
```

在常见平台上你很可能看到：

```
Size of double: 8 bytes
Size of joint_angles: 48 bytes
```

为什么？

因为：

```
1 个 double
≈ 8 bytes

6 个 double
≈ 6 × 8
= 48 bytes
```

于是你脑子里的数组不要再只是：

```
[10, 20, 30, 40, 50, 60]
```

而要逐渐变成：

```
内存 →

0x1000   joint_angles[0]   8 bytes
0x1008   joint_angles[1]   8 bytes
0x1010   joint_angles[2]   8 bytes
0x1018   joint_angles[3]   8 bytes
...
```

地址这里只是示意。

但这个思想非常重要：

> **C 数组是一段连续的数据存储空间。**

等学指针的时候：

```
joint_angles
```

和：

```
&joint_angles[0]
```

之间的关系就会突然变得非常有意思。

---

# 顺便认识 Linux 的 `file`

运行：

```
file robot_arm.c
```

它会告诉你这是文本/源代码一类文件。

再：

```
file robot_arm
```

你会看到类似：

```
ELF 64-bit LSB pie executable ...
```

不用看懂整句话。

现在只理解：

```
robot_arm.c
↓
C 源代码

gcc
↓

robot_arm
↓
Linux ELF 可执行文件
```

这正是 Day 1 的：

```
源代码 → 编译 → 可执行程序
```

今天从另一个角度重新看了一次。

---

# 35～40 分钟：自测

**1.**

```
double joints[6];
```

有几个元素？

答案：

```
6
```

合法下标：

```
0～5
```

不是 `1～6`。

**2. 第三个关节是什么？**

答案：

```
joints[2]
```

因为：

```
Joint 1 → [0]
Joint 2 → [1]
Joint 3 → [2]
```

**3. 哪个循环正确？**

```
for (int i = 0; i < 6; i++)
```

还是：

```
for (int i = 0; i <= 6; i++)
```

答案是第一个。

第二个最终会访问：

```
joints[6]
```

导致数组越界。

**4. Python：**

```
joints = [10, 20, 30]print(joints[1])
```

输出：

```
20
```

C：

```
double joints[3] = {10, 20, 30};

printf("%.0f\n", joints[1]);
```

也是：

```
20
```

**5. `sizeof(joints)` 是什么？**

不是：

> 数组有几个元素。

而是：

> 整个数组占多少 **byte（字节）**。

对于常见的：

```
double joints[6];
```

通常是：

```
6 × 8 = 48 bytes
```

**6. 为什么机械臂特别适合数组？**

因为机械臂天然有一组同类型数据：

```
Joint 1 angle
Joint 2 angle
Joint 3 angle
...
```

所以可以统一表示为：

```
double joint_angles[6];
```

再通过：

```
joint_angles[i]
```

批量处理。

---

今天真正需要建立的脑图是：

```
                  Array
                    │
       double joint_angles[6]
                    │
       ┌────────────┼────────────┐
       ↓            ↓            ↓
      [0]          [1]         ...[5]
       │
       ↓
     for 循环
       │
       ↓
joint_angles[i]
       │
       ↓
     if 检查
       │
       ↓
   6轴机械臂状态
```

我们现在已经从：

```
v0.1  printf 显示状态
 ↓
v0.2  scanf 输入状态
 ↓
v0.3  if/else 安全判断
 ↓
v0.4  while 控制循环
 ↓
v0.5  array + for
      六轴机械臂
```

走到了一个重要节点。

接下来暂时**不急着上指针**。更合理的是先学**函数 `function`**，因为现在 `main()` 已经越来越长。我们会把：

```
输入关节
检查关节
显示状态
判断是否移动
```

拆成类似：

```
read_joints();
check_joint();
print_status();
```

这正好对应你熟悉的 Python 函数思想，也会第一次让我们的机器人代码从“一个大文件里顺着写”进入**模块化程序设计**。再往后函数 + 数组掌握后，进入指针时，你就能真正理解为什么函数修改数组如此特殊，以及 C 如何高效传递机器人传感器数据。