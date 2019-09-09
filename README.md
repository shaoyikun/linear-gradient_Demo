# CSS利用linear-gradient实现条纹背景图
利用纯CSS实现一个条纹banner。  
+ [点我查看原教程](https://zhuanlan.zhihu.com/p/81776753)

## 写在前面
在网页设计过程中，各种尺寸、颜色、角度的条纹图案的设计无处不在。最近有遇到一个需求：实现如图所示的一个banner，要求可以自适应:  
![banner](https://raw.githubusercontent.com/syk2018/image/master/v2-dc0b5a1887d50fe29899d43150b01079_r.jpg)  
通过阅读 **《CSS揭秘》** 和 **CSS相关规范** 找到了比较合适的解决方案。文章关于对 **linear-gradient** 的介绍思路也大部分参考于 **《CSS揭秘》**。  
最简单，最暴力的实现方法是直接添加一个 **div** 并设置 **background-image** 和其他相关属性。由于要考虑到自适应的问题，你需要利用图像编辑器，为不同分辨率的屏幕设置不同尺寸的位图。并且网络加载图片的速度也会影响用户的体验。当然你也可以使用 **SVG** 来代替位图，但是鉴于SVG复杂的语法结构，这样做显然有些事倍功半。但是现在，利用   **CSS线性渐变（linear-gradient）** 的相关知识，我们可以轻松的实现我们的需求。

## 实现思路
首先为我们的banner设置一个div：  
``` 
<div class="banner"></div>
 ```  
为了便于我们做相关的调试，我们设置banner的高度为600px，背景颜色为red：  
``` 
.banner {
    background-color: red;
    height: 600px;
} 
```
之后它是这个样子的（是的目前确实有点丑）：
![banner](https://raw.githubusercontent.com/syk2018/image/master/%E5%9B%BE%E7%89%871.png)  
CSS中 **linear-gradient()** 函数常常被用作线性渐变。最简单的，如果我们向其中添加两个颜色参数，那么linear-gradient函数会实现两种颜色的垂直线性渐变效果。比如我们传入red，blue两个颜色：
```
.banner {
    background: linear-gradient(
    red,blue
    );
    height: 600px;
}
```
那么效果是这样的：  
![banner](https://raw.githubusercontent.com/syk2018/image/master/%E5%9B%BE%E7%89%872.png)  
在linear-gradient颜色参数后还可以设置对应的渐变起始点：   
```
.banner {
    background: linear-gradient(
    red 20% ,blue 80%
    );
    height: 600px;
}
```
这样做之后div顶部的20%的区域会被填充为red，底部20%的区域会被填充为blue，而剩余的60%的区域才会有渐变效果，也就是说，两个色标被拉近了：  
![banner](https://raw.githubusercontent.com/syk2018/image/master/%E5%9B%BE%E7%89%873.png)  
如果继续改为40%和60%，两个色标会继续被拉近：  
![banner](https://raw.githubusercontent.com/syk2018/image/master/%E5%9B%BE%E7%89%874.png)  
那如果把两个色标重合到一起，会发生什么？  
```
.banner {
    background: linear-gradient(
    red 50%,blue 50%
    );
    height: 600px;
}
```
它会变成这样：    
![banner](https://raw.githubusercontent.com/syk2018/image/master/%E5%9B%BE%E7%89%875.png)  
这个在
[《CSS图像（第三版）》](https://www.w3.org/TR/css3-images/)
的规范里有提到:  
> "If multiple color-stops have the same position, they produce an infinitesimal transition from the one specified first in the rule to the one specified last. In effect, the color suddenly changes at that position rather than smoothly transitioning."
"（如果多个色标拥有相同的位置，它们之间会产生一个无限小的过度区域，过度的起止颜色分别是第一个和最后一个指定值。从效果上看，颜色会在指定的位置突然变化，而不是一个平滑的渐变过程）"  
同样的原理，如果要创建超过两种颜色的条纹，也非常容易：  
```
.banner {
    background: linear-gradient(
    red 33.3%,blue 33.3%,blue 66.6%,yellow 66.6%
    );
    height: 600px;
}
```
它的效果是这样的：   
![banner](https://raw.githubusercontent.com/syk2018/image/master/%E5%9B%BE%E7%89%876.png)  
当然，为了避免每次改动条纹都要修改多处位置值，我们依然可以在CSS图像相关规范中找到捷径：  
> "If a color-stop has a position that is less than the specified position of any color-stop before it in the list, set its position to be equal to the largest specified position of any color-stop before it."
"（如果某个色标的位置值要比整个列表中在它之前的色标的位置值都要小，则该色标的位置值会被设置为它前面所有色标位置值的最大值）"
这意味着如果把第二个色标的位置值设置为0，那么它的位置就总会被调整为前一个色标的位置值。   

也就是说这两种写法实现的效果是一样的： 
```
.banner {
    background: linear-gradient(
    red 50%,blue 50%
    );
    height: 600px;
}
```
将blue的位置值设置为0:  
```
.banner {
    background: linear-gradient(
    red 50%,blue 0
    );
    height: 600px;
}
```
因此之前我们的三色条纹可以这样写：   
```
.banner {
 background: linear-gradient(
 red 33.3%,blue 0,blue 66.6%,yellow 0
 );
 height: 600px;
}
```
回想我们最早的需求，把代码中对应的颜色用我们需求中需要的颜色替代：   
```
.banner {
    background: linear-gradient(
    #0E4174 33.3%,#177ABB 0,#177ABB 66.6%,white 0
    );
    height: 600px;
}
```
你会发现我们离目标已经越来越近了：   
![banner](https://raw.githubusercontent.com/syk2018/image/master/%E5%9B%BE%E7%89%877.png)   
只不过现在它是水平方向的，而我们要让它变为倾斜方向。  
好在linear-gradient函数还可以接收一个角度参数。利用这个参数你可以轻松地实现斜向渐变。根据估算，我们设置这个角度值为145deg：   
```
.banner {
    background: linear-gradient(
    145deg,
    #0E4174 33.3%,#177ABB 0,#177ABB 66.6%,white 0
    );
    height: 600px;
}
```
效果已经相当接近了：   
![banner](https://raw.githubusercontent.com/syk2018/image/master/%E5%9B%BE%E7%89%878.png)  
现在我们只需要根据需求中的图片对各颜色的位置值做微调就可以了：   
```
.banner {
     background: linear-gradient(
     145deg,
     #0E4174 40%,
     #177ABB 0,
     #177ABB 80%,
     white 0
     );
     height: 600px;
}
```
最终的效果：  
![banner](https://raw.githubusercontent.com/syk2018/image/master/%E5%9B%BE%E7%89%879.png)  
## 参考资料
[CSS揭秘](https://book.douban.com/subject/26745943/)  
[CSS3图像（第三版）](https://www.w3.org/TR/css3-images/)