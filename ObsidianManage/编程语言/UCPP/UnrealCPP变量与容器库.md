# 第一章 UE基础变量和模块

## 1.1 什么是Unreal CPP?

* **适应虚幻引擎的底层C++框架**

  * Module配置和插件使用
  * UBT和UHT
  * 掌握Core模块的C++库: FString, Tarray, UE_LOG......
  * 熟悉了解掌握CoreUObject: 宏, GC, 序列化......
  * 熟悉C++ 和 BP交互的方式

* **学会Unral C++的标志是什么?**

  * 懂的解决各种编译链接错误, 常是因为Module配置出错
  * 懂的一些常见的编写套路: CreateDefaultSubObject, UPROPERTY
  * 理解UObject的内存管理机制, 不会经常造成内存崩溃
  * 可以在源码里找到自己想要的代码块

* **学习条件:**

  * 熟练使用蓝图
    * 怎么算学会蓝图了?
    * 蓝图太易用, 从而导致很多人轻视蓝图;
    * 蓝图五脏俱全, 类, 结构, 枚举, 接口, 回调等;
    * 蓝图也是可以讲究设计模式和结构的!
    * 蓝图程序也要易于重构 (指责划分清楚)
    * 蓝图也可以数据驱动, 灵活掌握引擎提供的各种辅助工具, 有机结合起来
    * 可以做到, 不代表一定要时时这么做, 特别是做原型的时候;

  * 熟悉引擎GamePlay框架概念
  * 学会引擎各模块功能, 动画蓝图, AI, UMG等...
  * 总而言之, 在自己工作领域把引擎使用明白

* Unreal CPP的学习要点:

  * Core核心 `Engine->Source->Runtime->Core` :多线程,容器等底层支撑的;
    * C++基础必须要巩固! 很多人栽在这上面
    * UE4使用C++11, 所以你需要学习到能看懂.
  * GamePlay 架构
  * Module-UBT-C#
  * 反射-UHT
  * CoreUObject-GC
  * 编写套路等
  * 各个模块的"地方习俗"



## 1.2 Unreal Engine 模块

### 1.2.1 Unreal Engine 的模板

* 虚幻程序的模板使用c#语言进行连接, 因此查看插件引用了哪些模板可以通过 有几个c#文件来判断.
* 模块的本事是封装好的DLL文件.

**Unreal 代码编写规则 :**

* 模块是Unreal 中最小的单位, 因此你在创建类时, 必须继承Unreal 的模块, 即使此类没有任何作用.
* 插件的编写不限制模块引用的数量.
* 模块与模块之间可以相互依赖. 比如A 继承了 模块B; 则C需要包含 模块B时, 只需要包含模块A 即可.

---

### 1.2.2 Unreal Engine 5 的基本构成

**UE5 由以下两大块构成 :**

* 引擎 Engine
  * 3.独立程序 Programs(不是在Engine里的Programs)    一个或者多个模块(即是项目, 也是插件; 是独立的.常用于UI等)
    * 编译完的 独立程序 在 Engine里的Programs里呈现
  * 2.引擎插件 Plugins      一个或者多个模块
    * 各种各样的插件
  * 1.引擎项目 Source   一个或者多个模块
    * Editor (跟编译器相关的)
    * ThirdParty (跟第三方库相关的)
    * Developer (跟开发相关的,安卓,移动,桌面等)
    * Runtime ()
* 项目 
  * 5.项目项目       一个或者多个模块
  * 4.项目插件       一个或者多个模块

> 注意: 模块就是我们Unreal Engine最小, 最不可分割的粒度

**细节补充 :**

* Engine
  * Shaders (渲染) 
  * Content (放编译器相关的内容), 显示的效果,本地化的东西(模型,蓝图)
  * Programs (放外部独立程序编译完的东西)
  * Config (存放引擎配置)
  * Build (跟编译相关的,方便去编译的)
  * External Dependencies (存放额外的依赖项)

**使用须知 :**

* 项目插件 是完全独立的个体. 项目插件 不可以直接报考引用 项目项目; 但 项目项目 可以包含或引用 项目插件.
* 引擎插件 可以包含 引擎项目, 引擎项目 是最高级的.所有大多数东西都在 引擎项目. 引擎项目 不可以包含或引用 引擎插件.
* 独立程序 即可以使用 引擎插件; 也可以使用 引擎项目;  独立程序 与 项目 是同级的

---

### 1.2.3  Unreal Engine 5 独立程序的代码结构

**解释 :**

* 程序文件

  * `[name.Build.CSharp]` : 管理本独立程序的依赖项(模块规则); 继承自`ModuleRules`是否有依赖于Unreal里的某个模块? 类似 #include "某某模块",而本文件的目的就是以这种方式,对使用到的模块进行链接; 用它来链接DLL模块.**(也可以通过右键程序,点击Properties来进行引用管理.)**

    ```Csharp
    //[name.Build.CSharp]文件
    // Copyright Epic Games, Inc. All Rights Reserved.
    //在本文件夹内引用模板
    using UnrealBuildTool;	//UBT 编译工具
    
    public class TestCpp : ModuleRules//ModuleRules可以帮助我们定制规则
    {
    	public TestCpp(ReadOnlyTargetRules Target) : base(Target)
    	{
    		PublicIncludePathModuleNames.Add("Launch");
    		PrivateDependencyModuleNames.Add("Core");
    		PrivateDependencyModuleNames.Add("Projects");
    
    		// to link with CoreUObject module:(链接到模板的语法)
    		// PrivateDependencyModuleNames.Add("CoreUObject");
    
    		// to enable tracing:
    		// AppendStringToPublicDefinition("UE_TRACE_ENABLED", "1");
    
    		// to enable LLM tracing:
    		// GlobalDefinitions.Add("LLM_ENABLED_IN_CONFIG=1");
    		// GlobalDefinitions.Add("UE_MEMORY_TAGS_TRACE_ENABLED=1");
    	}
    }
    ```

  * `[name.Target.cs]` : 管理本独立程序 编译的目标规则 ;继承自`TargetRules` 是包含第一目标, 固定程序, 和可执行程序的地方. 比如变成 游戏 客户端 手机端 可独立运行程序之类的. 如果是普通项目,则需要包含:`Game` `Editor` 等.

    ```csharp
    // Copyright Epic Games, Inc. All Rights Reserved.
    
    using UnrealBuildTool;
    using System.Collections.Generic;
    
    [SupportedPlatforms(UnrealPlatformClass.All)]
    public class TestCppTarget : TargetRules
    {
    	public TestCppTarget(TargetInfo Target) : base(Target)
    	{
    		//Type容器储存编译目标 .Program为编译的目标为独立程序(不是Game,Editor等,更多选择在TargetRules类里)
             Type = TargetType.Program;
            
    		IncludeOrderVersion = EngineIncludeOrderVersion.Latest;	
            
    		//LinkType里储存的是链接方式.Monolithic意为使用二进制的方式进行连接
            LinkType = TargetLinkType.Monolithic;
            
            //LaunchModuleName中储存的为: 我们当前的模块是哪个? "TestCpp" 意为我们当前使用的模块叫做TestCpp(文件夹名)
    		LaunchModuleName = "TestCpp";	
    
    		// Lean and mean 是否开发编译工具? false即为不需要,保持最简.
    		bBuildDeveloperTools = false;
    
    		// Editor-only is enabled for desktop platforms to run unit tests that depend on editor-only data
    		// It's disabled in test and shipping configs to make profiling similar to the game
    		bool bDebugOrDevelopment = Target.Configuration == UnrealTargetConfiguration.Debug || Target.Configuration == UnrealTargetConfiguration.Development;
            //只有在windows平台构建,其他平台均为false
    		bBuildWithEditorOnlyData = Target.Platform.IsInGroup(UnrealPlatformGroup.Desktop) && bDebugOrDevelopment;
    
    		// Currently this app is not linking against the engine, so we'll compile out references from Core to the rest of the engine
            //是否仅编译部分内容,为我们当前的独立内容使用
    		bCompileAgainstEngine = false;	//是否针对引擎编译;
    		bCompileAgainstCoreUObject = false;	//是否针对CoreUObject编译;如果使用了,需要给一个true;
    		bCompileAgainstApplicationCore = false;	//是否开启ApplicationCore模块;
    		bCompileICU = false;
    
    		// to build with automation tests:
    		// bForceCompileDevelopmentAutomationTests = true;
    
    		// to enable tracing:
    		// GlobalDefinitions.Add("UE_TRACE_ENABLED=1");
    
    		// This app is a console application, not a Windows app (sets entry point to main(), instead of WinMain())
    		bIsBuildingConsoleApplication = true;	//是不是一个命令台程序?true的话就是一个命令台程序;不是的话则为一个独立窗口;
    	}
    }
    ```

  * Private

    * name.h 文件夹里包含着`CoreMinimal.h` 这是Unreal 5 里最核心的内容; 常用的东西都在这里.

    ```c++
    // Copyright Epic Games, Inc. All Rights Reserved.
    
    #pragma once
    
    #include "CoreMinimal.h"
    ```

    * name.cpp 负责启动引擎的功能, 如日志系统,显示长度等

    ```c
    // Copyright Epic Games, Inc. All Rights Reserved.
    
    #include "TestCpp.h"	//包含头文件
    
    #include "RequiredProgramMainCPPInclude.h"
    
    DEFINE_LOG_CATEGORY_STATIC(LogTestCpp, Log, All);
    
    IMPLEMENT_APPLICATION(TestCpp, "TestCpp");	//模块
    
    INT32_MAIN_INT32_ARGC_TCHAR_ARGV()	//程序入口, 与main()函数相同功能
    {
    	FTaskTagScope Scope(ETaskTag::EGameThread);
    	ON_SCOPE_EXIT
    	{ 
    		LLM(FLowLevelMemTracker::Get().UpdateStatsPerFrame());
    		RequestEngineExit(TEXT("Exiting"));
    		FEngineLoop::AppPreExit();
    		FModuleManager::Get().UnloadModulesAtShutdown();
    		FEngineLoop::AppExit();//程序退出,Unreal官方安全的程序退出方法
    	};
    //注册对象,启动线程等
    	if (int32 Ret = GEngineLoop.PreInit(ArgC, ArgV))
    	{	
    		return Ret;
    	}
    
    	// to run automation tests in BlankProgram:
    	// * set `bForceCompileDevelopmentAutomationTests` to `true` in `BlankProgramTarget` constructor in "BlankProgram.Target.cs"
    	// add `FAutomationTestFramework::Get().StartTestByName(TEXT("The name of the test class passed to IMPLEMENT_SIMPLE_AUTOMATION_TEST or TEST_CASE_NAMED"), 0);` right here
    
    	// to link with "CoreUObject" module:
    	// * uncomment `PrivateDependencyModuleNames.Add("CoreUObject");` in `BlankProgram` constructor in "BlankProgram.Build.cs"
    	// * set `bCompileAgainstCoreUObject` to `true` in `BlankProgramTarget` constructor in "BlankProgram.Target.cs"
    
    	// to enable tracing:
    	// * uncomment `AppendStringToPublicDefinition("UE_TRACE_ENABLED", "1");` in `BlankProgram` constructor in "BlankProgram.Build.cs"
    	// * uncomment `GlobalDefinitions.Add("UE_TRACE_ENABLED=1");` in `BlankProgramTarget` constructor in "BlankProgram.Target.cs"
    	// you may need to enable compilation of a particular trace channel, e.g. for "task" traces:
    	// * add `GlobalDefinitions.Add("UE_TASK_TRACE_ENABLED=1");` in `BlankProgramTarget` constructor in "BlankProgram.Target.cs"
    	// you'll still need to enable this trace channel on cmd-line like `-trace=task,default`
    
    	// to enable LLM tracing, uncomment the following in `BlankProgram` constructor in "BlankProgram.Build.cs":
    	// GlobalDefinitions.Add("LLM_ENABLED_IN_CONFIG=1");
    	// GlobalDefinitions.Add("UE_MEMORY_TAGS_TRACE_ENABLED=1");
    
    	UE_LOG(LogTestCpp, Display, TEXT("Hello World"));
    
    	return 0;
    }
    
    ```

    ---

## 1.3 原生基础变量和UE5基础变量对比

**Unreal中,变量的语法与原生CPP略有不同 :**

| C/CPP                | Unreal 4/Unreal 5 | 解释                                   |
| -------------------- | ----------------- | -------------------------------------- |
| `int`                | `int32`           | 占32位内存,整形, 代表整数              |
| `char`               | `int8`            | 占8位内存,字节,C语言风格字符串         |
| `short int`          | `int16`           | 占16位内存, 可以表示更大的整数         |
| `float`              | `float`           | 占32位内存,可以表示小数                |
| `double`             | `double`          | 占32位内存,可以表示小数                |
| `long long`          | `int64`           | 占64位内存,可以表示整数,表示更大的整数 |
| `unsigned char`      | `uint8`           | 无符号字节,                            |
| `unsigned short`     | `uint16`          | 无符号的大整数                         |
| `unsigned int`       | `uint32`          | 无符号整数                             |
| `unsigned long long` | `uint64`          | 无符号超大字节                         |
| `char`               | `ANSICHAR`        | 字符                                   |
| `wchar_t`            | `WIDECHAR`        | 宽字符,储存更多的内容                  |
| `wchar_t`            | `TCHAR`           | Fstring的操作符                        |
| `unsigned char`      | `UTF8CHAR`        |                                        |
| `unsigned short`     | `UCS2CHAR`        |                                        |
| `unsigned short`     | `UTF16CHAR`       |                                        |
| `unsigned int`       | `UTF32CHAR`       |                                        |

**为什么Unreal 4/5 要自己定义结构?**

* Unreal是跨平台的引擎, 不同平台表现性质不一样

* 兼容反射, UBT会编译代码,本质为一行行去扫代码. 写成更短的形式有利于扫行

* 宏替换 做宏替换时,中间有空格的话容易出错.

* ```c++
  #define xxxx(v) v##_Name xx;
  xxxx(long long) 
      long long_Name //错误
  ```

---

# 第二章 FString字符串精讲

## 2.1 FString中文问题和基础赋值问题

**一. FString 的定义 :**

* 类似于CPP的模板库中的`string` 是专门针对的字符串的模板
* FString支持Tchar和Char

**二. FString 的使用场景与语法 :**

* 1.赋值现写的字符串

  * 语法 :`FString 变量名 = TEXT("字符串具体内容");` 

  * ```c++
    FString S = TEXT("ddddd");
    ```

* 2.将现成字符串赋值给新定义的字符串

  * 1 语法: `FString 变量名 = 想要给此变量赋值的变量` 

  * 2 语法: `FString 变量名(想要给此变量赋值的变量)`

  * ```c++
    FString S2 = S;
    FString S2(S);

* 3.使用指针对字符串赋值

  * 1 语法: `变量名 = 指针;`

  * ```c++
    TCHAR* p = TEXT("safdhuafhsd");
    S2 = p;
    ```

* 4.更改字符串前n个字符为

  * 1 语法:` FString 变量名(n(整数),内容);`

  * ```c++
    FString S2(5,P);	//将前5个字符更改为p
    ```

**三.FString的本质 :**

* FString的本质是一个容器, 形式上是一个类.
* 此容器的ArrayMax(最大储存极限)与ArrayNum(最小储存极限), 在正常情况下是相等的, 他的ArrayNum就是此容器内原本字符串的数量. ArrayMax则是计算了预留空间的数值. 在我们没有给此容器预留空间时, ArrayMax等于ArrayNum.使用语法==`FString 容器名(具体字符串,预留空间)`== 可更改ArrayMax, 但不会更改ArrayNum;

**四.FString中文的问题 :**

* 源文件编码格式必须为UTF-8
* 将UTF8转换为TCHAR,语法为==`TCHAR *p = UTF8_TO_TCHAR(S);`==
* 将TCHAR转换为FString;

**五.FString 兼容性问题 :**

* 兼容TCHAR char
* 如果转char, 需要用UTF8_TO_CHAR这种形式 如果直接转 则要求里面不能有中文

---

## 2.2 FString遍历和预分配内存的数据清除















































































