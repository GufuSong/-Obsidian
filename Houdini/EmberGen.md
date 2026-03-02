
# 节点字典 :

## Simulation 模拟节点

>以模拟节点为准模拟节点左边的节点控制特效的动态 ,  模拟节点右边的节点控制特效的颜色与渲染 .

- Domain
	- Voxel Count :  体素数量 .
	- Voxel Size :  单个体素大小 .23
- Force
	- Solid Ceiling :  与顶部边界发生碰撞 .
	- Buoyancy :  浮力 .
- Dissipation
	- Smoke Dissipation :  烟雾密度衰减速率 .
	- Temperature Dissipation :  温度衰减速率 .
- Combustion
	- Generated Smoke :  燃烧产生的烟雾 .
	- Flame Intensity :  火焰强度 .
	- Expansion :  烟雾范围 .

## Emitter :  Volume
>体积发射器

- Emission
	- Smoke Rate :  烟雾生成速率 .
	- Fuel Rate :  燃料生成速率 .
	- Temperature :  温度 .
	- Emission Gradient :  排放梯度 .

## Shading 着色
>控制场景的着色 .

- Flames
	- Shaping by flames :  火焰亮度 .
	- Flames Coloring :  火焰颜色 ,  控制火焰色着色方法 .
- Smoke
	- Render Smoke :  是否渲染烟雾 ?
	- Smoke Density min limit :  烟雾密度最小值钳制 .

## Render 渲染
>渲染序列帧 .

## Export: Particle
>导出ABC文件 

## Export: VDB
>导出VDB文件 .

## Scene 场景设置
>控制效果的场景 .

## Ground 地面
>控制场景中地面的效果 .

## Skybox 天空盒
>控制场景的天空选项 .

## Camera 相机
>控制渲染的Camera .

## Volume Processing 体积后处理
>控制体积的后处理 .
