
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


### ==Light 灯光==

#### `Environment Light`环境光
>基础环境光

#### `light`灯光
>光照单位 

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


### 算法封装

#### `point vop`顶点编程
>内部可以书写自定义的逻辑 .  将节点的属性调用进节点中 ,  注意 ,  vop节点仅支持单线程运算 .  

**一. 使用方法 :**

**1. 运行逻辑 :**
- 将几何体的所有属性分别输出至接口 .

**2. 输出数据 :**
- `P`:  位置 .
- `Cd`:  顶点颜色 .

#### `Point Wrangle`点处理工具 简称VEX
>使用编程语言调取属性 ,  书写逻辑 ,  返回数据 .

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

##### **2. 标识符与运算符 :**
- `@`:  属性变量声明符 ,  使用此符号表明的变量为数据属性 ,  可以添加新数据也可以从现有属性中调用其他数据 .
```c
vector @Test; //Test变量将会被写进属性里 .
@Test = 100;  //使用此标识符声明的变量读写时仍然要加上标识符 .

注意:
  - 此方式定义的属性在此节点运行结束后会写进内存,不会自动释放.
```

- `$`:  `$F`等价于`@Frame` 

**常见属性 :**
- `@Ptnum`:  VEX在运行时, 调取的每个点的临时编号 .  
- `@Frame`:  当前帧 .


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

