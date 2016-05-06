---
layout: post
title:  "Robotics Toolbox for MATLAB学习笔记（二）"
date: 2014-02-05 10:03 +0800
categories: 机器人
---

 

距离上次写学习笔记已经一个星期了，最近懒惰了，呵呵。重新走起！今天虽然是情人节+元宵节，但是学习丝毫不能落下。下面言归正传：

首先贴出[Robotics Toolbox for MATLAB学习笔记（一）](http://www.mirsking.com/programingalgorithm/learning-robotics-toolbox-for-matlab/ "Robotics Toolbox for MATLAB学习笔记（一）")的地址（点击链接直达）。

第三天（2014,2,14）操作臂逆运动学总结

## 操作臂逆运动学之——ikine()函数的学习

首先总结下前几天学习但没有总结的操作臂逆运动学函数ikine()。

所谓逆运动学是由操作臂的位姿求取对应的角坐标参数。

robotics工具箱中有ikine()函数可以方便实现。同时对于puma560这种六自由度的操作臂，还有更方便的ikine6s()函数。

### ikine6s()函数的参数

由于逆运动学解的不唯一性，ikine6s()函数提供了一些参数以限定解的形式。如下：

```
left or right handed	'l' , 'r'  
elbow up or down	'u' , 'd'  
wrist flipped or not flipped	'f' , 'n'
```

另外，如果无解，则该函数会返回NaN

### ikine()函数的参数

而对于ikine()函数。其调用形式有三种，如下：

```
Q = R.ikine(T)
Q = R.ikine(T, Q0, OPTIONS)
Q = R.ikine(T, Q0, M, OPTIONS)
```

其中，T是位姿矩阵；Q0是角坐标的初始估计；M是Mask矩阵，使用它表示只取矩阵中为1对应的角度。

而OPTIONS参数比较多。如下：

``` 
'pinv' use pseudo-inverse instead of Jacobian transpose  
'ilimit',L set the maximum iteration count (default 1000)  
'tol',T set the tolerance on error norm (default 1e-6)  
'alpha',A set step size gain (default 1)  
'novarstep' disable variable step size  
'verbose' show number of iterations for each point  
'verbose=2' show state at each iteration  
'plot' plot iteration state versus time  
```

好了，今天的逆运动学函数就总结到这里，最后的参数没来得及翻译，下次有机会再补上吧。

另外，下一阶段将进行，轨迹规划相关内容的学习和总结。监督自己加把劲吧！
