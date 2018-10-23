---
layout: post
title:  "Matlab绘制各种动态心形图并保存为Gif"
date:   2014-03-20 22:33 +0800
categories: Coding
---


“码农”，这个自从接触编程，就永远不可能丢掉的头衔，让多少程序猿痛并快乐着。

作为码农中的一员，却也时常给自己找些乐趣，比如最近很火的[2048游戏](http://gabrielecirulli.github.io/2048/)。

又比如：用Matlab画出一些漂亮的图片。先放一张图（注意，点击图片可以看到动态图哦）：

![heart-beats](http://cdn.mirsking.com/tools_configuration/heart-beats.gif)

还不错吧，下面就贴出源代码，造福大家：

```matlab
clc; clear  
x = linspace(-2,2,200);%200这个值越大，则画面越细腻  
[X,Y,Z] = meshgrid(x,x,x);  
I1 = (X.\^2+9/4\*Y.\^2+Z.\^2-1).\^3-X.\^2.\*Z.\^3-9/80\*Y.\^2.\*Z.\^3;  
p = patch(isosurface(X,Y,Z,I1,0));  
set(p, 'FaceColor', 'red', 'EdgeColor', 'none');  
view(3);  
axis equal ;  
axis off;  
light('Posi',[0 -2 3]); % 在(0,-2,3)点处建立一个光源  
lighting phong  
set(gca,'nextplot','replacechildren');  
% 记录电影  
XX = get(p,'XData');  
YY = get(p,'YData');  
ZZ = get(p,'ZData');  
for j = 1:20  
scale = sin(pi\*j/20);  
set(p,'XData',scale\*XX,'YData',scale\*YY,'ZData',scale\*ZZ)  
drawnow  
MakeGif('heart-beats.gif',j)  
end  
```

这里要调用MakeGif这个函数，如下：

```matlab
%% MakeGif save the pictures in the figure window to a dynamic photo.  
% example:  
% MakeGif(filename,i)  
% filename is photo's name you want to save with a extension name of  
% gif.  
% i means the ith picture of the dynamic photo. That's to say when  
% you plot the ith picture, use MakeGif(filename,i) to save it, and  
% after you finish the plot, the photo will contain all your picture.  
function MakeGif(filename,i)  
	f = getframe(gcf);  
	imind = frame2im(f);  
	[imind,cm] = rgb2ind(imind,256);  
	if i==1  
		imwrite(imind,cm,filename,'gif',
		'Loopcount',inf,'DelayTime',0.05);%感觉时间太短改这个，但是储存就很卡  
	else  
		imwrite(imind,cm,filename,'gif','WriteMode','append','DelayTime',0.1);%感觉时间太短改这个，但是储存就很卡  
	end  
end  
```

大量的注释，我就不多解释了。其原理就是每帧图像转换为index型图像数据存入gif文件中。

怎么样？不过瘾？再来一个（注意，点击图片可以看到动态图哦）：  

![heart-waves](http://cdn.mirsking.com/tools_configuration/heart-waves.gif)

同样附上源码：  

```matlab 
close all  
clear  
clc  
Times=10; [x,y]=meshgrid(-2:0.01:2);  
for i=1:Times  
	z(:,:,i)=-(17\*x.\^2-16\*y.\*abs(x)+17.\*y.\^2).\*i./5;  
end  
figure  
view([0 90]);  
hold on  
for i=1:Times  
	mesh(x,y,z(:,:,i));  
	drawnow;  
	MakeGif('heart-waves.gif',j)  
	hold on  
end  
hold off  
```

同样是调用MakeGif函数。

总结一下，核心有两个：  
1）构造漂亮的函数  
2）画图，每一帧调用一次MakeGif保存为动态图  

关于如何构造漂亮的函数，突然让我想到了笛卡尔的心型曲线了，什么时候有机会去搞搞。
