---
title: How to achieve turning something colored to gray?
tags:
    - color
    - webpage
    - picture
category:
    - [skills]
    - [CSS]
---
# 如何实现将彩色的图片/页面变成灰白色
适用场景：灰白色的图片/网页用于哀悼/纪念等活动
原理：将彩色图片变成灰白，涉及到颜色的属性

## basic knowledge
在计算机图像中，常常考虑使用的一种颜色系统（HSB，HSL，HSV）。这个颜色系统使用三项分类，分别叫做色相（hue）、饱和度（saturation）和明度（brightness）的系数。色调决定到底哪一种颜色被使用，饱和度决定颜色的纯度，亮度决定颜色的明暗程度。
下面是一些基本概念：
- 色相（Hue）
是色彩的基本属性，就是平常所说的颜色名称，如红色、黄色等。
- 明度（lightness）
色彩明度是指色彩的亮度。颜色有深浅、明暗的变化。比如，深黄、中黄、淡黄、柠檬黄等黄颜色在明度上就不一样，血红、深红、玫瑰 红、大红、朱红、桔红等红颜色在亮度上也不尽相同。这些颜色在明暗、深浅上的不同变化，也就是色彩的又一重要特征一一明度变化。
色彩的明度变化有许多种情况，一是不同种类颜色之间的明度变化，如：在未调配过得颜色白色明度最高、黄比橙亮、橙比红亮、天蓝比藏蓝亮、红比黑亮；二是在某种颜色中，加白色明度就会逐渐提高，加黑色明度就会变暗，同时它们的饱和度就会降低；
- 饱和度（saturation）
饱和度是指色彩的鲜艳程度，也称色彩的纯度。饱和度取决于该色中含色成分和消色成分（灰色）的比例。含色成分越大，饱和度越大；消色成分越大，饱和度越小。纯的颜色都是高度饱和的，如鲜红，鲜绿。混杂上白色，灰色或其他色调的颜色，是不饱和的颜色，如绛紫，粉红，黄褐等。完全不饱和的颜色根本没有色调，如黑白之间的各种灰色
- 灰度（Grayscale）（RGB相关）
灰度色是指在纯黑与纯白之间的一系列过渡颜色。我们平常所说的黑白照片、黑白电视，实际上都应该称为灰度照片、灰度电视才确切。灰度色中不包含任何色相，即不存在红色、黄色这样的颜色。
在RGB模式中三原色光各有256个级别。由于灰度的形成是RGB数值相等。而RGB数值相等的排列组合是256个，那么灰度的数量就是256级。其中除了纯白和纯黑以外，还有254种中间过渡色。

### 如何用代码实现图片的灰白效果？
这里是使用纯CSS来实现，考虑到上面的明度、亮度、色相（HSL），首先考虑使用HSL(a,b,c)函数。
- a表示颜色种类，取值范围0-360
- b表示饱和度，取值范围0%-100%
- c表示明度，取值范围0%-100%
这个是不同亮度下，颜色的变化情况。当c为0/1时，颜色变为黑色/白色。
<!-- ![](How-to-achieve-turning-something-colored-to-gray/hsl-lightness.png) -->
{% asset_img hsl-lightness.png %}

这个是不同饱和度下，颜色变化情况。当b为0时，颜色变为灰度色。
{% asset_img hsl-saturation.png %}

所以，由上可知，我们可以改变颜色的饱和度为0，以达到图像变成灰度图像的效果。<!--要改-->

不过，只改变hsl()的一个值，而不变其他的值并不容易。这里我们借助 filter 属性<!--要改-->
设置元素的颜色不饱和度为0，即可实现灰度色的效果。
```css
*{
    filter: saturate(0);
}
```
此外，可以直接设置灰度值。grayscale()提供了灰度的变化过程，设置为1时灰度最大，为0保持原始状态。<!--要改-->
```css
*{
    filter: grayscale(1);
}
```

示例：
原始状态
{% asset_img origin.png %}
调节不饱和度
{% asset_img bysaturation.png %}
调节灰度值
{% asset_img bygrayscale.png %}

综上，简单来说，让彩色的图片/网页变成灰白色，可以一步到位将不饱和度设置为0，或者有调节的需求就设置grayscale()的百分比值。
<!--结论和验证方法：not要改-->
1. 用于整个网页的
方法如上。

2. 仅用于单张图片（如头像）
方法如上

3. 特殊情况：页面整体灰白时，取消局部的灰白效果
比如：
目前想到的是用:not()伪类选择器来排除部分不需要变为灰度色的。
{% asset_img partly-eg.png %}

使用其他（非代码）方法实现图片的颜色转换？
使用一些在线工具、图像编辑软件等皆可实现。

分享<!--后面接实践部分博文-->

**参考链接:**
1. https://baike.baidu.com/item/色彩明度
2. https://baike.baidu.com/item/色彩饱和度
3. https://baike.baidu.com/item/灰度色彩模式/5158291
4. https://zh.wikipedia.org/wiki/HSL和HSV色彩空间
5. https://developer.mozilla.org/zh-CN/docs/Web/CSS/color_value
6. http://hk.uwenku.com/question/p-trjenixj-sr.html
7. https://stackoverflow.com/questions/1625681/dynamically-change-color-to-lighter-or-darker-by-percentage-css-javascript
8. https://css-tricks.com/almanac/properties/f/filter/
9. https://developer.mozilla.org/zh-CN/docs/Web/CSS/filter
10. https://developer.mozilla.org/zh-CN/docs/Web/CSS/filter-function/grayscale