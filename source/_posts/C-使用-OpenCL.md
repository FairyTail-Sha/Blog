---
title: 'C# 使用 `OpenCL`'
date: 2018-12-08 14:56:08
tags:
---

## 使用`OpenCL.Net`小demo尝试

## `openTK` vs `openCL.NET`

>`OpenTK` is a C# interface to `OpenGL`. `OpenCL.Net` is a C# interface to `OpenCL`.
>
>`OpenGL` is `OpenGL`, built for one purpose. `OpenCL` is `OpenCL`, built for another purpose.
>
>`OpenTK` has `OpenCL.Net` bindings, so you can actually use `OpenCL` with `OpenTK`.

## OpenCL and C#
### Example
For C# there exist many wrappers that offer an interface to communicate with OpenCL.

`OpenCL.NET`: 

This is one of the most low level wrappers out there. It offers a complete implementation of the OpenCL API for C# without adding any abstraction at all. So C\C++ examples are easily ported for this library. The only project page is currently on codeplex, which shuts down on 15.12.2017 but the package is available on NuGet

https://openclnet.codeplex.com/

`NOpenCL`: 

This library offers an abstract interface between C# and OpenCL.
The short-term goal is providing an easy-to-use abstract layer which provides access to the full capability of OpenCL without sacrificing performance.

https://github.com/tunnelvisionlabs/NOpenCL

`Cloo`:
`Cloo` is an open source, easy to use, managed library which enables .NET/Mono applications to take full advantage of the OpenCL framework.

https://sourceforge.net/projects/cloo/

## OpenCL 路难走啊
