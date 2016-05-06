---
layout: post
title:  "Robotics Toolbox for MATLAB学习笔记（一）"
date: 2014-02-05 10:03 +0800
categories: 机器人
---

Robotics Toolbox for MATLAB是由Peter I. Corke编写的第三方工具箱。提供了机器人轨迹规划的各种函数。

从今天开始要学习该工具箱并能熟练掌握。本文将记录每日的学习进度，以供将来查阅。

### 第一天（2014,2,5）Robotics Toolbox的安装与简单函数的学习

根据[官网的说明](http://www.petercorke.com/Robotics_Toolbox.html)，安装步骤如下：

1.  Download it from here in zip format (.zip).
2.  The Toolbox is tested with MATLAB R2011a.
3.  To install the Toolbox simply unpack the archive which will create
    the directory (folder) rvctools, and within that the directories
    robot, simulink, and common.
4.  Adjust your MATLABPATH to include rvctools Execute the startup file
    rvctools/startup\_rvc.m and this will place the correct directories
    in your MATLAB path.
5.  Run the demo rtbdemo to see what it can do
6.  To get the MEX version of rne visit the folder rvctools/robot/mex
    and follow the directions in the README file

安装完成后，即可正常使用工具箱中的相关函数。本次测试了以下几个简单函数：rotx，roty，rotz，rpy2tr，tr2rpy等几个函数。


这里简单介绍下rpy2tr函数：

```matlab
R = RPY2R(RPY, OPTIONS) is an orthonormal rotation matrix  
equivalent to the  specified roll, pitch, yaw angles  
which correspond to rotations about the X, Y, Z axes respectively.  
If RPY has multiple rows they are assumed torepresent  
a trajectory and R is a three dimensional matrix,  
where the last index corresponds to the rows of RPY.
```

就是说R = RPY2R(RPY, OPTIONS)是将欧拉角RPY（这个RPY是由分别绕X、Y、Z轴旋转的角度roll、pitch、yaw表示的）转换为旋转矩阵。如果RPY是多列矩阵，那么结果R表示的是最后一次的旋转矩阵。

另外这个函数有两个选项，‘deg’表示以度数为单位输入；‘zyx’表示按Z、Y、X轴旋转得到旋转矩阵。



### 第二天（2014,2,7）Link() SerialLink()  fkine()函数学习

#### link函数的调用格式 

```matlab
L = LINK([theta D A alpha])

L =LINK([theta D A alphaD sigma])

L =LINK([theta D A alpha sigma offset])

L =LINK([theta D A alpha], CONVENTION)

L =LINK([theta D A alpha sigma], CONVENTION)

L =LINK([theta D A alpha sigma offset], CONVENTION)
```

参数CONVENTION可以取‘standard’和‘modified’，其中‘standard’代表采用标准的D-H参数，‘modified’代表采用改进的D-H参数。参数‘alpha’代表扭转角，参数‘A’代表杆件长度，参数‘theta’代表关节角，参数‘D’代表横距，参数‘sigma’代表关节类型：0代表旋转关节，非0代表移动关节。另外LINK还有一些数据域：

```matlab
LINK.alpha %返回扭转角

LINK.A %返回杆件长度

LINK.theta %返回关节角

LINK.D %返回横距

LINK.sigma %返回关节类型

LINK.RP %返回‘R’(旋转)或‘P’(移动)

LINK.mdh %若为标准D-H参数返回0，否则返回1

LINK.offset %返回关节变量偏移

LINK.qlim %返回关节变量的上下限 [min max]

LINK.islimit(q) %如果关节变量超限，返回 -1, 0, +1

LINK.I %返回一个3×3 对称惯性矩阵

LINK.m %返回关节质量

LINK.r %返回3×1的关节齿轮向量

LINK.G %返回齿轮的传动比

LINK.Jm %返回电机惯性

LINK.B %返回粘性摩擦

LINK.Tc %返回库仑摩擦

LINK.dh return legacy DH row

LINK.dyn return legacy DYN row
```

特别要注意的是Link函数的参数顺序在新版本的Toolbox中发生了变换，由之前的L
= LINK([alpha A theta D])变为L = LINK([theta D  A  alpha ])

#### SerialLink函数的调用格式 

顾名思义，该函数是将多个由Link生成的连杆连接起来。

```matlab
bot = SerialLink([L1 L2 L3], 'name', 'my robot');
```

如果连杆是由L1 L2 等表示，则按照上式调用。

或者连杆有L(1) L(2)等表示，则可简洁地表示为

```matlab
bot = SerialLink(L 'name', 'my robot');
```

#### fkine函数的调用格式

fkine函数是用来计算操作臂正向运动矩阵（forward kinematics），其调用格式为：

```matlab
bot.fkine([pi/4 -pi/4]) %该式表示将bot操作臂的两个连杆分别旋转pi/4和 -pi/4。
```

#### plot函数的调用格式 

SerialLink函数还有一个plot方法，该方法可以直观地将操作臂的运动状态以图形的形式表示。其调用方式和fkine函数类似。
