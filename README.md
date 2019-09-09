# CSS利用linear-gradient实现条纹背景图
利用纯CSS实现一个条纹banner。  
+ [点我查看教程原地址](https://zhuanlan.zhihu.com/p/81776753)

## 写在前面
在网页设计过程中，各种尺寸、颜色、角度的条纹图案的设计无处不在。最近有遇到一个需求：实现如图所示的一个banner，要求可以自适应:  
![banner](https://raw.githubusercontent.com/syk2018/image/master/v2-dc0b5a1887d50fe29899d43150b01079_r.jpg)  
通过阅读 **《CSS揭秘》** 和 **CSS相关规范** 找到了比较合适的解决方案。文章关于对 **linear-gradient** 的介绍思路也大部分参考于 **《CSS揭秘》**。  
最简单，最暴力的实现方法是直接添加一个 **div** 并设置 **background-image** 和其他相关属性。由于要考虑到自适应的问题，你需要利用图像编辑器，为不同分辨率的屏幕设置不同尺寸的位图。并且网络加载图片的速度也会影响用户的体验。当然你也可以使用 **SVG** 来代替位图，但是鉴于SVG复杂的语法结构，这样做显然有些事倍功半。但是现在，利用   **CSS线性渐变（linear-gradient）** 的相关知识，我们可以轻松的实现我们的需求。