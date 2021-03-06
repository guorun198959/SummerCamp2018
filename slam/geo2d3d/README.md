# 二三维几何基础

标签（空格分隔）： SummerCamp SLAM

[toc]

---

## 1. 概述
一般来说，现阶段SLAM和SfM的基本理论依据是[多视图几何](http://www.robots.ox.ac.uk/~vgg/hzbook/)中的知识，本章将主要介绍二维和三维中的几何基础知识。

## 2. 如何学习本课程？

本课程将会介绍多视图几何中最基础的点线面和线性变换的知识，具有高度的概括性，建议读者结合本课程的内容以及多视图几何书籍进行学习。

*本课程要重点理解二维中的射影变换，三维中的相似变换和欧氏变换，针孔模型，本质矩阵，基础矩阵之间的关系。*

参考资料：
[多视图几何中文版](https://pan.baidu.com/s/1nuO4rkH#list/path=%2F)
[多视图几何英文原版](http://www.robots.ox.ac.uk/~vgg/hzbook/)

## 3. 课程内容
### 3.1. 齐次坐标
首先，我们约定$(x,y)$默认为列向量，$(x,y)^T$为行向量。

在欧氏空间二维平面上的一个点是由一个有序实数对（x，y）表示。我们可以给这对增加一个额外的坐标，给出一个三元坐标（x，y，1），我们声明代表相同的点。

这似乎是没什么区别，因为我们可以简单地通过添加或移除最后一个坐标来从一个点到另一个表示。我们现在着重从概念上来解释为什么最后一个坐标需要为1，毕竟其他两个坐标不受约束。三元坐标（x，y，2）呢？。在这里，我们作一个定义，说（x，y，1）和（2x，2y，2）代表同一点，当然对任何非零值K，（Kx，Ky，K）也表示同一点。正式地，点由三元坐标的等价类别表示，其中两个三元组在它们不同的公倍数时是等效的。这称为点的齐次坐标。给定一个三元坐标（Kx，Ky，K），我们在得到原来的坐标后除以K得到（x，y）。

读者会注意到，虽然（x，y，1）与坐标对（x，y）表示相同的点，但没有对应于三元坐标（x，y，0）的点。如果我们试着用最后一个坐标来划分，我们得到了无穷的点（x / 0，y / 0）。无穷远点就是这样产生的。它们是齐次坐标最后那个坐标为零的点。

现在我们知道了如何在二维欧氏空间中做到这一点，通过把点表示为齐次向量，然后可以把它扩展到射影空间，很明显，我们可以在任何维度上做同样的事情。欧氏空间的IRⁿ可通过将点表示成齐次向量而扩展到射影空间的IPⁿ。结果表明，二维射影空间中无穷远点形成一条线，通常称为无穷远处的直线。在三个维度中，它们在无穷远处形成平面。

### 3.2. 二维中的点和直线
二维平面中点的齐次表达为$x=(x,y,w)^T$，其物理意义等同于$(x/w,y/w)$,其中(x,y,0)是无穷远点。

对于二维中的直线可用方程$ax+by+c=0$表达,其对应的齐次坐标表达是$l=(a,b,c)^T$,$kax+kbx+kc=0$与前式代表同一条直线,即$kl=l$。

二维中的点和线存在以下关系：
1. **点在直线上**：点x在直线l上的充要条件是$x^Tl=l^Tx=0$.
2. **两直线相交**：两直线的交点为其的叉积$x=l \times l'$.
3. **两点的连线**：连接两点x,x'的直线为x,x'的叉积$l=x \times x'$.
4. **点到直线距离**: 点x=(x,y,1)到直线$l=(a,b,c)^T的距离为$$distance=\frac{x^Tl}{\sqrt{a^2+b^2}}$.

### 3.3. 二维中的二次曲线和对偶二次曲线
#### 3.3.1. 二次曲线的齐次表达
一般来说，椭圆，抛物线，双曲线统称为二次曲线，其在非齐次坐标中，二次曲线的一般方程为：
$$ax^2+bxy+cy^2+dx+ey+f=0$$
通过$x \rightarrow x/w,y \rightarrow  y/w$将方程齐次化可得:
$$ax^2+bxy+cy^2+dxw+eyw+fw^2=0$$
表示成矩阵的形式：
$$x^TCx=0$$
其中$C=\begin{bmatrix}a&b/2&d/2\\b/2&c&e/2\\d/2&e/2&f\end{bmatrix}$
可以看到C是一个对称矩阵，其乘以一个标量不影响其表达，因此C是一条二次曲线的齐次表达，二次曲线有5个自由度（对称矩阵6个元素减去一个比例因子）。

#### 3.3.2. 五点定义一条二次曲线
若点$x_i=(x,y)$在二次曲线C上，则：
$$(x^2,xy,y^2,x,y,1)^T \cdot (a,b,c,d,e,f)=0$$
通过把多个点提供的约束堆砌起来，得到一个齐次线性方程组的解即为二次曲线的解。

#### 3.3.3. 二次曲线的切线
**二次曲线的切线**：过（非退化）二次曲线C上点x的切线l由$l=Cx$确定.

#### 3.3.4. 对偶二次曲线
由于二维平面上点和直线之间的对偶性质，二次曲线也可由5条切线确定：
$$x^TCx=(C^{-1}l)^TCC^{-1}l=l^T(C^{-1})^Tl=l^TC^{-1}l=0$$

#### 3.3.5. 退化二次曲线
当det（C）=0，也就是C不满秩时，其表达的是退化二次曲线。

### 3.4. 二维线性变换
#### 3.4.1. 射影变换（Homography）
**定义**:*对于非奇异变换矩阵$H_{33}$,可提供二维点集之间的可逆映射：$x'=Hx$，这个映射关系称为射影变换，或者单应性变换（Homography）.*


根据直线和二次曲线定义：
**直线的射影变换：**直线l变换为$l'=H^{-T}l$
**二次曲线的射影变换:**二次曲线C变换为$C'=H^{-T}CH^{-1}$

由直线的射影变换可知，在射影变换条件下，直线变换后仍为直线，射影变换由于其特性，又被称为保线变换。
二次曲线的射影变换在任意两个非退化二次曲线中找到对应，也就是说，所有非退化二次曲线都是**射影等价**的。

#### 3.4.2. 变换的层次，仿射，相似，欧氏变换
|变换类型|矩阵|不变性质|自由度|正方形变换后|
|--------|--------|--------|-------|---------|
|射影|$H=\begin{bmatrix}A & t \\p^T & 1\end{bmatrix}$|直线，共点，共线，交比|8|![](homography2d.jpg)|
|仿射|$A=\begin{bmatrix}A & t \\0^T & 1\end{bmatrix}$|平行，面积比，平行长度比，无穷远线|6|![](affine2d.jpg)|
|相似|$S=\begin{bmatrix}sR & t \\0^T & 1\end{bmatrix}$|形状，长度比，夹角，虚圆点|4|![](similarity2d.jpg)|
|欧氏|$T=\begin{bmatrix}R & t \\0^T & 1\end{bmatrix}$|刚体变换，长度，面积|3|![](euclidean2d.jpg)|

由于R是正交单位阵（酉矩阵）只有一个自由度，因此对于相似变换和欧氏变换，在优化求解时都存在参数化问题，一般采用二维中的李群来对其进行流型上的优化。

### 3.5. 三维中的点和面
在三维空间中，点(X,Y,Z)在平面上可写成：
$$\pi_1X+\pi_2Y+\pi_3Z+\pi_4=0$$
齐次化到（X,Y,Z,W）有：
$$\pi_1X+\pi_2Y+\pi_3Z+\pi_4W=0$$
因此齐次表达为：
$$\pi^TX=X^T\pi=0$$

1. **三点确定一个平面:**由三个点$\begin{bmatrix}X_i\\1\end{bmatrix}$构成的平面为$\begin{bmatrix}(X_1-X_2) \times (X_2-X3) \\ - X_3^T(X1 \times X_2)\end{bmatrix}$


### 3.6. 三维中的直线
在三维空间中,两个点的连接或者两平面相交定义一条直线，由于三维中的直线拥有4个自由度，因此其表示相当难处理，因为4个自由度的对象在齐次坐标中用5维矢量表示，但问题是5维齐次矢量和平面的4维矢量很难在数学表达式中使用。

由于以上问题，直线的表达一般都由两个点的连接或者两个面的相交表示。这里面用到的核心知识是零空间的概念。

#### 3.6.1. 矩阵的零空间

### 3.7. 三维中的曲面和曲线
#### 3.7.1. 二次曲面
在三维齐次坐标系下，二次曲面由下列方程定义：
$$X^TQX=0$$
其中Q是一个4x4的对称矩阵，二次曲面的许多性质与平面中二次曲线的性质类似，其部分性质如下：
1. 一个二次曲面有9个自由度，对应4X4对称矩阵的10个元素减去一个尺度
2. 一般位置上的9个点确定一个二次曲面
3. 如果Q是奇异的，那么二次曲面是退化的，可由较少点来确定
4. 类似于二次曲线的极线，$\pi=QX$是二次曲面Q在点X处的极平面，当Q为非奇异并且X在Q外时，极平面由过X且与Q相切的射线组成的锥与Q相接触的点来定义。
5. 平面$\pi$与二次曲面Q的交线是二次曲线C。计算该二次曲线有些麻烦，这里就不具体介绍了。

#### 3.7.2. 三次绕线
三次绕线可以看成是2D曲线的3维类推，2维射影平面上的一条曲线可由下列方程给出的一条参数曲线描述
$$\begin{bmatrix}x_1\\x_2\\x_3 \end{bmatrix}=A\begin{bmatrix}1\\ \theta \\ \theta^2 \end{bmatrix}$$
其中A是非奇异的3*3矩阵,$\theta$是参数方程自变量。

类似地，一条三次绕线定义为三维空间中的一条曲线，其参数形式如下：
$$\begin{bmatrix}X_1\\X_2\\X_3\\X4 \end{bmatrix}=A\begin{bmatrix}1\\ \theta \\ \theta^2 \\ \theta^3 \end{bmatrix}$$
其中A是4*4的非奇异矩阵，所有的三次绕线都是射影等价的。


### 3.8. 三维中变换的层次
![3dtrans](https://gss0.bdstatic.com/-4o3dSag_xI4khGkpoWK1HF6hhy/baike/c0%3Dbaike72%2C5%2C5%2C72%2C24/sign=fb9ceb248535e5dd8421ad8d17afcc8a/83025aafa40f4bfb4fb39522034f78f0f7361860.jpg)
三维空间射影变换及其几个特殊形式在计算机视觉和机器人控制中应用广泛，

|变换类型|矩阵|不变性质|自由度|正方体变换后|
|--------|--------|--------|-------|---------|
|射影|$H=\begin{bmatrix}A & t \\p^T & 1\end{bmatrix}$|接触表面的相交和相切，高斯曲率的符号|15|![](homography3d.jpg)|
|仿射|$A=\begin{bmatrix}A & t \\0^T & 1\end{bmatrix}$|平面的平行性，体积比，形心，无穷远平面|12|![](affine3d.jpg)|
|相似|$S=\begin{bmatrix}sR & t \\0^T & 1\end{bmatrix}$|绝对二次曲线，形状|7|![](similarity3d.jpg)|
|欧氏|$T=\begin{bmatrix}R & t \\0^T & 1\end{bmatrix}$|形状大小体积，刚体变换|6|![](euclidean3d.jpg)|

### 3.9. 针孔相机和对极几何
#### 3.9.1. 针孔相机投影
相机拍摄的过程本质上是将三维世界投影到二维图像上，我们将这样的投影关系定义为：
$$x=proj(X)$$
对于理想的针孔模型，该投影过程是一个线性变换：
$$x=KX=\begin{bmatrix}fx &  & cx\\ & fy& cy\\ & & 1 \end{bmatrix}\begin{bmatrix} x\\y\\z \end{bmatrix}$$

理想的针孔模型相机在现实中是不存在的，因此产生了很多各样的投影模型和畸变模型以及其对应的标定方法，这些将在课程[slam/camera](../camera/README.md)中详细介绍。

#### 3.9.2. 对极几何
![epipolar](http://my.csdn.net/uploads/201203/30/1333071329_8845.jpg)
如图，设$p_l$和$p_r$是两个相机$O_l,O_r$在相机z=1平面上的点，其射线相交与P点，其中P在左右相机的坐标系下位置分别为$P_l,P_r$，右相机到左相机坐标系下的变换为[R t]:
$$P_l=RP_r+t  <=> P_r=R^T(P_l-t)$$
在$O_l$坐标系下，极平面为$P_l \times t$，且有：
$$(P_l-t)^T \cdot (t\times P_l)=P_r^TR^T\cdot (t\times P_l)=0$$
由于叉乘可等效为叉乘矩阵点乘：
$$a{\times}b={\left[a\right]_\times}b=\left[{\begin{array}{*{20}{c}}
0&{-{a_3}}&{{a_2}}\\
{{a_3}}&0&{-{a_1}}\\
{-{a_2}}&{{a_1}}&0
\end{array}}\right]\left[{\begin{array}{*{20}{c}}
{{b_1}}\\
{b{}_2}\\
{{b_3}}
\end{array}}\right]$$
将t化成叉积矩阵：
$$S=\begin{bmatrix}0&-t_z&t_y\\t_z&0&-t_x\\-t_y&t_x&0\end{bmatrix}$$
上式可化为：$P_r^TR^T\cdot SP_l=0$
令$E=R^TS$,即得：
$$P_r^TEP_l=p_r^TE p_l=0$$
将$P=K^{-1}x$带入有：
$$(K^{-1}x_r)^TEK^{-1}x_l=0$$
令$F=(K^{-1})^TEK^{-1}$,可得：
$$x_rFx_l=0$$

这里我们将E称为本质矩阵（Essential Matrix），F称为基础矩阵（Fundamental Matrix），[R t]为两图像之间的变换，由上面的推导可以发现这三者之间存在着一些对应关系，这个关系在SfM和SLAM初始化中扮演着重要角色。


## 3.10. 常用矩阵的求解
### 3.10.1. 二维射影变换矩阵H估计

### 3.10.2. 基础矩阵F估计

### 3.10.3. 本质矩阵E的估计

### 3.10.4. 三维相似变换矩阵S的估计

### 3.10.5. 从本质矩阵E分解R,t

## 4. 课程练习

现有分辨率为640*480的针孔相机，其焦距和主点坐标分别为fx=fy=400,cx=320,cy=240.用其拍摄了两张图片并匹配对应关键点，获得的匹配关系在[matches.txt](matches.txt)文件中，已知两相机之间的距离为0.05米，且第一个相机的位置为原点，请编写程序，根据已知的匹配关系和相机内参求解第二个相机的位置和姿态。

文件格式说明：
每行4个float型表示一对点匹配，分别代表
x_left,y_left,x_right,y_right








