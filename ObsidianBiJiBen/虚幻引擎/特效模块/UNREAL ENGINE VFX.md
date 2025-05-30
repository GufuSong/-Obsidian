---
date: 2024-12-24
aliases:
  - 虚幻引擎粒子特效模块详解
tags:
  - UnrealEngine
  - Niagara
  - Cascade
  - VFX
---



# UNREAL ENGINE VFX




## Ⅰ  Cascade







## Ⅱ  Niagara 

### Part 1. Niagara 基本概念

**1. 系统属性与公用参数详解 :**

* 作用 :  系统模块是发射器与蓝图( 外界 ) 连接的渠道 .  公有参数在 User Exposed 里 .  
* 被暴露的属性可以直接与外界交互 .

#### C 1.1 Niagara System的概念

**1. Niagara System 的构成**

* Niagara System 是由发射器构成.

> Emitter 发射器, 发射体. 在Unreal里, Add Emitter 指在Niagara System中添加新的发射器.



**2. Niagara System 的调用粒度**

* 在一个Level中, 一个 Niagara System 是最小的调用粒度. 无法在场景里直接摆放发射器.
* 按`?`可以播放一个 Niagara 的特效.



**3. Niagara 系统节点 :**

* 基础作用 :  总发射器 ,  控制影响本系统里的其他发射器 .  以及向外操作影响数据 .

**4. Niagara的User Exposed属性 :**

- User Exposed属性可以开放对外的控制接口 .  在细节面板, 蓝图中可以控制其数值影响内部发射器运行 .
- 这些变量属于System级别,  因此只能在系统节点中控制属性 .



**5. 使用须知 :**

* 什么是Niagara?
  * Niagara 是一个点模拟器. 这个发射器反复计算所有点拥有位置, 颜色, 等各种属性, 并传输到Render中渲染.
* Sprite 的本质 ?
  * 本质是一个渲染在上层的二维图形 .


---



#### C 1.2 Emitter 的特性与设置

**1. Emitter 的特性 :**

* Emitter 具有继承的特性, 在Niagara中, Emitter可以继承自`FX::Niagara Emitter`.
* Emitter 是逐行运行, 贴合编程的运行模式





**2. Emitter Properties的属性 :**

* `Emitter`:
  * `Local Space`: 是否使用对象坐标系. 
  * `Determinism`: 是否固定发射器的随机 .
  * `Random Seed`: 随机种子
  * `Sim Target`: 设置粒子运行方式 ,  CPU或GPU .


**3. Warmup 预热解算 **
- Warmup Time :  预热时间 .  注意 ,  GPU不支持预热 .

---



#### C 1.3 Emitter 的构成

**1. 基本概念 :**

* Emitter由什么构成?
  * Emitter由: `Emitter 节点`, `Particle 节点`, `Render 节点`

> Particle 粒子, (数)质点; 在虚幻里指发射单位. 
>
> Render 渲染

* Emitter 的架构 :
  * Module 模块
  * function 函数
  * Parameter 属性

* 不同类别的发射器有什么区别>
  * `Emitter`属性管理一个发射器属性. 
  * `Particle`管理一组粒子的属性.



**2. Empty 构成的基本节点 :**

* `Emitter`: 
  * `Settings / Spawn`:  在激活的一瞬间, 属性就已经被确定. 在后面不会有任何修改. 系统只会读取一次.
  * `Update`: 每一帧都去读写, 执行.

* `Particle`:
  * `Spawn`: 每一个粒子的拷贝构造函数, 确定每一个粒子刚出生时的属性.
  * `Update`: 每帧读写粒子在运行的过程中的变换的属性.

>注意 ,  Update为每帧运行 ,  因此需要考虑玩家性能不同所导致的帧数变化 .


**3. `Empty`的运行逻辑 :**

* `Emitter Spawn`: 构造函数, 仅在发射机被生成的那一刻运行.
* `Emitter Update`: 发射机从生成开始, 每一帧都在运算, 例如:速度对粒子的影响等.
* `Particle Spawn`: 在单个粒子生成时被调用, 例如初始的位置, 颜色等.
* `Particle Update`: 在粒子被生成时每一帧都在运算, 粒子的实时变换等.
* `Render`: Render 在收集当前帧的所有粒子之后, 使用这些属性渲染最终的模型和材质效果. 一个发射器可以有多个Render, 读取不同属性渲染不同的效果.

​		使用的属性会被binding, 所以在debug是通常是从binding往回找. 例如位置不对, 看一下位	置binding是哪个属性, 通过这个属性运算的过程寻找问题.

---



#### C 1.4 Emitter 的继承

**1. Emitter 继承的定义**

* 要想达成继承关系, 需要先拥有一个父类(基类) 发射器. 子类(派生类)发射器会继承父类发射器的所有功能与属性.
* 派生类也可以成为新的基类, 形成再继承.
* 按住`ALT`+`Shift`+`R`可以调出菜单, 查看继承关系. 



**2. 继承关系 :**

* `New emitter from a template` 从现有的模板继承新发射器, 如果发射器本身就是模板, 则不会发生继承关系, 仅为复制关系. (如果继承的是引擎自带的模板 ,  则不是继承关系 ,  可以增删模块)
* `Inherit from an existing emitter` 从现有的发射器继承新发射器(不可以删除模块)
* `Copy existing emitter` 复制现有发射器 



**3. 使用须知 :**

* 更改派生类的父级关系 :
  * `Selection` Setting 中可以更改继承的基类.

#### C 1.5 Render 属性详解 

**1. Render 的基本概念 :**

* 发射器发射出的例子经过程序运行后, 接受这些粒子的属性并将它们逐帧渲染出来.
* 发射器发射的是点, Render决定将这些点渲染成什么东西.
* Binding 接受每一个点的基本属性. 



#### C 1.6 渲染形态

**1. Sprite精灵的本质 :**

* 本质问一个正方形面片 ,  可以低成本的运行粒子 .
* 使用Particle Color 向材质传递颜色信息 .



**2.NESH 静态网格 :**

* 发射静态网格体 ,  常用于发射单个模型 ,  资源占用多 .



**3. Light 发射灯光 :**

* 常用于配合Sprite使用 ,  如火焰照明等 .  



**4.Ribbon条状轨道 :**

* 发射的点连成线 ,  常用于闪电 ,  丝带等 .



**5.Component 发射发射器 :**

* 发射可以发射粒子的发射器 .




#### C 1.7 Sprite渲染形态及其应用

##### C 1.7.1 sub UV 的使用

**一.`sub UV`的概念及其使用方法**

* 使用Sub UV 时 ,  需要在 Sprite Render 中设置Sub VU等相关选项 ,  
	* `Sub UV Size` 输入Sub UV 的XY轴信息数量 .
	* `Sub UV when Translucent` 交叉重叠 (需要在 Material 中使用 Particle SubUV节点)

>交叉重叠的作用为做出更平滑的Sub UV效果 ,  原理为帧和帧之间做重叠处理 ,  做交叉效果 .  


**二. 基于MotionVector的SubUV制作方式 :**

- 运行原理 :  MotionVector记录了方向信息 ,  大致意思为下一帧向哪里扭曲更符合真实效果 
- `Particle SubUV Properties` 

**三. 常见的Sub UV的补帧方法 :

**1. 相邻帧混合法 :**

- 原理 :
	- 在帧和帧之间有重叠的区域 ,  也就是播放下一帧时上一帧仍然重叠 .  此功能在Unreal 叫做`Sub UV Blending Enabled` .  使用本方法时 ,  仍然需要勾选`Sub UV when Translucent` 选项 .  此选项需要在Material 中要使用`Particle SubUV`采样器采样贴图才能生效 .

- 使用须知 :
	- `Particle SubUV` 采样器会同时采样两次纹理对象 ,  因此会造成较高的性能消耗 .

**2. 基于`MotionVector`的`SubUV`的制作方式

- 原理 :
	- 通过UV偏移 , 实时生成新的采样后的贴图 ,  达到更真实的效果 .  注意 ,  此方法需要MotionVector贴图 .

**1.`Sub UV Animation`序列帧采样节点 :**

* 作用 :  当材质球使用了Sub UV ,  则可以通过此节点快速更改UV采样 .
* 属性 ;
  * `SubUV Animation Mode ` :  
    * Linear 按顺序的线性变化
    * Infinite 无限循环 .
  * Random Start Frame :  第一帧随机 .
  * Start Lookup Offset :   开始查找偏移量
  * Sub UV Loop Count :  生命周期内循环的次数


##### C 1.7.2 CPU与GPU采样贴图颜色

> 粒子渲染颜色的本质为实时更改shader的矢量

**1. CPU 与 GPU 的区别 :**

* GPU 擅长处理大量粒子 ,  但是不支持事件等功能且必须设置边界 .
* CPU兼容性更高 ,  但不支持大量粒子 .



**1.`Sample Texture`GPU 纹理采样**

* 作用 :  读取外部的贴图 .  输出属性Sample Color .  此Color 根据 GPU 并行原理 ,  将根据UV的采样与像素的坐标与粒子坐标将矢量数据赋予至粒子上 .

> 原理为Sample Mesh 时 输出 Mesh UV ,  此Mesh UV为粒子在二维坐标系中的UV信息 .  根据Mesh UV 信息获取贴图发Color矢量信息 ,  输出相应的Sample Color ,  注意 ,  是输出Sample Color ,  不是直接修改Color 信息 .



**2.`Dynamic Material Parameters`传递粒子属性至材质**

* 作用 :  检测Render输入的材质中`Dynamic Parameter`节点 ,  并实现变量传递 .  
* 注意 ,  `DynamicMaterialParameter` 属性默认输出RGBA四个属性 ,  每个属性默认为浮点型 ,  在特定声明的情况下接口才能输出矢量数据 .



**3.`自定义CPU Sample Texture`模块**

* 将`SampleUV` 转换为 `DynamicMaterialParameter`属性 .  在Shader 里将贴图采样 ,  并输入UVs 即可 .


##### 1.7.3 Sprite 的基础属性
**1. Bindings 属性 :**

* `Position Binding`: 位置(使用坐标系由发射器设置决定)
* `Color Binding`: 颜色属性
* `Velocity Binding`: 速度绑定
* `Sprite Facing Binding` :  面朝向绑定  (Y 轴对齐的方向)
* `Sprite Alignment Binding` :  面对齐绑定  (Z轴对齐的矢量)
* `SubImageIndex` :  Sub UV 采样 ID



> Bindings 属性接受发射器的粒子属性, 属性为输出一个具体值的表达式.



**2. Facing Mode 面朝向 :**

* `Facing Camera` 平行于摄像机 .
* `Facing Camera Plane` 强制朝向相机
* `Custom Facing Vector` 自定义面朝向
* `Face Camera Position` 强制指向相机的原点  ,  透视感很强
* `Face Camera Distance Blend` 混合点和相机

>由于精灵粒子的本质是面片 ,  因此面法线默认垂直于摄像机 .


**3. Alignment 旋转对齐:**
* `Unaligned` 没有对齐
* `Velocity Aligend` 对齐至速度方向
* `Custom Alignment` 自定义对齐方向

**4. Sort Order 半透明渲染次序**

- 正常使用半透明渲染时会发生渲染覆盖问题 .  因此在本属性下调成渲染先后关系 .  数值越大 ,  渲染越靠前 .


**5. Sorting半透明粒子渲染排序 :**

- 解释 :
	- Sorting 决定了统一发射器下发射的多个半透明粒子的排序问题 ,  防止出现渲染出错的情况 .

- 参数调整 :
	- `None`: 不进行排序 .
	- `View Depth`: 根据场景深度排序 .

**6. Cutout 裁剪粒子 :**

- 作用 :
	- 使用更多的顶点裁剪正方形粒子 ,  达到更好的性能实现 .

- 选项 :
	- `Cutout Texture`: 裁剪贴图
	- `Opacity Source Mode`: 选择裁剪依据的通道 .
	- `Alpha Threshold`: 裁剪阈值 ,  值越大 ,  裁剪的范围越多 .
#### C 1.9 Light 灯光渲染形态

**1.`Light Render`灯光渲染**

- 作用 :
	- 令发射的粒子能够获得自发光 .

- 属性 :
	- `Use Inverse Squared Falloff` :  反比例衰减(反向平方衰减) ,  模拟真实灯光的灯光衰减 .
	- `Affects Translucency` :  是否照亮半透明 .
	- `Alpha Scales Brightness` :  是否受自身半透明属性影响 .
	- `Radius Scale` :  最小照亮范围半径 .
	- `Color Add` :  颜色偏向 .
	- `Renderer Visibillity` :  钳定发光序号 .

- **`Bindings:`**
	- `Radius Binding` 光照范围 .
	- `light Exponent Binding` 指数级衰减的指数 .
	- `Volumetric Scattering Binding` 光照影响雾的范围 (此数值与光源亮度有关 ,  光源月越亮 , 则越强 ,  当雾效和光过强时 ,  雾会延迟消散 .)

**2. 光照的两种模式:**

- 指数光照与反比例光照 .  控制他们的参数也均不一样 .

>光照无反应问题 :
>
>	光照强度不够高 .
>	范围不够强 .

**二. 使用须知 :**

- 光照默认为反比例光照 ,  优点在于真实 ,  但缺点为性能消耗大 .  Niagara 默认提供了指数级光照 .关系为指数越大 ,  中心与边缘的光照对比越强烈 .  最贴近真实的数值为2 - 3 .
- 光照范围会影响光照强度 .

- `Lighting` 关于光照与阴影:

	- `Volumetric Translucent Shadow`: 半透明接触阴影 .
	- `Contact Shadow`: 接触阴影 .
#### C 1.10 Ribbon 线条渲染形态
##### c 1.10.1 工具使用与基本定义

**1. Ribbon 的两种渲染方式 :**

- Ribbon :  拥有成长的过程 ,  是一个从零到有的过程 .

- Beam :  瞬间生成全部 ,  出现即全部 .



**2. Beam的必要模块 :**

- Emitter Update
	- Beam Emitter Setup 
- Particle Spawn
	- Spawn Beam (Beam生成)
	- Beam Width (Beam 宽度)		- Beam Twist Amount :  朝向


**3. Ribbon的必要模块 :**

* INITIALIZE RIBBON :  ribbon初始化

**3. Ribbon 解释 :

- 本质为将生成的片状粒子连接成线片 .


#### C 1.11 Mesh 网格体渲染形态

##### c 1.11.1 基础概念 :

**一. 必要选项 :**

- `Particle Mesh` :  输入需要发射的网格体 .
- `Override Materials` :  使用更改材质 .  可以增加多个材质 .

##### c1.11.2 Mess质量

**一. Unreal 中的质量 :**

- `质量 = 体积 * 密度` ,  在Engine 中也是如此 .  略有不同的是 ,  Unreal 中的体积为直接计算刚好能包裹住模型的长方体的体积 .

- 要想获得更真实的物理效果 ,  我们要了解Force和Velocity的关系 ,  令物体直接获得Velocity是忽视惯性反物理的 ,  真实的情况为先获得一个Force .
$$
F = m \cdot a\;\;\;(其中, F为力,\;m为质量,\;a为加速度)
$$
- 质量越大 ,  力产生的加速度越小 .  请注意这些正比于反比关系

**二. 使用须知 :**

- 如果出现由于Mass的质量导致大小物体间的速度差距过大 ,  这是由于Mass = 体积 * 密度, 而体积是一个Exp^3的值, 因此 ,  我们只要将他的Mass ^ 1/3 即可解决此问题 .

### Part 2. Niagara 系统模块库



#### C 2.1 什么是Niagara的系统模块库?

**1. 什么是Niagara的系统模块 ?**

* Unreal 官方为了避免重复劳动与提高代码重复利用性写出的, 类似CPP语言的STL库.

**2. 模块的作用是什么 ?**

* 模块是组成Empty的最小粒度, 假如把Empty封装为一个类, 这个类有运行的6个不同调用时机的基本行为, 那么这些模块就是在这些行为里负责具体做哪些事情.
* 一切模块本质是对 Random 参数的实时修改.

---

#### C 2.2 系统内局部自定义模块

**1. 定义方法 :**

* 系统内`Scratch Pad`窗口中的 :  Modules 中添加局部自定义模块 .

#### C 2.2 系统模块库汇总

> 本汇总以 Unreal Engine 5.4.4 作为基准.

##### c 2.2.1 Emitter Spawn

**`Audio`音频**



**`Decal`贴花**

**`Initial Decal Orientation`初始贴花方向**





**`Location`位置**



**`MAXScripts`脚本**



**`Math`数学**



**`Skeletal Mesh`骨骼网格**



**`Spawning`生成**



**`Sprite`精灵**



**`SubUV`子UV**



**`Utility`应用程序**





---

##### c 2.2.2 Emitter Update

**1. `Emitter`发射器设置**

**`Emitter State`设置发射器状态**

* 作用 :
  * 实时设置发射器存活状态.  默认状态为系统 .
* 细节面板 :
  * `Life Cycle Mode` :  生命周期模式 .
  * `Inactive Response` :  设置粒子发射周期完成时的状态.
    * `Complete` 优先保证粒子完整 :  等待粒子死亡后杀死发射器.
    * `Kill`发射器和粒子立刻死亡 :  立即杀死发射器 .
    * `Continue` :  停用发射器, 但在系统停止之前不会杀死发射器.
  * `Loop Behavior`: 发射器循环模式
    * Once :  不管周期是多久 ,  都仅发射一次. 
    * Infinite :  无限循环 .  (设置循环时间)
    * Multiple :  循环多次  (设置循环次数以及一次发射所需时间 .)
  * `Loop Duration` :  发射一次所需时间.  
    * Infinite :  无限时长
    * Fixed :  固定时长 ( 需要额外设置循环时间.)      
  * `Loop Delay` :  发射延迟 ,  可以选择仅延迟第一次循环 .



**2. `Spawn`生成**

**1. `Spawn Brust Instantaneous`一次性瞬间生成**

* 作用 :  一次性生成粒子.
* 细节面板 :
  * Spawn Count :  生成个数 .
  * Spawn Time :  延迟生成时间 .
  * Spawn Probability :  生成概率 .
  * Spawn Group :  模块组编号 ,  应用于仅影响个别发射器时做区分 . 



**2. `Spawn Rate`持续每秒发射**

* 作用 :  以秒为单位持续线性发射粒子 ,  将每秒发射粒子平均到每帧 .

* 细节面板 :

  * Spawn Rate :  每秒发射数量
  * Spawn Probability :  生成概率 .
  * Spawn Group :  模块组编号 ,  应用于仅影响个别发射器时做区分 . 

  

**3.`Spawn Per Frame`持续每一帧发射**

* 作用 :  以帧为单位持续线性发射粒子 ,  将每秒发射粒子平均到每帧 .
* 细节面板 :
  * Spawn Count :  每帧发射数量
  * Spawn  :  是否生成 . 输入布尔值 .
  * Spawn Group :  模块组编号 ,  应用于仅影响个别发射器时做区分 . 



**4.`Spawn Per Unit`运动距离超过单位时发射一个梨子**

* 作用 :  Niagara System 运动时 ,  经过设置单位则发射 .
* 细节面板 :
  * Spawn Spacing :  生成粒子数
    * Max Movement Threshold :  最大移动速度阈值 .
    * Movement Tolerance :  运动公差 .
    * Spawn Probability :  生成概率 .



**5.`Spawn Particles in Grid` 以矩阵的方式生成粒子**


**6.`Gravity Force`重力**




---

##### c 2.2.3 Particle Spawn

**一. `Initialization`初始化**

**1. `Initialize Particle`初始化粒子**

* 作用 :  初始化生成的粒子的属性 .
  * set : Lifetime ,  Mass ,  Material Random ,  Position ,  Sprite Size ,
  * get :  无
* 细节面板 :
  * Point Attributes 基础属性 ( 寿命 ,  颜色 ,  位置 ,  质量)
    * Lifetime Mode 设置粒子寿命 
      * Direct Set :  具体数值 - Lifetime :  输入浮点型 .
      * Random :  随机数值 - 设置最小与最大
    * Color Mode 粒子颜色属性 ( 使用材质的话勾选Unset)
      * Unset :  不设置颜色
      * Direct Set :  直接设置颜色
      * Random Range :  随机设置颜色
      * Random Hue :  随机着色模型 (色相 饱和度 亮度)
    * Position Mode 位置设置
      * Simulation  Position 位置模拟
      * Unset :  不设置
      * Direct Set :  直接设置
    * Mass Mode :  质量设置 ( 质量越大 ,  力厂效果越弱 ,  运动惰性 ,  不经常用 ,  有更多的平替效果 .)
      * Unset :  不设置
      * Direct Set :  直接设置
      * Random :  随机设置
  * Sprite Attributes 精灵粒子贴片属性 ( 长宽 ,  旋转) :  
    * Sprite Size Mode 粒子长宽设置 :  
      * Unset :  不设置
      * uniform :  长款相等
      * Random Uniform :  随机长宽
      * Non Uniform :  长宽不想等
      * Random Non Uniform :  随机长宽不想等 .
    * Sprite Rotation Mode 粒子旋转设置 :
      * Unset : 默认
      * Random :  随机
      * Direct Angle :  直接设置角度
      * Direct Normalized Angle :  直接设置 Normalized Angle (1 = 36°)
    * Sprite UV Mode 粒子UV设置: 
      * Unset :  默认
      * Random X / Y / XY
      * Custom : 自定义
  * Mesh Attributes 网格体粒子具体属性 ( 规模 )
    * Mesh Scale Mode 设置网格体归农 :  



**2.`Set New or existing parameter directly` 手动初始化属性**

* 可以在这里手动初始化变量 .



**二. `Location`位置**

**1.`Cylinder Location`圆柱位置**

* 作用 :  每次调用时, 都会set粒子location ,  并且排列的粒子整体形状为圆柱形状.

  * Get :  Local Space 局部坐标 .  
  * Set :  Position 

* 基础功能 :  
  * `Cylinder height` :  气缸高度 .
  * `Cylinder Radius` :  气缸半径 .
  * `Orientation Axis` :  定位轴 .
  * `Hemisphere X` :  半球X .
  * `Hemisphere Y` :  半球Y .
  * `Offset` :  抵消 .

  

**3.`Grid Location`矩阵位置**
* 需要搭配`Spawn Particles in Grid`矩阵生成节点使用 .  将粒子的位置按照矩阵排列成立方体 .


**4.`Shape`形状位置**

* 生成不同形状的粒子位置
* 属性 :
  * Shape
    * Shape Primitive :  选择生成的具体形状 . (不同形状有不同的属性 .)
  * Distribution 分布
  * Transform 变换



**5.`Static Mesh Location`基于静态网格的分布**

* 基于静态网格体进行发射 .  注意 ,  要想使用此功能需要在静态网格体设置中勾选 `Allow CPUWccess`允许CPU访问 .
* 此模块需要搭配`Sample Static Mesh`静态网格体采样使用 .
* 注意 ,  此模块为随机在一个三角面上生成位置 ,  因此不能区分面的大小 .会导致小面生成的粒子很密集 ,  大面生成的粒子很宽泛 . 

- 本功能内置了网格体采样( 5.1.1 之后 .) ,  已知会采样 :
	- Mesh UV 像素信息 (用于采样纹理 ,  给粒子着色)
	- Mesh Position 像素位置 (确定粒子的位置)
	- Nesh Normal 像素法线 (自定义粒子的面朝向)
	- Mesh Tangent 像素切线 (自定义粒子的面旋转)



**6.`Skeletal Mesh location`基于骨骼网格体的分布**

- 属性列表
	- Sampling 采样选择
		- Bones 骨骼
		- Triangles 三角面

>可以在蓝图添加Niagara组件在蓝图内使用niagara

**二 .  运动属性**

**1.`Add Velocity`添加速度属性**

**2.`Scale Color`颜色倍增**



---

##### c 2.2.4 Particle Update

**一. `Forces`解算器**

**`Solve Forces and Velocity`速度解算器**

* 作用 :
  * 实时计算粒子的属性 ,  并使用数学算法模拟 .  本模块是解算器基础模块 .

* 菜单面板
  * Acceleration Limit :  加速度限制 .
  * Speed Limit :  速度限制 .



**`Drag` 阻力解算器**

* 作用 :
  * Drag :  阻力参数 .
  * Rotation Drag :  旋转阻力参数 .

* Get and Set :
  * `PhysicsDrag` :  物理阻力 .
  * `PhysicsDragRotationalDrag` :   物理旋转阻力 .



**`Curl Noise Force`随机噪波扰乱力**

* 作用 :
  * 添加随机扰动的立场 .
* 参数 :
  * Noise Strength :  力强度
  * Noise Frequency :  力频率 ,   控制粒子运动形态 ,  数值越大越碎 ,  越小越整 .
  * Noise Quality / Cost :  噪波精细度 ,  控制噪波运动精细度 .
  * Pan Noise Field :  运动偏移 ,  修改细节 ,  效果不大 .
  * Mask Contribution :   根据速度的方向限制扰乱力的影响 .
    * Curl Noise Cone Mask Angle :  最大改变速度方向的角度
    * Curl Noise Cone Mask Falloff Angle :  最小改变速度方向的角度



**二. Lifetime 生存状态 **

**`Particle State`粒子状态**

* 作用 :  执行粒子的生存状态 .  多用于
* 菜单 :
  * `Kill Particles When Lifetime Has Elapsed` :  粒子是否接受死亡 ?


**三. 形态变化 :**

**`Scale Sprite Size by Speed`基于Velocity控制粒子Size**

- 作用 :
	- 基于速度做 Size 的线性插值 .  

- 参数列表 :
	- `Min Scale Factor` :  当速度为0时的形变 .
	- `Max Scale Factor` :  当速度为1时的形变 .
	- `Velocity Threshold` :  速度阈值 .  数值越大 ,  形变越小 .  反之 ,  反之 .
	- 


##### c 2.2.5 system Update

**`System State`系统状态**

* 细节面板
  * Loop Duration :  粒子发射周期 .

---



### Part 3. 变量与变量表达式

#### C 3.1 粒子属性的基本概念

**1. 什么是粒子属性 ?**

* 粒子属性为变量, 储存数值. 此变量可以在Selection面板中转换为表达式, 实现时刻输出不同的数值.



**2. 粒子属性在Niagara中有什么功能 ?**

* 粒子在Binding时需要输入变量, 也可以输入表达式. 输入的数值作为粒子渲染时的属性使用



**3. 怎么新建粒子属性 ?**

* 在`Parameters`面板中添加.



**4.Niagara 系统中变量的调用规则 :**

* Niagara 系统中的变量均为地址传递, 因此每次调用都会覆盖上次调用时的默认值.
* 在初始化被调用过的变量时, 可以使用其默认值对变量进行初始化, 否则虚幻引擎默认初始化为0.



> 注意 :  不同数据类型的属性可以相互转换 ,  比如实数可以转换成相等分量的矢量

---



#### C 3.2 Niagara 系统自带属性宝典

> Attribute Spreadsheet 可以查看粒子属性 .   如ID ,  标号等 .


**1. 基础粒子属性 :**
- ID :  按照粒子的生成顺序赋予 ,  当粒子死亡时 ,  新生成的粒子会继承死亡粒子的ID .
- Index :  记录粒子的生成顺序 ,  

###### c 3.2.1 ENGINE OWNER 引擎自带变量

**1. Position 系统位置**

* 实时储存 Niagara System 的世界坐标位置 .



###### c 3.2.2 PARTICLE 粒子属性

**1. Velocity速度**

* 粒子的速度属性 ,  算法为粒子位置实时加本矢量 .



**2. Position位置**



###### c 3.3.3 TRANSIENT 瞬间属性

**1.PhysicsDrag 阻力**

* 物理阻力属性





#### C 3.3 变量表达式的基本概念 

**1. 工具解释 :**

* 普通变量被调用后, 在相应的细节面板编辑具体的变量表达式.
* 单击单个变量, 点击右侧属性小三角可以生成一个表达式. 默认的表达式由变量与运算符构成. 单击默认生成的变量任然可以将其定义为新的表达式. 称其为表达式的嵌套.



**2. 程序的微观解释 :**

* 表达式输出的值会赋值给相应的变量. 并且为地址传递, 一切修改的结果都是直接更改相应内存的数据. 
* 直接地址传递的好处为避免无必要的内存消耗.
* 不同类型的变量所拥有的运算符也不一样.
* 只有表达式运算完之后, 才进行赋值.

---



#### C 3.4 Niagara 数学函数汇总

> 注意: 数学函数的参数列表不一定包含所有类型的变量, 请注意甄别.

**1. Niagara 中数学运算时常见的数据类型 :**

* 单个数值:
  * int ( 32位, 64位 ) ;   float ;  
* 矢量:
  * vector ( vector,  color,  position)
* 矩阵:
  * matrix( transform )

###### c 3.3.1 基础运算函数



#### C 3.5 数学函数进阶

##### c 3.5.1 随机函数

**1.`Random Range`范围内随机**

* 作用 :  随机返回一个浮点型 .
* 属性 :
  * `Minimum` :  最小随机数 .
  * `Maximum` :  最大随机数 .
  * `Evaluation Type` :  随机类型 ( 输出不同类型的数据有不同类型的枚举选择 ) .  
    * Spawn Only :  仅第一次随机
    * Every Frame :  每一次都随机 .
  * `Spawn Probability` :  每次生成数的概率 .



**2.`Uniform A Or B` 范围内可调节概率随机**

* 作用 :  随机返回A 或 B  ,  并可以改变概率 .
* 属性 :
  * A :  最小值
  * B :  最大值
  * `Distribution Weight` :  调整偏向性概率 ,  0.5为相同概率 .

##### c 3.5.2 曲线

**1.`Curve`曲线**
* 作用 : 根据时间输出特定曲线对应的值 .

### Part 4. 自定义模块

#### C 4.1 自定义模快工具介绍

**1. 基础面板 :**

* **`Script Details`脚本细节**
  * 脚本搜索可见性 `library Visibility`
    * `Unexposed` 不可见 ,  不曝光.
    * `Exposed` 可见
    * `Hidden` 私有, 隐藏.
* `Parameters`脚本变量 ( 引用脚本时, 脚本变量会自动附属至发射器属性里.)



#### C 4.2 模块节点

##### 一 .  函数调用入口与函数返回值 :

**1. `Input Map`输入属性 :**

* 功能 :  获取系统内所有属性的读写权限 .



**2.`Output Module`属性输出 :**



##### 二 . 变量读写节点 :

**1.`Map Get` 读取属性 :**

* 功能 :  读取相应的属性 .



**2.`Map Set`设置属性 :**

* 功能 :  设置相应属性 .



##### 

## Ⅲ  DirectX Shader









## Ⅳ Creative Thinking

### Part 1. 工具使用

#### C 1.1 Niagara 架构运用



##### c 1.1.1 Emitter继承

**1. 解释 :**

* 常见的 Emitter 由发射器状态 ,  粒子属性与解算器构成.

##### c 1.1.2 Curve使用技巧

- 先确定整体亮度 ,  再调整色相 .
- 在调整透明度时 ,  0透明度的地方容易黑色 ,  因此要保证暗部也具有一定色相 .
- Alpha 曲线可以一定程度上影响风格化效果 .  Alpha 曲线末端使用easy out的模式 ,  来缓解贴图边缘的暗影 .

##### c1.1.3 Camera细节

- Unreal中 ,  Camera的长度为20 ,  也就是距离相机原点 20个单位以内的物品会被自动裁剪 .

##### c1.1.4 Texture的伽马矫正及其压缩细节

- 贴图在导入引擎时 ,  会进行伽马矫正 ,  效果为灰部变亮 .  使用 power进行灰部削弱即可 .
- 贴图经过压缩之后无法储存负值信息 .

##### c 1.1.5 力与质量 


#### C 1.2 方法与算法

##### C 1.2.1 如何表达不同方向的速度 ?

**1. 解释 :**

* 使用Unreal 自定义模块 ,  获得各个方向不同的矢量数据 . 注意 , 这个矢量数据不一定必须是速度属性, 可以是未知属性 , 颜色属性, 只要是不同方向不同大小的矢量数据即可 .



**2. 工具实践 :**

* 可以使用`Particle Pawn::Cylinder Location` 获取不同方向的矢量 , 并自定义模块 ,  将它们转换为速度 .

##### C 1.2.2 特殊情况下的效果优化 

**1. 粒子强度与摄像机距离间的优化 :**

- 在特效中 ,  特效的远近会带来不同的观感 .  如发光特效在远距离所占屏幕像素较小 ,  不会引起别人的观感不适 .  但如果距离过近 ,  则会影响视觉感受 .  因此需要在材质系统里进行优化 .


#### 1.3 美术表达

##### **1.3.1 粒子对环境的影响 :**

**1. 使用灯光发射器进行环境光氛围渲染 :**

- 使用Light Render 进行灯光发射 ,  对环境光渲染
- 注意光照半径对关照强度的影响 .  光照强度本身也具有变化 .  尝尝为从小至大 .
##### **1.3.2 掌握运动的节奏 :**

**1. 特效打点 ,  节奏打点 :**

* 多次主体粒子的出现时要有时间变化 ,  如越来越快 ,  慢 - 慢 - 快 等

**3. 亮度层次与明暗节奏要清晰 :**


**4. 亮度爆炸与二次闪光 :**


##### **1.3.3 火花的运动模糊 :**

- 运动的物体会发生模糊 ,  模糊的视觉现象为边沿过渡与实体放大 .  随着速度的变化 ,  模糊效果减弱 ,  Size也会变小 .

##### 1.3.4 光斑

- 高亮物体往往伴随 ,  注意光斑的细节及颜色 .
- 物体在极高温剧烈燃烧的情况下 ,  光斑会呈现抖动 ,  忽明忽暗的效果 .

##### 1.3.5 不同烟雾的质感区别

**一. 烟雾的两种类型 :**

- puff: 烟雾突然爆开, 就像哈利波特那样, 有什么东西在里面突然消失 .
- Smoke: 持续存在的 ,  缓缓运动的 .  存活时间更长 .

- 烟雾越薄, AO, Normal应该越弱. (蒸汽/ 寒气/ 干冰) 烟雾越厚, AO, Normal应该越强, 光影感越强 .(燃油烟, 大型爆炸, 火灾)

- 烟雾在逐渐变淡时 ,  光会透过烟雾 ,  将烟雾照亮 .  总结 :  越浓的烟雾越暗 ,  越淡的烟雾越亮 .  并且要注意 ,  烟雾的浓度会逐渐变淡 .
##### 1.3.6 特效中常用的网格体粒子消失的方法 :

**一. 缩小法 :**

- 实时更改Size, 死亡时缩小的效果 .

##### 1.3.7 特效与模型的交融 :

- 可以利用模型的造型表达特效中需要表达的要素 .
- 注意UV 在 Material 的影响 .
# 解释与后记

## 1. 虚幻小寄巧 :

- 当矢量代表`Color`时 ,  矢量的模会影响虚幻的曝光系统 ,  模越大 ,  则越曝.

- 当使用半透明材质时 ,  有可能导致相机距离过远时无法被渲染 ,  本质是抗锯齿的问题 .  勾选`Responsive AA (半透明材质参加抗锯齿运算)` 即可 .

- 在虚幻引擎中 ,  RGB 三通道的质量不同 ,  Green的质量为最高的 .

- Unreal 光源传播的规则为反比例传播 ,  距离越近 ,  光源能量无限最大 .  随着距离的传播 ,  能量迅速减弱 .





























 
