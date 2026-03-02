
# Houdini 节点词典


## Ⅰ. SOP

### ==`Rop`== 导出模块

#### `rop alembic output` ABC 导出

###  ==`Create`==

#### `Geometry` 几何体
>常用于主动封装层级 .

#### `grid`网格


#### `box`盒体


#### `sphere`球体


#### `transform`变换
>编辑几何体的变换属性 .

#### `Circle` 圆环
>生成一个圆环

#### `tube`圆柱

#### `platonic`多面球


**一. 属性细节 :**

**1. Transform Order :**
- 控制Transform 三个矩阵的相乘顺序 (矩阵的相乘无交换律特性) 

**2. Pivot Transform 变换中心点 :**
- Pivot Transform :  中心点变换 .  输入`$CEX , $CEY , $CEZ`即可将变换中心更换为物体中心 .
- Pivot Rotate :  中心点旋转 .

### ==OUT==
#### `Mantra`曼陀罗渲染器
>图形渲染工具 .

**一. 节点属性 :**

**1. 基础属性 :
- `Valid Frame Range`:  序列帧范围 .
- `Camera`:  选择相机 .

**1. Images 图片:**
- `Output Picture`:  输出位置 .
#### ``
### ==`UV`==

#### `uv texture`UV 纹理
>根据不同的方法自动展开UV .  `空格 + 5` 可以打开UV编辑面板 .

**一. 节点解释 :**

**1. 属性面板 :**
- `Attribute Class`:  属性层级 ,  选择将属性输入的层级 .

#### `UV unWrap` UV展开
>根据面法线的角度与朝向计算UV属性 .
**一. 节点解释 :**

**1. 属性面板 :**
- `Spacing`:  展开大小 .
#### `uv quick shade` 快速浏览
>快速展开UV并渲染方格纹理

#### `uv Transform`UV变换
>将每个顶点的UV坐标均进行变换修改 .

#### `Material`指定材质
>优先为网格体指定材质 .


### ==Light And mat 灯光与材质==

#### `Environment Light`环境光
>基础环境光

#### `light`灯光
>光照单位 

#### `Material NetWork`材质网络


### ==Volume 体积==

#### `Volume`
**细节选项 :**
- `Name` :  初始化体素的名称 .
- `Initial Value` :  初始化每个体素的数值 .
- `Uniform Sampling`:  体素控制模式 .
- `Uniform Sampling Divs` :  根据`Uniform Sampling`选项的轴计算每个体素的大小 ,  并且控制整体的密度 .
- `Z Resolution Scale` : z 轴深度衰减 .

#### `isooffset`将已有模型转换至体积

**一. 细节选项 :**
- `Output Type` :  
	- `SDF Volume` :  输出SDF 体积 .  内部为负数 ,  外部为正数 .  
	- `Fog Volume`:  雾气体积 .
	- 

#### `Volume Wrangle` 
>针对体积的编程 .


#### `VDB from polygons`
>VDB转换时只会在目标内部创造体素 .


**一. 细节选项 :**
- `Voxel Size` :  体素大小 .
- `Distance VDB` :  使用SDF VDB.
- `Fog VDB` :  使用雾气 VDB.
- `Exterior Band Voxels / Interior Band Voxels` : 向内或向外多产生一些体素. 
- `Fill Interior` :  计算所有的内部体素 .


#### `VDB from Particle` 粒子转换为VDB



#### `VDB visualize tree` 显示VDB体素


#### `VDB combine` VDB混合


#### `VDB resample` 重新采样VDB
>对接口一的VDB 进行重新采样 ,  对齐至接口二输入的VDB


#### `VDB Smooth`平滑VDB
>对输入的VDB 体素密度进行平滑处理

- `Filter Voxel Radius` :  平滑半径 .
- `Iterations` :  平滑次数 .


#### `VDB activate` 激活VDB
>激活更多的体素区域 .

- `Position` :  位置Box扩大 .
- `Expand` :  等比扩大 .

#### `VDB vector split`
>`OpenVDB` 有Vecter VDB 和 Scalar VDB 两个类型 .  但大量VDB节点仅支持Scalar VDB .  此节点可以将一个Vecter VDB拆分为 三个 Scalar VDB .



#### `VDB vector from scalar` 基于ScaleVDB创建 VectorVDB


#### `volume visualization` 观察Volume
>可以重新映射体素数值 ,  观察Volume


#### `convert volume` 将体积转换为模型
>将体积转换为模型 .


### ==Copy==

#### `Copy and Transform` 复制并移动
 >复制并变换某一个物体 ,  可以复制多个物体 .


#### `Copy to Point` 复制顶点
>右侧输入接口输入顶点数据模型 ,  左侧输入接口输入复制模型 .
>-  默认情况下 ,  模型指向法线方向 .

**1. 注意事项 :**
- 右侧接口输入 :  模型的每个顶点 .  我们可以为这个顶点赋予旋转 ,  缩放信息 . 
- `Piece Attribute` :  根据顶点的同名属性 ,  选择复制不同的物体 .

### `Attribute` 属性编辑



#### `attribute delete`根据数据单元删除属性
>在细节面板选择删除某层级的数据 .


#### `Attribute Promote`属性层级转移
>可以将点或者线的属性向不同层级转移 .

#### `attribute from map`向顶点采样纹理信息

#### `attribute blur`属性模糊

#### `attribute Transfer`转移
>输入两个类 ,  相互之间转移互相的属性 .

- 可以通过距离转移等等 .

#### `Attribute Transform` 属性拷贝
>拷贝两个物体间的属性 .

#### `Attribute from volume`从体积继承属性
>从体积获取属性 .

#### `sort` 排序
>更改顶点序号的排序 .  会改变线的朝向
- `Reverse Point Sort`: 翻转排序 .

#### `reverse` 翻转排序
>数据序列按相反的顺序翻转 ,  把第一个元素变成最后一个，第二个变成倒数第二个，依此类推。

>也可以发现反转 .

#### `poly frame` 几何体框架-生成法线与切线属性

>**为几何体（尤其是曲线/表面）生成“坐标框架向量”属性**，例如 **Tangent（切线）** **Normal（法线）** 和 **Bitangent（副切线/双切线）**，这些向量常用于后续几何变形、Sweep、渲染、粒子方向控制等工作


#### `assemble`组装
>根据几何的拓扑或属性，将几何“拆分 / 归并”为可管理的 piece，并为后续流程自动建立 piece 相关属性 .

- **生成 piece 标识属性**
- **可选地打包（Pack）几何**
- **可选地创建 Name / path 等实例与约束友好属性**


#### `Color`赋予颜色
>书写顶点颜色

**1. 属性面板 :**
- Group :  输入作用元素的序号 .
- Group Type :  元素符号作用域 .
- class :  颜色赋予作用域 .
- Color :  颜色数据 .

#### `name`赋予name 属性



### ==变形节点==

#### `twist` 扭曲
>扭曲

#### `mountain` 打乱
>将每个顶点的P 或 N 属性随机加一个矢量 .
>本质为改变@p 属性 .

**一. 节点逻辑 :**

**1. 属性面板 :**
- `Attribute Names`:  选择随机的变量与其变量类型 .

#### `Subdivide`细分
>网格细分节点



| 参数名称                                     | 功能                                                                                                           | 技术解释 / 底层逻辑                                                       | 使用建议 / 注意点                                                       |     |
| ---------------------------------------- | ------------------------------------------------------------------------------------------------------------ | ----------------------------------------------------------------- | ---------------------------------------------------------------- | --- |
| **Group**                                | 指定哪些面 (或哪些几何子集) 要被细分                                                                                         | 只对被选中的子集做插值与细分，不对整个 mesh 处理                                       | 用于局部细节增强。不要一味把全部都细分，性能会爆炸                                        |     |
| **Creases**                              | 指定用于“加硬边缘 (crease)”的子集                                                                                       | 可以给某些边施加 crease 权重 (sharpness) 以保持锐边                              | 如果你想边缘不要被完全软化，就用 Crease                                          |     |
| **Algorithm**                            | 选择细分算法（Houdini Catmull-Clark / Mantra-兼容 / OpenSubdiv Catmull-Clark / OpenSubdiv Loop / OpenSubdiv Bilinear） | 不同算法在插值规则、边界处理、效率等方面不同                                            | 如果你希望与 Mantra 渲染兼容，用 Mantra 模式；如果你需要三角网格处理好，以 OpenSubdiv Loop 为例 |     |
| **Depth**                                | 细分的迭代次数                                                                                                      | 每次迭代是在上一层的基础上再细分一遍                                                | 不要开很高（通常 ≤ 3 比较安全），一旦太大，面数爆炸                                     |     |
| **Override Crease Weight**               | 是否用节点里设定的值覆盖已有的 crease 权重属性                                                                                  | 如果选中，就忽略几何体本身的 creaseweight 属性，统一用这个值                             | 有时候你 want 控制一致的硬边，就打开这个                                          |     |
| **Crease Weight**                        | 如果 override 被启用，用这个权重来做加硬强度                                                                                  | 越大越“硬”                                                            | 通常在 0 到几之间调试                                                     |     |
| **Generate Resulting Creases**           | 是否在输出网格里保留（继承） crease 边                                                                                      | 细分后仍可能保留一些硬边                                                      | 如果你需要后续继续建模保留 crease，就开这个                                        |     |
| **Close Cracks（关闭裂缝）**                   | 避免在局部细分边界出现裂缝                                                                                                | 有几个选项 (Don’t Close / Pull Cracks Closed / Stitch Cracks Together) | 局部细分组合几何时经常用，避免裂缝出现                                              |     |
| **Point Attributes / Vertex Attributes** | 控制除了位置 (P) 以外的属性（法线、UV、其他顶点属性）插值方式                                                                           | 用线性插值或更高级的插值策略                                                    | 若你的模型带有纹理坐标或控制属性，需要选择合适的插值方式                                     |     |

#### `ripple`涟漪
>涟漪 ,  圆形波状变形 .

#### `blendshapes` 形状混合
>形状混合 ,  可以平滑的混合两个形状 .

#### `sequence blend`序列帧变形
>序列变形 ,  可以在多个形状之间变形 .

#### `soft transform` 平滑变换
>可以平滑的变换顶点 ,  带动周围顶点变换 .

#### `lattice`晶格变形
>晶格变形

#### `cloth capture`

#### `cloth deform`

#### `remesh` 重新拓补
>将模型重新拓补 .



#### `delete small parts` 删除小部件
>删除小的拓补封闭的模型 .



#### `smooth` 平滑
>修改顶点的位置 ,  更平滑 .


#### `polyre duce` 面数聚合
>削减面数


#### `bend` 弯曲
>形变节点



#### `resample`重新采样
>主要作用于曲线或几何体 

**1. 控制点的密度（增加或减少细节)**
- **增加点：** 如果你有一根只有两个端点的直线，想让它像面条一样弯曲，你需要更多的点。Resample 可以均匀地在中间增加点。
- **减少点：** 如果一条线上点太多（过于细碎），Resample 可以通过增大间距来简化它，提高计算效率。

**2. 实现“等间距”分布**
这是该节点最常用的功能。无论原始曲线的点分布得多么乱，Resample 都能让所有的点按照你设定的 **长度（Length）** 均匀排布。

 **3. 产生平滑效果**
Resample 节点内置了 **Treat Polygons As Subdivisions（将多边形视为细分）** 选项。勾选后，它不仅会增加点，还会像“磨皮”一样让原本生硬转折的线条变得圆润平滑。

### 线条

#### `line`线条

#### `Curve`曲线
>创建一根曲线

#### `resample`对曲线进行重新采样
>可以增加曲线点的密度

#### `Carve` 裁切
>可以对曲线进行裁切


#### `Poly wire`转换成管
>可以将曲线转换成管

#### `sweep`清扫
>沿路径生成几何体 ,  分别输入曲线和路径 .

### ==`其他`==

#### `merge`合并

#### `null`空 

#### `object merge`继承其他节点数据

#### `Alembic` 几何体缓存导入
>导入几何体缓存

**一. 节点解释 :**
**1. 属性面板 :**
- Geometry 
	- `Load As`:  加载状态 .  可以加载为解压后的状态 .
	- `Poly Soup Primitive`:  转换至的格式 .

#### `unpack`解压格式文件
>将外部导入的文件解压缩 .

**一. 节点解释 :**
**1. 属性面板 :**
- Iterations :  解压次数 .  解压层级 ,  层级越高信息越多 .  属性面板的PolySupe可以查看压缩层级 .

#### `convert`格式转换
>将不同类型的数据转换为想要的类型 .

#### `ROP Alembic Output`导出几何体缓存
>需要勾选`Build Hierachy From Attribute`导出层级关系 .

#### `File`文件
>导入或读取文件 .

#### `Normal`计算vertic法线
>计算法线

**一. 节点解释 :**

**1. 属性面板 :**
- `Add Normal to`:  选择被添加属性的层级 .

#### `UV Quick Shake` 现实UV格子

#### `add` 转换为点结构

#### `fuse`合并距离相近节点
>可以合并距离相近节点


#### `join` 合并
>可以将多个独立或单独的东西合并为一个 .
>常用于合并曲线

#### `Poly extrude`挤出
>可以挤出面 


#### `time shift` 锁定时间
>锁定时间 ,  用于观察 

#### `bound`包围盒
>生成一个包围盒 

#### `scatter`点云
>把几何体转换成点云

- `Force Total Count` :  迭代次数 ,  控制顶点密度 .


#### `point from volume` 体积转换为点
>内部也会生成点 .

- `jitter scale` :  打乱强度

#### `rest`冻结
>冻结并拷贝某一帧的属性 ,  并输出vector 类型的rest 属性 ,  通常为某一帧的P属性 .



### 模拟, 解算与循环

#### `For- Each Parameter`基于面   的循环
>循环

- `Iteration Method`迭代模式:  可以选择迭代对象 ,  比如基于顶点的迭代 ,    
	- `By Pieces or Points` :  基于面数或者点数循环 .
- `Gather Method` :  
	- `Merge Each Iteration` : 每一次循环单独运算 ,  最后合并在一起 .
	- `Feedback Each Iteration` :  当前iteration的输出会作为下一次的输入 .  也就是重复迭代同一块内存 .
- `Piece Elements` :  迭代元素 .
- `Piece Attribute` :  迭代属性 .
- `Single Pass` :  输出某一次循环的结果 .

**一. 获取循环次数 :**
- 点击`Create Meta Import Node` 可以新建一个输出循环次数的节点 .
- `iteration` 属性输出循环次数(从0计数)

#### `For- Each Connected Piece` 基于 相连部分的 循环
>Connectivity 节点 为每个相连的面分配一个 class 属性 .


#### `For- Each With Feedback` 自定义循环次数
>

#### `solver`解算器
>解算几何体 .  用于每帧迭代物体 .

**一. 构成 :**
- `Perv-Frame` :  输出上一帧的结果 .  在每次计算时会读取上一帧的结果 .
>类似于Niagara 中的每帧运行 .

- `Input` :  输出每个接口的输入 .

### ==数学==

#### `add`加法
>+

#### `vector to float`矢量转换浮点
>break拆分

#### `float to vector`浮点转换矢量
>make vector

#### `random`随机场
>里面为随机矢量场 ,  pos输入采样点 .

**一. 使用方法 :**

**1. 参数列表 :**
- `Clamp Position to Integer`:  将输入值转换为整型 .
#### `subtract`减法
>-

#### `multiply`乘法
>*

#### `divide`除法
>/

#### `add constant`加上常量
>+=

#### `absolute`绝对值


#### `average`取平均值


#### `ceiling`取整
>舍弃小数点后面部分 .


#### `complement`一减
>1 -

#### `floor`向下取整
>舍弃小数后加一

#### `fraction`取小数
>舍弃整数部分

#### `minimum`取最小
>取最小

#### `maximum`取最大
>取最大

#### `modulo`取余
>取余


#### `multiply Add Constant`加乘加
>+ * +没你


#### `AA Noise`
**一. 使用方法 :**

**1. 属性细节 :**
- `Pos`:  输入被噪波的顶点位置 .
- `Amplitude`:  噪波振幅 .
- `Roughness`:  噪波表面曲折 .
- `Max Octaves`:  采样次数 .
- `Noise tpye`:  噪波类型 .

#### `Fit Range` 线性插值
>根据输入值以及输入值的值域 ,  线性输出输出值域中的值 .  `y = [(x-(srcmin))/(srcmax-srcmin)]*(desmax-desmin)`

**一. 使用方法 :**
- `Val`:  输入值 .
- `Srcmin`:  输入值值域最小值 .
- `Srcmax`:  输入值值域最大值 .
- `destmin`:  输出值值域最大值 .
- `destmax`:  输出值值域最小值 .


#### `Turbulent Noise`激烈噪波
>与AA Noise节点类似 ,  区别在于此噪波值域在 0 以上 .

#### `fetch` 提取
>获取父级的 Transform 信息并赋予给 下级节点 .


### ==读写内存==

#### `const`常量
>生命常量

### ==贴图烘焙==

#### `Labs Simple baker` 烘焙贴图




### 渲染与材质

#### `material builder`材质跟节点

#### `principled shader core`核心着色节点

#### `compute lighting`计算照明






### 图形编程

#### `point vop`顶点编程
>内部可以书写自定义的逻辑 .  将节点的属性调用进节点中 ,  注意 ,  vop节点仅支持单线程运算 .  

**一. 使用方法 :**

**1. 运行逻辑 :**
- 将几何体的所有属性分别输出至接口 .

**2. 输出数据 :**
- `P`:  位置 .
- `Cd`:  顶点颜色 .

#### `Point Wrangle`点处理工具 简称VEX
>使用编程语言调取属性 ,  书写逻辑 ,  返回数据 .  这是针对与顶点的编程 .

**一. VEX脚本基础使用 :**

##### **1. 变量类型 :**
- `vector`:  矢量 ,  vector无(3)/2/4分别代表声明几维矢量 .  
```c
//矢量的定义方法
vector Test = {1,1,1};  //方法一. {}大括号可以定义矢量.
Test = set(4,5,6);  //方法二. 使用set()函数定义矢量.此函数会返回矢量.在定义时无法使用set().

//矢量单个分量的调用方法
vector t = {1,0,3};
t.x = 1;  //可以通过点,读取变量内数据.
```

- `matrix` :  矩阵
```c++

```

- `p` 四元数

- VEX 支持数值声明
```c++
float Array[] = float[](array(1,2,3));
float Array[];
Array = {4,5,6}; //注意括号格式
```
##### **2. 标识符与运算符 :**
- `@`:  属性变量声明符 ,  使用此符号表明的变量为数据属性 ,  可以添加新数据也可以从现有属性中调用其他数据 .
```c
vector @Test; //Test变量将会被写进属性里 .
@Test = 100;  //使用此标识符声明的变量读写时仍然要加上标识符 .

注意:
  - 此方式定义的属性在此节点运行结束后会写进内存,不会自动释放.
```

- `$`:  `$F`等价于`@Frame` 
- `*` :  所有属性 ,  与排除号`^`搭配使用 .
- `^` :  排除号 .

**常见属性 :**
- `@Ptnum`:  VEX在运行时, 调取的每个点的临时编号 .  
- `@numpt`: 总点数 .
- `@Frame`:  当前帧 .
- `@Time` :  获取下方时间轴的时间 .  (非实时时间)
- `@up` :  物体的Y轴 ,  可以被赋值 .
- `p@orient` :  方向 ,  可以对物体进行旋转 ,  输入四元数.
- `@Transform` :  输入矩阵 ,  主要包含**旋转 + 缩放 + 剪切（shear）** 信息 .  `Transforn`比`orient`的优先级更高 .
- `@scale` :  矢量缩放 .  
- `@pscale` :  常量缩放
- `@group` :  声明一个组 ,  注意 ,  此值为bool 值 ,  确定这个点是否在这个组内  .
- `@pscale`: 长度

**曲线属性 :**
`wight`: 渲染宽度 .

---

- `&&`与 :  两个逻辑均达到返回真 .
- `||`或 :  满足一个即返回真 .
- `!`非 :  如不等于 .




##### 3. 函数
###### `fit(float value, float omin, float max, float nmin, float nmax)`:  线性变换函数 ,  与节点用法一致 .
>数学函数 ,  与节点用法一致 .
```c

使用说明:
	* 与 Fit Range 节点用法一致 .
```



###### `anoise(...)`抗锯齿噪波
>此节点输出Vector 或float 类型的返回值 ,  可以显性声明他的返回值 .
```c
float  anoise(vector pos)

vector  anoise(vector pos)

float  anoise(vector pos, int turbulence, float rough, float atten)

vector  anoise(vector pos, int turbulence, float rough, float atten)

float  anoise(vector pos, int periodX, int periodY, int periodZ)

vector  anoise(vector pos, int periodX, int periodY, int periodZ)

float  anoise(vector pos, int periodX, int periodY, int periodZ, int turbulence, float rough, float atten)

vector  anoise(vector pos, int periodX, int periodY, int periodZ, int turbulence, float rough, float atten)

///////////////参数列表详解 :
* turbulence 重复次数,重复次数越多,噪波越碎.
* rough 粗糙度,频率
* Atten 振幅

//////// 显性声明返回值 :
vector Test = anoise(@P); //根据左值的类型选择输出值 .
Test = vector(anoise(@P));  //矢量型显性声明.
Test = float(anoise(@P));  //浮点型显性声明.

```


###### `min()`取最小值
>取最小值 ,  运行原理 :  分别对比并返回值 .

###### `max()`取最大值
>取最大值

###### `avg()`取平均值
>取平均值

###### `abs()`取绝对值
>取绝对值

###### `ceil()`向上取整
>去掉小数部分并加一

###### `floor()`向下取整
>去掉小数部分

###### `frac()`取小数部分
>略

###### `rint()`四舍五入
>略

###### `lerp()`线性插值
>略

###### `clamp()`钳制
>略

###### `removepoint()`删除顶点
>删除顶点 ,  运行逻辑为遍历所有输入网格的顶点id ,  并删除符合条件的顶点.


```c++
int removepoint(int geohandle, int point_number);

int  removepoint(int geohandle, int point_number, int and_prims);

//参数列表 :
- geohandle 
    输入需要被操作接口的接口编号,为(0~3);

- point_number
	输入需要被删除的顶点ID.

and_prims
	若此参数为1，该函数将删除所有因移除点而产生的退化图元（例如顶点数少于3的闭合多边形或顶点数少于2的开放多边形）。
	若此参数为0，该函数仅会删除因移除点而导致无效的图元（例如顶点数少于4的四面体，或零顶点的体积体）。
```

###### `length()`模长
>求矢量模长 .

###### `distance()`求两点之间的距离
>输入两个矢量求距离 .


###### `quaternion()`四元数
>输入一个矩阵 ,  返回四维矢量

```c++
vector x,y,z;
y = set(0,1,0);
z = @N;
x = cross(y,z);
matrix3 m = set(x,y,z);

p@orient = quaternion(m); //依次确定三个轴的朝向,并将数值赋予/朝向, 实现旋转
```
###### `cross()`叉乘

###### `rotate()`旋转
```c++
rotate(输入三个轴朝向的矩阵,旋转弧度(3.14 = 180°),旋转轴);
```

###### `radians` 角度转弧度

###### `rand( )`值域`[0.1]`的随机数

###### `removepoint()`删除点
>删除点

- `geohandle`: 几何体Index，通常写成 `0` 表示需要删除哪个接口的内容 ?
- `point_number`: 要删除的点编号




###### `xyzdist()`输出点到几何体表面的距离
```c++
float xyzdist(deometry, origin)

//deometry 输入物体, 可直接输入接口Index.

  
//prim（int 引用输出）：距离 origin 最近的 primitive（面/元）编号。
    
//`uv`（vector 引用输出）：该 primitive 上 最近位置的 parametric 坐标（UV）。
    
//MaxDistance 最大距离
```

###### `primuv()`采样属性
```
type value = primuv(geometry, "attrib_name", primnum, uv);
```
- **geometry**：要采样的几何体（比如 `1` 表示第二个输入节点）
- **"attrib_name"**：要读取的属性名（如 `"P"`、`"N"`、`"Cd"` 等）
- **primnum**：primitive 索引（通常由像 `xyzdist()` 这样的函数输出）
- **uv**：primitive 内的 **参数坐标**（UV/uvw），表示在这个 primitive 的某个点上的位置

###### `point()`读取点属性
```
type value = point(int geometry_input, string attrib_name, int point_number);
```
- **geometry_input**：整数，表示输入几何体的编号。
    - `0` 表示当前 Wrangle 的第一个输入。
    - `1` 表示第二个输入（如果有连线）。
- **attrib_name**：要读取的属性名称，如 `"P"`（位置）、`"Cd"`（颜色）、`"pscale"` 等。
- **point_number**：点编号，是你要读取的具体点的索引（从 0 开始）。


###### `dihedral()`令A向量指向B向量
>输入两个矢量 ,  输出旋转矩阵 .

###### `acos()`根据余弦值求角度

###### `detail()` 读取属性
>输入 接口 ,  属性名 ,  安全值 ,  即可返回属性参数 .


###### `pcfind()`寻找范围内的矢量/点
>寻找附近的点 ,  返回数组
```c++
pcfind(int (属入数据接口 普遍为1), string (输入被遍历的属性), float3 (输入点的位置), float (半径), int (寻找最多的点数量));
```

###### `len()`查询数组长度

###### `getbbox_min()`
>输出包围盒的对角坐标

```c++
getbbox_min(几何体脚标);
```

###### `getbbox_max()`
>输出包围盒的对角坐标

```c++
getbbox_max(几何体脚标);
```

###### `nearpoint()`寻找最近的一个点
>寻找最近的一个点

```c++
nearpoint(geometryIndex, pt);
```

###### `nearpoints()`寻找范围内的所有点
>与pcfind() 用法相近


###### `neighbour()`寻找相接的点
>寻找同一个拓扑结构内的相邻的点 .  


```c++
neighbour(geometry, point_num, neighbour_nim);//可以指定选择第几个邻居.
```


###### `neighbourcount()`寻找相接点的数量
>统计一个点一共有几个相连点?

###### `neighbours()`寻找所有相接的点
>寻找相连拓扑结构中的所有相接点 .

###### `volumesample()`采样体积
>几何体采样体积

```c++
float volumesample(geometry, int primunm, pos);
float volumesample(geometry, string volumename, pos);
```

###### `volumegradient()`体积梯度
>记录体素与体素之间的变化率 ,  并且保留体素之间变化的方向 .
```c++
float volumesample(geometry, int primunm, pos);
float volumesample(geometry, string volumename, pos);
```


##### 4. 语句

###### `if`判断语句
```c++
if(a < 15)
    a = 0;
    
else if (a < 10)
    a = 1;
    
else
    a = 2;
  
```

###### `for()`循环语句
>使用方法等价于c 语言

###### `while()`条件判断循环语句
>使用方法等价于 c语言

###### `foreach()`数组循环
```c++
foreach(int index, arrayType Value, InArray)
{

} //语法会将数组中的每个元素值赋给Valur,执行脚标赋给Index.
```

#### 全局 ch 函数

##### `UI`界面 ch 函数使用方法

**1. 使用方法 :**
- `Copy Parameter`可以复制属性的地址 .
- `Paste Expressions / References` 可以粘贴至其他属性 .

##### `ch`函数在编程中的使用方法
>`../` 相对路径 ,  跳出当前层级

**一. `chs` 函数 :**
>`ch` 函数的作用是: 读取相应路径属性的数值

| chf(...)         | float   | 明确要求 **浮点数**。例如你知道这个参数是 float 或 slider。                             |
| ---------------- | ------- | ------------------------------------------------------------------- |
| chi(...)         | Int     | 整形                                                                  |
| chs(...)         | String  | 字符串                                                                 |
| chv(...)         | vector  | 向量                                                                  |
| chramp(属性, x轴数值) | ramp 类型 | 用于曲线／样条控制（ramp）参数：你输入一个 0~1（或可能超出）范围的值，得到 ramp 上对应处的值。非常用于渐变、动画控制等。 |


- 当你调用了作用域内不存在的变量时 ,  点击旁边的 加号按钮 ,  可以创建属性 .
- `Edit Parameter Interface` 初始化属性编辑 .  左侧面板可以添加属性 ,  右侧编辑属性 .
	- `Name` :  变量名称 .
	- `Label` :  UI 上显示的名称 .
>这个界面还具有删除变量 UI 的作用 .




## Flip 流体模块

### ==Particle Fluids== :

#### FLIP Fluid from Object :

**一. 从物体转换为流体 :**

**1. 作用 :**
- 在 OBJ 界面 ,  选择一个物体 ,  可以将其转换为流体 .  生成Dop

**Dop :**
- Flip 是一种模拟仿真方案 .  缺点为计算小型流体不够细腻 .

##### `FlipFluidObject`

**一. 属性参数 :**
- `Particle Seqaration` :  粒子数量精度 .
- `Particle Radius Scale` :  粒子半径大小
- `Closed Boundaries` :  与边界产生交互
- `Grid Scale` :  粒子SDF场大小 .
- `Initial Velocity` :  初始化速度 .
- `Add Temperature` :  添加粘性 .

##### `FlipSolver` 最大解算范围

##### `Flip Container` 计算容器
>它在 SOP 层级定义并初始化一个流体模拟域（domain），管理流体粒子与体素网格的分辨率（Particle Separation）以及容器边界条件，同时设定流体的物理属性（如密度、黏性、表面张力等）

| 参数                                                          | 含义 / 作用                                 | 对模拟 / 性能的影响 / 使用建议                                                  |
| ----------------------------------------------------------- | --------------------------------------- | ------------------------------------------------------------------- |
| **Particle Separation**                                     | 粒子间最小间隔／粒子密度控制值                         | 值越小 → 粒子越密 → 模拟越精细但计算量高。必须在 Solver 中也匹配这个值。                         |
| **Grid Scale**                                              | 相对于粒子间隔，用于控制体素网格 (voxel grid) 的缩放 / 分辨率 | 小一点的值能捕捉更多细节，但体素数目增多、内存／运算成本提升。                                     |
| **Domain (Size / Center / Implicit Bounds / Bounds shape)** | 定义容器空间 / 边界                             | 你需要让容器能覆盖流体运动范围，否则粒子会被删除或跑出边界。若输入一个几何 / 体积 (volume) 给它，就以此几何作为边界形状。 |
| **Density / Varying Density**                               | 给流体整体或每粒子指定密度                           | 如果你想模拟不同密度流体（如油水混合），就打开 “Varyi 并给粒子赋值。默认是统一密度。                      |
| **Viscosity / Surface Tension**                             | 控制流体的黏滞性 / 表面张力行为                       | 对小尺度流体行为（比如水珠、小波浪）影响较大。高表面张力值可能需要更多子步 (substeps) 以保持稳定。             |

## RBD 刚体模块
>总体的步骤可以视为 切割 - 解算 - 渲染

### Voronoi Fracture 泰森多边法破碎
>给定一组散点 ,  根据 Voronoi（泰森多边形）算法，把一个几何体切割成很多不规则碎块 .

>此节点会根据每个点的属性计算出新属性 ,  包括后自定义的点属性 .

**一. 接口含义 :**

- 接口一 :
	- **要被破碎的几何体** ,  必须是 **封闭体（watertight）** 才能正常生成碎块 .
- 接口二 :
	- 用于生成 Voronoi 的点 ,  每一个点 = 一个碎块的“中心” .
- 输出 :
	- 多个独立的 **primitive 块** .  每个碎块有独立的`name` 或`piece` .

**二. 细节面板 :**

- Name Attribute :
	- Overwrite :  覆盖原有Name属性 .
	- Append :  扩展Name属性 .
- Piece Prefix :  属性前缀 .
- Compute Interior Normals :  是否计算内部切割的法线 ?
- Interior Cusp Angle :  内切角度最大平滑角度 .  区分软硬边 .
-  Exterior Cusp Angle :  外切角度最大平滑角度 .  区分软硬边 .

- Interior Group :  内表面分组名称 .
- Exterior Group :  外表面分组名称 .

---

### `RBD material fracture` 刚体材料破碎
>`primary fracture` 控制破碎迭代次数 .  每次迭代会在第一次迭代的总数上进行相乘 .

**一. 细节面板 :**

- `Material Type` :  切割材质类型 .  控制不同的切割材质 .

- `Primary fracture`
	- `Fracture Ration` :  控制每个点的切割概率 .
	- `Scatter Points` :  破碎切割点 .  直接控制碎块的数量 ,  每个点对应每个碎块 .
	- `noise type` :  控制体积撒点的体积noise .
	- `volume resolution` :  体素大小 / 精度 .
	- `noise frequency` :  noise 频率 .
	- `cutoff density` : 密度 .



---

### Exploded View 破碎观察剖面图

---
### Assemble 组装
>输出Picked 属性 ,  把破碎的每一个碎块视为一个点 并输出为Picked 属性 .

- `Create Name Attribute` :  生成Name 属性 .  为每个独立的拓扑结构生成Name属性 .
---
### `dop import`dop 数据导入 
>将Dop 模拟中模拟出来的对象 ,  几何体 ,  变换 ,  速度等数据拉回到SOP 进行后续处理与渲染 .


---
### `RBD Packed Object` 
>导入RBD 数据进行结算

**一. 细节面板 :**
- `Creation Frame` :  框架帧 ,  控制全局帧数 .
- `Object Name` :  类名称 , 刚体碎块的整体名称 .
- `Initial object Type` :  


