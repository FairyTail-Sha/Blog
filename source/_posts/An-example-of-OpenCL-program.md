---
title: An example of OpenCL program
date: 2019-01-10 11:24:24
tags: OpenCl
---

In this section we will discuss(详述) all **the necessary steps** to run an OpenCL application.

# Basic software requirements

略【基础软件配置】

# Installing and setting up an OpenCL compliant computer

安装CUDA即可，因为其中会带有OpenCL

# 在 opencl 中实现 saxpy 例程

```C
// 普通C代码
void saxpy(int n, float *a, float *b, float *c)
{
    for (int i = 0; i < n; ++i)
        c[i] = n*a[i] + b[i];
}
```

```c
// OpenCL Code
#include <stdio.h>
#include <stdlib.h>
#include <CL/cl.h>
#define VECTOR_SIZE 1024

// Kernel 程序
const char *saxpy_kernel =
"__kernel                                   \n"
"void saxpy_kernel(float alpha,     \n"
"                  __global float *A,       \n"
"                  __global float *B,       \n"
"                  __global float *C)       \n"
"{                                          \n"
"    //Get the index of the work-item       \n"
"    int index = get_global_id(0);          \n"
"    C[index] = alpha* A[index] + B[index]; \n"
"}                                          \n";

int main(void)
{
    int i;
// 测试数据初始化
    float alpha = 2.0f;
    float *A = (float *)malloc(sizeof(float) * VECTOR_SIZE);
    float *B = (float *)malloc(sizeof(float) * VECTOR_SIZE);
    float *C = (float *)malloc(sizeof(float) * VECTOR_SIZE);
    for (i = 0; i < VECTOR_SIZE; i++)
    {
        A[i] = i;
        B[i] = VECTOR_SIZE - i;
        C[i] = 0;
    }
//平台-设备-上下文-命令设置
    // 平台
    cl_platform_id * platforms = NULL;
    cl_uint num_platforms;

    cl_int clStatus = clGetPlatformIDs(0, NULL, &num_platforms); // 获得平台数目
    platforms = (cl_platform_id*)malloc(sizeof(cl_platform_id)* num_platforms); // 根据平台数目为 platforms 分配空间
    clStatus = clGetPlatformIDs(num_platforms, platforms, NULL); // 获得平台id s

    // 设备
    cl_device_id *device_list = NULL;
    cl_uint num_devices;

    clStatus = clGetDeviceIDs(platforms[0], CL_DEVICE_TYPE_GPU, 0, NULL, &num_devices);
    device_list = (cl_device_id*)malloc(sizeof(cl_device_id)* num_devices); // 根据设备数目为 device_list 分配空间
    clStatus = clGetDeviceIDs(platforms[0], CL_DEVICE_TYPE_GPU, num_devices, device_list, NULL); // 获得设备id s

    // 上下文
    cl_context context;
    context = clCreateContext(NULL, num_devices, device_list, NULL, NULL, &clStatus);

    // command queue
    cl_command_queue command_queue = clCreateCommandQueue(context, device_list[0], 0, &clStatus);
// 计算前准备，开辟空间 Copy数据
    // 为每个数组开辟空间
    cl_mem A_clmem = clCreateBuffer(context, CL_MEM_READ_ONLY, VECTOR_SIZE * sizeof(float), NULL, &clStatus);
    cl_mem B_clmem = clCreateBuffer(context, CL_MEM_READ_ONLY, VECTOR_SIZE * sizeof(float), NULL, &clStatus);
    cl_mem C_clmem = clCreateBuffer(context, CL_MEM_READ_ONLY, VECTOR_SIZE * sizeof(float), NULL, &clStatus);

    // Copy A,B to device | Host -> Device
    clStatus = clEnqueueWriteBuffer(command_queue, A_clmem, CL_TRUE, 0, VECTOR_SIZE * sizeof(float), A, 0, NULL,NULL);
    clStatus = clEnqueueWriteBuffer(command_queue, B_clmem, CL_TRUE, 0, VECTOR_SIZE * sizeof(float), B, 0, NULL, NULL);
// Program - Create Build
    // Create Program from kernel source code
    cl_program program = clCreateProgramWithSource(context, 1, (const char **)&saxpy_kernel, NULL, &clStatus);

    // Build Program
    clStatus = clBuildProgram(program, 1, device_list, NULL, NULL, NULL);
// Kernel - Create SetArgs Execute
    // Create OpenCL Kernel
    cl_kernel kernel = clCreateKernel(program, "saxpy_kernel", &clStatus);

    // Set args of kernel
    clStatus = clSetKernelArg(kernel, 0, sizeof(float),(void *)&alpha );
    clStatus = clSetKernelArg(kernel, 1, sizeof(cl_mem), (void *)&A_clmem);
    clStatus = clSetKernelArg(kernel, 2, sizeof(cl_mem), (void *)&B_clmem);
    clStatus = clSetKernelArg(kernel, 3, sizeof(cl_mem), (void *)&C_clmem);

    // Execute the OpenCL kernal
    size_t global_size = VECTOR_SIZE; // 要处理的整个列表
    size_t local_size = 64; // 一次要处理的数量
    clStatus = clEnqueueNDRangeKernel(command_queue, kernel, 1, NULL, &global_size, &local_size, 0, NULL, NULL);
// 读取结果
    // Read C  | Device -> Host
    clStatus = clEnqueueReadBuffer(command_queue, C_clmem, CL_TRUE, 0, VECTOR_SIZE * sizeof(float), C, 0, NULL, NULL);
// 后续处理 - 等待 显示 释放资源
    // Clean up and wait for all the comands to complete.
    clStatus = clFlush(command_queue);
    clStatus = clFinish(command_queue);

    // Display
    for (size_t i = 0; i < VECTOR_SIZE; i++)
    {
        printf("%f * %f + %f = %f\n", alpha, A[i], B[i], C[i]);
    }

    // Finally release all OpenCL allocated objects and host buffers;
    clStatus = clReleaseKernel(kernel);
    clStatus = clReleaseProgram(program);
    clStatus = clReleaseMemObject(A_clmem);
    clStatus = clReleaseMemObject(B_clmem);
    clStatus = clReleaseMemObject(C_clmem);
    clStatus = clReleaseCommandQueue(command_queue);
    clStatus = clReleaseContext(context);

    free(A);
    free(B); free(C);
    free(platforms);
    free(device_list);

    system("pause");
    return 0;
}
```

>**C++字符换行**  
在C/C++语言中，可能我们要书写的一个字符串太长了，放在一行上影响代码的可读性。这时我们就需要多行书写了。  
字符串多行书写有两种规则：  
>
> * 在字符串换行处加一个反斜杠’\’，下一行前不能有空格或者Tab键；
> * 使用双引号。

# OpenCL program flow

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
15. Finally releases all OpenCL allocated objects and host buffers.

```C
" __kernel void matrix_mult("
"     const int Ndim,"
"     const int Mdim,"
"     const int Pdim,"
"     __global const float* A, "
"     __global const float* B, "
"     __global float* C)"
" {"
"     int i = get_global_id(0);"
"     int j = get_global_id(1);"
" "
"     int k;"
"     float tmp;"
" "
"     if ((i < Ndim) && (j < Mdim)) {"
"         tmp = 0.0;"
"         for (k = 0; k < Pdim; k++)"
"             tmp += A[i*Pdim + k] * B[k*Mdim + j];"
"         C[i*Mdim + j] = tmp;"
"     }"
" }"
```