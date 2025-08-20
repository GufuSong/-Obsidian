
# 项目开发文档

## Ⅰ. 项目开发前梗概


### $.1 Content 目录管理 :

**一. 项目立案需求 :**

**1. 梗概 :**
- 用于制作作品集的专用项目 .

**2. 目录梗概 :**

- **`Content`**
	- `SongGuFu_VFXContent`: 主要资产文件 ,  存放与技能或特效相关的程序美术资产 .
		- `Ability`: 存放特效主体 .
		- `CurveTable`: 专门用于存放CurveTable ,  但Curve仍然与Ability一同储存 .
		- `Element`: 专门用于存放基础Element ,  方便管理与维护 .
		- `MaterialFunction`: 专门用于存放基础MaterialFunction .
		- `GameBP`: 存放GamePlay , GAS 等程序逻辑 ,  与所有架构型程序资产 .
			- `Environment`: 环境, 所有关卡都需要适配本文件夹中的程序.
				- `B_`
		- `Map`: 存放游戏关卡 .

	- `ArtContent`: 存放用于制作特效的外部导入的数字资产 .
		- `FX_Texture`: 存放特效纹理 .
		- `FX_Model`: 存放特效模型 .

	- `OtherContent`: 存放与特效无关的外部资产 .


### $.2 Project Setting: 

**一. Engine :: Collision :

**1. Object Channels :**
- GameEffect - Ignore


>为什么更改后期盒子的运行模糊会改变曝光?


