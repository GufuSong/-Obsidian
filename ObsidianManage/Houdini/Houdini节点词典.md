
# Houdini 节点词典

## Ⅰ. SOP

###  ==`Create`==


#### `grid`网格


#### `box`盒体


#### `sphere`球体


### ==`其他`==

#### `merge`合并

#### `transform`变换

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

