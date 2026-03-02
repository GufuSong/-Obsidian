**节点参数补充 :**

- Base Parameter 基础属性 :
	- **Tilling Mode_关系 :**  
		- `Relative to Input` :  相对于输入 .
		- `Relative to Parent` :  相对于父项 .
		- `Absolute` :  绝对 .

	- **Tilling Mode_平铺模式 :**
		- `No Tilling` :  无平铺 .
		- `Horizontal Tilling` :  水平平铺 .
		- `Vartical Tilling` :  垂直平铺 .
		- `H and V Tilling` :  H V 平铺 .

# Substance 3D Designer

## Ⅰ. 形状处理

### Level 2. 图像处理

#### ==模糊 :==

##### ==`Blur`全局模糊==

##### ==`Blur HQ Grayscale`高清模糊==

##### ==`Bevel`倒角模糊==

##### ==`Non Uniform Blur Grayscale`非均匀灰度模糊==

##### ==`Pow GrayScale`颜色幂运算==

- 作用 :
	- 调整灰度图像对比度和亮度分布 ,  其核心功能是通过幂函数（Power-law）对输入的灰度值进行非线性变换 .

##### `Slope Blur Color` 坡度模糊
>用一张高度图的“坡度方向”，去推动彩色图像发生方向性的局部模糊 / 侵蚀 / 扩散。
>可以使用高度图制作具有方向的模糊效果 .


##### `non uniform blur grayscale`不均匀模糊
>对灰度图像进行“模糊强度在空间中不一致的模糊处理”，由另一张灰度图控制每个区域的模糊程度。

- `Anisotropy` 各向异性
	- 当 Anisotropy 值为 **0** 时，它退化为一种类似均匀的模糊（没有方向性）。
	- 当 Anisotropy 提升接近 **1** 时，模糊将更明显地沿着由 **Angle** 设置的方向扩展，而横向方向的模糊减少。
- `Asymmetry` 不对称性
	- Asymmetry 将使得模糊在某一方向上的采样权重偏重，产生 **偏向一侧的模糊效果**。

##### `radial blur grayscale`模糊式旋转
>沿着某一个点模糊旋转 .


---

####  ==扭曲与形变==

##### `Warp`扭曲
>暗的像素会向量的像素的位置偏移
>如果强度为正数则算法为暗部向量不偏移暗部的位置保持不变所以整体会变暗若强度为负数则亮的部分会像暗的位置偏移亮度不变整体会变亮 .


##### `non uniform directional warp color`非均匀方向性偏移颜色
>**利用方向图，让彩色纹理在局部以不同方向、不同强度产生自然流动与扭曲的高级 Warp 节点**，非常适合做 **风、流体、侵蚀、能量类效果**。

- `Input` 输入源图片 .
- `Intensity` 强度 .
- `Direct` 方向 .
>`Trail Fade` :  可以将纹理朝向其他方向推进 .

##### `vecotr Warp `矢量扭转

##### `Multi Directional warp grayscale`多方向扭曲
>**对灰度图像进行“多方向叠加式扭曲”的形变节点**，核心目标是**打破程序纹理的规律性，让结果更自然、更随机**。
>可以用于挤出高光 .

##### `Vector morph`法线矢量形变
>输入法线进行像素偏移 .

##### `Trapezoid transfrom`梯形形变
>把输入的图像向梯形变换 .


##### ==`Directional Warp`定向扭曲==

**一. 作用 :**

- 基于方向控制的动态扭曲工具，能够根据输入的灰度图或方向图对目标纹理进行沿特定方向的变形处理。

**二. 参数细节 :**

- **灰度驱动强度**：利用灰度图的亮度值控制扭曲强度，0（黑色）表示无扭曲，1（白色）表示最大强度，中间值按比例过渡 .
- **动态控制**：支持与其他节点（如噪波、渐变）联动，实现复杂的方向变化和强度调整 .
- **方向性扭曲**：通过定义方向（如角度或矢量）对输入图像进行拉伸、压缩或偏移，适用于模拟自然流动、动效果或纹理的定向变形


##### ==`Transform 2D`2D变换==



#### ==明暗与色调==

##### ==`Curve`曲线==
>curve 曲线处理 .

##### ==`Gradient Map`渐变映射==

>可以选择输入的类似 ,  并将其重新映射为新的数据 .

##### `Auto Level`自动调色


#####  `Levels`色阶直方图


#####  `Gradient Map`渐变映射

##### `glow grayscale`灰部发光
>对灰度图的亮部做向外扩散的发光式模糊，用于强调高亮区域与边缘过渡。


##### `histogram scan`直方图扫描
>从一张灰度图中，精确“抠出某一段明度区间


##### `Inver Grayscale` 反相

>颜色反转 ,  1 - .


##### `Grayscale Conversion`灰度转换

>将输入的多通道纹理转换为单通道纹理


##### `Histogram Select`直方图色阶
>颜色重新映射

**一. 作用 :**

**1. 逻辑 :**
- 计算规定范围内的数值并将这些数值重新映射 . 类似线性插值 .

**2. 数学逻辑 :**
- 输入参数`Position` ,  根据参数`Range` 得到`[Position - range, Position + Range]`的值域 .  选出这些像素并依据参数`Contrast` 对这些像素进行重新赋值, 赋值值域为`[1, Contrast]` .


##### `Histogram Scan`直方图扫描
>获取图像上某颜色的范围 .


##### `Slope Blur Grayscale`斜率模糊

>类似偏移采样 .  根据“坡度图”的方向，把采样点推向某个方向再平均
>能够获得手指擦拭的效果 .


##### `Vector Warp grayscale`向量场扭曲

**一. 核心功能**

1. **基于向量场扭曲灰度图**
    
    - 该节点使用 **红色（R）和绿色（G）通道** 作为向量场的 **X 和 Y 方向位移**，对输入的灰度图进行像素偏移（类似 UV 扭曲）。
        
    - **蓝色（B）通道通常被忽略**（因为灰度图只需要 2D 向量场）。
        
2. **应用场景**
    
    - 创建 **流体效果**（如水流、熔岩的流动痕迹）。
        
    - 模拟 **自然侵蚀**（如风蚀、水蚀对表面的影响）。
        
    - 生成 **有机纹理**（如扭曲的血管、裂纹蔓延）。
        
    - 与其他节点（如 **Directional Warp**、**Slope Blur**）结合，增强动态效果。

##### `Mirror Color` 镜像


---

#### ==图像生成==



##### ==`Carresian to polar grayscale`笛卡尔变换至极坐标==

- 作用 :
	- 极坐标变换 ,  扭曲图案至圆形 .


##### **`blend`运算符混合**

**1. 参数列表 :**
- 属性列表
	- `Blending Mods`
		- Copy :  只显示前景层 ,  常用于前景层的Alpha 通道叠加 .

>Opacity 输入 Foreground 的透明度 .

**2. 使用须知 :**
- 注意接口排序的运算律 .
- 不透明度仅影响Fore .

##### `start blush`光斑生成
>常用于光斑制作

##### 1.3 `Gradient Linear`UV线性渐变

##### 1.2 `Shape`生成基础图案

##### 1.1 `Uniform Color`纯颜色

##### `cross section` 获取贴图横截面


##### `rt caustics`实时焦散
>光线经过反射或折射后，在表面或体积中形成的高亮能量聚集图案。

##### `make it tile patch`制造平铺纹理
>把一张“不可平铺（Non-Tileable）的局部图像”，转换成“可无缝平铺（Tileable）的纹理块（Patch）”


##### ==`Edge Detect`查找边缘==
>输入图像 , 输出像素不同的边缘 .

##### ==**`Tile Sample`平铺采样**==

**一. 作用 :**

**1. 使用场景 :**
- 将图案缩小并平铺在图案上 .

**2. 参数列表 :**
- Instance Parameters :
	- **Pattern :**
		- `Pattern` :  选择每个Cell的图案 .  Pattern Input 为输入接口端图案 .
	- **Size :**
		- `Size Mode` :  缩放模式 .
		- `Size` :  调整X , Y轴的缩放 .
		- `Size Random` :  随机缩放 .
		- `Scale` :  大小的整体倍率 .
		- `Scale Random` :  随机倍率 .
	- **Color :**
		- `Blending Mode` :  混合模式 .  在支持Color平铺的前提下支持Alpha混合 .
		- `Color Random` :  颜色随机 ,  可以调整不同类型颜色的随机值 .
	- **Position :**
		- `Position Random` :  随机位置 .
		- `Offset` :  偏移 .  隔行偏移 .

**3. 使用须知 :**
- 使用时每个格子均视为一个Cell ,  每个Cell会按照从左到右, 从上至下一次赋予一个ID序号 ,  此ID序号 .  在输入数据有Alpha的情况下 ,  会根据此ID序号的大小决定遮挡关系 ,  ID大的图层会遮挡ID小的图层 .


##### ==`Mosaic Grayscale` 灰度图马赛克==

**一. 作用 :**

- 将灰度图像分割为规则或随机的马赛克（块状）图案的工具。它的核心功能是通过像素化或几何化的方式重构输入图像，生成类似瓷砖、像素块或蜂窝结构的灰度效果。



##### ==`Distance`距离==

>输出黑色像素到白色像素到距离 ,  可以用于灰部渐变 .

**一. 节点作用 :**

**1. 功能 :**
- 输入一张 **二值图（Binary Mask）或灰度图**
- 计算每个像素到 **最近白色区域（或边缘）的距离**，并输出一张 **灰度图**（越亮表示距离越远）。

**2. 接口 :**
- `Source Input` :  这是 **主要输入**，用于定义 距离计算的基准区域
- `Mask Inout` :  这是一个 **可选输入**，用于 **限制距离计算的范围**（仅在被遮罩覆盖的区域生效）。

##### ==`Bevel`倒角或斜面==

>输入一张 **高度图（Height Map）** 或 **二值遮罩（Binary Mask）**，Bevel 会检测边缘并生成斜面过渡。
>可以返回常量的高度信息或方向矢量信息 .

##### ==Text==
>生成文字


#### 三. Flood 区域检测

##### ==`Flood Fill`区域填充==

**一. 节点作用 :**

**1. 功能 :**
- 输入一张高度图（Height）或遮罩（Mask）**，节点会检测 连续的像素区域**（根据高度或颜色相似性）。
- 对检测到的区域进行 **标记（Indexing）** 或 **填充（Filling）**，生成新的遮罩或灰度图。
- 输出根据每个区域形成的包围盒 ,  依据此包围盒输出每个像素在包围盒内的归一化坐标(左上角为原点) .

##### ==`Flood Fill to gradient`==

**一. 节点作用 :**

**1. 功能 :**
- 输入Flood Fill 信息 ,  生成依据`Flood Fill` 信息的平滑渐变常数值 .  可以达到自然过渡的材质效果 .

##### ==`Flood Fill to Random Color`区域赋予随机颜色

**一. 节点作用 :**

**1. 功能 :**
- 输入Flood Fill 信息 ,  生成依据`Flood Fill` 信息的随机颜色块 .

##### ==`Flood Fill to Random GrayScale区域赋予随机颜色

**一. 节点作用 :**

**1. 功能 :**
- 输入Flood Fill 信息 ,  生成依据`Flood Fill` 信息的随机灰度块 .


### Level 3. 数据处理

#### 一. 通道处理 :

###### `RGBA Merge` RGBA 合成
>合成RGBA的通道 .



##### 3.2 **`Ambient Occlusion(HBAO)`转换AO**

- 作用 :
	- 可以调整生成AO贴图的质量 .Quality

#### 二. 法线处理 :

##### ==`Normal`转换法线数据==

##### ==`Height to Normal World Uint`高度图转换法线==

##### ==`Normal Combine`法线合并==