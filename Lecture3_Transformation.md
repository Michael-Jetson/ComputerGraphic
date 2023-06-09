# 变换

## 基础概念

变换只有两种，一种是模型变换，一种是视图变换

![Games101_L3_4](./assets/Games101_L3_4.png)

模型的变换就是不同的对象进行变换，比如说下图中机器人的旋转，物体形变（第二个图中机器人将字母压扁）等

![Games101_L3_6](./assets/Games101_L3_6.png)

![Games101_L3_7](./assets/Games101_L3_7.png)

视图的变换就是三维到二维的投影，人生活的是三维的世界，但是眼镜（或者说相机）看到的是二维，这个过程就是投影

![Games101_L3_8](./assets/Games101_L3_8.png)

## 二维变换

二维变换是可以和矩阵联系起来的

### 缩放（Scale）变换

我们对下图中的钟表图像做一个缩放变换

![Games101_L3_10](./assets/Games101_L3_10.png)

其数学形式可表达为
$$
x^\prime=sx\\
y^\prime=sy\\
\left[
\begin{matrix}
x^\prime\\
y^\prime
\end{matrix}
\right]
=
\left[
\begin{matrix}
s&0\\
0&s\\
\end{matrix}
\right]
\left[
\begin{matrix}
x\\
y
\end{matrix}
\right]
$$
![Games101_L3_12](./assets/Games101_L3_12.png)

我们称这个对角阵为缩放矩阵

当然，也可以使用比例不等的缩放矩阵，这样不同坐标轴就可以进行不均匀的缩放，$x$轴按照$s_x$进行缩放，$y$轴按照$s_y$进行缩放

![Games101_L3_13](./assets/Games101_L3_13.png)

### 翻转变换

相对某个坐标轴进行镜像翻转，进行操作的矩阵称为**反射矩阵（Reflection Matrix）**

![Games101_L3_14](./assets/Games101_L3_14.png)

### 剪切变换（Shear）

这是一个复杂一些的变换，也叫切变，这个操作就像是沿着坐标轴去拽图像一样

如下图所示，任何一个点的纵坐标没有变化，但是横坐标会发生变化，而且移动的程度是跟其纵坐标成正比的

![Games101_L3_15](./assets/Games101_L3_15.png)

### 旋转变换（Rotate）

下图是一个二维旋转，绕着原点进行逆时针旋转，如果后面不加说明，则默认是绕原点逆时针旋转

![Games101_L3_16](./assets/Games101_L3_16.png)

旋转变换的矩阵如下

![Games101_L3_17](./assets/Games101_L3_17.png)

### 线性变换

所有的线性变换，实际上都可以当次一个矩阵

![Games101_L3_18](./assets/Games101_L3_18.png)

## 齐次坐标（Homogeneous coordinates）

有一种变换很特殊，也就是平移变换，这个变换看起来很简单，但是有一个问题，就是如何将其转化为矩阵乘法形式

![Games101_L3_22](./assets/Games101_L3_22.png)

看起来这是一个很难的问题，我们可以将其记为下图中的形式，但是这不是一种线性变换，我们也不希望平移变换是一种特殊情况或者一个特例，而是想找到一种通用的表示

![Games101_L3_23](./assets/Games101_L3_23.png)

这个问题的解决方法就是齐次坐标，齐次变换可以给出一个统一的形式来表示物体坐标，可以对于点和向量，增加一个维度来表示（下图所示），这样就可以有一个很好的性质：可以为平移等各种变换找到一个统一的表示

![Games101_L3_24](./assets/Games101_L3_24.png)

在这种齐次坐标表示下，我们对一个点进行了平移变换，然后我们就可以直接得到一个平移操作后的点，如果我们对向量进行这种操作，那么得到还是同一个向量，不会有任何改变，实现了向量的平移不变性

如果我们换一个角度思考，第三个维度也可以表示是点还是向量（下图所示），保证了向量和点在各种变换下的正确性

![Games101_L3_25](./assets/Games101_L3_25.png)

如果齐次坐标向量乘以一个非零常数，那么还是同一个坐标向量，这个很简单但是很有用

## 仿射变换（Affine Transformation）

仿射变换（Affine Transformation）是一种特殊的几何变换，它可以改变图形的尺寸、形状和位置，但在变换过程中，会保持线段的“直线性”和“平行性”。

也就是说，经过仿射变换后，所有原来的直线仍然是直线，且原来平行的线仍然是平行的。此外，仿射变换也保持了比例尺和中点的性质，即线段的中点经过变换后仍然是变换后的线段的中点，而比例分点也保持了相同的比例。

仿射变换包括以下几种基本类型的变换：

1. **平移（Translation）**：物体在空间中按照某一方向移动一定的距离，但不改变其大小和形状。
2. **缩放（Scaling）**：物体的大小按照一定的比例进行增大或缩小，可以在不同的轴向上使用不同的比例因子，但不改变其形状。
3. **旋转（Rotation）**：物体绕着某个点（在二维中为一点，在三维中为一条线）按照一定的角度进行旋转，但不改变其大小和形状。
4. **切变（Shearing）**：物体在某一方向上进行扭曲，形状发生改变，但不改变其面积（在二维）或体积（在三维）。

在计算机图形学中，这些仿射变换可以通过矩阵乘法实现，这使得变换的组合和复合变得非常容易。例如，我们可以先对一个物体进行缩放，然后进行旋转，最后进行平移，这三个操作可以通过三个矩阵的乘积来表示，从而得到一个单一的变换矩阵，用来描述这个复合的仿射变换。

最后，值得注意的是，仿射变换是一种线性变换，但并不保持角度和长度。例如，旋转保持长度但改变角度，而缩放保持角度但改变长度。

如果我们使用齐次坐标系，那么就可以统一所有的仿射变换，代价不过是多引入了一个维度

![Games101_L3_26](./assets/Games101_L3_26.png)

即有

![Games101_L3_27](./assets/Games101_L3_27.png)

## 逆变换（Inverse Transform）

将操作反过来（或者说逆向操作）就是逆变换，一般称为某变换的逆变换，数学上使用逆矩阵完成逆变换

![Games101_L3_28](./assets/Games101_L3_28.png)

当然注意一下，矩阵乘法不具有交换性，也就是说变换也不具有交换性，进行逆变换的时候也要注意顺序

![Games101_L3_34](./assets/Games101_L3_34.png)



如下图所示，变换的顺序是从$A_1$到$A_n$，如果想逆变换也要从$A_n^{-1}$到$A_1^{-1}$![Games101_L3_35](./assets/Games101_L3_35.png)

## 分解复杂变换

也可以使用拆解的方式，将一个复杂变换拆成若干基本变换，比如说下图中的绕非原点的旋转，可以将其拆解为三种变换

1. 平移中心到原点
2. 旋转
3. 平移回去

![Games101_L3_36](./assets/Games101_L3_36.png)

## 三维变换

与齐次坐标下的二维变换一致，加上额外的维度就可以得到三维齐次坐标系下的三维坐标

![Games101_L3_38](./assets/Games101_L3_38.png)

相应地，坐标变换矩阵是一个4x4的矩阵

![Games101_L3_39](./assets/Games101_L3_39.png)

缩放和平移的矩阵如下

![Games101_L4_6](./assets/Games101_L4_6.png)

绕三个坐标轴旋转的旋转矩阵如下图所示

我们可以把任意旋转分解为三次基本的绕坐标轴的旋转

![Games101_L4_7](./assets/Games101_L4_7.png)

![Games101_L4_8](./assets/Games101_L4_8.png)

## 罗德里格斯公式（Rodrigues’ Rotation Formula）

实际上，的确有这样一个公司，直接将任意的旋转进行分解，也就是罗德里格斯公式

这个公式的作用，就是给出了一个旋转矩阵，描述了物体绕轴$n$旋转了角度$\alpha$的情况

![Games101_L4_9](./assets/Games101_L4_9.png)

# 观测变换（Viewing Transformation）

闫令琪老师说国内教材喜欢将观测变换分为视图变换（View）和投影（Projection）变换，在这里，观测变换就是视图变换+投影变换

## 视图变换

什么是视图变换呢？我们考虑一下拍照的场景

- 首先找到一个好的拍摄场地并且安排好人员位置（这就是模型变换，或者说安排好模型位置，改变的是模型）
- 然后找到一个好的角度来摆放相机位置（这就是视图变换，改变的是相机）
- 然后进行拍照（完成投影变换）

至此，三维到二维的投影完成，也称为一个MVP变换

![Games101_L4_11](./assets/Games101_L4_11.png)

视图变换是摆放相机，那么如何去摆放相机呢？

我们首先定义相机的**位置（Position）**，然后定义相机的拍摄角度（或者说摆放角度），然后为了防止相机旋转，定义一个**向上方向（Up direction）**

![Games101_L4_12](./assets/Games101_L4_12.png)

下一步就是如何进行视图变换，人坐在车上的时候，如果不向外看，是无法感觉自己在移动，相机也是如此，如果相机与场景中的物体一起运动（相对位姿不变），那么拍出来的照片也是一样的，考虑到这一点并且进一步思考，可以让相机固定于原点，并且相机永远向-Z方向看，向上方向永远是Y轴正方向（约定俗成）

![Games101_L4_13](./assets/Games101_L4_13.png)

所以，我们可以将相机和物体都进行移动，相机移动到原点，这样图像不会有改变，或者说得到的结果一样，但是操作会简化

![Games101_L4_14](./assets/Games101_L4_14.png)

具体操作

- 平移$e$到原点
- 旋转$g$到-Z方向
- 旋转$t$到Y轴
- 旋转$g\times t$到X

当然这一系列的操作显得很繁琐，所以可以使用一个总的变换矩阵表示

![Games101_L4_15](./assets/Games101_L4_15.png)

我们可以将这个视图变换拆为两个变换：平移和旋转，即有
$$
M_{view}=R_{view}T_{view}
$$
视图变换操作的是相机，其他物体跟着变换

## 投影变换（Projection transformation）

有两种不同的投影方式，**正交投影（Orthographic projection）**和**透视投影（Perspective projection）**

![Games101_L4_18](./assets/Games101_L4_18.png)

### 正交投影

在正交投影中，投影线（也就是从三维空间中的点到二维空间中的点的线）是垂直于投影平面的。因此，正交投影不会因为物体与观察者的距离改变而产生大小或形状的变化，也就是说，它没有考虑透视效果。正交投影常用于工程图或者CAD等需要精确度的应用场景。

![Games101_L4_20](./assets/Games101_L4_20.png)

实际上，会使用其他的方法来进行正交投影

一般的，会使用六个数来描述一个空间中的立方体，然后我们将这个形状映射到一个标准正方体（如下图所示），过程是先平移然后缩放

![Games101_L4_21](./assets/Games101_L4_21.png)

具体的数学形式如下

![Games101_L4_23](./assets/Games101_L4_23.png)

### 透视投影

透视投影则考虑了透视效果，即物体与观察者的距离越远，物体看起来越小。在透视投影中，投影线并不是垂直于投影平面的，而是从三维空间中的点发散出来，并且都汇聚于一个固定的点（观察点或者称为消失点），形似一个四棱锥（如下图所示）。因此，透视投影能够创建出一种更真实的三维效果，更符合人类的视觉感知。透视投影也是在图形学中应用最广泛的投影。

![Games101_L4_19](./assets/Games101_L4_19.png)

透视投影带来的一个效果就是，三维空间中的平行线不再相交

那么透视投影应该怎么完成呢，我们很难直接写出来透视投影的矩阵，但是我们可以将其拆为两部分

如下图所示，我们先将远平面f和中间的位置进行“挤压”，将其变为一个立方体，然后做一次正交投影就可以解决，这个过程中Z值不会改变，也就是说距离不会发生改变

![Games101_L4_28](./assets/Games101_L4_28.png)

这里，“挤压”的矩阵记为$M_{persp->ortho}$，意思是透视到正交的矩阵

我们从侧面看（如下图），坐标为$(x,y,z)$的点，根据相似三角形原理，可以对XY轴进行挤压

![Games101_L4_29](./assets/Games101_L4_29.png)

这样就可以得到XY坐标变换之后的公式，当然，我们无法知道深度信息，也就是说Z轴是未知的，这里以unknown表示

![Games101_L4_30](./assets/Games101_L4_30.png)

我们可以解出来第一、二、四行，不过第四行除了可以是“0 0 1 0”形式，还可以是“0 0 0 z”形式，但是后者中包含变量，计算的时候需要实时带入z的真实值，对于计算来说是一个很麻烦的事情，涉及开辟内存和传值操作，所以一般不使用这种方法

![Games101_L4_31](./assets/Games101_L4_31.png)

当然我们注意一下，在近平面上的点坐标是不会改变的，同时远平面上点的Z坐标也不会改变，我们可以利用这两点

首先如下图所示，我们有一个通用的变换公式，我们带入近平面上的点，将z换成n，然后我们就发现，对于近平面上的点，这个映射会将其映射回自身

![Games101_L4_33](./assets/Games101_L4_33.png)

所以第三行必须满足上图下方的公式，但是$n^2$与xy无关，所以前两个元素为0

同时，在远平面上还有一个性质，我们根据这个性质，可以列方程式

![Games101_L4_34](./assets/Games101_L4_34.png)

然后就可以解出来AB的值

![Games101_L4_35](./assets/Games101_L4_35.png)

这样，我们就可以完成这个变换矩阵的计算