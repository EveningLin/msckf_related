# msckf_related
msckf_related将会收录我对msckf的注释版和改进，可能存在谬误，还请大佬多加指正
## 一. msckf_commended注释版
参考代码 ：[宾大Vijay Kumar实验室开源的双目版本MSCKF算法](https://github.com/KumarRobotics/msckf_vio) 
### 1.1 前端
### 1.2 后端
核心观点：系统状态 不一定等同于 滤波器状态
具体的说，在这个系统中
下图取自[邱笑晨的解释](https://www.bilibili.com/video/BV1Y4411g7FJ/?vd_source=0ada5c95f58cb319a77c62f35a4c0057)
![image](https://github.com/EveningLin/msckf_related/assets/110521494/519f889c-b1f9-456b-9df3-1a84405347ee)


_更新阶段_
(1)线性展开实现过程

(2)为什么要乘上左零空间

重投影误差中包含了相机位姿和特征点，但是在滤波器方程中实际上是不包含特征点的，所以通过乘上左零空间消除特征点。

· 左零空间：矩阵A的左零空间就是矩阵A^T的零空间

· 消去过程：

· 怎么找到左零空间：利用svd中Σ的零奇异值

【延申】如何证明svd中Σ的零奇异值对应的U就是左零空间呢
具体来说，H_fj 被分解成了 U * Σ * V^T 的形式，如果 (H) 的秩是 (r)行数是（m），那么 (Σ) 的前 (r) 个对角元素是非零的，而后面的元素都是零。考虑 (U) 的最后 (m - r) 列（即 (U) 的右列），这些列对应的奇异值都是零。由于 (U) 是正交矩阵，这些列在行空间中是正交的，并且它们张成了一个与 (H) 的行空间正交的子空间。因此，这些列（或者它们的线性组合）可以近似看作是 (H) 的左零空间的一个基。

【延申】具体步骤

![image](https://github.com/EveningLin/msckf_related/assets/110521494/520b8f8a-b69d-4bd5-bad8-809c6578ee16)

(3)为什么要对H_x进行QR分解？

为了对H_x进一步降低维度
【延申】怎么证明等价性？

### 1.3 可观性修正
可观性修正有两种不同的和新方法，分别是FEJ和OCKF
FEJ参考论文：[A  First-Estimates Jacobian EKF for Improving
 SLAM Consistency](https://www.sci-hub.se/10.1007/978-3-642-00196-3_43)
OCKF参考论文：[2012-TR: Observability-constrained Vision-aided Inertial Navigation]()
#### 1.3.1 前置知识
（1）可观性

这个系统在进行状态估计时，哪些自由度是可以被估计出来的。

能观性通过Observability Matrix（能观性矩阵）体现，系统Unobservable的状态维数是这个矩阵零空间的维数。

【延申】对于视觉惯性slam,什么样的运动使系统可观性退化，什么样的运动使其可观性增强？

①以(几乎)恒定的加速度直线或者是转圈，都会使得尺度不可观

②无旋转的运动，全局方向的所有3自由度均不可观

【todo！！！！】证明部分还有一点模糊


参考论文：[VINS on Wheels]([https://www.sci-hub.se/10.1007/978-3-642-00196-3_43](https://ieeexplore.ieee.org/stamp/stamp.jsp?tp=&arnumber=7989603))

本文时使用轮式里程计和平面假设来缓解上述问题

（2）可观性分析手段

1）局部可观性矩阵local observability matrix

（可能有偏颇的）定义：在有限时间步内，滤波器的状态向量可以通过m（滤波器状态估计状态）确定  

![image](https://github.com/EveningLin/msckf_related/assets/110521494/301defcd-4aa8-4a80-819c-d38cdd60e59d)


![image](https://github.com/EveningLin/msckf_related/assets/110521494/45efc573-93e5-42d3-989c-01186a729d34)

![image](https://github.com/EveningLin/msckf_related/assets/110521494/b5fc1990-20ef-4433-af65-6c08cc24d8ee)
系统在k到k+n-1的时间段内是局部可观察的，该时间段的局部可观察矩阵M为秩n是充分必要的


#### 1.3.2 First Estimate Jacobian （FEJ）
![image](https://github.com/EveningLin/msckf_related/assets/110521494/f4713f30-c67b-404a-854b-9fac90faef1f)

【todo！！！】为什么要做这两步替换？为什么能保证自由度无偏移？
证明来自[Genearalized analysis and improvement of the consistency for EKF-based SLAM](https://people.csail.mit.edu/ghuang/paper/tr/TR_slam_genconsistency.pdf)


