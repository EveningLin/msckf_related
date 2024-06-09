# msckf_related
msckf_related将会收录我对msckf的注释版和改进，可能存在谬误，还请大佬多加指正
## 一. msckf_commended注释版
参考代码 ：[宾大Vijay Kumar实验室开源的双目版本MSCKF算法](https://github.com/KumarRobotics/msckf_vio) 
### 1.1 前端
### 1.2 后端
核心观点：系统状态 不一定等同于 滤波器状态
具体的说，在这个系统中
下图取自[邱笑晨的解释]( https://www.bilibili.com/video/BV1Y4411g7FJ/?vd_source=0ada5c95f58cb319a77c62f35a4c0057)
![image](https://github.com/EveningLin/msckf_related/assets/110521494/519f889c-b1f9-456b-9df3-1a84405347ee)


_更新阶段_
(1)线性展开实现过程

(2)为什么要乘上左零空间。
重投影误差中包含了相机位姿和特征点，但是在滤波器方程中实际上是不包含特征点的，所以通过乘上左零空间消除特征点。
左零空间：矩阵A的左零空间就是矩阵A^T的零空间
消去过程

