
# Houdini 节点词典


## Ⅰ. SOP

###  ==`Create`==

#### `Geometry` 几何体
>常用于主动封装层级 .

#### `grid`网格


#### `box`盒体


#### `sphere`球体


#### `transform`变换
>编辑几何体的变换属性 .

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



### ==Copy==

#### `Copy and Transform` 复制并移动
 >复制并变换某一个物体 ,  可以复制多个物体 .


#### `Copy to Point` 复制顶点
>右侧输入接口输入顶点数据模型 ,  左侧输入接口输入复制模型 .
>-  默认情况下 ,  模型指向法线方向 .

**1. 注意事项 :**
- 右侧接口输入 :  模型的每个顶点 .  我们可以为这个顶点赋予旋转 ,  缩放信息 . 
- `Piece Attribute` :  根据顶点的属性选择复制不同属性的物体 .




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


### ==读写内存==

#### `const`常量
>生命常量


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
- `@Frame`:  当前帧 .
- `@Time` :  获取下方时间轴的时间 .  (非实时时间)
- `@up` :  物体的Y轴 ,  可以被赋值 .
- `p@orient` :  方向 ,  可以对物体进行旋转 ,  输入四元数.
- `@Transform` :  输入矩阵 ,  主要包含**旋转 + 缩放 + 剪切（shear）** 信息 .  `Transforn`比`orient`的优先级更高 .
- `@scale` :  矢量缩放 .  
- `@pscale` :  常量缩放


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

#### `attribute delete`根据数据单元删除属性
>在细节面板选择删除某层级的数据 .


#### `File`文件
>导入或读取文件 .

#### `Normal`计算vertic法线
>计算法线

**一. 节点解释 :**

**1. 属性面板 :**
- `Add Normal to`:  选择被添加属性的层级 .

#### `mountain` 打乱
>将每个顶点的P 或 N 属性随机加一个矢量 .

**一. 节点逻辑 :**

**1. 属性面板 :**
- `Attribute Names`:  选择随机的变量与其变量类型 .

#### `Color`赋予颜色
>书写顶点颜色

**1. 属性面板 :**
- Group :  输入作用元素的序号 .
- Group Type :  元素符号作用域 .
- class :  颜色赋予作用域 .
- Color :  颜色数据 .

#### `Subdivide`细分
>网格细分节点

| 参数名称                                     | 功能                                                                                                           | 技术解释 / 底层逻辑                                                       | 使用建议 / 注意点                                                       |
| ---------------------------------------- | ------------------------------------------------------------------------------------------------------------ | ----------------------------------------------------------------- | ---------------------------------------------------------------- |
| **Group**                                | 指定哪些面 (或哪些几何子集) 要被细分                                                                                         | 只对被选中的子集做插值与细分，不对整个 mesh 处理                                       | 用于局部细节增强。不要一味把全部都细分，性能会爆炸                                        |
| **Creases**                              | 指定用于“加硬边缘 (crease)”的子集                                                                                       | 可以给某些边施加 crease 权重 (sharpness) 以保持锐边                              | 如果你想边缘不要被完全软化，就用 Crease                                          |
| **Algorithm**                            | 选择细分算法（Houdini Catmull-Clark / Mantra-兼容 / OpenSubdiv Catmull-Clark / OpenSubdiv Loop / OpenSubdiv Bilinear） | 不同算法在插值规则、边界处理、效率等方面不同                                            | 如果你希望与 Mantra 渲染兼容，用 Mantra 模式；如果你需要三角网格处理好，以 OpenSubdiv Loop 为例 |
| **Depth**                                | 细分的迭代次数                                                                                                      | 每次迭代是在上一层的基础上再细分一遍                                                | 不要开很高（通常 ≤ 3 比较安全），一旦太大，面数爆炸                                     |
| **Override Crease Weight**               | 是否用节点里设定的值覆盖已有的 crease 权重属性                                                                                  | 如果选中，就忽略几何体本身的 creaseweight 属性，统一用这个值                             | 有时候你 want 控制一致的硬边，就打开这个                                          |
| **Crease Weight**                        | 如果 override 被启用，用这个权重来做加硬强度                                                                                  | 越大越“硬”                                                            | 通常在 0 到几之间调试                                                     |
| **Generate Resulting Creases**           | 是否在输出网格里保留（继承） crease 边                                                                                      | 细分后仍可能保留一些硬边                                                      | 如果你需要后续继续建模保留 crease，就开这个                                        |
| **Close Cracks（关闭裂缝）**                   | 避免在局部细分边界出现裂缝                                                                                                | 有几个选项 (Don’t Close / Pull Cracks Closed / Stitch Cracks Together) | 局部细分组合几何时经常用，避免裂缝出现                                              |
| **Point Attributes / Vertex Attributes** | 控制除了位置 (P) 以外的属性（法线、UV、其他顶点属性）插值方式                                                                           | 用线性插值或更高级的插值策略                                                    | 若你的模型带有纹理坐标或控制属性，需要选择合适的插值方式                                     |
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