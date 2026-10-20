我给你制定一套完整的 《机器学习 → 深度学习 → 生成式人工智能 → 具身智能》110个 Task 学习计划。

这套计划不是单纯按照 sklearn 或 PyTorch 教程排列，而是以你的机器人工程（感知与控制）专业为主线，把数学、算法、编程和真实项目串起来，同时融入你上传的 AE/VAE 课程材料。

预计 4–6个月完成第一轮，每天一个 Task，难点可以重复学习，不为了赶进度跳过知识。

# 一、整体课程路线

Phase 1 · 传统机器学习

Task 001–025 · NumPy / sklearn / 统计学习

Phase 2 · 深度学习与计算机视觉

Task 026–050 · PyTorch / CNN / YOLO

Phase 3 · Transformer 与多模态

Task 051–065 · Attention / ViT / VLM

Phase 4 · 生成式人工智能

Task 066–088 · AE / VAE / Diffusion / Flow Matching

Phase 5 · 机器人学习与具身智能

Task 089–110 · RL / Imitation / Diffusion Policy / VLA

## 二、Phase 1：传统机器学习（Task 001–025）

核心目标：理解机器学习为什么有效，能够独立完成一个 sklearn 项目。

|Task|具体教学内容|状态|
|---|---|---|
|001|机器学习是什么？样本、特征、标签、NumPy|已完成|
|002|线性回归：\(y=wx+b\)、fit、predict、MSE|已完成|
|003|训练集、测试集、泛化、欠拟合、过拟合|已讲解|
|004|验证集、K-Fold交叉验证、数据泄漏|下一课|
|005|NumPy矩阵运算：Shape、Axis、Broadcasting||
|006|Pandas：DataFrame、读取CSV和数据统计||
|007|数据预处理：缺失值、异常值、数据清洗||
|008|回归评价指标：MAE、MSE、RMSE、R²||
|009|线性回归的数学原理：最小二乘法||
|010|梯度下降：如何一步步寻找最优参数||
|011|多元线性回归：多个传感器特征预测||
|012|特征缩放、标准化、Pipeline||
|013|逻辑回归：从回归走向分类||
|014|Sigmoid函数、概率输出、分类阈值||
|015|混淆矩阵、准确率||
|016|Precision、Recall、F1-score||
|017|ROC、PR曲线与分类阈值选择||
|018|KNN：最近邻分类与距离||
|019|决策树：信息增益、最大深度||
|020|随机森林：集成学习||
|021|SVM：最大间隔分类||
|022|K-Means：无监督聚类||
|023|PCA：降维、特征空间、方差||
|024|异常检测：识别传感器故障||
|025|超参数搜索、模型比较与阶段综合项目||

阶段项目：机器人电机状态分析系统

输入：PWM、电压、编码器转速等传感器数据。

输出：预测电机转速、判断异常工况、比较模型效果。

最终掌握：`NumPy + Pandas + sklearn + Matplotlib`。

## 三、Phase 2：深度学习（Task 026–050）

核心目标：真正理解神经网络、反向传播和 PyTorch，进入机器人视觉。

|Task|具体教学内容|
|---|---|
|026–030|PyTorch Tensor、GPU、偏导数、链式法则、Autograd、梯度下降|
|031–035|`nn.Module`、线性层、激活函数、损失函数、SGD/Adam、Batch/Epoch|
|036–040|MLP回归与分类、权重初始化、Dropout、训练曲线、模型保存|
|041–044|图像张量、卷积核、Stride、Padding、Pooling、CNN架构|
|045–047|MNIST图像分类、数据增强、迁移学习|
|048–050|目标检测、Bounding Box、IoU、YOLO推理与小规模微调|

阶段项目：AI视觉识别系统

使用 PyTorch/CNN 训练分类器，再使用 YOLO 进行目标检测，尝试用摄像头识别小车前方的物体。

## 四、Phase 3：Transformer 与多模态（Task 051–065）

核心目标：理解现代大模型为什么能够处理语言、视觉和跨模态信息。

|Task|具体教学内容|
|---|---|
|051|Token、Embedding|
|052|向量表示和语义空间|
|053|Query、Key、Value 的直觉|
|054|Scaled Dot-Product Attention|
|055|Multi-Head Attention|
|056|Positional Encoding|
|057|Transformer Encoder|
|058|Transformer Decoder|
|059|使用 PyTorch 搭建小型 Attention 模型|
|060|Vision Transformer：Patch Embedding|
|061|ViT图像识别实验|
|062|CLIP：图文对齐|
|063|VLM：视觉语言模型|
|064|VLA基础：从视觉、语言到动作|
|065|综合项目：摄像头图像 + 文本理解|

这一阶段会把你之前学的线性代数真正用起来。

例如 Attention 中：

\[ \operatorname{Attention}(Q,K,V) =\operatorname{softmax}\left(\frac{QK^T}{\sqrt{d_k}}\right)V \]

为什么是矩阵相乘？为什么需要转置？为什么除以 \(\sqrt{d_k}\)？

都会从几何直觉到矩阵运算一步步解释。

## 五、Phase 4：生成式人工智能（Task 066–088）

这部分重点融入你上传的《1_AE-VAE.pptx》，并扩展到现代生成模型。

|Task|具体教学内容|
|---|---|
|066|AutoEncoder：为什么要压缩信息？|
|067|PyTorch实现自编码器|
|068|Denoising AE：去噪自编码器|
|069|Representation Learning：潜在空间与特征表示|
|070|生成模型需要的概率论：高斯分布|
|071|KL散度：为什么要约束概率分布？|
|072|VAE：从确定性编码到概率编码|
|073|Reparameterization Trick：重参数化|
|074|ELBO：最大似然与变分推断|
|075|VAE重建损失：BCE与MSE|
|076|PyTorch训练MNIST VAE|
|077|Latent Space：采样与插值|
|078|结合上传的VAE实验材料复现与分析|
|079|GAN：生成器与判别器|
|080|Diffusion：前向加噪过程|
|081|Diffusion：反向去噪过程|
|082|从零编写简化版DDPM|
|083|条件生成与Classifier-Free Guidance|
|084|Latent Diffusion与图像生成|
|085|Normalizing Flow与概率密度变换|
|086|Continuous Normalizing Flow、ODE基础|
|087|Flow Matching：向量场与生成轨迹|
|088|Flow Matching实验及生成模型综合比较|

阶段项目：从 VAE 到 Diffusion 的生成实验

先训练一个 VAE 生成手写数字，然后理解 Diffusion 如何逐步去噪，最后制作二维 Flow Matching 轨迹生成实验。

这一阶段很重要，因为很多机器人生成式策略模型会利用类似的概率建模和生成思想。但我们不会把 Flow Matching 或 Diffusion 当作所有机器人任务都必须使用的算法。

## 六、Phase 5：机器人学习与具身智能（Task 089–110）

目标：把之前学习的 ML、深度学习、生成模型逐步连接到机器人动作与决策。

|Task|具体教学内容|
|---|---|
|089|Robot Learning：机器人究竟学习什么？|
|090|机器人数据集：Observation、Action、Reward|
|091|模仿学习：Behavior Cloning|
|092|PyTorch训练动作预测网络|
|093|机器人轨迹和序列数据表示|
|094|Action Chunking：分段动作预测|
|095|Diffusion Policy原理|
|096|Diffusion Policy简化实验|
|097|LeRobot框架与数据格式|
|098|LeRobot离线训练和评估|
|099|强化学习：MDP与Reward|
|100|Bellman Equation|
|101|Q-Learning|
|102|Deep Q-Network|
|103|Policy Gradient|
|104|PPO基本原理|
|105|仿真环境、Sim-to-Real|
|106|VLA模型架构与动作Token|
|107|VLA推理流程与部署约束|
|108|ROS2视觉感知与模型通信|
|109|机器人动作策略综合实验|
|110|最终综合项目与模型评估|

最终项目：机器人感知—决策—动作闭环

在仿真环境中完成一个小型机器人学习任务，例如根据视觉或状态输入预测运动动作。再结合你的 ROS2 小车，尝试部署可行的感知与控制模块。

强化学习、模仿学习和 VLA 的真实部署，会以硬件能力、安全边界和训练资源为准，不要求110课内完成大型机器人基础模型训练。

## 七、每个Task怎么教？

以每天约40分钟为标准。

|时间|模块|要求|
|---|---|---|
|0–5分钟|复习上一课|提问、知识串联|
|5–20分钟|理论与数学|为什么、类比、ASCII图、公式直觉|
|20–35分钟|动手项目|完整代码、运行方法、解释与结果|
|35–40分钟|总结与自测|直接提供标准答案、常见错误与迁移练习|

另外有三项执行规则：

1. 数学和算法同步学。 学梯度下降时讲偏导数，学 PCA 时讲特征向量，学 VAE 时讲高斯分布和 KL 散度，不把数学知识一次性堆在前面。
    
2. 每7个Task做一次复盘。 检查理解程度、代码运行情况和能否迁移到机器人应用；必要时安排补强课，而不是机械进入下一编号。
    
3. 周末提供约3小时的选做项目。 平时40分钟完成必要知识，周末把若干知识点组合成小项目。
    

## 八、当前进度与下一课

当前：Phase 1 / Task 003

3 / 110

Task 001–002已完成，Task 003已讲解；刚刚补充了超参数和 random_state。

下一课：Task 004 — K-Fold交叉验证

我们会用同一组 PWM–RPM 数据，让模型接受多轮训练与验证，比较不同划分对预测误差的影响，并认识常见的数据泄漏。

开始 Task 004

这110个Task是一份第一轮学习路线图，不是学完110天就达到专家水平。 真正的阶段目标是：从能够解释并训练基本模型，逐步成长为能够独立搭建、训练、评估和集成机器人 AI 模块的开发者。