---
title: Thoughts after the first translation
tags:
  - thoughts
  - web
  - tips
  - translation
comments: true
category:
  - - notes
date: 2020-07-26 22:45:16
update: 2020-07-26 22:45:16
---

# 第一篇译文后的思考与记录

最近参与了 [掘金翻译计划](https://github.com/xitu/gold-miner)，翻译了第一篇 [译文](https://github.com/xitu/gold-miner/blob/master/article/2020/javascript-tips-child-constructors-text-selection-inline-workers-and-more.md) -- [英文原文地址](https://medium.com/front-end-weekly/javascript-tips-child-constructors-text-selection-inline-workers-and-more-606bc050ee24)，主要就是介绍写 Javascript 应用时，可能遇到的“难题”的解决方法。

通篇看下来，并没有看到有什么特别难的问题，感觉更像是日常遇到的小问题的总结记录。比如：
```
- 为什么设置原型的构造函数是必要的？
- 取消双击后的文本选中
- 如何去除字符串内的文件扩展名
- 使用三目运算符时省略第二个表达式
- 清除 Yarn 中的缓存
- innerText 在 IE 中有效，但在其他浏览器中无效
- 为什么在 Array.prototype.map 里使用 parseInt 会返回 NaN？
- 无需单独的 Javascript 文件即可创建 Web Workers
```
这几项，都是比较简单的东西了，如果不知道的话，Google 一下也能迅速找到解决方案。也许，是因为
> some **solutions** to common JavaScript problems

所以，文章侧重于解决方案（具体的代码），而不是原理性的东西。（实质上只是一遍“搬运文章”，没有什么实质内容）

感觉翻译完这篇文章，还是略有后悔的，当初的选择也不多，没仔细看就直接承包翻译了。
后面一看，这真的能算技术文章？只不过是零碎的知识拼凑起来的四不像。私以为，还不如 “最好用的xx个 Chrome 扩展推荐”，这种文章，毕竟偶尔也能看到一些耳目一新的东西。

要说收获，也还是有的。
- 翻译一篇英语文章，也能锻炼一下自己的英语能力，也勉强算是有所产出。如果能从中学习到一些知识，那更是划算。
- 熟悉一下文档/文章的一些格式规范
- 了解了双击可以选中单词/短语，算是一个 tip

感觉以后翻译还是自己去选文会比较好，掘金社区的翻译文章最近的大部分貌似都是一个人选出来，质量难免层次不齐。当然，也有推荐好文的机制，文章经过推荐人的筛选后供他人翻译，而且数量也不会很多。

不过，自己从哪些来源挑选高质量的文章也是一个问题。。。
{% asset_img articles.png %}