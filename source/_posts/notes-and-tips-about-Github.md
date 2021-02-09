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

## 修复一个 git 代理的问题（疑似）
最近在将本地新建的仓库推送到远程时，遇到如下的问题，输入了正确的用户名和密码却认证失败：
> Fatal: AggregateException encountered.
remote: Invalid username or password.
fatal: Authentication failed for 'xxx'

经过一番[搜索](https://github.com/Microsoft/Git-Credential-Manager-for-Windows/issues/57)，使用如下命令，可以在<code>.git\credential.log</code>中查看日志寻求原因
```
git config credential.writelog true
```
查看日志后发现，可能是如下问题
```
---> (内部异常 #0) System.Net.Http.HttpRequestException: 发送请求时出错。 ---> System.Net.WebException: 请求被中止: 未能创建 SSL/TLS 安全通道。
```
问题类似[这里](https://github.com/microsoft/Git-Credential-Manager-for-Windows/issues/613#issuecomment-383428591)，可能是 TLS 1.2 的问题，有可能是[GCM的版本过老](https://github.com/microsoft/Git-Credential-Manager-for-Windows/issues/613#issuecomment-383433233)所致（不过，19年才装的，感觉不像是）或者是 [proxy 不支持TLS 1.2](https://github.com/microsoft/Git-Credential-Manager-for-Windows/issues/613#issuecomment-411549266)（有一定可能）

后面重装了Git，重新<code>git push</code>出现了如下的问题
```
remote: Invalid username or password.
fatal: Authentication failed for 'xxx'
```
看来问题是没有解决。
思考最近做过的事情，设置了2FA；切换了代理客户端 V2rayN 至 clash，重设了git 的 http/https 代理。遂尝试取消代理（结合上文）。
```
git config --global --unset http.proxy
git config --global --unset https.proxy
```
再次 <code>git push</code> 远程分支，成功无报错。

**思考**：问题虽然暂时解决了，但是还是避免不了使用代理。有可能是clash客户端配置的问题或者其他原因（2FA），仍待进一步解决。