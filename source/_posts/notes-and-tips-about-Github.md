---
title: notes and tips about Github
tags:
  - Github
  - tips
comments: true
category:
  - - skills
  - - notes
date: 2020-06-24 23:09:12
update: 2020-06-24 23:09:12
---

# 一些关于Github的tips
记录关于Github上有趣、有帮助的小tips~~

**索引**
- [在profile显示编码时间统计](#waka)

### <a id="waka"></a>在profile显示编码时间统计
实例图片如下：
{% asset_img example.png %}
具体的操作步骤可以根据[这个项目](https://github.com/matchai/waka-box)的README操作，
在此补充一些细节：
- 在wakatime是设置你想统计（编码）时间的编辑器/IDE等，具体可以跟着指示操作。
- 在设置 schedule.yml 时，除了按照README外，需要设置<code>uses: 你的Github username/waka-box@master</code>
- 完成准备工作后，要让统计面板显示在profile，只需在右上角的 Customize your pin 中选择显示即可。