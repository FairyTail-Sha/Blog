---
title: DLL使用概述
date: 2019-03-13 15:45:55
tags: C++ 混合编程
---

*最近在一直做C#桌面绘图软件的算法并行优化，最后发现C#就是那样适合业务逻辑，
和界面的开发，很多高效的计算需要借助混合编程，充分使用各种语言的优势*

# DLL概述

先放代码，然后一点点解释【也是网络上查的，总结】

```c
// dllTest.h
extern "C" _declspec(dllexport) int __stdcall test01(int a, int b, int c);

// dllTest.cpp
#include "stdafx.h"
#include "CreateDll.h"
int __stdcall test01(int a, int b,int c)
{
    return a + b + c;
}

```

## `extern "C"`

extern "C"说明导出函数使用C编译器，则函数名遵循C编译器的函数名修饰规则，  
不加extern "C"说明使用C++编译器的函数名修饰规则，两种规则区别如下：  

* （1）C编译器的函数名修饰规则  
  对于`__stdcall`调用约定，编译器和链接器会在输出函数名前加上一个下划线前缀，函数名后面加上一个“@”符号和其参数的字节数，例如`_functionname@number`。`__cdecl`调用约定仅在输出函数名前加上一个下划线前缀，例如`_functionname`。`__fastcall`调用约定在输出函数名前加上一个“@”符号，后面也是一个“@”符号和其参数的字节数，例如`@functionname@number`
  
* （2）C++编译器的函数名修饰规则  
C++的函数名修饰规则有些复杂，但是信息更充分，通过分析修饰名不仅能够知道函数的调用方式，返回值类型，甚至参数个数、参数类型。不管`__cdecl`，`__fastcall`还是`__stdcall`调用方式，函数修饰都是以一个“?”开始，后面紧跟函数的名字，再后面是参数表的开始标识和按照参数类型代号拼出的参数表。对于`__stdcall`方式，参数表的开始标识是“@@YG”，对于`__cdecl`方式则是“@@YA”，对于`__fastcall`方式则是“@@YI”。

C++编译时函数名修饰约定规则：

 __stdcall调用约定：  

 1、以"?"标识函数名的开始，后跟函数名；

 2、函数名后面以"@@YG"标识参数表的开始，后跟参数表；

 3、参数表以代号表示： 

| 符号 | 含义          |
| ---- | ------------- |
| X    | void          |
| D    | char          |
| E    | unsigned char |
| F    | short         |
| H    | int           |
| I    | unsigned int  |
| J    | long          |
| K    | unsigned long |
| M    | float         |
| N    | double        |
| _N   | bool          |
| ...  | ...           |

 PA--表示指针，后面的代号表明指针类型，如果相同类型的指针连续出现，以"0"代替，一个"0"代表一次重复

4、参数表的第一项为该函数的返回值类型，其后依次为参数的数据类型,指针标识在其所指数据类型前；

5、参数表后以"@Z"标识整个名字的结束，如果该函数无参数，则以"Z"标识结束。

      其格式为"?functionname@@YG*****@Z"或"?functionname@@YG*XZ"，例如    
      int Test1(char *var1, unsigned long)-----?Test1@@YGHPADK@Z

      void Test2()-----"?Test2@@YGXXZ" 

__cdecl调用约定:

规则同上面的_stdcall调用约定，只是参数表的开始标识由上面的"@@YG"变为"@@YA"。

__fastcall调用约:

规则同上面的_stdcall调用约定，只是参数表的开始标识由上面的"@@YG"变为"@@YI"。

| C++            | 描述           | C#      | 描述         | 字节数                   |
| -------------- | -------------- | ------- | ------------ | ------------------------ |
| char           | 字符           | sbyte   | 字节         | 1                        |
| usigned char   | 无符号字符     | byte    | 字节         | 1                        |
| wchar_t        | 无符号字符     | char    | 字符         | 2                        |
| bool           | 布尔值         | byte    | 字节         | 1                        |
| short          | 短整型         | short   | 短整型       | 2                        |
| unsigned short | 无符号短整型   | ushort  | 无符号短整型 | 2                        |
| int            | 整型           | int     | 整型         | 4                        |
| unsigned int   | 无符号整型     | uint    | 无符号整型   | 4                        |
| long           | 长整型         | int     | 整型         | 4                        |
| unsigned long  | 无符号长整型   | uint    | 无符号整型   | 4                        |
| float          | 单精度浮点数   | float   | 单精度浮点数 | 4                        |
| double         | 双精度浮点数   | double  | 双精度浮点数 | 8                        |
| long double    | 长双精度浮点数 | decimal | --           | long double-8 decimal-10 |

# 调用 DLL 函数

https://docs.microsoft.com/zh-cn/dotnet/framework/interop/calling-a-dll-function

从平台调用返回的结构必须是在托管代码和非托管代码中表示形式相同的数据类型。 

这些类型称为 **blittable 类型**，因为它们不需要转换（请参阅 Blittable 类型和非 Blittable 类型）。 

若要调用返回类型为 non-blittable 结构的函数，可定义与 non-blittable 类型大小相同的 blittable 帮助程序类型，并在函数返回后转换数据。

以下 System 命名空间中的类型即是 **blittable 类型**：

* System.Byte
* System.SByte
* System.Int16
* System.UInt16
* System.Int32
* System.UInt32
* System.Int64
* System.UInt64
* System.IntPtr
* System.UIntPtr
* System.Single
* System.Double

| Non-blittable  类型 | 说明                                           |
| ------------------- | ---------------------------------------------- |
| System.Array        | 转换为 C 样式数组或 SAFEARRAY。                |
| System.Boolean      | 转换为 1、2 或 4 字节的值，true 表示 1 或 -1。 |
| System.Char         | 转换为 Unicode 或 ANSI 字符。                  |
| System.Class        | 转换为类接口。                                 |
| System.Object       | 转换为变量或接口。                             |
| System.Mdarray      | 转换为 C 样式数组或 SAFEARRAY。                |
| System.String       | 转换为空引用中的终止字符串或转换为 BSTR。      |
| System.Valuetype    | 转换为具有固定内存布局的结构。                 |
| System.Szarray      | 转换为 C 样式数组或 SAFEARRAY。                |

传递结构