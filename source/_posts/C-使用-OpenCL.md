---
title: 'C# 使用 `OpenCL`'
date: 2018-12-08 14:56:08
tags: OpenCL
---

# 使用`OpenCL.Net`小demo尝试

## `openTK` vs `openCL.NET`

>`OpenTK` is a C# interface to `OpenGL`. `OpenCL.Net` is a C# interface to `OpenCL`.
>
>`OpenGL` is `OpenGL`, built for one purpose. `OpenCL` is `OpenCL`, built for another purpose.
>
>`OpenTK` has `OpenCL.Net` bindings, so you can actually use `OpenCL` with `OpenTK`.

## OpenCL and CS

### Example

For C# there exist many wrappers that offer an interface to communicate with OpenCL.

`OpenCL.NET`:  
This is one of the most low level wrappers out there. It offers a complete implementation of the OpenCL API for C# without adding any abstraction at all. So C\C++ examples are easily ported for this library. The only project page is currently on codeplex, which shuts down on 15.12.2017 but the package is available on NuGet

<https://openclnet.codeplex.com/>

`NOpenCL`:  
This library offers an abstract interface between C# and OpenCL.
The short-term goal is providing an easy-to-use abstract layer which provides access to the full capability of OpenCL without sacrificing performance.

<https://github.com/tunnelvisionlabs/NOpenCL>

`Cloo`:
`Cloo` is an open source, easy to use, managed library which enables .NET/Mono applications to take full advantage of the OpenCL framework.

<https://sourceforge.net/projects/cloo/>

## OpenCL 路难走啊

# 矩阵乘法 使用`OpenCL`实现

## `OpenCL`使用基本过程

1. Allocates memory for host buffers and initializes them.
2. Gets platform and device information. This is discussed in detail in Chapter 2, OpenCL Architecture.
3. Sets up the platform.
4. Gets the devices list and chooses the type of device you want to run on.
5. Creates an OpenCL context for the device.
6. Creates a command queue.
7. Creates memory buffers on the device for each vector.
8. Copies the Buffer A and B to the device.
9. Creates a program from the kernel source.
10. Builds the program and creates the OpenCL kernel.
11. Sets the arguments of the kernel.
12. Executes the OpenCL kernel on the device.
13. Reads back the memory from the device to the host buffer. This step is optional, you may want to keep the data resident in the device for further processing.
14. Cleans up and waits for all the commands to complete.
15. Finally rele

## 实现过程

1. Allocates memory for host buffers and initializes them.
2. Gets platform and device information. This is discussed in detail in Chapter 2, OpenCL Architecture.
3. Sets up the platform.
4. Gets the devices list and chooses the type of device you want to run on.
5. Creates an OpenCL context for the device.
6. Creates a command queue.
7. Creates memory buffers on the device for each vector.
8. Copies the Buffer A and B to the device.
9. Creates a program from the kernel source.
10. Builds the program and creates the OpenCL kernel.
11. Sets the arguments of the kernel.
12. Executes the OpenCL kernel on the device.
13. Reads back the memory from the device to the host buffer. This step is optional, you may want to keep the data resident in the device for further processing.
14. Cleans up and waits for all the commands to complete.
15. Finally rele