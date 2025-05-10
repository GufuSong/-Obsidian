
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
>+ * +

#### `Negate`相反数
>相反数 ,  乘以-1 .

#### `Power`指数
>指数次方

#### `Round to Integer`四舍五入
>四舍五入

#### `Square Root`开根号
>根号计算

#### `Anti-Aliased Noise`反走样噪波
>斜率更平滑的噪波数据 .  可以更自定义 .

**一. 使用方法 :**

**1. 属性细节 :**
- `Amplitude`:  噪波振幅 .
- `Roughness`:  噪波表面曲折 .
- `Max Octaves`:  采样次数 .
- `Noise tpye`:  噪波类型 .

### ==读写内存==

#### `const`常量
>生命常量



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

#### `point vop`自定义节点
>内部可以书写自定义的逻辑 .  将节点的属性调用进节点中 ,  注意 ,  vop节点仅支持单线程运算 .

**一. 使用方法 :**

**1. 运行逻辑 :**
- 将几何体的所有属性分别输出至接口 .

