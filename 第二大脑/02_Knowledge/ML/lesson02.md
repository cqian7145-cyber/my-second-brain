可以，今天的 **19:30 机器学习任务直接提前到现在开始，定时任务本身不变。**

# Day 2｜训练集与测试集：怎么判断模型是真的学会了，而不是“背答案”？

昨天我们已经做到：

```
电机电流 X
    ↓
LinearRegression
    ↓
电机温度 y
```

并认识了：

```
Feature
Label
Model
fit()
predict()
```

今天结束时，我们会把昨天的模型升级成真正的机器学习实验：

```
全部数据
   ↓
拆分
   ↓
训练集          测试集
  ↓               ↓
学习规律          不许偷看
  ↓               ↓
模型 ─────────→ 考试
                  ↓
              计算误差
```

今天核心只回答一个问题：

> **模型在训练数据上表现很好，能不能说明它真的学会了？**

答案：

> **不能。**

---

# 0～5 分钟｜复习昨天

昨天：

```
model.fit(X, y)
```

意思：

```
把问题和答案
一起交给模型

X → 输入
y → 正确答案

模型通过训练
找到 X 和 y 的关系
```

然后：

```
model.predict(new_X)
```

意思：

```
新输入
 ↓
训练好的模型
 ↓
预测答案
```

例如：

```
5.5A
 ↓
model
 ↓
49℃
```

---

## 昨天其实埋了一个问题

我们用了：

```
1A → 31℃
2A → 34℃
3A → 37℃
...
8A → 63℃
```

训练模型。

然后如果又拿：

```
3A
7A
```

去测试它。

其实有点像：

```
老师：

“这是考试答案。”

学生：
看完了。

老师：

“现在考试。”

题目：
刚才那套原题。
```

考了 100 分能证明学生真正会了吗？

当然不能。

机器学习完全一样。

---

# 5～20 分钟｜核心概念

## 一、为什么需要训练集和测试集？

假设你训练一台机器人识别：

```
地面
障碍物
人
桌子
```

你给它看了 1000 张图片。

如果最后你还用这 1000 张图片考试：

```
训练图片
   ↓
训练模型
   ↓
还是这些图片
   ↓
准确率 99%
```

这个 99% 没那么有意义。

因为模型可能只是：

> **记住了训练数据。**

真正的问题应该是：

> 给它一张它以前从来没有见过的新图片，还能不能判断正确？

---

# 生活类比

真正学习：

```
练习册
 ↓
学习

考试卷
 ↓
检验能力
```

所以机器学习里：

```
Training Set
训练集
=
练习册
```

```
Test Set
测试集
=
期末考试
```

---

# 二、数据应该怎么拆？

例如有 100 条机器人电机数据。

通常可以：

```
全部数据：100 条

        ↓

80 条
Training Set

20 条
Test Set
```

也就是：

```
80%
训练

20%
测试
```

---

# 三、最重要的一条规则

## 测试集绝对不能参与训练

正确：

```
Training X + Training y
        ↓
      fit()
        ↓
      Model
        ↓
    Test X
        ↓
    predict()
        ↓
Prediction
        ↓
与 Test y 比较
```

错误：

```
全部数据
 ↓
fit()

然后再从里面拿一部分
说这是测试集
```

因为：

> 模型已经提前看过答案了。

---

# 四、scikit-learn 怎么拆？

今天认识：

```
train_test_split()
```

使用：

```
from sklearn.model_selection import train_test_split
```

然后：

```
X_train, X_test, y_train, y_test = train_test_split(    X,    y,    test_size=0.2,    random_state=42)
```

第一次看到变量这么多可能有点乱。

拆开看：

```
原来的：

X
y

      ↓ split

X_train
训练输入

y_train
训练答案

X_test
考试输入

y_test
考试标准答案
```

---

# 五、一个非常重要的数据结构

记住这张图：

```
                     Dataset
                        │
                ┌───────┴───────┐
                ↓               ↓
            Training           Test
                │               │
          X_train y_train   X_test y_test
                │               │
                ↓               │
               fit              │
                ↓               │
              model             │
                │               │
                └───────→ predict
                                │
                                ↓
                             y_pred
                                │
                       compare with y_test
```

以后：

```
Linear Regression
KNN
SVM
Random Forest
Neural Network
CNN
Transformer
```

基本都会出现类似结构。

---

# 六、test_size=0.2 是什么？

```
test_size=0.2
```

意思：

```
20%
给测试集

80%
给训练集
```

例如：

```
20 条数据
```

大概就是：

```
16 条训练

4 条测试
```

---

# 七、random_state=42 又是什么？

`train_test_split()` 默认会随机打乱数据。

第一次：

```
训练集：
1 3 4 5 8 ...

测试集：
2 6 7 ...
```

第二次可能变：

```
训练集：
1 2 5 6 7 ...

测试集：
3 4 8 ...
```

那每次结果都会略有差别。

所以：

```
random_state=42
```

相当于：

> 固定洗牌方法。

这样你我运行同一段代码时，结果一致。

`42` 没有什么神秘机器学习含义。

换成：

```
random_state=1
```

也可以。

---

# 20～35 分钟｜项目实战

今天继续昨天的项目。

目录可以变成：

```
ml-learning/

day01_linear_regression/
    motor_temperature.py

day02_train_test_split/
    motor_model_test.py
```

今天我们增加更多实验数据。

---

## 完整可运行代码

```
import numpy as npimport matplotlib.pyplot as pltfrom sklearn.linear_model import LinearRegressionfrom sklearn.model_selection import train_test_splitfrom sklearn.metrics import mean_absolute_error, r2_score# ==========================================# 1. 机器人电机实验数据# ==========================================motor_current = np.array([    1.0,    1.4,    1.8,    2.2,    2.6,    3.0,    3.4,    3.8,    4.2,    4.6,    5.0,    5.4,    5.8,    6.2,    6.6,    7.0,    7.4,    7.8,    8.2,    8.6])motor_temperature = np.array([
```

运行：

```
python motor_model_test.py
```

---

# 先观察第一件事

应该看到：

```
Training samples: 16
Test samples: 4
```

因为：

```
20 个样本

20 × 0.8
=
16 个训练样本

20 × 0.2
=
4 个测试样本
```

---

# 第二件事：模型从来没看过测试集答案

训练：

```
model.fit(    X_train,    y_train)
```

注意这里没有：

```
X_testy_test
```

所以模型不知道：

```
Test Sample 1
真正温度是多少

Test Sample 2
真正温度是多少
```

---

# 第三件事：正式考试

```
y_pred = model.predict(X_test)
```

数据流：

```
X_test
未知的新电流
   ↓
model
   ↓
y_pred
预测温度
```

然后我们才拿：

```
y_test
```

出来对答案。

---

# 35～40 分钟｜MAE 到底是什么？

今天只重点掌握一个指标：

# MAE

Mean Absolute Error

中文：

> **平均绝对误差**

假设模型预测：

```
真实       预测

40℃       42℃
50℃       49℃
60℃       63℃
```

误差：

```
|40 - 42| = 2

|50 - 49| = 1

|60 - 63| = 3
```

平均：

\[ MAE= \frac{2+1+3}{3} = 2 \]

所以：

```
MAE = 2℃
```

意思极其直观：

> 模型平均大约会预测错 2℃。

---

# 机器人类比

假设你以后预测机械臂末端位置：

```
MAE = 0.005 m
```

就是：

```
平均误差
≈ 5 mm
```

这就比抽象说：

```
loss = 0.00251
```

更容易理解。

---

# R² 今天只建立直觉

代码里还有：

```
r2_score()
```

得到：

```
R²
```

今天不要研究它的完整公式。

只先记：

```
R² 接近 1
通常表示模型拟合不错
```

例如：

```
R² = 0.98
```

通常很好。

```
R² = 0.2
```

说明模型解释能力比较弱。

但以后你会学到：

> **不能只看 R² 判断模型好坏。**

今天 MAE 比 R² 更重要。

---

# 40～45 分钟｜把程序的数据流彻底吃透

这是今天最重要的一张图。

```
机器人实验数据
────────────────────

电流
温度

      ↓

X, y

      ↓

train_test_split()

      ↓

┌─────────────────┐
│                 │
↓                 ↓

Training          Test

X_train           X_test
y_train           y_test

↓
model.fit()

↓

Model

        ↓

       X_test

        ↓

     predict()

        ↓

      y_pred

        ↓

y_pred vs y_test

        ↓

MAE
R²
```

---

# 为什么这套结构非常重要？

因为以后换模型：

```
LinearRegression
        ↓
KNN
        ↓
SVM
        ↓
RandomForest
        ↓
Neural Network
```

虽然模型不同：

```
model = ...
```

但整体流程基本还是：

```
split
 ↓
fit
 ↓
predict
 ↓
evaluate
```

这就是 `scikit-learn` 最重要的统一设计思想之一。

---

# 举一反三 ①｜机器人故障检测

以后可能有：

```
X：

电流
电压
振动
温度
转速

↓

Model

↓

y：

正常 / 故障
```

一样必须：

```
Training Set
+
Test Set
```

否则：

> 机器人可能只是记住以前出现过的故障。

遇到新的故障数据就失效。

---

# 举一反三 ②｜YOLO 为什么也分 train / val / test？

你以后学 YOLO 会看到：

```
dataset/

train/
val/
test/
```

现在应该开始理解了。

不是 YOLO 特有。

是机器学习普遍逻辑：

```
train
=
学习

validation
=
训练过程中调参数

test
=
最后考试
```

今天我们暂时只学习：

```
train
test
```

`validation` 后面再加入。

---

# 举一反三 ③｜VLA 也一样

未来训练机器人：

```
摄像头
+
关节状态
+
语言指令

↓

VLA

↓

Action
```

也必须问：

> 它在没见过的新场景还能完成任务吗？

比如训练时：

```
红杯子
白桌子
```

测试：

```
蓝杯子
木桌子
```

还能抓吗？

这叫：

> **Generalization**

泛化。

今天其实已经第一次接触到了“泛化能力”。

---

# 今天最重要的新概念

## Generalization

中文：

> 泛化。

可以理解成：

```
没见过
也会做
```

学生真正学会：

```
换一道题
还能做
```

机器人真正学会：

```
换一个场景
还能完成任务
```

机器学习真正学会：

```
新数据
还能预测正确
```

---

# 举一反三 ④｜为什么机器人尤其需要泛化？

因为现实世界不可能和训练数据一模一样。

真实机器人面对：

```
不同光照
不同物体
不同角度
不同房间
不同用户
传感器噪声
```

所以：

```
Training Performance
```

好看没什么了不起。

真正重要的是：

```
Unseen Environment
       ↓
Robot
       ↓
still works
```

这就是具身智能以后反复讨论的问题。

---

# 今天必须亲手做的三个实验

不要只运行原代码。

## 实验 A：test_size 改成 0.5

```
test_size=0.5
```

观察：

```
Training samples
Test samples
```

变成多少。

答案：

```
20 条数据

10 train
10 test
```

然后观察 MAE 有没有变化。

---

# 实验 B：修改 random_state

从：

```
random_state=42
```

改：

```
random_state=1
```

再次运行。

你会发现：

```
测试数据变了

MAE 也可能发生变化
```

为什么？

因为：

> 考试题变了。

---

# 实验 C：制造垃圾数据

把某个温度：

```
47.5
```

故意改成：

```
90
```

比如：

```
motor_temperature = [
    ...
    90,
    ...
]
```

再次训练。

观察：

```
回归直线
MAE
R²
```

发生什么变化。

这代表现实中的：

> Sensor Error / Outlier

传感器异常值。

你以后学习：

```
异常检测
数据清洗
Robust Regression
```

就是为了解决这些问题。

---

# 常见错误

## 错误 1：先 fit 再 split

错误：

```
model.fit(X, y)

train_test_split(...)
```

这已经泄露测试信息。

正确顺序：

```
先 split
↓
只用 train fit
↓
最后 test
```

---

## 错误 2：用测试集训练

错误：

```
model.fit(
    X_test,
    y_test
)
```

测试集不是训练材料。

类比：

> 不能提前把期末考试卷给学生练。

---

# 错误 3：测试数据越多越好？

也不一定。

例如只有：

```
100 条数据
```

如果：

```
90 条测试
10 条训练
```

考试很多。

但模型根本没多少东西可以学。

所以需要平衡。

常见：

```
80 / 20

70 / 30
```

---

# 错误 4：MAE=0 就一定完美？

在训练集：

```
MAE = 0
```

不一定好。

可能只是：

> 背得特别熟。

如果测试集：

```
MAE 很大
```

说明：

```
Train 很好

Test 很差
```

这就是马上会遇到的重要问题：

> **Overfitting**

过拟合。

今天先知道名字即可。

---

# 今日自测

## Q1

为什么机器学习不能只看训练集表现？

### 答案

因为模型可能只是记住训练数据。

真正需要知道的是：

> 面对没见过的数据还能不能正确预测。

---

# Q2

`fit()` 应该使用哪组数据？

A：

```
X_train
y_train
```

还是 B：

```
X_test
y_test
```

### 答案

A。

```
model.fit(
    X_train,
    y_train
)
```

---

# Q3

测试阶段输入模型的是什么？

### 答案

```
X_test
```

模型产生：

```
y_pred
```

然后：

```
y_pred
vs
y_test
```

---

# Q4

MAE = 2℃ 是什么意思？

### 答案

模型的预测温度和真实温度：

> 平均大约相差 2℃。

---

# Q5

为什么 `random_state=42`？

### 答案

固定随机拆分结果。

这样：

```
每次运行
train/test
基本保持一样
```

方便：

```
调试
比较
复现实验
```

42 本身没有特殊机器学习意义。

---

# Q6

什么叫泛化？

### 答案

模型对于：

> **没有见过的新数据**

依然能够正确工作。

机器人语言：

```
训练环境
没见过这个场景

↓

机器人仍然能完成任务
```

就是泛化。

---

# 今日总结

昨天的机器学习是：

```
X
↓
fit
↓
model
↓
predict
↓
y
```

今天升级成：

```
Dataset
    ↓
Split
    ↓

Train              Test
  ↓                  ↓
fit               unseen data
  ↓                  ↓
Model ─────────→ predict
                     ↓
                  y_pred
                     ↓
               compare y_test
                     ↓
                    MAE
```

今天必须带走六个词：

```
Training Set
Test Set
train_test_split
MAE
Generalization
Overfitting
```

其中最核心的思想只有一句：

> **机器学习真正关心的不是模型能不能做好见过的题，而是能不能做好没见过的题。**

---

# 下一课自然产生的问题

现在你已经会：

```
电流
 ↓
预测
 ↓
温度
```

这是：

> **Regression 回归**

因为答案是连续数字：

```
43.2℃
51.7℃
62.1℃
```

但是机器人还有大量问题不是预测数字。

例如：

```
传感器数据
 ↓
???

正常
or
故障
```

或者：

```
距离 + 速度
 ↓
???

安全
or
危险
```

这时候：

```
Linear Regression
```

就不合适了。

于是下一课进入机器学习第二大类：

# Day 3｜Classification + KNN

我们会做一个真正的机器人小项目：

> **根据电机温度、振动和电流，让模型判断电机是“正常”还是“故障”。**

并第一次理解：

```
Regression
预测一个数

vs

Classification
预测一个类别
```

然后亲手使用：

```
KNeighborsClassifier
```

这会是你第一个真正的**分类模型**。