# 前言: 

* 本课程面向重点为艺术创作, 面向行业为影视后期特效, 应用与三维动画(部分二维动画), 广告, 电影, 电视剧, 广告.
* 特效分类:
  * 物理特效: 在片场制作的真实的物理效果
  * 特效化妆: 采用化妆的方式呈现的特殊效果
  * 后期特效: 使用电脑对特效进行呈现.
* 后期特效分类:
  * 三维场景特效:
    * 传统特效类: 粒子, 学, 爆炸, 破碎, 海洋流体, 群集
    * 非传统特效类: 程序建模, 材质, 植被分类, 植物生长
  * 三维角色特效: 毛发, 布料
  * 二维特效: 采用AE, NUKE 等后期合成软件制作的特效效果(光效, )

![6](F:\Desktop\技能\计算机编程语言\编程语言\C++\UnrealEngineCplusplus\Maps\luePrint\6.png)

**行业信息获取渠道 :**

* vimeo
* odforce
* dise Effects;

# Houdini

## 一. Sop基础

* surface 几何体节点

### 1. Houdini界面基础操作

#### 1.1 Houdini基础操作(merge)

**1. 视图面板 :** 

* 切换模型显示模式(实体和线框): `空格 + W`  手动操作则为视图窗口右上角正方体图表;
* 左上角还可以选择是否隐藏没有被调用的模型等

**2. 属性菜单 :**

* 正上方属性面板前方的图标是节点类型
* 调整数值时, 按住鼠标中键, 会更快/慢的变化数据.

**3. 节点面板 :**

* 双击节点可以进入细节层级(细节层级的属性面板会发生变化.)
* 节点的显示与层级有关;
* 鼠标放置节点上可以更改节点的基本现实
* 中建点击节点可以跳出节点信息面板
* 按P键会新建临时属性面板

---

#### 1.2  Houdini基础操作2(edit)

**1. 视图面板 :**

* 视图面板可以选择: 组物体, 几何体具体组件
* 左侧面板 吸铁石: 吸附组件功能.
* 右侧为各种不同的显示效果

**2. 基础功能栏: **

* 保存, 同常规

==**`DK::edit`**==

* 作用: 储存对几何体进行编辑的数据节点;

---

### 2. Layout 搭建案例

#### 2.1 案例搭建 - 基础位置

* ==`create::grid`==
  * 作用: 新建一个平面



* ==`DontKnow::transform`==
  * 更改网格体变换矩阵0



* ==`DK::camera`==
  * 新建摄像机, 视图视角右上角为查看当前摄像机. 左侧面板点击锁定可以锁定当前视角;
  * 属性面板

---

#### 2.2 案例搭建 - 基础位置2

**解释 :**

* 不要更改`Genmetry` 属性的面板, 容易出错. 
* 节点属性节点, `Momary`为内存占用的意思.
* 在一个根目录多个子物体不同位置时,可以妙用`transform`节点达到目的.

​           

* ==`DK::object_merge`==
  * 跨节点通信, 更改属性面板, 指定其他节点里的节点; (输出指定的节点输出的数据), 在属性面板中的Object1 里指定节点.
* ==`DK::NULL`==
  * 空节点, 无实际更改, 配合`object_merge`使用.

---

#### 2.3 案例搭建 - 相机动画

**1. 单个节点的快捷操作属性讲解 :**

* 叹号: 相关信息
* 向下箭头bypass: 令此节点失去作用
* 雪花loke: 锁定/冻结 当前节点
* Template: 多选节点
* 眼睛: 蓝色圈(显示时显示,渲染不显示) 紫色圈(渲染时显示, 显示时不显示.)

**2. 根节点快捷属性:**

* 绿色: 可选中
* 蓝色:是否显示

**3. 相机动画 :**

* 时间栏:
  * 可以调整时间
  * 每秒帧率与总时长在设置里更改.
  * 实时播放状态, 勾选real time toggle
* k帧方法:
  * `alt and con` +左键.
* 动画编辑器:
  * `shift + 左键` 属性面板为属性编辑器.
  * `F G H` 居中快捷键
* 拍屏:
  * 拍屏时间起始与时间轴自动匹配
  * `size` 里调整分辨率与画框长宽比
  * 拍屏时`Esc` 退出拍屏.
  * 保存拍屏(可输出序列帧和视频):
    * `File-Export - QuiteTime Movie` 保存mov格式视频
    * output File : 选择保存地址.'
  * 保存序列帧:
    * `$F`表达式: 输出图片按顺序+1;

---

#### 2.4 相机的显示设置

**1. 相机基础属性 :**

* `Transform` 变换信息
* `Render` 相机在视窗中的显示方式
* `View` :
  * Icon Scale: 视窗图标大小
  * Resolution: 调整宽高比(提供了很多的参考方式)
  * Pixel Aspect Ratio: 图片纹素比
  * Projection: 透视关系
  * Focal Length: 焦距
  * Aperture: 焦距微调
  * Near Clipping: 近裁面(0.1)
  * Far Clipping: 远裁面(此比例会影响透视)
  * Screen (上下两个): 画面压缩
  * Enable Background Image: 是否导入图片.

**2. 视口相机设置 :**

* 左上角菜单`Persp`调整, 或快捷键.

**3. 视窗设置 :**

* 右下角小眼睛图标
* 透视图下加载图片:
  * `Background - Persp`: 指定图片.
* 更改视图视角远近裁面:
  * 1.`camera - Near/Far Clip Planes` 调整远近裁面

---

### 3. `Maya`输出`abc`到`Houdini`

**一, 关于Maya的设置 :**

* 勾选ABC插件;
* 导出前请检查设置;'

**二. 关于Houdini的设置 :**

* 使用Alembic节点导入ABC格式文件.

**`DK::Alembic`** 导入ABC

* 导入ABC, File Name里输入文件路径
* 导入的为压缩后的文件.
* `属性列表::Geeometry:`
  * Load As: 读入时, 以什么形式读入. 为枚举型
    * Alembic Delayed Load Primitives ABC导入的默认压缩格式.
    * Unpack Alembic Delayed Load Primitives 无压缩状态, 在下方Poly Soup Primitives 选择No Poly Soup Primitives 即可获得 Polygons 的文件

**`unpack`**解压缩节点

* Iterations: 解压缩层级. 0层即为未解压.

**`Convert`将格式转换为Polygons** 

* 点, 顶点, 线, 面均为正常状态, 和Maya里面的一样.

**`rop_alenbic_output` 导出ABC**

* 导出ABC设置.
* Build Hierarchy From Attribute: 导出层级关系.

**3. ABC的属性 :**

* Point P: 每个顶点的位置
* Prim path: 顶点的层级关系路径.
* 顶点法线, 切线 与 顶点UV信息.

---

### 4. Houdini UV系统

#### 4.1 vertex 和 UV的理解.

> 空格 + 5 可以切换`HoudiniUV`面板.
>
> 请在DDC工具中展UV. 

**Vertex 顶点:**

* Vertex顶点区别与Unreal的顶点索引不同. Vertex为Point的下级, 而顶点索引的地级为Point.
* 详细请见Unreal渲染管线与基础图形学.

---

#### 4.2 Houdini 自动展UV节点

**`attribdelete`删除属性节点**

* 可以删除对象的属性. 具体在属性面板操作.

**`UVtexture`UV投射节点**

* 属性面板:

  * Texture Type: 展UV模式, 

    * Orthographic: UV投射, 从设置的轴向将UV映射至模型.[Projection Axis: 设置投射方向, 可以从X, Y的几种方向投射];  

      ​	缺点: 只能保证从单个方向的UV投射正常.

    * Perspective From Camera: 从摄像机视角投射

**`UVunwrap` 按面角度展UV**

* 将模型的面分为: 向左, 右, 前后, 上下. 并将这些面排布在UV象限中

**`UVquickshade`黑白图检测**

* 为模型贴上黑白图检测, 如果模型无UV信息, 则自动展UV.

**`UVtransform`UV变换**

* 对UV的采样做加法. 更改`uv`的缩放, 平移, 旋转等.

**`uvautoseam`系统AI展UV**

* 新的自动展UV节点 .



#### 4.3 手动展UV节点

**1.`UVFlatten`手动剪裁UV节点**

* 工具使用 :
  * `Shift` 剪裁边

**2.`uvLayout`UV布局**

---

### 5. Houdini贴图材质系统

#### 5.1 贴图材质的基本工作采用

**1. 材质工具介绍 :**

* `Material Palette`为材质菜单 .  左边的为材质模板库 .  新创建的材质球在 mat 里出现 .   
* 左键按住节点拖值模型上即可完成材质球赋予 .



**2. 渲染工具介绍 : **

* `Render View`为默认的渲染节点 ,  可以选择渲染的摄像机和渲染器 .
* 渲染器使用 Mantra ,  在out 面板创建 mantra 渲染器即可使用 .































































​                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   



























