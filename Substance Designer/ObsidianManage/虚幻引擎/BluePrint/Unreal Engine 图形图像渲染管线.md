# Unreal BluePrint 渲染管线的实现

* 本套笔记基于Unreal Engine 5中的可视化编程工具, 实现软渲染流水线的实现. 
* 本笔记为理论实现知识, 无实现具体工业流程的方法论, 更多是关于虚幻引擎底层渲染逻辑的梳理以及世界观.
* 本笔记可以拓宽日常创作的思路, 以及了解计算机底层实现逻辑等. 

**注意: 以下是使用到的蓝图都详解**

[^Event Receive Draw HUD]: 事件调用;
[^Draw Rect]: 像素画点函数;









# ==第一部分: 基础认知补充==

# 通识*第一课: 基础物理_机械波

* 认识机械波
* 波的形成
* 认识波速, 波长, 频率三者的关系

---

## 1.1 机械波

* 机械波分为: 
  * 横波( 光波 / 电磁波 )
    * 波的传播方向与质点的运动方向为上下运动, 垂直运行;
    * 横波看起来来像 三角函数, 最高的点被称为波峰, 最低点被称为波谷. 
    * 波峰与波谷的间隙被称为波长
  * 纵波( 声波 )
    * 波的传播方向与质点的振动方向是相同的
      * 纵波因为传播方向与质点的振动方向是相同的, 因此分为 密部 与 疏部 .
      * 密部与疏部之间的间隙被称为波长.

**一. 波速 :**

* 波速 = 波长 / 周期[^01]; 距离 / 时间
  * 波速由介质决定

[^01]: 波峰与波谷长度的和, 被称为周期; 波长的长度等于一个周期(T的单位是s, 是时间单位). 周期的定义为波前进一个波长所用的时间.

**二. 频率 :**

* T是一个周期用了多少秒, 频率f 是1秒多少个周期T, 因此: f = 1 / T;
  * **定义 :** 一秒经历了几个周期.
* 波速固定, 介质不变. 波长由频率决定, 频率越大, 波长越小. 频率越小,波长越大.

---

## 1.2 机械波的干涉

* 频率相同的两列波叠加, 是某些区域的振动加强, 某些区域的震动减弱, 而且站震动加强的区域和振动减弱点区域相互分开.
* 波峰和波峰(波谷和波谷)相遇是叠加最强的, 波峰和波谷相遇是叠加最弱的.

---

## 1.3 机械波的衍射

* 波的衍射是指在媒质中由于有障碍物或其他的不连续性而引起波改变传播方向的现象。如障碍物的尺寸[远大于](https://baike.baidu.com/item/远大于/7256186?fromModule=lemma_inlink)波长，则衍射不明显;如障碍物的尺寸与波长相近时，则衍射最明显;如障碍物的尺寸远小于波长时，虽然还有衍射,但是在障碍物背部边缘附近将形成一个没有波的区域(即[声影区](https://baike.baidu.com/item/声影区/5421363?fromModule=lemma_inlink))。
* 惠更斯原理帮助我们
* 理解了波为什么会大声衍射现象;

---

## 1.4 光与波粒二象性

* 光是一种电磁波, 光具有波粒二象性;
* 光具有波粒二象性: 
  * 波(用波来解释): 干涉和衍射
  * 光子(用粒子来解释): 光电效应, 一个光子想象为带有能量包的粒子.

---

## 1.5 颜色是怎么形成的

* 光的色散
  * 白光通过棱镜后, 被分解成各种颜色的光的现象
  * 色带(光谱)的顺序是: 红橙黄绿蓝靛紫;
  * 光的色散现象表明: 白色是由各种颜色形成的.
* 可见光的频率和能量:
  * 波长(频率)可见光的能量不一样, 在100到700nm范围内的光可以被人来的视网膜感知到.
  * 光子能量公式: E = h / f (h = 6.63^10 ^ -14 J * S)
* 颜色的本质:
  * 颜色的本质是频率
* 原色:
  * 光学三原色(加色): RPG红, 绿, 蓝 (加色模式, 越加越白, 越加越亮)
    * Red: [1, 0, 0]
    * Green: [0, 1, 0]
    * Blue: [0, 0, 1]
  * (减色):CMYK 青色(Cyan), 洋红色(Magenta), 黄色, 黑色(颜色越加越黑)
    * Cyan: [0, 1, 1]
    * Mageta: [1, 0, 1]
    * Yellow: [1, 1, 0]
  * 人类绘画艺术:HSB(HSV)
    * H: Hue色相
    * S: Saturation饱和度
    * V: Brightness 或 Value 明度
    * 本质是RGB颜色立方体 视角下 六个角的颜色;
      * 蓝[0, 0, 1]
      * 品红[1, 0, 1]
      * 红[1, 0, 0]
      * 黄[1, 1, 0]
      * 绿[0, 1, 0]
      * 1青[0, 1, 1]![3](F:\Desktop\技能\计算机编程语言\编程语言\C++\UnrealEngineCplusplus\Maps\luePrint\3.png)

**解释 :** 上方图面为颜色三维模型: 往中间去 代表饱和度, 中心为白色; 往下去是明度变化.

* HSL颜色模型: 与HSB原理相同,数值不同
  * H: Hub色相
  * S: Saturation: 饱和度
  * L: Lightness或Intensity亮度 ![4](F:\Desktop\技能\计算机编程语言\编程语言\C++\UnrealEngineCplusplus\Maps\luePrint\4.png)
  * SHV只有下面的圆锥, HSL颜色更多, 有明度[1,0]的所有颜色
  * 比较不同的点: 亮度最高时, 会变成白色. 亮度最低时,会变成黑色.只有亮度为0.5时, 饱和度才会达到最鲜艳. 饱和度为0 时,颜色为灰色

---

# 通识*第二课: 屏幕与图片

## 1.1 像素和子像素

* 手机屏幕和电脑屏幕均由像素(Pixel)组成, 是一个一个的小块.
* 像素由子像素(亚像素Sub - Pixel)组成. 子像素由三个不同颜色发光的晶格(红绿蓝)顺序排列组成.

---

## 1.2 纹素(二维纹理)

* 纹素(Texel , 全名texturelement 或 texture pixrl的合成字) 是纹理元素的简称, 特使计算机图形纹理空间中的基本单元. 
* 纹理具有多维纹理

---

## 1.3 体素(三维纹理)

* 体素是体积元素(Volume Pixel), 三维纹理是实心的,每一个坐标单位都有对应的数据. 体素主要运用于(医疗,拍片等), 具有内部数据

---

## 1.4 图片

* 图片是一种统称. 总体上分为点阵图 和 矢量图.
* 位图, 是图像. 所包含的信息是由像素来度量的. 使用分辨率与色彩位数区分质量.
* 图形, 矢量图. 根据几何特性来绘制. 

---

## 1.5 图形与图像的关系

* 图形显示到显示器上变为图像.
* 图形中有图像的部分, 图形中的纹理就是图像.
* 纹理(一维纹理, 二维纹理, 三维纹理)

---

## 1.6 纹理

* 纹理分为: 一维纹理, 二维纹理, 三维纹理

**体积材质 :**

* 体积云就是一种三维纹理, 把体积云切成面包片一样, 里面也有东西.

# 第三课: 帧缓冲区 视觉暂留 双缓冲 帧率

## 1.1 帧缓冲区

* 一帧所需要的数据, 包含多个缓冲区, 如颜色缓存, 深度缓存, 模板缓存等等. 如下是延迟渲染模式一帧所包含的缓存.
  * 一个支持(OpenGL)渲染的窗口,可能包含一下组合:
    * 至多4个颜色缓存
    * 一个深度缓存
    * 一个模板缓存
    * 一个积累缓存
    * 一个多重采样缓存
  * Unreal Engine中的缓存窗口:
    * Base Color
    * Specular
    * Suburface Color
    * world Normal
    * 等12种

**注意 :**

* 帧缓冲必须 规格 大小统一

---

## 1.2 视觉暂留

* 光残留于视网膜的现象.

---

## 1.3 光栅扫描

* 逐行扫描: 速度慢但是画质稳定
* 隔行扫描: 速度快, 画质不稳定
  * 原理:显示设备表示运动图像的方法，每一站被分割为奇偶两场图像交替显示．

---

## 1.4 为什么要采用双缓冲?

* 单缓冲区会导致画面闪烁, 因为绘制新的画面时需要清空缓冲

**双重缓冲区原理 :**

* 前台缓冲: Front Buffer 当前显示在屏幕上的缓冲区颜色
* 后台缓冲: Back Buffer 在后台长在填充的缓冲区颜色, 填充完毕后会和前台缓冲区交换并显示在你的屏幕上
* 有效防止屏幕闪烁的问题. 操作系统自行调控此功能

---

## 1.5 帧率

* 位图显示在屏幕上的频率, 简称为FPS;

---

# 第四课: 渲染管线

---

## 1. 顶点数据

**一. 顶点内储存数据 :**

* 顶点世界坐标, 顶点在模型原点坐标系里的坐标.  注意: 不是世界坐标系, 是轴点的坐标系.
* 顶点颜色: 顶点上储存的颜色数据.
* 顶点切线: 
* 顶点法线: 顶点法线垂直于面法线
* UV坐标: 顶点在UV上的坐标

---

## 2. 认识图元

* 点: Points
* 线: Lines
* 线带: Line_Strip
* 线循环: Line_Loop
* 三角形: Triangles
* 三角形带: Triangle_Strip
* 三角形扇: Triangle_Fan

**概念 :**

* 几何着色器的操作单位是图元 ( 增加 , 删除 )

---

## 3. 渲染管线

**定义 :**

* 在计算机中, 将3D模型转化为屏幕上的图像需要经过一系列的处理步骤, 这个处理步骤就是图形流水线.

**一. 渲染基本流程 :**

* `数字资产创建阶段->引擎资产调整阶段->CPU处理->GPU处理`

![1](F:\Desktop\技能\计算机编程语言\编程语言\C++\UnrealEngineCplusplus\Maps\luePrint\1.png)

---

## 4. Early - Z 技术

* 减少机能消耗, 提前进行深度计算.

---

## 5. 简述学科区别

* 计算机图形学: 三维模型-> 绘制 -> 二维图像  (三维渲染(标准渲染, PBR, 光线追踪))
* 数字图像处理: 二维图像->分析处理->得到其他信息或生成某种特效... (PS里 图像处理滤镜, 混合模式, 边缘检测, 模糊, 后期处理)
* 计算机视觉: 二维图像->分析处理->三维信息 (根据图片生成三维场景, 如计算机视觉, 或OpenCV)

# ==第二部分: 使用BluePrint实现顶点绘制==

**本部分使用BluePring用法:**

* Event Receive Draw HUD[^Event Receive Draw HUD]
* Draw Rect[^Darw Rect]

# 第五课: 使用蓝图实现点的绘制

**一. Event Receive Draw HUD事件 :**

* 与Draw Rect 绘制函数配套使用

**实现过程 :**

* 使用蓝图在二维坐标系上绘制一个点.
* 自定义结构体: 1.坐标信息;  2.顶点颜色
* 循环结构体数组, 调用绘制函数;

![2](F:\Desktop\技能\计算机编程语言\编程语言\C++\UnrealEngineCplusplus\Maps\luePrint\2.png)

---

# 第六课: 光线与材质表面的交互

**光线与材质表面交互公式 :**

* 光照颜色 乘以 物体表面所有颜色:
* 光照颜色[R, G, B] BaseColor[R, G, B] 

**解释 :**

* BaseColor 也被称为反射率, 分量中的数字代表各个颜色反射的程度.如[1.0.0],反射红色100%; 绿色0%; 蓝色0%;
* 如果BaseColoe: [1, 1, 1] 则为反射所有光, 呈现为白色.
* 数学意义为 : 矢量的相乘(矩阵的乘法)

---

# ==第三部分: 实现渲染管线中的线与线框绘制==

# 第七课: 绘制线条BresenhamDraw理论

**一. 画线算法的两个要求 :**　

* 效率高,  准确

**二. 画线的基础知识: 斜率**

* 日常生活中, 如何表示倾斜程度?  坡度比
* 定义: 通常把坡面的 垂直高度h 和 水平宽度的比 叫坡度比, 即tan a值(a为坡度与水平面夹角)

**三. 直线斜率的定义 :**

* 一条直线的倾斜角a 的正切值叫做这条直线的斜率, 通常用k表示, 即` k = tan a(a != 90)`.

**四. 斜率的计算 :**

* 假设有 `p1 [ x , y ], p2[ r , g ]` , `k = (y - g) / (x - r)` 
* 斜截式方程:` y = kx + b`  , `当x = 0时, y = b`;

**五. 屏幕画线理论 :**

* 数学意义上, 直线是光滑的. 但由于屏幕是由像素构成, 因此点亮线经过的像素是现代计算机画线的方法.
* 斜率大于一, 把每个店的x和y的位置交换, 经过计算出来的点, 再交换即可.
* 斜率小于一, 按照x轴隔单位依次计算即可.
* 斜率等于一, 同小于一计算.

**六. 解释 :**

* 举例: 有点`P[0,0]` 和 `P1 [8,3]` , 这条线的 `k = 3/8 = 0.375`;  则x轴每个单位的 对于y轴的长度L 为 0.375 = L / 1 ;  L = 0.375;
* x1 = 0; x2 = 0.375; x3 = 0.75; 0.75大于0.5(因为像素的大小为1, 点在像素中心, 点距离像素顶端距离为0.5), y向上跳一格; 当向上跳一格时, L值要-1;(几何意义为: 像素的中点到当前一次函数中, x点的值的距离.) 也就是, 0.75 -1 = -0.25; 此时距离到达像素中心还有0.25, -0.25+0.375 = 0.125, 0.125 + 0.375 = 0.5, 等于0.5 不向上跳格
* 规律: 斜率k 等于 L, L<0.5, 向上点亮Y, 随后L -1; 然后继续 + L, if L<0.5, 向上跳格 -1;一共执行 x次, x = x1 - x0;

**七. K >1 的特殊情况 :**

* K > 1 时, 使用同样的方法会造成断点问题.
* 可以将每个点的 分量交换, 使K<1;
* 将输出的每个激活的像素格的 分量 分别交换即可.

**八. 第二个点不在第一个点的右边 :**

* 交换两个点的位置即可.

**九. K的步进方向朝下的时候 :**

* 取k的绝对值;
* 若y0 < y1; 则步进为1
* 若相反, 则步进为 -1;

# 第八课: BresenhamDraw基本逻辑

**一. 流程 :**

* 判断斜率 k 是大于1 还是小于1, 如果k 大于1我们就把起点坐标的x和y交换位置, 中点坐标的x和y交换位置, 并记录;
* 判断起点坐标的x是否在终点坐标x的后边, 如果是就把起点坐标和终点坐标交换;
* Y方向的不仅判断是1 还是-1;
* X方向每次步进一个像素, 并累计计算d值(d的初始值为0), 如果累计d值>0.5, Y方向就步进一步, 并将d值减1.

```c++
//Bresenham画线算法
struct intV2
{
	int x;
	int y;
};
void SWAP_INT(int &a,int &b)
{
	int temp;
	temp = a;
	a = b;
	b = temp;
}
void drawLine(intV2 pt1, intV2 pt2)
{
	int disY = abs(pt2.y - pt1.y);
	int disX = abs(pt2.x - pt1.x);

	int xNow = pt1.x;	//起始点坐标
	int yNow = pt1.y;

	int stepX = pt1.x < pt2.x ? 1 : -1;	//步进长度
	int stepy = pt1.y < pt2.y ? 1 : -1;

	int sumStep = disY;
	bool useXStep = true;	//步进方向为x还是y;



	if (disX < disY) {	//如果disY 大于 disX, 则斜率大于一
		sumStep = disY;
		useXStep = false;
		SWAP_INT(disX, disY);
	}

	//初始化d值 (D1 与 D2 差的正负)
	float p = abs (1.0f * disY / disX);	std::cout << "debug_k:  " << p << std::endl;

	float L = 0;
	for (int i = 0; i < sumStep; i++) {
		std::cout << "[" << xNow << "," << yNow << "]" << std::endl;
		if (L > 0.5) {
			if (useXStep) {	//次步进方向;
				yNow += stepy;
				std::cout << "debug_步进:    " << "被调用" << std::endl;
			}
			else {
				xNow += stepX;
			}
			L -= 1;
		}
		if (useXStep) {	//主步进方向
			xNow += stepX;
		}
		else {
			yNow += stepy;
		}
		L = L + p;
		std::cout << "debug_L:   " << L << std::endl;
	}

}

void test01()
{
	struct intV2 Pt1;
	struct intV2 Pt2;
	Pt1.x = 0;
	Pt1.y = 0;
	Pt2.x = 8;
	Pt2.y = 3;

	drawLine(Pt1, Pt2);
}

int main()
{
	test01();
}
```



# 第九课. BresenhamDraw 画线算法的优化 

**一. 优化目标:**

* 整形运算快于浮点型运算, 因此需要针对浮点型运算进行优化(斜率的计算)

**二. 优化理论 :**

* 由于 k = dy / dx; dy = y1 - y0;  dx = x1 - x0;

```c++
  int dy = y1 - y0;
  int dx = x1 - x0;
  float k = 1.0f* dy / dx; //浮点型的运算
```

* 然后根据d 的值 决定步进 并循环

 ```c++
  for(int i = 0; i < sumStep; i++)
  {
      std::cout << "[" << xNow << "," << yNow << "]" << std::endl;
  		if (L > 0.5) {	//L > 0.5 <=> L - 0.5 > 0;
  			if (useXStep) {	//次步进方向;
  				yNow += stepy;
  				std::cout << "debug_步进:    " << "被调用" << std::endl;
  			}
  			else {
  				xNow += stepX;
  			}
  			L -= 1;
  		}
  		if (useXStep) {	//主步进方向
  			xNow += stepX;
  		}
  		else {
  			yNow += stepy;
  		}
  		L = L + p;
  		std::cout << "debug_L:   " << L << std::endl;
  	}
  }
  ```

* 将第四行代码中的`L > 0.5` 等价于为 `L - 0.5 > 0` 将 `L - 0.5` 看作为一个整体 `e` ; 又因为`L` 变量的初始值为`0` 因此` L  = 0` <=> `L - 0.5 = -0.5` ;得出 `e` = -0.5;

* 因此, 当变量`e`的默认值为 -0.5; 第四行代码可以更改为: `if(e > 0)` 

```c++
  float e = -0.5;
  if(e > 0) {...}
  ```

* 此时, 代码中的浮点型变量还剩: e, k;  以及`L + 斜率k`; 当`e`乘以 2 时, 结果为整形,;

* 而对于斜率`k` , `k = disY / disX;` <=> `k * disX = disY` 

* 因为 `e = -0.5` , `e = -0.5 * 2dx` , `e = -dx` 可以优化为整形, 但是会改变e 的数值;

```c++
  int e = x0 - x1; //e = -dx; 因为dx = (x1 - x0) 所以-dx = (x0 - x1)
  ```

* 因为 `if( e > 0) ` 由于e的数值发生改变, 要想保持不等式结果一致, 则需要乘以一个大于0的数字. 由于e 乘以的是dx,  如果dx大于0, 则不等式也大于0, 不等式结果不变.

* 关于步进算法的累加部分, `e = e + k` <=> 由于上方`e = -0.5 * 2dx`  因此要想保持等式一致, `k = k*2dx` 即可.由于`k = dy / dx` ; `k = dy / dx * 2dx` ; 得出`k = 2dy` 因此, 累加表达式更改为: `e = e + 2dy` 

* 当发生步进时, L 值 -1 (第12行代码), `L = L - 1` <=> `e = e - 2dx`  

* 最重, 就实现了全部优化

# 第十课: 网格模型的关键数据

**一. 顶点存储的信息 :**

* Vertices和顶点数组(VertexArray和VertexBuffer)

  * 顶点数组 或 顶点缓冲区 存储模型中所有的顶点
    * 顶点数组: 储存顶点的数组; 本质是一个 结构体数组
    * 顶点索引: 顶点的ID, 每个顶点有一个四维矢量构成. 计算机无法存储过多的矢量数据. 因此通过顶点索引来代表每个顶点. 顶点索引为int , 如 0. 1. 2. 3.等 本质是通过顶点数组的下标来获得数组数据.
    * 顶点索引 存储在 索引缓冲区中;
    * 顶点 与 顶点索引 的关系: 一个四边形由4个顶点构成, 由6个索引构成,.

  > 与常规的点 顶点 的区别不同. 一个四边形可以写为
  >
  > `Point arr[] = {v0, v1, v2, v3};`
  >
  > 由两个索引构成: 三角形[0, 1, 2] 和 三角形[0, 2, 3] 构成;
  >
  > 但是如果 按照传统的 点 顶点 理论来说:
  >
  > 两个三角形由: 
  >
  > v0[x,y,z];
  >
  > v1[x,y,z];
  >
  > v2[x,y,z];
  >
  > v3[x,y,z];
  >
  > 总共多占用了 3*4 - 6+4 = 2
  >
  > 对比索引算法, 传统算法 会多占用2单位的数据类型

  * 在Unreal里, 使用Vector容器储存顶点数据

* 顶点法线, 顶点切线与顶点负法线

  * 这三个元素互相垂直, 构成切线空间(局部坐标系)
  * 负法线数据没有存储在顶点里, 是靠 叉乘 算出来的

* UV信息和顶点颜色信息

**二. 图元类型 :**

* 顶点POINTS
* 线段LINES
* 线带[^01]LINE_STRIP
* 线循环LINE_LOOP
* 三角形TRIANGLES
* 三角形带TRIANGLE_STRIP
* 三角形扇TRIANGLE_FAN

[^01]: 首尾没有连接

**三. 三角面结构体:**

* TriangleFace

  * IncdexArray 数组, 由三个int 构成的顶点ID变量

* Modle结构体

  * Verts 存储所有的顶点数组(顶点组成的数组)
  * Faces 面数组, 存储过程三角面的面索引, 存储的数据类型为 TriangleFace.
  * ModelMatrix: 记录对象坐标系坐标的: 平移, 旋转, 缩放等数据

* ```c++
  struct TriangleFace
  {
      int IndexArray [];
  }
  struct Modle
  {
      Vector vector;
      TriangleFace Faces;
  }
  ```

**四. LOD 细节层级 :**

* LOD层级越低, 三角面越多; 反之, 越少.
* 本质是场景优化的方法

# 第十一课:线框模式渲染管线CPU运载部分

* 采用三角形图元进行绘制
* 给Modle结构体增加模型矩阵
* 线框模型渲染管线
* 加载模型的核心函数

**一. 渲染线框需要的渲染管线流程 :**

* CPU:
  * 1) 模型数据调用, 加载电脑内存( 本章内容 )
  * 2.设置渲染状态
  * 3.调用DrawCall
* GPU:
  * 顶点着色器(T&L, TransFormation变换, Light光照计算; 模型变换, 物体放进关卡里的过程: 物体空间=> 世界空间=> 相机空间(视图变换) )
  * 投影变换(正交投影: 近裁面与远裁面相等大小  透视投影: 近裁面小于远裁面; 近裁面的作用是, 物体到相机裁剪的部分; 远裁面是相机远处裁剪模型的距离)
    * 相机的宽高比(aspect Ratio) = 宽 / 高;作用是节省计算量(只需要知道宽高比, FOV, H(近裁面) , F(远裁面) );
  * 剪裁(将画框以外的物体裁剪掉, 将没有完全在画框里的物体的一半裁剪掉. 随后进行透视除法(除以W), 解释为:[x,y,z,w]; 将[x,y,z] 分量分别除以w, 分量的值会被限制在[-1,1];
  * 背面剔除(就是与面法线相反的面会被剔除)
  * 屏幕映射(将顶点映射在屏幕上)
  * 光栅化时画线(将屏幕上的顶点进行画线)
  * 写入帧缓冲-> 显示到屏幕上

**二. 核心函数: Get Section from static Mesh :**

**用法 :** 解析`StaticMesh` 数据类型的各个信息, 类似结构体的`break` 

**输入 :**

* In Mesh: 加载网格信息 获取结构体信息
* LOD: 细节层级设置
* Section Index: 模型区域顶点(根据材质球)选择性索引.

**输出 :**

* Ve: 顶点信息
* Tri: 三角形信息
* Nor: 法线信息
* UVs: UV信息
* Tang: 切线信息

**三. 使用BluePrint 对CPU处理过程进行实现思路 :**

**前置概念 :**

1) **虚幻 UStaticMesh 类的认知**:[知乎关于StaticMesh的读写和创建](https://zhuanlan.zhihu.com/p/599090384)

* Mesh(网格, 理解为模型) 的基本概念为: LOD 和 Section. LOD是模型根据不同的层级切换不同的形态, 越远模型越简陋. Section, 称为子网格或多维子材质. 就是一个材质球所包含的面. 如一个正方体有6个面, 一个面有6个顶点; 总共有36个顶点. 那么, 假如每个面都给一个材质球, 那么其中 材质ID 公有 0.1.2.3.4.5 , 其中, 每个ID公有六的顶点使用它.
* 此类还包括每个顶点的数据, 详细见上.
* StaticMesh 有 RenderData, RawMesh(RM), MeshDesCription(Desc)进行解析:
* 场景中最终渲染效果使用的CPU断数据就是RenderData中的数据; RenderData的调用结构:
  * 调用StaticMesh
  * 调用RenderData
  * 选择LOD层级
    * VertexBuffers 顶点缓冲数据:
      * PositionVertexBuffer(坐标)
      * StaticMeshVertexBuffer
        * UV
        * 法线切线
      * ColorVertexBuffer(顶点色)
    * IndexBuffer(三角索引)
    * Sections
  * 这就是这个类在CPU阶段所使用的储存结构

2) **虚幻Transform 数据类型**[Unreal Transformation](https://zhuanlan.zhihu.com/p/619109058#:~:text=Unreal Transformation )

* 可以跟矩阵类型相互转换, 本质是Gameplay层面上方便快捷的修改某个Actor的变换，FTransform是一个更高级别的矩阵类型，它包含三个矩阵：Rotation（旋转）、Translation（平移）和Scale3D（缩放）。这三个矩阵可以组合在一起形成一个完整的变换。FTransform在游戏引擎中被广泛用于表示对象在3D空间中的位置和变换，如游戏角色、场景中的物体等。

3) **Matrix 为矩阵类型** : M行N列 组成的 元素集合.

**逻辑解释 :**

* 创建一个`void LoadModel(StaticMesh InLoadMesh, Transform InModeMatrix)` 
* 创建 Modle结构体数组 MyModleArray
* 判断输入的InLoadMesh是否为空, 如果不为空:
* 向MyModleArray 添加一个 空 模型, 使用MakeModel 填充一个无意义元素, 并返回这个无意义元素的下标(封装成函数比较好)
* 声明一个变量`int TempAodleIndex`储存 空元素 下标, 默认值为-1. (如果默认值为0, 但此时数组内没有元素,则下标为0指向的内存空间无意义)
* 调用核心函数`Get Section from Static Mesh` , 将 `StaticMesh InLoadMesh` 传入此函数, 
* `MyModleArray[TempAodleIndex].Verts = GetStctionFromStaticMesh.Vertices;`(注意: 在BluePrint里, 使用Get指定数组下标对元素进行指定; 使用Set members in 节点对结构体数组的已经确定下标的元素进行编辑)
* `MyModleArray[TempAodleIndex].ModelMatrix = InModeMatrix;` (在蓝图里, Transform 和 Matrix可以相互转换)
* `GetStctionFromStaticMesh.Triangles()` 里储存的是一些索引, 也就是按顺序排列的所有顶点ID, 如下标0-2 就是一个三角行等
* 因此, 我们创建一个intArray: `TempTriangles` 接收`GetStctionFromStaticMesh.Triangles()` 返回的三角形索引;
* 对于这个TempTriangles的详细解释如下: 
  * 首先, TrinangleFace 三角形索引的格式类似结构体数组:{{0,1,2},{3,4,5},{6,7,8}}; 但是GetStctionFromStaticMesh.Triangles()的数组形式是整形数组, 因此每三个元素构成一个三角面. 如: 模型有4个顶点,每个三个则构成一个三角形. 那如何告诉计算机每三个一组呢? 
  * 第一组:0,1,2;  第二组: 3,4,5; 第三组: 6,7,8; 第四组: 9,10,11;
  * 由于每组固定是三个元素, 因此, 我们固定拿到每组第一个元素的下标即可.
  * 一0; 二3; 三6; 四9;等 他们构成一个线性变换; 设Y = kX + b; 已知 Y和X; 得出k + b = 0; 2k + b = 3;
  * 得出 k = 3; b = -3; 也就是 Y = 3X - 3;
  * Y是每个图元的顺序, X是当前图元的首元素下标;
  * 求出每个图元的顺序与首位元素后, 将其放进新变量(结构体嵌套)里即可
* 使用 LENGTH 节点求出数组长度, 输出int, 做除法运算.
* 使用`for(int i = 1; i < (sizeof(TrinangleFace)/sizeof(TrinangleFace[0])) / 3; i++)` `i` 的数值是组数,将其带入线性公式: `i * 3 - 3` 便求出了每组第一个元素; 我们定义一个`int FirstIndex` 变量来储存每组第一位元素下标
* 在Modle 结构体数组中, 将前面的用来定义 空元素下标的 整形变量`TempAodleIndex` 输入数组的索引中, 也就是`MyModelArray[TempAodleIndex].Faces(TraingleFace类型的数组) = 每一组元素组成的数组` 使用BluePriont就是: Make, makeArray.
* 将TempTriangles 数组中, 下标为: First+1, +2, +3,

---

# 第十二课: 线框渲染管线: 顶点着色器GPU阶段

**蓝图着色器的组织形式 :**

* IShader (Interface接口) 在Unreal中, 形式上是一个 抽象类, 有以下成员函数: 
  * VertexShader: 顶点着色器
  * FragmentShader: 片面着色器, 像素着色器
  * GetShaderType: 着色器类型
* 着色器有以下几种类型, 这些类通过多态来实现内部功能:
  * WireframeShader(线框着色器)
  * FlatShader
  * GroundShader
  * PhongShader

**实现逻辑(定义渲染模式) :**

* 定义一个枚举变量: `enum EShaderType {None, EwireframeShader };` 

* 定义一个抽象类 `class IShader {virtual void VertwxShader(int InFaceMun, int InVertexNum , HUD InHUD) = 0;  virtual void FragmentShader() = 0; virtual EShaderType GetShaderType() = 0;};` 并定义3个纯虚函数  

* 定义子类`class WirftameShader : public IShader` 

* 重载纯虚函数`VertexShader();` 

* 重载纯虚函数`GetShaderType(){return EWireframeShader; }` 

* ```c++
  EShaderType SpawShader(EshaderType eshaderType) //在文件夹里新建BPShaderHUD, 创建本函数
  {
      IShader TempShader = None;
      switch(EShaderType)	//使用枚举型switch
      {
          case EwireframeShader{
              WirftameShader.W1;	//在Unreal 使用 "Construct" 节点来定义对象 
              return W1.GEtShaderType();     
          }
          case None{
              return None;
          }
      }
  }
  ```

* 在上节课的CPU阶段, 在LoadModle函数中, 为MyModleArray.ModleShader赋值, 调用SpawShader函数, 传入一个枚举类型, 并赋值.

# 第十三课: 模型变换计算和蓝图实现

**实现方法 :**

* Unreal 已经给了现成的算法节点, 为`Transform Location` 节点. 

![5](F:\Desktop\技能\计算机编程语言\编程语言\C++\UnrealEngineCplusplus\Maps\luePrint\5.png)

**逆转换节点 :**

* `inverse Transform Locrtion`  输入物体的世界坐标系和世界坐标系, 获得物体在对象坐标系中的坐标

# 第十四课: 相机空间和世界坐标空间之间的转换

**解释 :**

* 相机本身也是一个物体, 因此与物体 - 世界空间 流程一样

**总结 :**

* Get Section from Static Mesh
  * Vertions: 储存顶点ID
  * Triangles: 每三个数组组成一个三角面



# 第三部分总结 

1) #### bresenhamDraw理论, 以及它的优化算法

   1) 1.判断斜率是否大于一, 如果大于,起点坐标x,y交换位置,并记录;
   2) 判断起点坐标x是否在终点最小的后边,如果是就把起点坐标和终点坐标交换
   3) 判断步进方向
   4) x每次步进一个像素, 累计计算d值, 如果累计d>0.5, y步进一步, 并将d值减一
   5) 整体算法优化, 免去浮点型的计算.

# ==第四部分==

# 第十五课： 矩阵









































​	



[^Darw Rect]: 像素画点函数;
