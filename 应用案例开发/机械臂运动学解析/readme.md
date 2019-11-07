# <center>机械臂运动学解析<center>  
机械臂的运动学解析分为正解和逆解。  
正解是指已知L1的旋转角度α和L2的旋转角度β，求末端点E的坐标(X,Y,Z)。
逆解是指已知末端点E的坐标(X,Y,Z)，求L1的旋转角度α和L2的旋转角度β。
下面两幅图依次是机械臂的侧视图和俯视图。  
    <div align="center">![avatar](/机械臂运动学解析/1.JPG)</div>   
    <div align="center">![avatar](/机械臂运动学解析/2.JPG)</div>   
其中，OA能左右旋转，AB、BC能上下旋转，CD始终保持水平状态，DE始终保持垂直状态。  
参数：
  
**L1 = OA ≈ 128mm，L2 = AB = 136mm，L3 = BC = 160mm，L4 = CD = 77mm，L5 = DE = 52mm**  

首先，我们来看机械臂的运动学正解，即已知α、β，求(X,Y,Z)。  
结合俯视图和侧视图可以看出，俯视图中的R等于侧视图中的AG+GH+IE。  
令**R = AC = AG + GH + IE = AB&times;cosα + CB&times;cosβ + CD = L2&times;cosα + L3&times;cosβ + L4**
所以：  
**X = R&times;cosγ = (L2&times;cosα + L3&times;cosβ + L4)&times;cosγ**  
**Y = R&times;sinγ = (L2&times;cosα + L3&times;cosβ + L4)&times;sinγ**  
**Z = OA + HI = OA + (FG - CI) = OA + [(BG - BF) - CI] = OA + AB&times;sinα - CB&times;sinβ - DE = L1 + L2&times;sinα + L3&times;sinβ - L5**  

最后，**(X,Y,Z)=((L2&times;cosα+L3&times;cosβ+L4)&times;cosγ,(L2&times;cosα+L3&times;cosβ+L4)&times;sinγ,L1+L2&times;sinα+L3&times;sinβ-L5)**  


接下来，我们看机械臂的运动学逆解，即已知(X,Y,Z)，求α、β。  
**R = &radic;(X<sup>2</sup> + Y<sup>2</sup>)**  
**γ = arctan(Y/X)**  
**AH = R - CD = &radic;(X<sup>2</sup> + Y<sup>2</sup>) - L4**  
**AC = &radic;(AH<sup>2</sup> + CH<sup>2</sup>) = &radic;(AH<sup>2</sup> + (L5 + Z - L1)<sup>2</sup>)**  
**&ang;CAB = arccos((AB<sup>2</sup> + AC<sup>2</sup> - BC<sup>2</sup>)/(2&times;AB&times;AC)) = arccos((L2<sup>2</sup> + AC<sup>2</sup> - L3<sup>2</sup>)/(2&times;L2&times;AC))**  
**&ang;CAH = arctan(CH/AH) = arctan((L5 + Z - L1)/AH)**  
**α = &ang;CAB + &ang;CAH**  
**&ang;ABC = arccos((AB<sup>2</sup>+BC<sup>2</sup>-AC<sup>2</sup>)/(2&times;AB&times;BC)) = arccos((L2<sup>2</sup>+L3<sup>2</sup>-AC<sup>2</sup>)/(2&times;L2&times;L3))**  
**&ang;ABF = 90&deg; - α**  
**&ang;CBF = &ang;ABC - &ang;ABF**   
**β = 90&deg; - &ang;CBF**
