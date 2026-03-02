
# UCPP基础语法

## Level Ⅰ .  部署测试环境 :  将主函数暴露至 Blueprint

### 1.1 在模块下创建测试环境

**一. 在默认模块下创建文件 :**

- 创建位置 :
	- `项目跟目录 / Source / 默认模块 `

- 创建内容 :
	- 创建一个自定义名称的源文件与头文件 .


**二. UCPP的必要宏包含引用 :**

```c++
#include "CoreMinimal.h"    //包含运行UPP的基础内容

#include "文件名.generated.h"    //将此文件的代码映射到BludePrint时产生的胶水代码储存在此文件内

```

**三. 启动程序代码 :**
- 头文件 :
```c++
#pragma once

#include "CoreMinimal.h"
#include "TestFunctionLibrary.generated.h" // TestFunctionLibrary为文件名

UCLASS() 
class UTestFunctionLibrary : public UObject
{
	GENERATED_UCLASS_BODY() 

public:
	UFUNCTION(BlueprintCallable) //宏代码,反射至蓝图.
	static int32 K2_Main();
};
```

- 源文件
```c++
#include "头文件.h"

UE_DISABLE_OPTIMIZATION

UTestFunctionLibrary::UTestFunctionLibrary(class FObjectInitializer const& InObjectInitializer)
    :Super(InObjectInitializer)
{

}

int32 UTestFunctionLibrary::K2_Main()
{
    return int32();
}

UE_DISABLE_OPTIMIZATION
```

### 1.2 封装至插件

**1. 创建插件 :**

- 在左上角可以创建插件 .
### 1.3 屏幕和日志打印

**一.`GEngine->AddOnScreenDebugMessage()`在屏幕上打印字符串**
```c++
// 参数列表
UEngine::AddOnScreenDebugMessage
(
	int32 Key, // 消息的唯一标识符(用于避免重复或覆盖) Key = -1：适合临时消息，每次调用新增一条。 Key >= 0：适合需要更新的消息，相同 Key 会覆盖旧消息。
    
	float TimeToDisplay, // 显示时间
	FColor DisplayColor, //显示颜色
	const FString& DebugMessage, //打印文字
	bool bNewerOnTop, //默认为 true。新消息显示在屏幕顶部，否则显示在底部。
	const FVector2D& TextScale //字体缩放
)
```

>字符串输入方法 :
>	`TEXT("输入的问题")`


**二.`UE_LOG()`在日志上打印字符串**

**注意 :**
- `UE_LOG()` 是一个有参宏 .  并非函数 .
```c++
#define UE_LOG(
    CategoryName, 
    Verbosity, 
    Format, 
    ...
    )
```

**一. 参数列表解析 :**

- `CategoryName` :  指定日志的分类名称。每个日志分类通常对应一个特定的模块或系统（如渲染、动画、网络等）
- `Verbosity` :  指定日志的详细级别，控制日志的输出级别。不同的级别表示不同的重要性和用途。
	-  `Fatal`: 致命错误，通常会导致程序崩溃。
    
	- `Error`: 错误信息，表示严重问题。
    
	- `Warning`: 警告信息，表示潜在问题。
    
	- `Display`: 显示信息，通常用于重要的运行时信息。
	
	- `Log`: 一般日志信息，用于调试。
    
	- `Verbose`: 详细日志信息，用于更详细的调试。
    
	- `VeryVerbose`: 非常详细的日志信息，通常用于极端情况下的调试。
- `Format` :  输入字符串 .

>日志打印所处文件夹位于 :  /项目文件 / Saved / Logs / 项目名称.log

### 1.4 常用的宏指令

**1.`CoreMinimal.h` 虚幻基础核心功能库**

- 提供必要的数学运算函数等方法 .

**2.`UFUNCTION(BlueprintCallable)`将函数反射至蓝图**



**2. 在默认模块下创建新目录并引用 :**

- 需要以使用`/`标明路径 . 例如
```c++
#include "Test01/Test01.h"
```

## Level Ⅱ .  变量

### 2.1 基础变量

**一. 原生CPP变量 :**

**1.`bool`布尔型 :**

- 代表布尔 ,  Ture = 1 ; false = 0 ;

**2.`char`字符

- 表示单个字符

**3.`int32`整形

- 考虑跨平台之后的整形表达 .

**4.`float`浮点型**

- 表达小数 `0.f`  .

**5.`double`高精度浮点型

- 更精确的浮点型 ,  使用 `0.0` 表达 .

**6.`long`更短的整形

- `10l`

**7.`long long`更长的整形

- 数值更大的整形 .

>`signed`修饰符 ,  表达变量为有符号变量 .  `unsigned` ,  表示为无符号变量 .

**8.`wchar_t`字符串

- 表示字符串


### 2.2 Unreal 内置变量 :

**1.`int8`

- 本质为`signed char` ,  虚幻放置变量中空格做的优化代码 .  源码为`typedef signed char    int8`

>`typedef`关键字 ,  类似引用 .  语法为 `typedef 被定义 定义名`


**2.`int16`

- 本质为`int` ,  32位 4 个字节 .  `typedef signed short int	int16;` 

**3. `int 32`**

- 普通的有符号int

**4. `int64`**

- 值域更大的整形 .  赋值尾缀后常加 ll .  

**5. `ANSICHAR` 字符串 :**

- 窄字符串

**6. `WIDECHAR`宽字符串 :**



**二. 变量标识符 :**

**1. `u`无符号:**

- 代表正数无符号范围 .

