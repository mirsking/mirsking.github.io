---
layout: post
title:  "Visual Studio添加Armadillo线性代数库实现矩阵运算"
date: 2014-03-16 10:05 +0800
categories:  工具配置
---

Armadillo是一个C++开发的线性代数库，其可以使用LAPACK进行加速，从而取得了速度和易用性上的平衡。从而可以方便高效地把Matlab中的程序方便地移植到C++中。  

Armadillo也提供了python的接口armanpy。同时在Armadillo基础上也开发了许多实用的包，如用于快速机器学习的MLPACK。  
详细介绍可以看官网：[Armadillo](http://arma.sourceforge.net/)

由于最近做毕设需要将Matlab的算法移植到C++上，所以安装了这个库，虽然简单，却也学习到了一些东西。  

下面是安装过程及一些要注意的，记录一下，方便以后查阅。有需要的童鞋也可以一步一步跟着做一做。

###  到官网下载Armadillo的源文件  

 这里要注意的是，今天为止最新的版本是armadillo-4.100.2.tar.gz。而4.0以后的版本貌似只支持visual studio 2010以上的编译器，如果使用的是visual studio 2008的编译器，会提示编译器版本过低。所以像我这样不得不用2008的就只能下载低版本：我使用的是armadillo-3.920.4.tar.gz，测试可用。下边是下载链接。  

[armadillo-3.930.4.tar.gz（2008可用）](http://sourceforge.net/projects/arma/files/armadillo-3.930.4.tar.gz/download)  

[armadillo-4.100.2.tar.gz（2010以上版本可用）](http://sourceforge.net/projects/arma/files/armadillo-4.100.2.tar.gz/download)  

### 安装方法
以armadillo-3.930.4.tar.gz为例，下载完成后直接解压到任意目录，比如：X:/armadillo-3.930.4/  
将X:/armadillo-3.930.4/include加入到visual studio 包含文件目录中，VS2008 是在 “工具-\>选项-\>VC++目录-\>包含文件”这里设置；vs2013则更改到了项目属性页中。

### 安装 LAPACK  
[官网地址](http://icl.cs.utk.edu/lapack-for-windows/lapack/index.html) 但是其需要单个文件单独下载，而且下载速度不敢恭维。所以我这里打包上传到了百度网盘，下载请[点击](http://pan.baidu.com/s/1o6I1Mxc)。压缩包中有WIN32位和WIN64位两种dll和lib文件，可以根据需要选择。  

下载后，同样解压到任意文件夹，如：X:\\Lapack\\  
以WIN32为例：  
将X:\\Lapack\\WIN32添加到visual studio 库文件目录中，方法同上。

###  打开Armadillo的LAPACK
功能（armadillo4.0以上的版本，默认打开，请直接跳过）。  

打开X:/armadillo-3.930.4/include\\armadillo\_bits目录中的config.hpp文件。  
找到并将以下两行前的注释去掉，改为：  

```cpp
#define ARMA_USE_LAPACK  
#define ARMA_USE_BLAS
```

### 写个测试程序

```cpp
#include <iostream>  
#include <armadillo>  
int main()  
{  
arma::mat A = arma::randu(5, 5) * 10;  
arma::mat B = arma::inv(A);  
A.print("A = \n");  
B.print("inv（A） = \n");  
return 0;  
}  
```

在项目属性中的链接器中添加 libblas.lib，liblapack.lib，liblapacke.lib 编译生成。  

这里要特别注意一点，就是在运行前要将X:\\Lapack\\WIN32目录中的三个dll以及X:\\Lapack\\目录中的两个dll拷贝到生成的.exe对应目录中，否则在运行时会提示缺少dll，而报错。

好了，尽情享受Armadillo线性代数库带来的matlab算法移植的便捷吧！
