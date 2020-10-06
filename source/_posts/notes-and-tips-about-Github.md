---
title: notes and tips about Github
tags:
  - Github
  - tips
comments: true
category:
  - ['skills ']
  - ['notes ']
date: 2020-06-24 23:09:12
update: 2020-06-24 23:09:12
toc: true
---

# 一些关于Github的tips
<!--more-->
记录关于Github上有趣、有帮助的小tips~~

**索引**
- [在profile显示编码时间统计](#waka)
- [加速 git clone 下载](#accelerateGit)

## <a id="waka"></a>在 profile 显示编码时间统计
实例图片如下：
{% asset_img example.png %}
具体的操作步骤可以根据 [这个项目](https://github.com/matchai/waka-box) 的README操作，
在此补充一些细节：
1. 在 wakatime 是设置你想统计（编码）时间的编辑器/IDE等，具体可以跟着指示操作。

2. 在设置 schedule.yml 时，除了按照README外，需要设置 uses: 你的Github username/waka-box@master

3. 完成准备工作后，要让统计面板显示在 profile，只需在右上角的 Customize your pin 中选择显示即可。

## <a id="accelerateGit"></a>加速 git clone 下载
出于一些原因，git clone 下载 Github 上大型项目的速度十分缓慢，解决方法可以是为 git 添加上全局代理。
代码示例如下，分别为 http 和 https 的链接地址加上代理
```git
git config --global http.proxy socks://127.0.0.1:1080 #代理本地监听协议，地址和端口
git config --global https.proxy socks://127.0.0.1:1080
```
取消代理设置
```git
git config --global --unset https.proxy #global
git config --unset https.proxy          #once
```
参考图片，设置代理前
{% asset_img before.png %}
设置代理后
{% asset_img after.png %}

参考链接：
- [加速 git clone](https://www.zhihu.com/question/27159393)