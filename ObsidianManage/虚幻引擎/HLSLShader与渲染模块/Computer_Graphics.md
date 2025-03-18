---
date: 2025-01-23
---
# Computer_Graphics

## Class Ⅰ .  介绍

**1. 课程包括哪些内容 ?**

* 光栅化
* GPU 硬件编程的运行原理
* 几何 :  曲线和曲面
* 如何实现光线追踪 ,  路径追踪 ,  光线传播方法
* 动画与模拟

>每秒渲染帧数大于30帧为实时渲染 ,  否则称之为离线渲染 .



**2. 学科间的差异 :**

* 计算机视觉 :  一切需要猜测的均为计算机视觉 .  分析与理解 .
* 深度学习 :  GAN等 ,  生成式AI .
* 数字图像处理 :  将图片处理后生成新的图片 .

>随着学科间发展 ,  界限也越来越模糊 .  


---

## Class Ⅱ .  平面解析几何

### **一. 矢量 :**

**1. 矢量的加法 :**

* 几何意义 :
	- 三维空间中的平移运动 ,  速度 ,  加速度 等 .

- 数学意义 :
	- 矢量的相加 = 相应分量分别相加 .


**2. 矢量的减法 :**

* 几何意义 :
	* 求被减向量到减量的方向与距离 (  矢量起点在同一个点上 .  )

>被减向量的终点 -> 减数向量的终点.


**3. 矢量的长度 :**

- 几何意义 :
	- 当矢量的长度为 1 时 ,  我们称他为单位向量 .

* 数学意义 :
	* 矢量的模 = 根号下 各分量的平方相加

>图形学上 ,  矢量默认为列向量 .  转置可以转换行和列 .


**4. 矢量的点乘 :**

* 几何意义 :
	* 判断矢量间的角度 ,  点乘的结果是一个数字 .  当数字大于 0 ,  说明矢量间的角度小于90° ; 等于 0 ,  则夹角为 90° ,  小于 0 , 则夹角大于90°
	* 一个向量投影到另一个向量 ( 矢量起点相同 ,  矢量终点做被投影矢量的垂线 .  垂线相交的点与矢量起点的长度为点乘的结果 .)

>当运算的矢量均为单位矢量 ,  则点乘直接为矢量间夹角的余弦 .

>点乘符合的运算律 :
>	乘法交换律;
>	乘法结合律; 

* 数学意义 :
	* 矢量的模乘以夹角的余弦 .



**5. 矢量的叉乘 :**

- 几何意义 :
	- 输入两个矢量 ,  得到一个新矢量 ,  此矢量均垂直于输入的矢量 .  方向由坐标系决定 .  面向矢量AB ,  如果A 转向 B 为 顺时针 ,  则新矢量指向你 ,  反之则指向对向 .

>叉乘在图形中的作用 :
>	判断里和外 ( 法向 )
>	判断左和右 ( 坐标系 )

* 数学意义 :
	* 矢量的模乘以夹角的正弦 .




### **二. 矩阵 :**




---

## Class Ⅲ . Transform One

### 一. Matrix 的数学运算:

- 1. `matrix的数学定义 :`
	- 由 n 行 ,  m 列构成的矩阵数字网格 ,  被称之为`matrix` .

- 2. 用矩阵表示矢量 :
	- 由于矩阵可以包含任何正数的行和列 ,  因此矢量可以被视为`1 * n` 或 `n * 1`的矩阵 .  

-  3. 矩阵的转置 :
	- 给定$r * c$的矩阵$M$ ,  $M$的转置$(Transpose)$表示为$M^T$ ,  是$c * r$矩阵 .  数学意义为将矩阵的行和列进行调换 .

- 4. 矩阵的乘法 :
	- 一个$r * n$的矩阵$A$ 可以乘以一个$n * c$的矩阵$B$ .  其结果(表示为$AB$)是一个$r * c$的矩阵 .
$$
		AB 
		=
		\begin{bmatrix}
		a_{11} &a_{12}\\
		a_{21} &a_{22}
		\end{bmatrix}
		\begin{bmatrix}
		b_{11} &b_{12}\\
		b_{21} &b_{22}
		\end{bmatrix}
		=
		\begin{bmatrix}
		a_{11}b_{11}+a_{12}b_{21} &a_{11}b_{12}+a_{12}b_{22}\\
		a_{21}b_{11}+a_{22}b_{21} &a_{21}b_{12}+a_{22}b_{22}
		\end{bmatrix}
		
$$

- 5. 特殊的矩阵 :
	- 单位矩阵 ,  任何矩阵乘以单位矩阵 ,  其结果不变 .
$$
\begin{bmatrix}
1&0 \\
0&1
\end{bmatrix}
$$

- 逆矩阵 :
	- 给定$r * c$的矩阵 $M$ , $M$ 的逆矩阵表示为 $M^{-1}$ ,  $M * M^{-1}$ 一定为单位矩阵 .  逆矩阵的具体计算很麻烦 ,  推荐使用现成的计算函数 .

### 二. 二维图像的Scale Transform :


1. **Scale Transform数学公式 :**
	- `x prime = s * x; y prime = s * y` 其中 ,  `x , y`为二维坐标系的坐标轴 .
	- 使用矩阵来表达 :
$$
	\begin{bmatrix} 
	x`\\              
	y`                
	\end{bmatrix} 
	=
	\begin{bmatrix}
	s&0\\
	0&s
	\end{bmatrix}
	\begin{bmatrix}
	x\\
	y
	\end{bmatrix}
	=   
	\begin{bmatrix}
	s*x\\
	s*y
	\end{bmatrix}
$$
	- 其中, 这个矩阵被称之为`Scale Matrix` :
 $$\begin{bmatrix}
	s&0\\
	0&s
	\end{bmatrix}$$
	- 当 `x,y,z` 三轴的缩放数值并不相同 ,  则`Scale Matrix`为(其中,  $s_x ,s_y$ 分别为`x,y`轴向上的缩放比例 ) :
$$
\begin{bmatrix}
s_x&0\\
0&s_y
\end{bmatrix}
$$
	 - 当然 ,  Scale 也可以用来表现反转效果 .  当 $x` = -x ; y` = y$ 时, 二维图像会已Y轴进行反转 ,  `Scale Matrix` 则表达为 :
$$
	\begin{bmatrix}
	-1&0\\
	0&1
	\end{bmatrix}
$$

### 三. 二维图像的切变 (Shear Matrix) :

**1. 什么是切变 ?**

- 假设一个正方形 ,  左下角的顶点处于原点位置上 ,  左上角与右上角的顶点平移 $a$ 个单位 .  求像素在移动前与移动后的位置关系 .

**2. 切变矩阵 :**

$$
	\begin{bmatrix} 
	x`\\              
	y`                
	\end{bmatrix} 
	=
	\begin{bmatrix}
	1&a\\
	0&1
	\end{bmatrix}
	\begin{bmatrix}
	x\\
	y
	\end{bmatrix}
	=   
	\begin{bmatrix}
	x+ay\\
	y
	\end{bmatrix}
$$
- 根据这个矩阵得出 ,  像素的y轴并没有发生变化 ,  而x轴承线性函数 $x` = x + ay$ .



### 四. 二维图像的Rotate旋转变换 :

**1. 三角函数基础( $θ$ 代表角度) :**
$$
cosθ 
=
\frac{邻边}{斜边};    

sinθ = 
\frac{对边}{斜边};

tanθ = 
\frac{对边}{邻边};
$$
$$
secθ = \frac{斜边}{邻边};
cscθ = \frac{斜边}{对边};
cotθ = \frac{邻边}{对边};
$$

**1. Rotate Matrix 旋转矩阵公式( $θ$ 代表角度) :**
$$
R_θ 
=
\begin{bmatrix}
cosθ&-sinθ\\
sinθ&cosθ
\end{bmatrix}
$$

**2. 旋转公式的推导过程 :**

- 假设一个边长为一的正方形绕原点旋转$θ$度 ,  则根据三角函数定理 ,  右下角顶点的坐标为$(cosθ, sinθ)$ , 左上角的顶点坐标为 $(-sinθ, cosθ)$ , 因此得到旋转矩阵 .

>注意 ,  逆时针旋转为正数角度 ,  反之为负数角度 .

>旋转必须围绕着原点进行旋转 .


**3. 顺时针旋转 $-θ$ 度 :**

- 已知 ,  $cos -θ = cos θ$ 因此 ,  在主对角线的元素数值不变 .  而 $sin -θ = -sin θ$ 因此 ,  得出公式 :
$$
R_{-θ }
=
\begin{bmatrix}
cosθ&sinθ\\
-sinθ&cosθ
\end{bmatrix}
=
R^T
$$
- 从而得出性质 :  
$$
R_{-θ}\;=\;R_θ^{-1}\;=\;R^T
$$
- 因此 ,  如果要求 $R$ 的逆矩阵 ,  只需要求 $R$ 的转置矩阵 .  在数学上 ,  这种矩阵被称为**正交矩阵**.


$$

$$

### 五. 线性变换的概念 :

- 线性变换的基本形式 :
$$
\begin{align*}
x` = ax + by \\
y` = cx + dy
\end{align*}
$$

- 矩阵的形式 :
$$
\begin{bmatrix}
x` \\
y`
\end{bmatrix}
=
\begin{bmatrix}
a&b \\
c&d
\end{bmatrix}

\begin{bmatrix}
x \\
y
\end{bmatrix}
$$

- 总结 :  使用一个方阵乘以一个矢量获得新矢量的形式 ,  我们统称为他**线性变换**


### 六. Translation齐次坐标 :

**1. 图像的平移变换 :**

- 解释 : 
	- 图像的平移变换很简单 ,  只需要对应分量作加法即可 :
$$
\begin{align*}
x` = x + t_x \\
y` = y + t_y
\end{align*}
$$
	- 但问题为 :  这个简单的变换却无法直接使用**线性变换**的形式直接表达 ,  需使用单位矩阵进行转换(此变换也被称之为仿射变换Affine Transformations):
$$
\begin{bmatrix}
x`\\
y`
\end{bmatrix}
=
\begin{bmatrix}
1&0\\
0&1
\end{bmatrix}
\begin{bmatrix}
x\\
y
\end{bmatrix}
+
\begin{bmatrix}
t_x\\
t_y
\end{bmatrix}
$$
	- 同时也证明 ,  平移变换不属于线性变换的范畴 .  但如果我们不希望把平移当成特殊情况去面对呢? 有什么办法把所有的变换用一个最简单的线性变换来表示呢 ?  解决办法就是引入齐次坐标 .

**2. 齐次坐标系 :**

- 给二维矢量添加第三个坐标 w ,  其中 ,  w 为 1 时表示point ,  w 为 0时表示vector :
	- $2D \; point = (x , y , 1)^T$
	- $2D\;vector = (x , y , 0)^T$

>很多人会有疑问 ,  为什么要把point 和 vector 区别对待呢 ?  因为向量没有位置 ,  移动向量不会改变向量本身 .  也就是说 :  向量具有平移不变性 .  所以 ,  将 vector 第三矢量改为 0 ,  目的是为了保护他的属性不被改变 .

>vector + vector = vector ; w = 0
>point - point = vector ; w = 0
>point + vector = point ; w = 1
>point + point = 这两个点的中点 .
>我们只观察他们的 w 分量 ,  会发现经过如此处理 ,  矢量的属性标记均为合理 .


- 将其使用矩阵表达 $(t_x,t_y表示移动的方向与距离)$  :
$$
\begin{bmatrix}
x`\\
y`\\
w`
\end{bmatrix}
=
\begin{bmatrix}
1&0&t_x\\
0&1&t_y\\
0&0&1
\end{bmatrix}
\begin{bmatrix}
x\\
y\\
1\\
\end{bmatrix}
=
\begin{bmatrix}
x + t_x\\
y + t_y\\
1
\end{bmatrix}
$$
- 也就是说 ,  只要我们引入齐次坐标 ,  就实现了将平移变换的**线性变换** 


**3. 齐次坐标系变换与结构表达 :**

- `Scale`缩放矩阵 $s_x\; , s_y\;$ 表示缩放倍率
$$
S(s_x\; , s_y\;)\;=\;
\begin{bmatrix}
s_x& 0& 0\\
0& s_y& 0\\
0& 0& 1
\end{bmatrix}
$$

- `Rotation`旋转矩阵 $α$ 表示旋转角度
$$
R(α)\;=\;
\begin{bmatrix}
cos\;α&-sin\;α&0\\
sin\;α&cos\;α&0\\
0&0&1
\end{bmatrix}
$$

- `Translation`平移矩阵 $t_x\;,t_y\;$表达平移方向与距离
$$
T(t_x\;,t_y\;)\;=
\begin{bmatrix}
1&0&t_x\\
0&1&t_y\\
0&0&1
\end{bmatrix}
$$

>注意 ,  只有在仿射变换时 ,  第三行的元素才为 0, 0, 1 ;

$$
\begin{bmatrix}
x`\\
y`\\
1
\end{bmatrix}
=
\begin{bmatrix}
a&b&t_x\\
c&d&t_y\\
0&0&1
\end{bmatrix}
\cdot
\begin{bmatrix}
x\\
y\\
1
\end{bmatrix}
$$
- 其中 ,  `a , b , c , d` 决定缩放与旋转 ,  $t_x\;,t_y$ 决定平移的单位 .
### 七. 逆变换 , 合成变换与分解变换 :

**1. 什么是逆变换 :**

- 将已经经过变换的图像退回至没变换时 .  
- 逆变换的数学意义为 :  乘以相应变换的逆矩阵 .

>注意 ,  矩阵的乘法不符合交换律 ,  仅符合结合律 .


**2. 多个变换的运行规律 :**

* 默认时 ,  矢量统一放在表达式的最右边 ,  并根据变换的过程从右到左依次排列 ,  如先旋转45° ,  再平移(1 , 0) 个单位 ,  则写成以下方式 :
$$
T_{(1\;,0)} \; * \; R_{45} \;* \;
\begin{bmatrix}
x\\
y\\
1
\end{bmatrix}
=
\begin{bmatrix}
1&0&1\\
0&1&0\\
0&0&1
\end{bmatrix}
\;
\begin{bmatrix}
cos\;45°&-sin\;45°&0\\
sin\;45°&cos\;45°&0\\
0&0&1
\end{bmatrix}
\;
\begin{bmatrix}
x\\
y\\
1
\end{bmatrix}
$$
>注意 ,  变换矩阵从右到左依次运算 .  

- 它的函数性质可以表示如下 :
$$
A_n(...A_2(A_1(x)))\;=\;A_n...A_2\;*\;A_1\;*
\begin{bmatrix}
x\\
y\\
1
\end{bmatrix}
$$
- 我们可以巧妙利用矩阵的乘法结合律进行更方便的运算 .  也就是说我们可以对其的所有变换矩阵进行集中运算 ,  最后得到一个 3x3 的矩阵 ,  再用此矩阵进行对矢量的变换 .  这个过程为**合成变换** 
- 当**合成变换**构成一个复杂矩阵时 ,  先运行左上角的线性变换 ,  还是右边的平移呢 ?  答案为先进行线性变换 ,  然后再平移 .   因此 ,  我们可以直接将上方的矩阵简化为以下矩阵 :
$$
T_{(1\;,0)} \; * \; R_{45} \;* \;
\begin{bmatrix}
x\\
y\\
1
\end{bmatrix}
=
\begin{bmatrix}
1&0&1\\
0&1&0\\
0&0&1
\end{bmatrix}
\;
\begin{bmatrix}
cos\;45°&-sin\;45°&0\\
sin\;45°&cos\;45°&0\\
0&0&1
\end{bmatrix}
\;
\begin{bmatrix}
x\\
y\\
1
\end{bmatrix}
=
\begin{bmatrix}
cos\;45°&-sin\;45°&1\\
sin\;45°&cos\;45°&0\\
0&0&1
\end{bmatrix}
\begin{bmatrix}
x\\
y\\
1
\end{bmatrix}
$$
- 使用简便的合成时需要注意 ,  合成矩阵的运算顺序为先线性变换 ,  再进行平移 .  因此简化时需要遵守图形的运行逻辑 ,  确保他们的运行逻辑统一 .

- 当然 ,  矩阵的变换不仅可以用来合成 ,  也可以用于**分解** .  举个例子 :  一个正方体的左下顶点$c$位于$(0.5,0.5)$,  我们如何让他沿着顶点$c$进行旋转呢 ?  (旋转矩阵仅在左下角顶点位于原点时生效 .)以下是运算过程 :
$$
T(c)\;\cdot R(α)\;\cdot T(-c)
$$
- 此公式的几何意义为 ,  先平移-c个单位 ,  将图像平移至原点 ,  再进行旋转 ,  之后再平移回去 .  这就是关于**变换的分解**的典型运用 .  

### 八. 3D Transformations

**1. 基础的3D Transformations :**

- 三维空间变换的原理与二维空间变换的数学原理一致 ,  因此三维空间变换在形式逻辑上与二维空间变换一致 .

**2. 三维变换的齐次坐标系 :**

- 与二维空间的齐次坐标系道理相同 ,  仅多了z轴轴向 .
	- $2D \; point = (x , y , z , 1)^T$
	- $2D\;vector = (x , y , z, 0)^T$

$$
\begin{bmatrix}
x`\\
y`\\
z`\\
1
\end{bmatrix}
=
\begin{bmatrix}
a&b&c&t_x\\
d&e&f&t_y\\
g&h&i&t_z\\
0&0&0&1
\end{bmatrix}
\cdot
\begin{bmatrix}
x\\
y\\
z\\
1
\end{bmatrix}
$$
- 在此仿射变换矩阵中 , `[a , i]` 决定了缩放与旋转 ,  `[t_x , t_z}`决定了平移单位 .

**3. 合成变换 :**

- 使用齐次坐标矩阵进行变换时 ,  合成变换的矩阵第四行永远为 :  0,0,0,1 ; 合成之后的`[t_x , t_z}`仍然决定了平移单位 .  而左上角的3x3取余则表达了顶点在三维空间中的线性变换 .

- 当**合成变换**构成一个复杂矩阵时 ,  先运行左上角的线性变换 ,  还是右边的平移呢 ?  答案为先进行线性变换 ,  然后再平移 .   




## Class Ⅳ .  Transform Two

### 一. 3D Transform  Matrix:

**1. Scale Transform Matrix :**
$$
S(s_x,\;s_y,\;s_z)\;
=\;
\begin{bmatrix}
s_x&0&0&0\\
0&s_y&0&0\\
0&0&s_z&0\\
0&0&0&1
\end{bmatrix}
$$
- 其中 ,  $S(s_x,\;s_y,\;s_z)$ 分别代表在各自轴位上的缩放关系 .


**2. Translation Matrix :**
$$
S(t_x,\;t_y,\;t_z)\;
=\;
\begin{bmatrix}
1&0&0&t_x\\
0&1&0&t_y\\
0&0&1&t_z\\
0&0&0&1
\end{bmatrix}
$$
- 其中 ,  $S(t_x,\;t_y,\;t_z)$ 分别代表在各自轴位上的平移关系 .


**3. Rotation around x-, y-, or z-axis :**

$$
R_y(α)\;=\;

\begin{bmatrix}
1&0&0&0\\
0&cos\;α&-sin\;α&0\\
0&sin\;α&cos\;α&0\\
0&0&0&1
\end{bmatrix}
$$

$$
R_z(α)\;=\;
\begin{bmatrix}
cos\;α&0&sin\;α&0\\
0&1&0&0\\
-sin\;α&0&cos\;α&0\\
0&0&0&1
\end{bmatrix}
$$

$$
R_x(α)\;=\;
\begin{bmatrix}
cos\;α&-sin\;α&0&0\\
sin\;α&cos\;α&0&0\\
0&0&1&0\\
0&0&0&1
\end{bmatrix}
$$

- 其中 ,  $R_x,R_y,R_z$ 分别代表以相应轴向为中心线的转动 .  所有复杂的旋转都可以视为分别绕 xyz 三轴旋转的组合 .


**4. 罗德里格斯旋转公式 :**

- 罗德里格斯旋转公式可以将任意轴的旋转总结为一个矩阵 .  原理为通过将复杂旋转分解为 xyz 三轴向的分别旋转得出来的结果 .
$$
R(n,α)=cos(α)I+(1-cos(α))nn^T+sin(α)
\begin{bmatrix}
0&-n_z&n_y\\
n_z&0&-n_z\\
-n_y&n_x&0
\end{bmatrix}
\}N(双矩阵)
$$
>$I$为单位矩阵 ,  $\}n$为伴随矩阵 ,  


- 其中 ,  n为旋转轴(矢量) ,  α为旋转角度 .  默认情况下 ,  n 轴的起点为原点 .


### 二. Viewing Transformation :

**1. View / Camera Transform 的概念 :**

- 首先 ,  思考如何拍一张照片 ? :
	- 找一个漂亮的风景将人聚集在此 .  (**Mode**l Transformation)
	- 为相机寻找一个好的拍摄角度 .  (**View** Transformation)
	- 拍照 ! (**Projection** Transformation 投影变换)

	- 如此 ,  我们便获得一张照片 .  我们将此过程称之为 MVP 变换 .

- `View Transformation`即是为相机寻找一个好的角度 ,  并为投影变换做准备 .  在寻找一个好的拍摄角度之后 ,  将相机与需要渲染的数据**相对静止**的进行变换 ,  

**2. 如何确定相机的摆放 :**

-  $\overline e$ :  `Position` 位置 
-  $\hat g$ :  `Look-at / gaze direction ` 看向哪? 角度
-  $\hat t$  :  `Up Direction` 向上方向

**3. 如何进行视图变换 ?**

- 将相机的位置视为处于标准位置 .  处于原点 ,  y轴向上 ,  看向-z轴 .  数学过程为 :
	- 平移至原点 .
	- 看向 -z 轴 .
	- 向上方向的矢量旋转至对齐 y轴 .

- 将此过程使用矩阵来表达 :
	- 假设一个矩阵 :  $M_{view}\;=\;R_{view}\;T_{view}$ 
	- 将位置转换至原点 : 
$$
T_{view}=
\begin{bmatrix}
1&0&0&-x_e\\
0&1&0&-y_e\\
0&0&1&-z_e\\
0&0&0&1
\end{bmatrix}
$$

	- 将旋转值旋转至正确角度
$$
R_{view}=
\begin{bmatrix}
x_{\hat g \times \hat t}&y_{\hat g \times \hat t}&z_{\hat g \times \hat t}&0\\
x_t&y_t&z_t&0\\
x_{-g}&y_{-g}&z_{-g}&0\\
0&0&0&1
\end{bmatrix}
$$
	- 使用此变换后 ,  相机便能转换至原点 .  **而视图变换操作相机 ,  其他数据跟随相机平移变换 , 保持相对静止 .  即可完成视图变换 .  此时的变换还维持至三维空间中 . **


### 三. Projection Transformation :

**1. 投影的两个变换种类 :**

- Orthographic Projection :  正交投影
	- 把Camera 理解为一个面 ,  形成方形体 .  将此方体内的所有物品投影至一锥体的横截面上 .
	- 将像素与Camera平面连接 , 连线穿过Camera的平面 ,  所穿过的交点即为像素的显示点

- Perspective Projection :  透视投影 .
	- 把Camera 理解为一个点 ,  形成四棱锥 .  将此锥内的所有物品投影至一锥体的横截面上 .
	- 将像素与Camera原点连接 , 连线穿过Camera的平面 ,  所穿过的交点即为像素的显示点

### 四. Orthographic Projection

**2. 在计算机上实现Orthographic Projection :**

- 1) 理论做法 :

	- Camera 放在原点 ,  看向-z ,  模型坐标的(0,0,1)指向Y轴方向 ;
	- Drop Z coordinate ;
	- 将数据 Translate and Scale 至原点为中心 ,  `[-1,1]` 的正方形平面上 .

- 2) 正式做法 :

	- 我们定义一个空间中的立方体 ,  `[l , r] [b , t] [f , n]`(右左 , 下上 , 远近); 在数值上 ,  `l < r ; b < t` 但与之相反的是`n < f`(原因为Camera 为看向 -z方向 .  Unreal 看向-x方向. );

	- 将此方体变换至 "canonical cube"(正则立方体)`[-1,1]^3`;

- 3) Orthographic Projection Transformation Matrix :

$$
M_{ortho}\;
=\;
\begin{bmatrix}
\frac{2}{r-l}&0&0&0\\
0&\frac{2}{t-b}&0&0\\
0&0&\frac{2}{n-f}&0\\
0&0&0&1
\end{bmatrix}
\;
\begin{bmatrix}
1&0&0&-\frac{r+l}{2}\\
0&1&0&-\frac{t+b}{2}\\
0&0&1&-\frac{n+f}{2}\\
0&0&0&1
\end{bmatrix}
$$
- 将中心移动至原点 ,  然后将XYZ的宽度变换为 2 .  

- 4) 注意点 :
	- 由于Camera look at -z ,  所以 (n > f ) ;  此原因也是OpenGL API 使用左手坐标系的原因 .
	- 暂时不考虑旋转变换 .

>正交透视变换默认将场景投射为正方体 ,  在屏幕空间变换时会将正方形变换为屏幕大小 .
### 五. Perspective Projection

**1. 回顾齐次坐标系 :**

- 在齐次坐标系中，一个点 `(x,y,z,w)` 的第四个分量 `w` 只要不为零，就可以表示一个顶点 .  具体来说：

	1. **w≠0**：此时点` (x,y,z,w)` 可以转换为三维空间中的点 $(\frac{x}{w},\frac{y}{w},\frac{z}{w})$因此，w 不为零时，该齐次坐标代表一个有效的顶点。
    
	2. **w=1**：这是最常见的情况，因为此时齐次坐标`(x,y,z,1)` 直接对应三维空间中的点 `(x,y,z)`，计算更简便 .

**2. 透视投影与正交投影的关系 :**

- 透视投影的几何概念 :  从一个点开始 ,  向远处的平面延伸出为一个四棱锥 Frustum.  其中地面f大于相机面n.

- 正交投影的几何概念 ：一个 Cuboid .
- 那么 ,  透视投影的远平面 ,  缩小至与近平面相等大小 ,  转换为正交投影 ,  这样 ,  即可更简单的将投影投影至进平面 .

**3. 透视投影转化为正交投影 :**

- camera视角有近平面 n ,  远平面 f .  透视投影下 ,  f > n .  我们只需要将 f  进行缩放 ,  缩放至与 n 的大小相当 ,  即可转化为正交投影 .

- 在缩放的过程中 ,  有以下特性 :
	- 近平面 n 保持不变 .  
	- 远平面 f 的中点不受缩放影响 .

**4. Camera 渲染范围内所有顶点的 $M_{persp->ortho}$ 透视->正交的计算矩阵 .
$$
M_{perp->ortho}\;
=\;
\begin{bmatrix}
n&0&0&0\\
0&n&0&0\\
0&0&n+f&-nf\\
0&0&1&0
\end{bmatrix}
$$
>n 是顶点的-z 轴坐标 (Unreal的-x轴) , f 是 Camera 的远平面中心点z轴坐标 .


## Class Ⅴ .  Rasterization One

#### 一. 光栅化的概念 :

- 当场景中的视角通过变换 ,  处于正交投影 ,  我们该如何把他绘制在屏幕上 ?  这就是光栅化的作用 .

#### **二. 如何定义Perspective Projection的field(视椎) ?**

- 1) 宽高比 width , height .
- 2) Vertical Field of View :  垂直视野 ,  相机垂直于 `width` 的边的夹角 .  简称 fov .

>知道宽高比 , 垂直视野 ,  可求出水平视野 .


**1. $fov$求解公式 :

- Camera 的原点与近平面的距离为$n$ ,  近平面的高度一半( z轴坐标的绝对值 )定义为$t$ ,  则$fov$等于 :

$$
\tan\frac{fovY}{2} = \frac{t}{abs(n)}
$$


#### 三. Screen and Screen Space

**1. Screen 的概念 :**

- Screen 在计算机中实际上是二维数组 ,  按像素有序排列 .  K则表示像素的多少 .  而屏幕也是典型的光栅成像设备 .  

- Raster = Screen in German ,  Raster 光栅化是德语的屏幕意思 .
	- Rasterize == Drawing onto the Screen .

- Pixel ( 简写 ,  short for "Picture element" )
	- Color is a mixture of (red , green , blue)

**2. Screen Space :**

- 屏幕的左下角为屏幕的原点 ,  像素的Index均写成(x , y) 注意 ,  0为第一位 .
- 对于每个像素的中心 ,  应统一为 (x + 0.5 , y + 0.5) ,  也就是像素的右上角上 .
- 我们认为屏幕像素的坐标应该为 (0 , 0) to (width , height) .


**3. Canonical Cube to Screen :**

- 将标准立方体变换至匹配Screen :
$$
M_{viewport}\;=\;
\begin{bmatrix}
\frac{width}{2}&0&0&\frac{width}{2}\\
0&\frac{height}{2}&0&\frac{height}{2}\\
0&0&1&0\\
0&0&0&1
\end{bmatrix}
$$
- 缩放正确的倍率后 ,  将中心移动至原点 .  此变换矩阵z轴不参与运算 .

**4. 现代显示器的运行原理 :**

- 通过 内存/显存 里特定的区域 ,  将其映射到屏幕上 .  这是现代显示器常见的显示方式 .  


#### 四. 光栅化的过程

**1. 光栅化的过程 :**

- 将三维空间中的数据投射至屏幕上 .  也就是把多边形拆成像素并变换至屏幕空间 .

**2. 为何三角形是图形中的基本图元?**

- 任何多边形均可以解构为三角形 .
- 三角形为二维图形 .
- 给出三个点的属性 ,  形成的三角形内的任何一个像素的属性均可求 .  ( 在三角形内部如何插值 )


**3. 在屏幕上给出一个三角形 ,  求被三角形覆盖的像素 :**

- A Simple Approach :  Sampling ,  什么是采样 ?  采样是一个Function ,  在不同的地方问函数值是多少 ?  这个过程被称之为采样 .  采样就是把一个函数离散化的过程 .  

- 屏幕空间采样就是 :  有一个三角形 ,  判断屏幕每一个像素的中心是否在三角形内 .

**4. 屏幕空间采样的实现过程 :**

- 给定一个三角形 t ,  给定像素的坐标 x , y .  

```伪代码
inside(t, x, y) = 
{
1 像素(x, y) 在三角形t 内;

0 不在三角形t 内 
}
```


- 遍历屏幕的像素 ,  假设一共有xmax * ymax个像素 .

```c++
for (int x = 0; x < xmax; ++x)
{
	for(int y = 0; y < ymax; ++y)
	{
		image[x][y] = inside(tri, x + 0.5, y + 0.5);
		// x + 0.5, y + 0.5的意义为像素的中心.
	}
}
```

- inside 实现的算法逻辑 :

	- 假设有三角形 $P_1,P_2,P_3$ 和点 Q ;
	- 建立矢量 :  $(P_1, P_2)$  与 $(P_1,Q)$ 并做叉乘运算 ,  此结果可以得出Q在矢量的左侧或右侧 .  如果叉乘的结果Z值为正 ,  则Q在左侧 .    
	- 以此类推 ,  只要点Q均在三角形三条边 ( 三个点的排列必须依次排列, 逆时针或顺时针 ) 的同一侧 .
	- 如果点恰巧在边界上 ,  要么不作处理 ,  要么特殊处理 .


**5. 常见的光栅化优化方法 :**

- 绘制一个三角形不需要检测屏幕的所有像素 ,  因此 ,  我们需要构建一个三角形的包围盒 .  这个包围盒的名字为 :  ais aligned bounding box ,  缩写为AABB .

- Incermental Triangle Traversal 增量三角形遍历 .  用更复杂的算法检测三角形覆盖的像素 ,  减少像素的inside次数 .

>人眼对绿色更为敏感 ,  因此有些光栅化设备的绿色像素组成会更多一些 ,  同理 ,  虚幻引擎对纹理的绿色通道也有更好的支持 .


## Class Ⅵ .  Rasterization Two

### 一. Antialiasing 抗锯齿/反走样 

**1. Aliasing 锯齿 :**

- 采样是图形学中广泛的做法 ,  采样所带来的问题也是广泛存在的 ,  我们称呼此类的问题为 :  Sampling Artifacts .  采样失真/混淆/混影/伪像 .
- 典型的Artifact 有 :  摩尔纹 ,  锯齿 ,  车轮效应 .  
- 问题的本质为 :  信号的变化过快 ,  以至于采样的速度跟不上变化的速度 .

**2. Antialiasing 的一些方法 :**

- 先模糊需要采样的信号 ,  再对模糊过的信号进行采样 .  
- 模糊的方法 :  使用一定大小的低通滤波器 ,  对其进行卷积即可 .
- 对于任何像素 

## Class Ⅶ .  Shading One

