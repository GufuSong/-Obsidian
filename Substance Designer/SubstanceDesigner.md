# Substance 3D Designer

## 第一部分:  前置基础

### 1.1 



## 第二部分:  基础节点

### 2.1 图形节点

**`Shape`基础图形节点**

* 返回值:
  * 返回一张基础图形的遮罩
* 属性面板 :
  * **`TNSTANCE PARAMETERS::Pattera`更改输出图案**
    * 矩形, 正方形等
  * **`TNSTANCE PARAMETERS::Tiling`平铺**



**`Polygon1`无边缘模糊的多边形节点**

* 返回值 :
  * 多边形遮罩图
* 属性面板 :
  * **`TNSTANCE PARAMETERS::Sides`调整边的数量**
  * **`TNSTANCE PARAMETERS::Explode`从中点连接顶点分割图形**
  * **`TNSTANCE PARAMETERS::Gradient`中心虚化**



**`Polygon2`边缘模糊的多边形**

* 返回值 :
  * 多边形遮罩图
* 属性面板 :
  * **`TNSTANCE PARAMETERS::Sides`调整边的数量**



**`Splatter Circular`圆形排列节点**

* 返回值 :
  * 黑白遮罩
* 属性面板 :
  * **`TNSTANCE PARAMETERS::Pattern Amount`调整排列数量**
  * **`TNSTANCE PARAMETERS::Pattern Amount Random`调整图案半径**
  * **`TNSTANCE PARAMETERS::Ring Amount`向圆心扩展**
  * **`Pattern::Pattern`调整形状**
  * **`Pattern::Pattern Specific`调整图形透明度**
  * **`Pattern::Symmetry Random`反转图形**