
# Unreal Engine 开发基础课程

## Ⅰ. 虚幻高级程序基础

### 1.1 工程目录结构与调试基础 

#### 一. UE CPP 与 工程目录  :

**1. UE CPP 与 普通 CPP 的区别 :**

- 普通CPP无法进行内存管理 ,  如关键字`new`则是在内存中开辟堆区内存 , 在不需要使用此空间时则应该`delete`内存空间 .  
>内存的擦写成本极高 ,  清楚内存一般是修改标记 .
- 虚幻CPP 则可以自动管理内存 . 

**2. CPP工程目录结构 :**

- `.vs`:  本地的VS配置 ,  智能提示等等 .  放置配置文件 ,  缓存文件等 .  每个主机分别有一套`.vs`工作环境 .  不可删/无需打包 .
- `Binaries`:  编译文件 ,  生成`.dll`文件 ,  所有的蓝图 ,  cpp等 ,  最后都会打包为`.dll`文件 .  `.pdb`为调试链接文件 .  此文件夹可再生成 .
- `Config`:  引擎本地配置信息 .  不可删 .
- `Content` :  资产文件夹 .  不可生成 .
- `Intermediate`:  中间文件 ,  存放所有代码的过渡代码 ,  可再生成 ,  可删 .
- `Saved` :  游戏日志 ,  游戏本地配置 ,  游戏自动保存( Autosaves ) 等 .  生成文件 ,  在确定无用时可删 .
- `Source` :  源文件 .  不可删
- `.sln`:  入口文件 ,  可再生成 .
- `.uproject`:  入口文件, 删了项目作废 .
- `.db`:  缓存文件 .

>在拷贝项目时 ,  `Config` , `Content` , `Source` , `.uproject` 为重要文件 ,  其他文件可自动生成 .


#### 二. 调试与断点 :

**1. 非调试模式启动 :**

- `Ctrl + F5`:  直接唤醒虚幻引擎 .  

**2. 调试模式启动 :**

- `F5`:  准许在VS中监视引擎启动 .

**3. 断点与调试 :**

- 点击左侧面板的菜单栏可以增加断点 ,  用于在调试模式下排查调试问题 .

#### 三. 常见的错误 :

**1. 分别编译错误 :**

- CPP项目出现红色下划波浪线 ,  错误原因为程序环境无法找到头文件信息 .
- 由于CPP编译的四个步骤 :  预处理 - 编译 - 汇编 - 链接 ,  在链接阶段 ,  程序准许重复声明 ,  但禁止重复定义(程序带花括号) .
- 解决办法 :  
	- 重新生成`.sln`文件 .
	- 右键 `项目名称` 属性 ,  观察VC++目录 ,  可能是注册表路径错误 .  并且此问题不会干扰编译 .  点击右侧三角重新手动配置即可解决报错 .

**2. 启动目标平台问题 :**

- 启动目标平台一定要指定 Win64 .

#### 四. `override`关键字 :

**1. 作用 :**

- 在声明子类重写父类的虚函数时 ,  可能写错函数名 .  但是如果写了`override`关键字时 ,  编译器会帮你检查父类中是否含有此关键字 ,  如果没有同名虚幻数 ,  则报错 .  此功能可以防止比误 .


#### 五. 安装调试用符号

**1. 调试符的作用 :**

- 可以参加调试引擎 ,  用来知道引擎实现的方式 .  



### 1.2 UBT 与 UHT 详解

#### 一. 虚幻引擎的结构

**1. 虚幻引擎的整体结构 :**

- 1. 硬件层的对接 .
- 2. 中间层 ,  链路 管理工具等
-  3. 产品层 .


**2. UBT - Unreal Build Tool :**

- UBT 是一个自定义工具 ,  用于管理跨各种构建**配置构建**虚幻引擎4 ( UE4 ) 源代码过程 ,  此工具附加在引擎之外 ,  管理模块与模块之间的关系 .  
- UE4有很多模块 ,  每个模块都有一个`build.cs`控制其构建方式的文件 .
- 优势为更灵活的配置工程特性 ,  如配置AI行为的模块 ,  只需要引入AI模块即可 .  


**3. UHT - Unreal Header Tool :**

- UHT 是一个支持`UObject`系统的自定义解析和代码生成工具 .
- 专门用于做头文件的关系梳理 .  能够根据引擎的特殊标记 ( 紫色的标记宏 )  标记的不同特性给UHT使用 .  UHT 会根据这些宏生成自定义的代码以实现各种与`UObject`的功能 .  ( 当你按 F7 后 ,  UBT就开始工作了 ,  UHT会帮助你生成中间文件 ,  再进行编译 ,  比如`.generated.h`文件即是UHT帮助你生成的中间文件 .) 
- UHT的目的是为了帮助虚幻做到反射机制 ,  与蓝图通信等 .


### 1.3 项目目录结构

#### 一. visual Studio 的操作单位 :

**1. 解决方案 :**

- 每个解决方案可以看作为一个操作单位 .  解决方案里有不同区域的项目 .

**2. 源码文件与自定义文件 :**

- 源码文件对比自定义文件在标签页多出一把锁头 .  



#### 二. 项目工程目录结构 :

**1. 引用 :**
- 查询当前项目引用了哪些库 .  

**2. 外部依赖项 :**
- 我们的工程项目依赖引擎项目里的资产内容 .

**3. Config :**
- 配置文件 .

**4. Source :**
- `项目名`:  虚幻引擎默认把你创建的项目认为是一个模块 ,  这个模块的名字就是项目的名字 .

**三. 项目`Source`文件 :**

- `项目名(仅本文件夹内文件可编辑) :`
	- `Build.cs`:  `Cshape`文件 ,  UBT与UHT均是`Cshape` .  此文件作用与UBT .
	- `项目名.cpp , 项目名.h`:  注入模块 .  将自己写的模块装载/挂载至引擎中 . `IMPLEMENT_PRIMARY_GAME_MODULE( FDefaultGameModuleImpl, UECPP, "UECPP" );`宏可以把你的模块注入到引擎里 ,  模块便可以运行在引擎中 .
	- `项目名GameModeBase.cpp , 项目名GameModeBase.h`:  赠送的 ,  无用途 .
- `项目名.Target.cs , 项目名Editor.Target.cs`:  目标管理工具 ,  给编辑器使用 ,  告知编辑器引用了本模块 .  令本模块可以运行在编辑器中 .
- `项目名.uproject`:  启动名称 ,  包含版本等信息 .

>模块就类似军队的不同兵种 ,  好处为轻量化的更专精的运行 .


### 1.4 编译类型与编译状态

#### 一. 编译器运行状态 :

**1. Shipping 发行 :**

- 无控制台命令 ,  统计数据和性能分析 .  无额外性能开销 ,  可以直接满状态运行产品 .

**2. Development 开发 :**

- 允许编辑器发射查看代码更改 .

**3. DebugGame 调试游戏 :**

- 调试游戏代码 .

**4. `Debug` 调试 :**

- 必须在编辑器上加 `-debug` 参数才能反射查看代码更改 .


#### 二. 命名规则 :

**1. 虚幻引擎CPP命名规则 :**

- 模板类以T作为前缀 ,  如TArray等 .
- UObject派生类均为U前缀 .
- AActor派生类均以A为前缀 .
- `SWidget` slate类型派生类都以S为前缀 . Slate就是虚幻UI的实现框架 .
- 全局对象使用G开头 ,  如`GEngine` .  引擎实例指针 ,  为全局变量 .
- 抽象接口以I 前缀.
- 枚举 E 开头. 有穷序列的集合 .
- bool 变量均为 b 开头.
- 其他大部分或自定义数据类型为 F 开头, 如FString , FName .
- typedef 的以原型名前缀为准 . 虚幻中如果以F开头的语法要么是自定义数据, 要么是别称类型 .
```c++
//typedef 用于给负责变量起简化名称

typedef unsigned int uint;    // 将unsigned int 简化为 uint;
```
>虚幻引擎遵守帕斯卡命名法则 .  (每个单词的首字母均大写 .)

- 在编辑器和C#里, 类型名是去掉前缀过的 .
- UHT在工作的时候需要你提供正确的前缀 ,  你必须要遵守 .  


**二. 资源明明规则 :**

- Level/Map → `L_`
- Blueprint → `BP_`
- Widget Blueprint → `WBP_`
- Material → `M_`
- Static Mesh → `SM_`
- Skeletal Mesh → `SK_`
- Texture → `T_`
- Particle System → `PS_`

#### 1.5 面向对象七大原则

**一. 单一职责原则 :**
- 设计本质出发，一个类尽量专注处理一件事情，或是一类事情（管理器）
- 函数也应该只处理一件事情 .

**二. 里氏转换原则 :**
- 多态, 父类存虚函数, 子类重写他 .  将子类的指针存放在父类的指针中 ,  这个过程就叫里氏转换 .
- 当两个类存在继承关系时，子类可以替代父类（可以使用父类类型指针的地址，但是你无法使用子类指针存储父类指针的值）
- 任何基类出现的地方，子类出现是没有问题; 
- 速记法：父亲可以抱起儿子，儿子无法抱起父

**三. 依赖倒置原则 :**
- 高层模块与低层模块：往往在一个应用程序中，我们有一些低层次的类，这些类实现了一些基本的或初级的操作，我们称之为低层模块；另外有一些高层次的类，这些类封装了某些复杂的逻辑，并且依赖于低层次的类，这些类我们称之为高层模块

>例妆我们锻造首饰，那么锻造中我们可能会生产黄金，白银，青铜等手镯。我们编写一个锻造递数，通过外部传参，告知我生产类型。参数可能是原子参数(枚举)。那么当诉求发生变化时，例如函数内需要知道金属的密度，来更改锻造方法，那么方法就无法解决此问题（原因是单从参数枚举无法获知金属密度值），这时我们可！象一个金属类，类内部构建一个虚函数，用来获取密度。传递参数从枚举修改为金属类. 那么函数内就只需要依赖抽象接口函数了，不依赖细节值。

**四. 迪米特原则 :**
- 量少的去了解其他与你无关的内容 .
- 低类之间的耦合（低耦合高内聚）.

**五. 接口隔离原则 :**
- 客户端不应该依赖它不需要的接口 .  一个类对另一个类的依赖应该建立在最小(不是最少, 是最细) 的接口上 
- 使用多个专门的接口比使用单一的总接口好 ,  不要把所有的接口都声明在一起 .

**六. 开闭原则 :**
- 对需求扩展开放 ,  对修改封闭 .
- 不要改已经写好的功能 ,  当新需求产生啥应该降低对旧功能的修改, 以规避旧有逻辑的连锁反应 .

**七. 组合/ 聚合复用原则 :**
- 组合设计方案优于继承设计方案 .  继承层级越深, 子类行为, 属性越臃肿 .  
- 组合方式可以大大提升逻辑功能的复用, 并且更加灵活 .
- 缺点:  设计内容耦合 .
- 组件设计就是典型的组合设计原则 .

>程序设计没有绝对的对与错 ,  效率与结果才是验证结果的依据 .

### $ 快速引入头文件

**一. 寻找相对路径 :**

**1. 手动寻找需要引用的文件相对路径 :**
- 输入需要使用的类名 ,  快捷操作转到声明 .
- 悬停鼠标至菜单栏 ,  class 之前的路径引擎均存储为相对路径 .

**2. 使用VA获取头文件 :**
- 右键需要使用的类名, 快捷操作`Add Unclude` .
- 也可以点击类成员函数 ,  也可以快速操作 .


---
### $. 配置项目模块引用

>模块配置在 `模块名称.Build.cs`.

**一. 公有引用与私有引用 :**
- 当其他模块引用本模块时 ,  可以直接引用本模块的公有引用 .  但是无法引用到私有引用中的模块 .

>注意 ,  修改模块引用后 ,  需要重新生成`.sin`文件 ,  否则会出现文件引用错误 .

---

### $. 声明模块

>虚幻引擎只支持手动声明模块 .

>虚幻引擎对于模块的编辑不支持热加载 .

**一. 声明路径 :**
- 在项目下`Source`文件夹下创建文件夹 .  此文件夹名称必须与模块名称一致 .
- 在新创建的文件下声明`Private` 和 `Public` 注意区分大小写 .
- 创建`模块名.Build.cs` ,  并输入以下内容 :
```cs
// Copyright Epic Games, Inc. All Rights Reserved.

using UnrealBuildTool;

public class 模块名称 : ModuleRules
{
	public 模块名称(ReadOnlyTargetRules Target) : base(Target)
	{
		PCHUsage = PCHUsageMode.UseExplicitOrSharedPCHs;  //应用级别,不需要修改
	
		PublicDependencyModuleNames.AddRange(new string[] { "Core", "CoreUObject", "Engine", "InputCore" , "Paper2D" }); //输入引用的其他模块

		PrivateDependencyModuleNames.AddRange(new string[] {  });
		
		//是否启用公有文件夹?不需要注释即可
		PublicIncludePath.AddRange(new string[] { "模块名/Public" });
		//是否启用私有文件夹?不需要注释即可
		PrivateIncludePath.AddRange(new string[] { "模块名/Private" });

		// Uncomment if you are using Slate UI
		// PrivateDependencyModuleNames.AddRange(new string[] { "Slate", "SlateCore" });
		
		// Uncomment if you are using online features
		// PrivateDependencyModuleNames.Add("OnlineSubsystem");

		// To include OnlineSubsystemSteam, add it to the plugins section in your uproject file with the Enabled attribute set to true
	}
}

```

>可以抄袭其他模块的方法 .

- 在Public 或 模块跟目录中 声明头文件与源文件 .
```
模块名.h
模块名.cpp
```

- 在头文件中添加以下内容 .
```c++
// Fill out your copyright notice in the Description page of Project Settings.

#pragma once

#include "CoreMinimal.h"
#include "Modules/ModuleInterface.h"

class 模块名:public IModuleInterface
{
protected:
	virtual void StartupModule() override;
	
	virtual void ShutdownModule() override;
};

```

- 在源文件中添加以下内容
```c++
#include "模块名.h"

void 模块名::StartupModule()
{
}

void 模块名::ShutdownModule()
{
}

IMPLEMENT.MODULE(模块名(模块类型), 模块名(模块名))  //模块注入宏
```

- 在启动文件中注册新的模块 
- 在`项目名.uproject` 中
```
"Modules": [
		{
			"Name": "UECPP",
			"Type": "Runtime",
			"LoadingPhase": "Default",
			"AdditionalDependencies": [
				"Engine"
			]
		}, //这里添加逗号
		
		{
			"Name": "模块名",  //修改模块名
			"Type": "Runtime",
			"LoadingPhase": "Default" //最后一项没有逗号
			
		}
		
	]
```

- 在项目编译配置cs中注入模块 .  注意 ,  是模块之外的 ,  管理项目的cs文件 ,  不要搞混了 .
```c++
// Copyright Epic Games, Inc. All Rights Reserved.

using UnrealBuildTool;
using System.Collections.Generic;

public class UECPPTarget : TargetRules
{
	public UECPPTarget( TargetInfo Target) : base(Target)
	{
		Type = TargetType.Game;
		DefaultBuildSettings = BuildSettingsVersion.V2;
		ExtraModuleNames.AddRange( new string[] { "UECPP" ,"模块名"} );  //添加逗号,添加模块名
	}
}
```

```cs
// Copyright Epic Games, Inc. All Rights Reserved.

using UnrealBuildTool;
using System.Collections.Generic;

public class UECPPEditorTarget : TargetRules
{
	public UECPPEditorTarget( TargetInfo Target) : base(Target)
	{
		Type = TargetType.Editor;
		DefaultBuildSettings = BuildSettingsVersion.V2;
		ExtraModuleNames.AddRange( new string[] { "UECPP" ,"模块名"} );  //添加逗号,添加模块名
	}
}

```

- 最后 ,  关掉所有的虚幻编辑器 ,  并重新生成所有的解决方案 .  (最顶上的)








---
## Ⅱ. 类

### 引擎架构类

#### $. `UClass`类型基础

>在CPP的编程中 ,  无法直接使用类作为参数 .  所有的类在编译阶段均会被拆散 ,  仅保留作用域 .  为了解决这个问题 ,  虚幻引擎会在编译阶段为类生成快照 ,  `UClass` 就是虚幻用于储存类的快照模板 .  是特定的用于描述类的数据类型 .

>在Content文件夹内的C++类本质就是`UClass`快照资产 .  设计目的是将类作为参数进行传递 ,  或者传输至BP使用 .

**一. 什么是UClass ?**

**1. 原理 :**
- UClass 用来**描述类的结构信息**，支持：运行时获取类名/父类/所有属性/所有函数 ,  动态创建对象（SpawnActor/ConstructObject 等）,  支持序列化、网络同步、蓝图、GC等 .

**2. 作用 :**
- `UClass` 本质为虚幻中特定的用于描述类的数据类型 ,  继承自UObject 
- 设计目的为帮助开发者将类作为参数进行传递 ,  但传递的不是真正的类 ,  而是传递类的资产模板 .  也就是UClass 仅能存储类模板, 不能直接存储类 .
- 只有U类有传递资产模板的功能 ,  每一个U类都有一个函数为`StaticClass` ,  可以获取当前类的资产模板 .

**二. 获取类模板 :**

**1. 限制 :**
- 只有U类才有`StaticClass()`函数 ,  返回UClass 变量 ,  也就是类模板 .

#### $. `UObject`

##### `StaticClass()`获取当前实例的UClass指针 .

>返回的是一个指向`UClass`的指针 ,  这个对象包含该类的元数据 (属性, 函数, 继承关系, 序列化信息) ,  是一个用于描述类型的对象 . 

---

##### `CreateDefaultSubobject<>()` 创建默认子对象 

>模板函数 ,  返回一个实例指针 .

```c++
CreateDefaultSubobject<UPaperFlipbookComponent>(TEXT("BirdRenderComp"));  //不可以重复命名 .
```


---
#### $. Actor 类基础

>`UObject` 是塑造Actor的根 .  

**一. 创建Actor :**

**1. 静态创建 :**
- 创建Actor后直接拖拽进场景 .

**2. 动态创建 :**
- 在游戏运行中动态生成Actor .  C++的类与BP的类生成规则相同 ,  可以直接在BP中生成CPP类 .

>在编辑器启动时, 游戏已经启动了 .  这是UE的特性 .  区别在于普通蓝图的每帧运行没有被调用 .
>在创建蓝图时, 并非直接从Actor 中继承 ,  而是UBlurprintActorBase ,  这是蓝图特定的Actor类, 自带一个根组件 ,  为蓝图提供Transform .

**二. 蓝图类引用规则 :**

**1. Object Reference /  Class Reference  :**
- 硬引用 .  内存被引用时不会被释放 .

**2. Soft Object Reference
- 软引用不会影响内存的释放 .
- 软引用 ,  对于虚幻的内存管理 ,  当内存被占用时不会影响内存被释放 . 
- 出现互相引用时 ,  变量能被强制释放 .

**三. Actor 默认类构成 :**

**1. 构造函数 :**
- 禁止随意增加函数参数 ,  只能自己去额外写接口 .  所有 UObject（包括 AActor）实例都是通过引擎内部机制（如 `SpawnActor`、`NewObject`）创建的，调用的是默认构造函数，且只接受一个 `FObjectInitializer&` 参数。它不会传递你定义的额外参数 .
>仅对于虚幻原生的类禁止加参 .  自定义类可以加参 .
- 即使你自己定义了带额外参数的构造函数，也会被引擎忽略，Actor 始终由默认构造器初始化 。

**2. `BeginPlay()` :**
- 开始游戏时调用逻辑 .

**3. `Tick(float DeltaTime)`:
- 每帧运行 .

>须知, 禁止在Actor内部生成自己 ,  会导致栈溢出 .


**四. Actor 创建后运行流程 :**
- 构造函数调用 .

- 初始化成员变量 .

- 如有蓝图 ,  则初始化蓝图数据 .

- 构建组件 .
	- 并不是将组件New出来 ,  而是初始化组件的一些属性 ,  一些关系 .

- Begin Play (标志Actor 被创建到世界当中 .)

- Tick .

##### $. ==Actor 类成员函数==

**一. Actor 的创建流程 :**
- 构造函数
- 初始化成员变量
- 如有蓝图 ,  初始化蓝图数据
- 构建组件 ,  但并不是开辟新的内存 ,  而是构建属性或关系 
- `BeginPlay`
- `Tick`
---

###### `GetWorld()`获取指向当前Actor所处的UWorld实例的指针.
>返回当前Actor所属UWorld的指针 ,  可直接解引用调用函数 .


---

###### `Destroy()`强制消亡自身 .
>调用自身Destroy函数 执行强制消亡指令 .

**一. 参数说明 :**
**1. `bNetForce`**
- 是否强制网络同步删除 ?

**2. `bShouldModifyLevel`**
- true: 先修改关卡, 再删除Actor . (修改关卡即为把Actor移出场景) .
- flast: 先删除Actor ,  再修改关卡 .
---

###### `SetLifeSpan`延时删除
>延时删除 ,  功能与Destroy 类似

---

###### `Destroyed()`回调删除函数
>虚函数 ,  在进行删除操作前可以调用自身 .  

---

###### `EndPlay()`结束运行
>虚函数 ,  对象被彻底清除时回调 ,  回调会进行删除类型通知 .

```c++
EndPlay(const EEndPlayReason::Type EndPlayReason); //输入枚举

/*Destroyed 当actor 或 conponent 彻底被删除时
LevelTransition关卡切换时删除回调（非关卡流）
EndPlaylnEditor编辑器关闭时，回调通知
RemovedFromWorld关卡流切换被释放时调用
Quit游戏退出时被删除回调
*/

```


##### $. Actor 类成员属性

###### `RootComponent` 跟组件

>`USceneComponent`类型 .  用于储存跟组件 .


#### $. ==`Super` 类==
>注意, 在虚幻引擎中 ,  只要重写父类的虚函数 ,  则必须显示调用父类同名函数 .  原因如下:
>
>  - 父类中的虚函数往往包含对引擎核心行为的调用，例如 `BeginPlay()` 中设置组件初始化、`Tick()` 中处理延迟动作、检查销毁条件等。若你重写而不调用 `Super::`，这些引擎逻辑将不会执行，可能导致组件不生效、事件不触发或生命周期异常 .
>  
>  - 蓝图中重写事件，如 `BeginPlay`，也需加“Add Call to Parent Function”节点来确保父类逻辑执行

**一. 虚幻引擎变成规则 :**

**1. 原因规则 :**
- 注意, 在虚幻引擎中 ,  只要重写父类的虚函数 ,  则必须显示调用父类同名函数 .  
- 父类中的虚函数往往包含对引擎核心行为的调用，例如 `BeginPlay()` 中设置组件初始化、`Tick()` 中处理延迟动作、检查销毁条件等。若你重写而不调用 `Super::`，这些引擎逻辑将不会执行，可能导致组件不生效、事件不触发或生命周期异常 .

**2. 解决办法 :**
- 你仍然可以在执行重写函数之前或之后执行父类的函数逻辑 ,  确保程序正常运行 .
- 蓝图中重写事件，如 `BeginPlay`，也需加“Add Call to Parent Function”节点来确保父类逻辑执行 .

**二. `Super`关键字使用作用 :**

**1. 作用 :**
- Super 本质是宏 ,  也就是当前派生类的父类的别名 .
- 用于解决调用父类逻辑的问题 .

**2. 使用须知 :**
- 如果采用多继承 ,  则Super 只会指向第一继承父类 .

#### $. `USceneComponent` 场景组件类

>虚幻中大部分都场景组件都继承自此类 .

##### `SetupAttachment()` 设置父子依附关系

>设置组件与组件的父子级 .

```c++
BirdRenderComp->SetupAttachment(RootComponent);  //BirdRenderComp 组件是RootComponent 组件的子级.
```


#### `APawn`

>在虚幻引擎中代表配角

##### 成员函数

###### `SetupPlayerInputComponent` 键盘输入映射

>`PlayerInputComponent->BindAction()` 的作用就是把一个 **输入事件（Action Mapping）** 绑定到你的 **成员函数**，这样当指定按键／按钮被按下、松开等事件发生时，就会自动调用你写的函数。

```c++
PlayerInputComponent->BindAction("Jump", IE_Pressed, this, &AMyCharacter::JumpPressed);

//"Jump" —— 输入映射名，需在 Project Settings → Input → Action Mappings 里定义
//`IE_Pressed / IE_Released / IE_Repeat` —— 输入事件类型（按下／松开）
//`this` —— 输入函数所在的对象实例
//`&AMyCharacter::JumpPressed` —— 当事件发生时要调用的函数指针, 不能有参数.

```





### 功能与工具类

#### $. 全局函数

#### `LoadObject` 局部对象

>**从一个字符串路径（Asset 路径）查找并加载一个 `UObject`（引擎资源）对象**，返回其指针。如果这个资源已经在内存里，它就直接返回已有对象；否则会从磁盘加载它。


```c++
UPaperFlipbook* MyFlipbook = LoadObject<UPaperFlipbook>(  
nullptr,  
TEXT("/Game/Flipbooks/MyAnim.MyAnim")  
);  
  
if (MyFlipbook)  
{  
FlipbookComponent->SetFlipbook(MyFlipbook);  
}

//运行时如果路径正确，它会把这个动画资产载入，并返回给你一个 `UPaperFlipbook*` 指针。如果加载失败，会返回 `nullptr`
```









---

#### $. ==`FMath` 数学类==

##### `RandRange`范围内随机


```c++
RandRange<float>(0,1);
```


#### $. `UGameplayStatics` 静态功能函数库

>虚幻CPP API的功能函数库 ,  用来提供大量常用的游戏相关函数 .

#### $. UWorld非静态工厂函数类

>蓝图中也有工厂函数 .  在名称下面是否具有 '目标是xxx' 字段 ,  如有 ,  则为普通的成员函数 ,  如没有 ,  则为工厂函数 .  在CPP中 ,  添加Actor 一般使用 `UWorld` 类.

**一. `UWorld`基础 :**

**1. 什么是`UWorld`
- `UWorld` 是引擎中表示整个“世界”（地图＋运行状态）的核心对象，用于管理当前场景中所有活动元素，包括 Levels , Actors、Components、GameMode、GameState，以及关卡等组成部分 . 
- 无法直接声明UWorld实例 ,  引擎会生成多套UWorld ,  需要Actor成员函数返回指针调用 .
- [UWorld | Unreal Engine 5.6 Documentation | Epic Developer Community](https://dev.epicgames.com/documentation/en-us/unreal-engine/API/Runtime/Engine/Engine/UWorld?utm_source=chatgpt.com) 即可查看UWorld官方文档 ,  如果图标为f+绿色小方块则代表 非静态成员变量 .

---
##### `SpawnActor`生成Actor
>`UWorld::SpawnActor()` ,会在当前场景中真正生成一个新的 Actor 实例 ,  并返回被生成实例的指针 .

**一. 使用方法 :**

**1. 语法 :**
```c++
//七个重载函数,也可以为空
GetWorld->SpawnActor<输入生成的Actor类>();
GetWorld->SpawnActor<输入生成的Actor类>(UClass* inClass);
```

>虚幻的CPP反射机制可以在逻辑运行时改变类的结构 .

---

##### ==`UKismetSystemLibrary()`==


##### `PrintString()`打印字符串

```c++
PrintString(const UObject* WorldContextObject, const FString& InString, bool bPrintToScreen, bool bPrintToLog, FLinearColor TextColor, float Duration)

//WorldContextObject 输入存在UWorld里的类指针,函数可以通过此类寻找UWorld.
//InString 输入字符串
//bPrintToScreen 是否打印至屏幕?
//bPrintToLog 是否打印至Log?
//TextColor 字体颜色
//Duration 显示时间
```

---

#### $ `GEngine`全局变量

##### `AddOnScreenDebugMessage` 添加屏幕调试信息

```c++
AddOnScreenDebugMessage(int32 Key,float TimeToDisplay, FColor DisplayColor, const FDtring& DebugMessage, bool bNewerOnTop, const FVector2D& TextScale)
//Key如果是-1则添加新的消息，不会覆盖旧有消息
//如果不是-1则更新现有消息，效率更高

//bNewerOnTop当key为-1时有效，直接添加到队列最上层
```


#### $. `ConstructorHelpers` 资产操作类

##### `FObjectFinder` 加载资产

>加载一个对象资产 ,  可以在资产浏览器中复制引用 .

```c++
//声明了BirdBookObj的ConstructorHelpers::FObjectFinder<UPaperFlipbook>类型的结构体.

ConstructorHelpers::FObjectFinder<UPaperFlipbook> BirdBookObj(
		TEXT("/Game/Anim/PF_Redbird.PF_Redbird")
	);
	
	BirdRenderComp->SetFlipbook(BirdBookObj.Object);
```



---

#### $. `FInternationalization`

##### `::Get().SetCurrentCulture(Loc);`设置语言环境

>引擎和语言使用同一套语言环境 ,  配置游戏的语言环境 ,  可以配合FText 使用 .






#### $.`FPaths` 路径获取类
>储存各种路径

##### `::ProjectDir()`项目路径
>输出项目路径 .  项目文件夹的路径 .


```c++
FString FilePath = FPaths::ProjectDir() / TEXT("UECPP.uproject");
```


#### $. `FFileHelper`文件读取

##### `::LoadFileToString()`读取文件
>输入路径 ,  返回bool 值 . 通过引用传递文件内容 .

```c++
FString FileData;

FFileHelper::LoadFileToString(FileData, *FilePath);

//FileData 引用传递
//*FilePath 输入文件路径
```


### ==宏指令==

#### 标记宏
##### `__VA_ARGS__`标记宏: 可变参数宏 
>在宏中用来接收可变参数 ,  将其变成参数进行传递 .

```c++
LOG_DISPLAY(MSG, ...) UE_LOG(FLLOG, Display, TEXT("msg"), ##__VA_ARGS__);
//可以传递可变参数
```

---
##### `UFUNCTION(Exec)`指令函数声明
>指令标记 ,  在声明函数时粘贴至上一行 ,  在BeginPlay后可以在引擎内通过`~`控制台直接调用函数 .
>此功能只有在场景内存在实例时才能调用 .

>只有`GameMode` `Pawn` `Play Controller` `GameState` 可以创建指令函数


---


#### `UPROPERTY()`变量反射与声明
>它会告诉 Unreal Header Tool (UHT) 去 **生成额外的代码**，使这个成员变量可以被编辑器、蓝图、序列化（保存/加载）、垃圾回收、网络复制等系统识别和管理。

>在类中声明的每一个U类必须加上此标记宏 .


| 说明符                | 意图             |
| ------------------ | -------------- |
| EditAnywhere       | 属性在编辑器中所有地方可编辑 |
| VisibleAnywhere    | 在编辑器可见但不可编辑    |
| BlueprintReadWrite | 在蓝图中可读写        |
| BlueprintReadOnly  | 在蓝图中只能读取       |
| Category="名称"      | 编辑器详情面板中的分类名   |


#### 调试宏
---
##### `TEXT(" ")`文本构建宏
>根据平台自动生成宽字符串 .


---
##### `UE_LOG`
>发行打包版本可以屏蔽调试 
>- 日志会写入本地缓存 ,  构建日志需要传入三个参数 .

**一. 参数列表 :**

**1. 日志分类 (可以自定义):**
- 决定了内容输入到控制台的分类项 .
- 1.LogTemp .
**2. 冗余度 :**
- 1.Fatal（会终止进程）致命问题
- 2.Error错误问题(红色警告)
- 3.Warning、Display、Log(较常用的日志分类项)
- 4.Verbose（将日志详细信息记录到日志文档，但不向控制台输出）
- 5.VeryVerbose（将日志详细信息记录到日志文档，但不向控制台输出）
**3. 日志内容 :**



**二. 占位符 :**
```c++
UE_LOG(LofTemp, Log,TEXT("我今年%f岁, 身高%f厘米"), 15, 192.38);
//此时他会把%f 替换为 15 .
//我今年15岁, 身高192.38厘米
```

>引擎仅提供了一下几种占位符 :
>%d
>%s
>%f
---

##### `DECLARE_LOG_CATEGORY_EXTERN`==声明== 自定义日志分类
>注意, 此操作需要在头文件中完成, 并且只需要完成一次即可 .


```c++
DECLARE_LOG_CATEGORY_EXTERN(CategoryName, DefaultVerbosity, CompileTimeVerbosity)
//CategoryName:自定义日志分类名称开头
//DefaultVerbosity:日志默认级别,默认为Log
//CompileTimeVerbosity:日志编译级别,高于此级别的不会被编译,一版使用All

```

---

##### `DEFINE_LOG_CATEGORY()`==定义==自定义日志分类
>此操作必须在源文件中进行 

```c++
DEFINE_LOG_CATEGORY(CategoryName)
//CategoryName:自定义日志分类名称开头
```

#### 字符转码

##### `ANSI_TO_TCHAR()`



## Ⅲ. 数据类型

**一. 引擎数据类型 :**

- bool代表布尔值（永远不要假设布尔值的大小)。BOOL将不会进行编译。
-　TCHAR代表字符型（永远不要假设TCHAR的大小）。
-　uint8代表无符号字节（占1个字节）。
-　int8代表有符号的字节（占1个字节）。
-　uint16代表无符号“短整型"（占2个字节）。
-　int16代表有符号“短整型”（占2个字节）。
-　uint32代表无符号整型（占4字节）。
-　int32代表带符号整型（占4字节）。
-　uint64代表无符号”四字”(8个字节）。
-　int64代表有符号"四字”(8个字节）。UE
-　float代表单精度浮点型（占4个字节)。
-　double代表双精度浮点型（占8个字节）。
+　PTRINT一个符号整数和一个指针一样大小（用来标记指针的大小）（永远不要假设PTRINT的大小)）。


### $. 编码格式

**一. 虚幻引擎的编码 :**
- 虚幻引擎仅支持UTF8 和 UTF16的编码器 .

>编码解决的是文本问题 .  最初的编码仅支持字母 .  

**二. 文本编码的格式 :**
- 二进制, ASCII, UTF8, UTF16 .

**三. 虚幻引擎 字符串处理情景 :**
- ANSICHAR  ANSICHAR
- WINDECHAR  WINDECHAR
- TCHAR  UTB8CHAR
- CHAR  UCS2CHAR
- CHAR16  UTF16CHAR
- CHAR 32  UTF32CHAR
>虚幻引擎提供转码功能 ,  但是所有的转码目标均为TCHAR .


### $. 对象字符串
>尽量不要使用C++的`String`库 .

##### &. `FName`
>资源命名字符串 ,  FName通过一个轻型系统使用字符串 .  在此系统中 ,  特定字符串即使会被重复使用 ,  在数据表中也只储存一次 .  FNames 不区分大小写 ,  他们不可变 ,  不可被操作 .

>FNames的存储系统和静态特性决定了通过键进行FNames的查找和访问速度较快。FName子系统的另一个功能是使用散列表为FName转换提供快速字符串。

```c++
//构建 FName
	FName n1 = TEXT("am");
	FName n2(TEXT("ap"));

	//输出FName.
	UE_LOG(LogTemp, Log, TEXT("%s"), *n1.ToString());

```

 
###### `==` 运算符重载
>检测运算符是否相等 .

#####  Localization Dashboard 本地化面板

>Localization Dashboard 工具可以批量化对开发源文件中所有点`FTEXT`类型进行采集并且翻译 .

**一. 使用方法 :**

- 在`Gather from Taxt Files`选项里的`Search Directories` 中填写源文件跟目录 .
- 点击最下方的Cultures工具 ,  添加新的翻译语言 ,  并检索所有语言 .  
- 最后可以批量的根据每个`FTEXT`的命名空间与键值 , 批量化的翻译所有内容 .
- 翻译完成后 ,  记得保存 ,  并Count Words 与 Compile Text .
- 翻译后的内容会生成一个新文件 ,  储存在引擎中 .


---
###### `.ToString()` 转换至String 

---
##### & `FText`
>表示一个显示字符串，用户的显式文本都需要由FText进行处理。支持格式化文本，不提供修改函数，无法进行内容修改

```c++
//创建方式
	FText t1 = NSLOCTEXT("UECPP","k1","OK")  //参数1: 空间名称 .  参数2: 键值 .  参数3: 内容 .
```
```c++

//创建方式 二
#define LOCTEXT_NAMESPACE "UECPP" //提前声明命名空间
FText t2 = LOCTEXT("k2", "yes");  //省略提前声明命名空间
#undef LOCTEXT_NAMESPACE

```

---
###### `.ToString()` 转换至String 

---

###### `::AsPercent()` 百分比转换

>输入浮点型 ,  返回百分比 .

```c++
t1 = FText::AsPercent(0.51);

//t1 = 0.51%
```

---

###### `::AsMemory()` 打印内存

>打印内存大小 .

---
###### `::AsNumber()` 打印数字

>输入数字 .

---

###### `.EquelTo()` 比较Text 是否相等
>不在乎大小写

---

###### `.EqualToCaseIgnored()`比较Text是否相等
>在乎大小写 .

---

###### `::Format()` 格式化文本
>格式化文本 ,  

```c++
FText::Format(LOCTEXT("k3", "My name is {0}, {1}year old this year.", t1, t2));
```

---






##### &. `FString`
>可以被操作的字符串。开销大于其他类字符串类型

**一. 声明语法 :**
```c++
void AUECPPGameModeBase::FunString()
{
	FString s1 = TEXT("a");
	FString s2(TEXT("b"));
}
//注意, 字符串必须使用TEXT宏 ,  注意区分对象型字符串与字符串 .
```
>注意, 字符串必须使用TEXT宏 ,  注意区分对象型字符串与字符串 .

**二. `FString`做函数参数类型

>需要添加`const` 修饰符 ,  否则编译报错 .

```c++
void TranslateLocText(const FString Loc);
```



###### ==`*` 解引用 运算符重载:==
>将对象型字符串转字符串 ,  返回普通字符串 .

###### `==`逻辑比较 运算符重载
>可以比较字符串的长度 .


###### `/`路径分割符号 运算符重载
>跨平台的路径分割


###### `+=`/`+` 添加符
>添加字符串尾缀





###### `::FromInt()`整形转换至String
>工具函数

###### `::SanitizeFloot()`浮点转换至String
>工具函数

###### `.Contains()`是否包含子字符串
>检查此字符串是否包含子字符串 ,  返回布尔值 .

```c++
FString s5 = TEXT("Hello");
FString s6 = TEXT("ell");

bool b1 = s5.Contains(s6);
```

###### `.Find()`查找
>查找子字符串在第几位(0开始) , 

```c++
FString s5 = TEXT("Hello");
FString s6 = TEXT("ell");

int32 i2 = s5.Find(s6 , ESearchCase::CaseSensitive, ESearchDir::FromEnd);  //CaseSensitive不区分大小写,
//IgnoreCase 为区分大小写
//ESearchDir::FromEnd 开始排序位置,从后往前数.ESearchDir::FromBegin 从前往后数
```


###### `.Equels()`比较
>比较字符串是否相等 .


###### `.Replace()`替换函数
>将子初换里面的某一种字符替换为其他字符

```c++
s1 = s1.Replace(TEXT("a"), TEXT("b")); //将a替换为b
```

###### `.Split()`
>裁切字符串 ,  可以使用任何字符作为切割点 ,  分割左右两边的字符 .返回布尔值 ,  

```c++
s1 = TEXT("Hello|World");

FString Left;
FString Right;
s1.Split(TEXT("|"), &Left, &Right);  //输入指针 .
```

###### `.ParseIntoArray()` 

>最后返回Int ,  表示裁切了多少段字符 . 
```
TArray<FString> OutStr;
s1.ParseIntoArray(OutStr, TEXT("|"));  //OutStr 输入引用,存放裁切的字符串; TEXT("|")输入裁切点.
```


##### &. `FCString()`

###### `::Atoi()`非对象型字符串转int
>一定要输入非对象型字符串 .


-----
###### `::Atof()`非对象型字符串转float
>一定要输入非对象型字符串 .

-----

##### `::Printf()`格式化函数
>可以搭配占位符 ,  返回非对象字符串 .

```c++
FString s1 = Printf(TEXT("我今年%f"),18.f);
```

---

##### `.ToString()` 转换至字符串
>Text 转换至 字符串 .

```c++
*n1.ToString();  //解引用至TEXT 
```




#### $. `FVector`矢量

##### `ToString()`转换至对象字符串
>返回一个对象字符串 .
