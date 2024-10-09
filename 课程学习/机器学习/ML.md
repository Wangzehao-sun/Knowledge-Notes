## 机器学习笔记

资源：https://github.com/kaieye/2022-Machine-Learning-Specialization 吴恩达机器学习

##### **机器学习**（Machine Learning，ML）

是一种让计算机通过从数据中自动学习模式和规则，进而做出决策或预测的技术。

> **机器学习研究的是计算机怎样模拟人类的学习行为，以获取新的知识或技能，并重新组织已有的知识结构，使之不断改善自身**。从实践的意义上来说，机器学习是在大数据的支撑下，通过各种算法让机器对数据进行深层次的统计分析以进行「自学」，使得人工智能系统获得了归纳推理和决策能力

##### **机器学习**的三大类型

1. **监督学习**（Supervised Learning）：

   - 在监督学习中，模型在带有“标签”的数据上进行训练。**right answer given**
   - 常见的任务包括分类（预测值离散）和回归（预测值连续）。

   **例子**：房价（回归）、肿瘤预测（分类）、多特征分类

   <img src=".\attachments\image-20240920111200152.png" alt="image-20240920111200152" style="zoom:50%;" /><img src=".\attachments\image-20240920111235605.png" alt="image-20240920111235605" style="zoom:50%;" /><img src=".\attachments\image-20240920111307925.png" alt="image-20240920111307925" style="zoom:50%;" />

2. **无监督学习**（Unsupervised Learning）：

   - 无监督学习中的数据没有标签，模型需要在没有明确指导的情况下发现数据中的模式或结构。
   - 常见任务包括聚类（将数据分组）和降维（降低数据的复杂性）。

   **例子**：将一组用户按行为进行聚类，找到不同用户群体的共同特征。

   <img src=".\attachments\image-20240920112127351.png" alt="image-20240920112127351" style="zoom: 80%;" />

3. **强化学习**（Reinforcement Learning）：

   - 强化学习是通过奖励和惩罚机制进行学习的。模型（或智能体）在环境中采取行动，尝试通过与环境的交互最大化累积奖励。
   - 适用于那些具有顺序决策过程的问题，如机器人控制、游戏策略等。

   **例子**：让一个机器人学习如何在一个房间内找到最短的路径到达目的地。机器人根据它采取的行动（移动）是否正确（接近目标或远离目标）获得奖励或惩罚。

#### Chapter 1、机器学习要素

##### 1.1 基本概念

每个**样本（数据点）**包含有多个**特征**和**标签**。一组样本构成的集合称为**数据集**（Data Set），一般将数据集分为两部分：**训练集**和**测试集**。

通常用一个𝐷 维向量$𝒙 = [𝑥_1, 𝑥_2, ⋯ , 𝑥_𝐷]^T$ 表示所有特征构成的向量，称为**特征向量**（Feature Vector）

对一个预测任务，输入特征向量为 𝒙，输出标签为𝑦，我们选择一个函数集合ℱ，通过**学习算法 𝒜 **和一组训练样本𝒟，从ℱ中学习到函数𝑓 ∗ (𝒙)．这样对新的输入𝒙，就可以用函数𝑓 ∗ (𝒙)进行预测．

![image-20240920111431324](.\attachments\image-20240920111431324.png)

##### 1.2 机器学习三要素

* ==模型==：最初的模型也成为**假设函数 h**， 机器学习的目标是从**假设空间**里找到一个**模型**来近似真实映射函数𝑔(𝒙)或真实条件概率分布𝑝𝑟(𝑦|𝒙)，包括线性空间和非线性空间

![image-20240920113009945](.\attachments\image-20240920113009945.png)

* ==学习准则==（**损失函数**）：一个好的模型 𝑓(𝒙, 𝜃∗ ) 应该在所有 (𝒙, 𝑦) 的可能取值上都与真实映射函数𝑦 = 𝑔(𝒙)尽可能的吻合，为此需要最小化损失函数 $ℒ(𝑦, 𝑓(𝒙; 𝜃))$，即模型的好坏可以用期望风险来衡量：

  ![image-20240920114553812](.\attachments\image-20240920114553812.png)

  由于不知道真实的数据分布和映射函数 ，实际上无法计算其期望风险 ℛ(𝜃)．给定一个训练集 𝒟，我们可以计算的是经验风险（Empirical Risk），即在训练集上的平均损失：

  ![image-20240920114450433](.\attachments\image-20240920114450433.png)

  常见的损失函数有：

  1. **0-1损失函数**

  2. **平方损失函数**：回归任务

  3. **交叉熵损失函数**：分类任务或衡量概率分布的差异

     

* **==优化算法==**：如何找到最优的模型𝑓(𝒙, 𝜃∗) 就成了一个最优化（Optimization）问题，机器学习的训练过程其实就

  是最优化问题的求解过程。

  

  <img src=".\attachments\image-20240920151034006.png" alt="image-20240920151034006" style="zoom:67%;" />

  > 如图所示，假设函数为$h_{\theta}(x)$ ,损失函数为$J(\theta_1)$ ,训练的目标就是希望通过减少$J(\theta_1)$,找到一个尽可能拟合训练数据点的假设函数h

  

  1. **超参数优化**：是用来定义模型结构或优化策略的参数，通常是一个组合优化问题，**主要靠经验决定**

     

  2. **参数优化**：即模型𝑓(𝒙, 𝜃)中的参数  𝜃。最简单、常用的优化算法就是**梯度下降法**，即首先初始化参

     数$𝜃_0$，然后按下面的迭代公式来计算训练集𝒟 上风险函数的最小值：

     ![](.\attachments\image-20240920150744735.png)<img src=".\attachments\image-20240920152113914.png" alt="image-20240920152113914" style="zoom:50%;" />

     其中$\alpha$ 表示每次更新的步长，及学习率。==学习率太小，收敛太慢；学习率太大，振荡难以收敛。==

     > 值得注意的是，跟下山一样，我们并不能保证一定能走到全局最优解，很有可能陷入局部最优点。
     >
     > 此外，在进行参数更新时，为了保证方向符合梯度方向，每次全部的参数都要同时更新。
     
     > [!IMPORTANT]
     >
     > 随机梯度下降算法是机器学习算法的基础，所谓‘随机’即是指随机的从训练集中选取小批量样本来，并使用该小批量样本的梯度来近似整个训练集的梯度，从而根据**损失函数**对模型参数进行更新。
     > $$
     > ∇_xf(x) = [
     > \frac{∂f(x)}{∂x_1},\frac{∂f(x)}{∂x_2}, . . . ,\frac{∂f(x)}{∂x_n}]^⊤ \\
     > (w, b) ← (w, b) −\frac{η}{|B|}
     > ∑
     > _{i∈B}
     > ∂_{(w,b)}
     > l
     > ^{(i)}
     > (w, b)
     > $$
     > 
     
     

#### Chapter 2、多参数线性回归

##### 2.1 线性回归的定义

给定n维的特征向量 $x= (x_1,x_2,...,x_n)^T \in \mathbb{R}^n$ ，线性回归的假设是：需要预测的目标值 Y 与输入特征值之间满足线性函数，即：
$$
y = f(x;\theta) = \theta_0+\theta_1x_1+\theta_2x_2+...+\theta_nx_n
$$
其中 $\mathbb{\theta} = (\theta_0,\theta_1,...,\theta_n)\in \mathbb{R}^{n+1}$ 。为了更加的简化和统一，引入一个 $x_0=1$，定义 $\overline{x} = (x_0,x)\in \mathbb{R}^{n+1}$，则可将上面的公式变换为：
$$
y = f(\overline{x};\theta) = \theta^T* \overline{x}
$$
多参数线性回归的目的也是为了找到一个 $f$ 能够尽可能的拟合输入特征值，即找到损失最小的 $f$ 。假设现有训练集 $\mathbb{D} = \{(\mathbf{x}^{(i)},y^{(i)})\}_{i=1}^{m}$ ，并使用平方损失作为损失函数，则代价函数为：
$$
\mathcal{L}(\theta_0,\theta_1,...,\theta_n) = \frac{1}{m} \sum_{i=1}^{m}\frac{(f(\mathbf{x}^{(i)};\theta)-y^{(i)})^2}{2}
$$
梯度更新的过程即为：

![image-20240924111415142](.\attachments\image-20240924111415142.png)

利用梯度迭代更新是解决机器学习任务最一般的技术手段，如上述推导过程所示，多元线性回归同样可以使用这样的方法。而在下面，我们会额外介绍两种从数学的角度的针对线性回归求解的技术手段。

##### 2.2 两种技术手段：

1. **最小二乘法：**	

   对于训练集 $\mathbb{D} = \{(\mathbf{x}^{(i)},y^{(i)})\}_{i=1}^{m}$ ，可知$\mathbf{x}^{(i)}=(1,x^{(i)}_1,x^{(i)}_2,...,x^{(i)}_n)^\top\in \mathbb{R}^{n+1}$, 故令 $\mathbf{y} = (y^{(i)}_1,y^{(i)}_2,...,y^{(i)}_m)$ 表示所有的样本标签值，而训练集特征矩阵表示为：
   $$
   \mathbf{X} =
   \begin{pmatrix}
   \mathbf{x}^{(1)\top} \\
   \mathbf{x}^{(2)\top} \\
   \vdots \\
   \mathbf{x}^{(m)\top}
   \end{pmatrix}
   =
   \begin{pmatrix}
   1 & x^{(1)}_{1} & \cdots & x^{(1)}_{n} \\
   1 & x^{(2)}_{1} & \cdots & x^{(2)}_{n} \\
   \vdots & \vdots & \ddots & \vdots \\
   1 & x^{(m)}_{1} & \cdots & x^{(m)}_{n}
   \end{pmatrix}.
   $$
   令 $\textbf{w}=(w_0,w_1,w_2,...,w_n)^\top$ 表示需要学习的参数，则在整个训练集上的平均损失函数可以表示为：
   $$
   \mathcal{L}(\mathbf{w}) 
   = \frac{1}{2m} \sum_{i=1}^{m} (y^{(i)} - f(\mathbf{x}^{(i)}; \mathbf{w}))^2
   = \frac{1}{2m} \sum_{i=1}^{m} (y^{(i)} - \mathbf{w}^\top \mathbf{x}^{(i)})^2
   = \frac{1}{2m} \|\mathbf{y} - \mathbf{X} \mathbf{w}\|^2.
   $$
   想要求取的结果为使得 $\mathcal{L}(\mathbf{w}) $ 最小的 $\textbf{w}$ :
   $$
   \hat{\mathbf{w}}_{LS} \in arg\min_{\mathbf{w}} \mathcal{L}(\mathbf{w}).
   $$
   其中 $\mathcal{L}(\mathbf{w}) $ 是关于 $\textbf{w}$ 的凸函数，所以当L的一阶导为0时取得最优解：

   
   $$
   0 = \frac{\partial \mathcal{L}(\mathbf{w})}{\partial \mathbf{w}} \Bigg|_{\mathbf{w} = \hat{\mathbf{w}}_{\text{LS}}} = - \frac{1}{2m} \mathbf{X}^{\top} \left( \mathbf{y} - \mathbf{X} \hat{\mathbf{w}}_{\text{LS}} \right).
   $$
   
   $$
   \hat{\mathbf{w}}_{\text{LS}} = \left( \mathbf{X}^{\top} \mathbf{X} \right)^{-1} \mathbf{X}^{\top} \mathbf{y}.
   $$
   通过上述方法我们求得了最优的参数 $\hat{\mathbf{w}}_{\text{LS}}$ 

   > [!IMPORTANT]
   >
   > 最小二乘法的几何意义
   >
   > <img src=".\attachments\image-20241009110516521.png" alt="image-20241009110516521" style="zoom:50%;" />
   >
   > 在几何上，观测数据 $\mathbf{y}$ 可以看作是一个 n 维向量，解释变量矩阵 $\mathbf{X}$ 定义了一个在 n 维空间中的 p 维子空间，这个子空间是由 $\mathbf{X}$ 的列向量所张成的。最小二乘法通过寻找 $\hat{\mathbf{w}}$ 来确定一个预测向量 $\hat{\mathbf{y}}=\mathbf{X} \hat{\mathbf{w}}$，使得预测向量 $\hat{\mathbf{y}}$ 与观测数据 $\mathbf{y}$ 最近。由于这个预测向量 $\hat{\mathbf{y}}$ 一定在由 $\mathbf{X}$ 的列向量所张成的子空间中，所以即为观测数据 $\mathbf{y}$ 在由 $\mathbf{X}$ 张成的子空间上的**正交投影**。
   >
   > <img src=".\attachments\image-20241009110103977.png" alt="image-20241009110103977" style="zoom:50%;" />
   >
   > 

2. **最大似然估计：**

   实际应用中。我们永远不可能考虑到所有的影响因素，我们的设备也不是完美的。这意味着即使我们对完全相同的数据实例进行采样，我们也可能会得到与评估相应响应的次数一样多的响应变量的不同值。因此，我们可以明确地将这种不确定性引入我们的模型中，即从建模条件概率𝑝(𝑦|𝒙)的角度来进行参数估计：

   
   $$
   y = f(\textbf{x};\textbf{w}) = w_0+w_1x_1+w_2x_2+...+w_nx_n+\epsilon
   $$
   

   其中 $\epsilon \sim \mathcal{N}(0, \sigma^2)$ ，则对于每一个样本输入，模型输出可以表示为：

   
   $$
   y^{(i)} = f(\mathbf{x}^{(i)}; \mathbf{w}) + \epsilon_i = \mathbf{w}^\top \mathbf{x}^{(i)} + \epsilon_i.
   $$
   

   由于 $\epsilon_i$ 具有一定的随机性，所以即使 $\mathbf{x}^{(i)}$ 固定，输出 $y^{(i)}$ 也不固定。因此，以模型参数和输出为条件变量的关于 $y^{(i)}$ 的概率密度函数为：

   
   $$
   p(y^{(i)} | \mathbf{x}^{(i)}, \mathbf{w}, \sigma) = \mathcal{N}(\mathbf{w}^\top \mathbf{x}^{(i)}, \sigma^2).
   $$
   

   最大似然估计（Maximum Likelihood Estimation，MLE）是指找到一组参数 $\hat{\mathbf{w}}$ 使得似然函数 $𝑝(\mathbf{𝒚}|\mathbf{X};\mathbf{w},\sigma)$ 最大：

   
   $$
   \hat{\mathbf{w}}_{ML} = \arg\max_{\mathbf{w}} \, p(y_1, \dots, y_n | \mathbf{x}_1, \dots, \mathbf{x}_n, \mathbf{w}, \sigma)
   = \arg\max_{\mathbf{w}} \, p(\mathbf{y} | \mathbf{X}, \mathbf{w}, \sigma).
   $$

   $$
   \mathcal{L}(\mathbf{w}) = p(\mathbf{y} | \mathbf{X}, \mathbf{w}, \sigma) = \prod_{i=1}^{n} p(y^{(i)} | \mathbf{x}^{(i)}, \mathbf{w}, \sigma).
   $$

   

   详细计算过程为：

   <img src=".\attachments\image-20241009115334405.png" alt="image-20241009115334405" style="zoom: 80%;" />

#### Chapter X、实践技术手段

##### X.1 特征缩放

##### X.2 学习率