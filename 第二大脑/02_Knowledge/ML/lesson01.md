可以，今天就提前开始 **机器学习 Task 001**。以后每天 19:30 的机器学习课都按你刚刚指定的模板来：**回顾 → 核心概念 → 动手项目 → 数据流 → 举一反三 → 常见错误 → 自测答案 → 下一课自然引出问题**。

# Day 1｜第一次真正训练一个机器学习模型

今天我们只做一件事：

> **让计算机从数据里自己学出“电机电流 → 电机温度”的关系。**

不碰神经网络，不碰 VAE，不碰复杂数学。

今天结束时，你会真正搞懂机器学习最核心的 5 个词：

```
feature
label
model
fit
predict
```

并且亲手训练出你的第一个 `scikit-learn` 模型。

---

## 0～5 分钟｜先建立机器学习直觉

传统编程是：

```
规则 + 数据
    ↓
程序
    ↓
答案
```

比如你自己写：

```
if current > 8:
    print("电机可能过热")
```

这里的规则：

```
current > 8
```

是你写的。

---

机器学习反过来：

```
数据 + 答案
    ↓
机器学习算法
    ↓
自动学出规则
```

例如我们收集：

```
电流(A)     温度(℃)

1           31
2           34
3           37
4           41
5           45
6           50
7           56
8           63
```

你没有告诉机器：

```
温度 = 4.5 × 电流 + 24
```

而是直接把数据给它。

机器自己找规律。

---

# 5～20 分钟｜核心概念：机器到底在“学”什么？

想象你是机器人实验室的工程师。

你发现机械臂电机：

```
电流越大
   ↓
发热越严重
```

于是你想让机器人提前预测：

> 当前电流是 5.5A，电机大概多少度？

---

## 1. Feature：特征

我们输入给模型的信息叫：

> **Feature**

通常写成：

\[ X \]

今天只有一个特征：

```
motor current
电机电流
```

所以：

```
X = 电流
```

---

## 2. Label / Target：答案

希望模型预测的东西：

> Label / Target

通常写：

\[ y \]

今天：

```
y = motor temperature
```

所以：

```
X → y
```

就是：

```
电流
 ↓
机器学习模型
 ↓
温度
```

---

# 一个非常重要的区别

不要把：

```
X
```

理解成数学课里的 x。

机器学习里的 `X` 通常是一整张表。

例如：

```
          电流
样本1      1
样本2      2
样本3      3
样本4      4
```

以后可能变成：

```
      电流  转速  环境温度  负载
样本1  2    800    25      10
样本2  4   1200    26      30
样本3  6   1500    30      50
```

所以机器学习喜欢把输入写成：

```
samples × features
```

---

# 3. Model 是什么？

今天我们使用：

```
LinearRegression
```

线性回归。

你可以暂时把它理解成：

> 找一条最合适的直线穿过数据。

像这样：

```
温度
 ↑
65|                    ●
60|                  /
55|               ●
50|             ●
45|          ●
40|        ●
35|     ●
30|  ●
  +----------------------→ 电流
```

数学形式：

\[ y=wx+b \]

其中：

```
x = 电流
y = 温度

w = 斜率
b = 截距
```

但注意：

> `w` 和 `b` 不是我们手写的。

机器自己学。

---

## 4. fit 是什么？

这是今天最重要的函数：

```
model.fit(X, y)
```

生活类比：

```
学生看到很多例题
        ↓
总结规律
        ↓
学会方法
```

机器学习：

```
X + y
 ↓
fit()
 ↓
得到模型参数
```

---

## 5. predict 是什么？

训练完成后：

```
model.predict(...)
```

就相当于：

> 给模型一道它以前没见过的新题。

例如：

```
5.5A
 ↓
model
 ↓
预测温度
```

---

# 到这里先记住整个框架

```
历史数据
    ↓
X = 输入特征
y = 正确答案
    ↓
model.fit(X, y)
    ↓
模型学到规律
    ↓
新数据
    ↓
model.predict()
    ↓
预测结果
```

这套结构以后：

```
KNN
Random Forest
SVM
神经网络
CNN
Transformer
VAE
VLA
```

本质上都逃不开：

```
输入
 ↓
模型
 ↓
输出
```

只是复杂程度不同。

---

# 20～35 分钟｜项目实战

今天项目：

> **Robot Motor Temperature Predictor**

建议建立：

```
ml-learning/
└── day01_linear_regression/
    └── motor_temperature.py
```

如果还没安装：

```
pip install numpy matplotlib scikit-learn
```

---

## 完整代码

直接创建：

```
import numpy as np
import matplotlib.pyplot as plt

from sklearn.linear_model import LinearRegression


# =========================
# 1. 准备机器人电机实验数据
# =========================

motor_current = np.array([
    1,
    2,
    3,
    4,
    5,
    6,
    7,
    8
], dtype=float)

motor_temperature = np.array([
    31,
    34,
    37,
    41,
    45,
    50,
    56,
    63
], dtype=float)


# =========================
# 2. 构造机器学习输入 X 和答案 y
# =========================

X = motor_current.reshape(-1, 1)

y = motor_temperature


# =========================
# 3. 创建模型
# =========================

model = LinearRegression()


# =========================
# 4. 训练模型
# =========================

model.fit(X, y)


# =========================
# 5. 查看模型学到了什么
# =========================

print("slope w =", model.coef_[0])
print("intercept b =", model.intercept_)


# =========================
# 6. 预测一个新的电流
# =========================

new_current = np.array([[5.5]])

predicted_temperature = model.predict(new_current)

print(
    f"When current = 5.5A, "
    f"predicted temperature = "
    f"{predicted_temperature[0]:.2f} °C"
)


# =========================
# 7. 可视化
# =========================

plt.scatter(
    motor_current,
    motor_temperature,
    label="Measured data"
)

x_line = np.linspace(1, 8, 100).reshape(-1, 1)

y_line = model.predict(x_line)

plt.plot(
    x_line,
    y_line,
    label="Learned model"
)

plt.xlabel("Motor Current (A)")
plt.ylabel("Motor Temperature (°C)")
plt.title("Robot Motor Temperature Prediction")

plt.legend()
plt.grid()

plt.show()
```

---

# 运行

```
python motor_temperature.py
```

大概会看到：

```
slope w = 4.488...
intercept b = 24.428...

When current = 5.5A,
predicted temperature = 49.11 °C
```

同时出现一张：

```
散点
+
拟合直线
```

---

# 这时候停一下

你刚刚并没有写：

```
temperature = 4.488 * current + 24.428
```

而是机器通过：

```
model.fit(X, y)
```

自己得到了：

```
w ≈ 4.49
b ≈ 24.43
```

于是模型实际上学出了：

\[ temperature \approx 4.49\times current+24.43 \]

这就是：

> **训练。**

---

# 今天你一定要亲手做三个实验

不要只运行一次。

## 实验 1：预测 3.5A

改：

```
new_current = np.array([[3.5]])
```

先自己猜：

大概应该：

```
40℃左右
```

然后运行。

---

## 实验 2：预测 7A

```
new_current = np.array([[7]])
```

你会发现：

模型预测值会接近已有实验数据：

```
56℃
```

---

## 实验 3：预测 20A

```
new_current = np.array([[20]])
```

这里会发生一件非常重要的事情。

模型仍然会给答案。

但这个答案：

> **不一定可靠。**

因为你的训练数据只有：

```
1A ~ 8A
```

突然问：

```
20A
```

模型只能把直线继续往外延长。

这叫：

> **Extrapolation**
> 
> 外推。

机器学习第一个重要经验：

> **模型会给答案，不代表答案可靠。**

---

# 35～45 分钟｜完整理解数据是怎么流的

现在看整个程序。

```
真实机器人实验
      ↓
收集数据
      ↓
motor_current
motor_temperature

      ↓

X
输入特征

y
目标答案

      ↓

LinearRegression()

      ↓

fit(X, y)

      ↓

模型学到：

w
b

      ↓

new_current

      ↓

predict()

      ↓

predicted_temperature
```

换成机器人系统：

```
Sensor Data
     ↓
Machine Learning Model
     ↓
Prediction
     ↓
Robot Decision
     ↓
Action
```

以后可能变成：

```
Camera
IMU
LiDAR
Motor State
     ↓
Neural Network
     ↓
Robot Action
```

你今天其实已经摸到了 Robot Learning 最底层的结构。

---

# 为什么 X 要 reshape？

你看到：

```
X = motor_current.reshape(-1, 1)
```

原本：

```
motor_current.shape
```

是：

```
(8,)
```

类似：

```
[1 2 3 4 5 6 7 8]
```

但 sklearn 希望数据长成表格：

```
样本 × 特征
```

所以：

```
reshape(-1, 1)
```

变成：

```
[
 [1],
 [2],
 [3],
 [4],
 [5],
 [6],
 [7],
 [8]
]
```

shape：

```
(8, 1)
```

意思：

```
8 个样本

每个样本
有 1 个 feature
```

---

# 为什么不能直接：

```
model.fit(motor_current, y)
```

因为 sklearn 不确定：

```
你这是：

8个样本 × 1个特征？

还是：

1个样本 × 8个特征？
```

所以需要明确告诉它。

---

# 举一反三 ①：机械臂

今天：

```
电流
 ↓
模型
 ↓
温度
```

以后：

```
关节角
 ↓
模型
 ↓
末端位置
```

甚至：

```
θ1
θ2
θ3
 ↓
Neural Network
 ↓
x
y
z
```

这就能学习机械臂运动学。

---

# 举一反三 ②：机器人故障检测

输入：

```
电流
温度
振动
转速
```

模型：

```
Random Forest
```

输出：

```
正常

or

故障
```

这时候不再是 regression。

而是：

> Classification

分类。

---

# 举一反三 ③：移动机器人

可能输入：

```
LiDAR readings
speed
distance
```

输出：

```
turn left
go straight
turn right
```

还是：

```
X
 ↓
model
 ↓
y
```

---

# 举一反三 ④：具身智能

再往后：

```
Camera image
+
Robot joint state
+
Language instruction

        ↓

      VLA

        ↓

Robot actions
```

看起来复杂很多。

但抽象后还是：

```
X
↓
model
↓
y
```

所以今天这一课绝对不是“小儿科”。

这是以后所有 AI 模型共同的骨架。

---

# 常见错误

## 错误 1

```
model.predict([5.5])
```

可能报：

```
Expected 2D array
```

应该：

```
model.predict([[5.5]])
```

因为：

```
1个样本
×
1个特征
```

---

## 错误 2

把：

```
fit
```

和：

```
predict
```

搞反。

永远记：

```
fit
=
学习

predict
=
考试
```

---

## 错误 3

认为模型找到的是绝对真理。

不是。

模型学到的是：

> 数据中的规律。

如果数据不好：

```
垃圾数据
 ↓
机器学习
 ↓
垃圾模型
```

经典：

> Garbage In, Garbage Out.

---

# 今日自测

### 1. X 是什么？

A. 模型输出  
B. 输入特征  
C. 模型答案

答案：

**B**

---

### 2. y 是什么？

今天：

```
A 电机电流
B 电机温度
```

答案：

**B**

---

### 3.

```
model.fit(X, y)
```

是什么意思？

答案：

> 用训练数据让模型学习 X 和 y 之间的关系。

---

### 4.

```
model.predict([[5.5]])
```

是什么意思？

答案：

> 给训练好的模型一个新输入 5.5A，让模型预测温度。

---

### 5. 为什么是：

```
[[5.5]]
```

而不是：

```
[5.5]
```

答案：

因为 sklearn 的输入格式是：

```
samples × features
```

`[[5.5]]` 表示：

```
1 个样本
1 个特征
```

---

# 今天一定要搞懂的一句话

机器学习不是：

> AI 神秘地算答案。

而是：

```
收集数据
  ↓
定义 X 和 y
  ↓
选择模型
  ↓
fit
  ↓
学习参数
  ↓
predict
```

今天我们做的是：

```
Motor Current

↓

Linear Regression

↓

Motor Temperature
```

---

# 今日完成标准

你真正完成 Task 001，需要做到：

```
① 能运行完整代码

② 能看到拟合直线

③ 能自己修改 new_current

④ 知道 X 和 y 分别是什么

⑤ 能解释 fit 和 predict 区别

⑥ 能解释为什么 X 需要 reshape(-1, 1)

⑦ 知道预测 20A 为什么值得怀疑
```

前 4 个做到还只是“代码运行”。

7 个都能解释才叫：

> **Day 1 学会了。**

---

# 下一课预告

现在会产生一个非常自然的问题：

> 今天模型拿所有数据学习，然后又看着这些数据判断自己效果好不好，那它是不是有可能只是在“背答案”？

这会引出机器学习极其核心的一课：

# Day 2｜训练集和测试集：怎么知道机器人是真的学会了，而不是背答案？

我们会第一次使用：

```
train_test_split()
```

然后理解：

```
Training Set
    ↓
学习

Test Set
    ↓
真正考试
```

再引出：

```
MAE
R²
overfitting
```

之后才进入 KNN、分类、决策树等算法。

你现在先把上面的 `motor_temperature.py` **亲手运行一次，并依次测试 `3.5A / 7A / 20A`**。这就是今天最重要的动手部分。