---
title: math.net overview
date: 2019-01-11 13:44:53
tags:
---

math.net
Math.NET Numerics is an opensource numerical library for .Net.
Math.NET初衷是开源建立一个稳定并持续维护的先进的基础数学工具箱，以满足.NET开发者的日常需求。

Math.NET Numerics
Math.NET Numerics是核心功能是数值计算。主要是提供日常科学工程计算相关的算法，包括一些特殊函数，线性代数，概率论，随机函数，微积分，插值，最优化等相关计算功能。

目前项目涉及到的计算仅有矩阵相关的，另外尝试使用其中的与BLAS相结合的内容

Math.NET Spatial
Math.NET Spatial旨在成为.Net和Mono的几何库。

dense matrix 密集矩阵

```cs
MathNet.Numerics.LinearAlgebra CreateMatrix
Public Static Functions
    Matrix<T> Dense<T>(int rows, int columns, T[] storage)
    //Create a new dense matrix with the given number of rows and columns directly binding to a raw array. The array is assumed to be in column-major order (column by column) and is used directly without copying. Very efficient, but changes to the array and the matrix will affect each other.
    Matrix<T> Dense<T>(int rows, int columns, Func<int, int, T> init)
    //Create a new dense matrix and initialize each value using the provided init function.
    Matrix<T> Dense<T>(int rows, int columns, T value)
    //Create a new dense matrix and initialize each value to the same provided value.
    Matrix<T> Dense<T>(int rows, int columns)
    //Create a new dense matrix with the given number of rows and columns. All cells of the matrix will be initialized to zero. Zero-length matrices are not supported.
    Matrix<T> Dense<T>(DenseColumnMajorMatrixStorage<T> storage)
    //Create a new dense matrix straight from an initialized matrix storage instance. The storage is used directly without copying. Intended for advanced scenarios where you're working directly with storage for performance or interop reasons.
    //直接从初始化的矩阵存储实例创建一个新的密集矩阵。存储直接使用, 无需复制。适用于高级方案, 在这些方案中, 您直接使用存储以获得性能或互操作的原因。

    Matrix<T> DenseDiagonal<T>(int rows, int columns, Func<int, T> init)
    // 对角矩阵 Create a new diagonal dense matrix and initialize each diagonal value using the provided init function.
```

定义非零元素的总数比上矩阵所有元素的总数为矩阵的稠密度。
在矩阵中，若数值为0的元素数目远远多于非0元素的数目，并且非0元素分布没有规律时，则称该矩阵为稀疏矩阵；与之相反，若非0元素数目占大多数时，则称该矩阵为稠密矩阵。
